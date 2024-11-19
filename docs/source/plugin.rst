Plugin Development Guide
========================

Introduction
------------

This guide aims to instruct users on how to add and develop new plugins to the system based on existing plugin references. By reading this guide, you will learn how to add new plugins on the backend, how to call these plugins on the frontend, and how to manage plugin dependencies.

Environment Setup
-----------------

- IoTSploit framework

Backend Plugin Development
--------------------------

1. **Create a New Plugin File**

   In the backend plugin directory (e.g., `plugins`), create a new Python file, such as `your_plugin.py`.

2. **Write a Plugin that Conforms to `exploit_spec.py`**

   First, your plugin needs to implement the three methods defined in `ExploitPluginSpec`:

   - `initialize(self, device_plugin: Optional[Any] = None)`
   - `execute(self, target: Optional[Any] = None) -> ExploitResult`
   - `cleanup(self)`

   Below is an example showing how to write a plugin that conforms to the specification:

   .. code-block:: python

      import logging
      import pluggy
      from typing import Optional, Any
      from sat_toolkit.core.exploit_spec import ExploitResult
      from sat_toolkit.core.base_plugin import BasePlugin

      logger = logging.getLogger(__name__)
      hookimpl = pluggy.HookimplMarker("exploit_mgr")

      class MyCustomExploitPlugin(BasePlugin):
          def __init__(self):
              super().__init__({
                  'Name': 'My Custom Exploit',
                  'Description': 'A custom exploit plugin example.',
                  'License': 'Your License',
                  'Author': ['Your Name'],
                  'Parameters': {
                      'param1': {
                          'type': 'int',
                          'required': True,
                          'description': 'Description of param1.',
                      },
                      'param2': {
                          'type': 'str',
                          'required': False,
                          'description': 'Description of param2.',
                      },
                      # Add more parameters...
                  },
              })

          @hookimpl
          def initialize(self, device_plugin: Optional[Any] = None):
              # Initialize the plugin, optionally using a device plugin
              self.device_plugin = device_plugin
              logger.info("Plugin initialized with device_plugin: %s", device_plugin)

          @hookimpl
          def execute(self, target: Optional[Any] = None) -> ExploitResult:
              # Get parameters
              param1 = self.options.get('param1')
              param2 = self.options.get('param2')

              logger.info("Executing exploit with param1: %s, param2: %s", param1, param2)

              try:
                  # Execute core logic
                  result_data = self.perform_exploit(param1, param2, target)

                  # Return success result
                  return ExploitResult(success=True, message="Exploit executed successfully.", data=result_data)
              except Exception as e:
                  logger.error("Exploit execution failed: %s", e)
                  # Return failure result
                  return ExploitResult(success=False, message=str(e), data={})

          @hookimpl
          def cleanup(self):
              # Perform cleanup operations
              logger.info("Cleaning up after exploit execution.")

          def perform_exploit(self, param1, param2, target):
              # Implement your exploit logic
              # For example, perform some operations on the target
              # Return result data
              result = {
                  "target": target,
                  "param1": param1,
                  "param2": param2,
                  "status": "Exploit performed successfully."
              }
              return result

   - **Inherit from `BasePlugin`**: Ensure your plugin inherits from `BasePlugin` to gain basic plugin functionality.
   - **Use `@hookimpl` Decorator**: Use the `@hookimpl` decorator on the `initialize`, `execute`, and `cleanup` methods so that `pluggy` can correctly recognize and call them.
   - **Define Plugin Information in `__init__`**: Include name, description, license, author, and parameters in the `__init__` method. This information will be used for plugin management and frontend display.
   - **Parameter Management**: Define the required parameters in `Parameters`. The frontend will generate the parameter input interface based on this information.
   - **Exception Handling**: Use a try-except block in the `execute` method to catch exceptions, ensuring the plugin does not crash on errors and returns appropriate error messages.

   Place your plugin file in the plugin directory (e.g., `plugins`). The plugin manager will automatically discover and load plugins that conform to the specification.

Considerations
--------------

- **Follow the Specification**: Ensure your plugin method signatures match those defined in `exploit_spec.py`.
- **Consistent Naming**: The plugin name (e.g., `MyCustomExploitPlugin`) should correspond to the file name for easy management.
- **Logging**: Use the `logging` module to record the plugin execution process for easier debugging and maintenance.
- **Dependency Management**: If your plugin requires additional dependencies, add them to `pyproject.toml` and install them using Poetry.

Frontend Adjustments
--------------------

1. **Retrieve Plugin List**

   In `plugins_page.dart`, the frontend retrieves the list of available plugins from the backend via an HTTP interface. Ensure your new plugin is included in this list, with the specific interface being `/api/list_plugin_info/`.

2. **Dynamically Generate Parameter Input Interface**

   When the user selects a plugin to execute, the application will dynamically generate a parameter input popup window based on the plugin's `Parameters` information.

3. **Send Execution Request**

   After the user fills in the parameters, the frontend will package the parameters into JSON and send them to the Django backend's execution interface (e.g., `/api/execute_plugin/`). The request body includes the plugin name and parameters, for example:

   .. code-block:: json

      {
        "plugin_name": "Your Plugin Name",
        "parameters": {
          "param1": 123,
          "param2": "value"
        }
      }

4. **Handle Execution Results**

   After the backend executes the plugin, it will return the results to the frontend. The frontend needs to parse the results and display the execution status and related information to the user.

Common Issues
-------------

- **Q1: Plugin not showing in the frontend plugin list?**
  - Ensure the plugin is correctly placed in the plugin directory.
  - Check if the plugin metadata is complete.
  - Restart the backend service to ensure the plugin is loaded.

- **Q2: Plugin execution error indicating missing dependencies?**
  - Ensure necessary dependencies are added to `pyproject.toml`.
  - Run `poetry install` to install dependencies.

- **Q3: Frontend unable to get plugin parameter input interface?**
  - Check if the plugin's `Parameters` definition is correct.
  - Ensure the backend interface returns plugin information with parameter details.