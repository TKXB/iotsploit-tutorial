API
===

API Reference
=============

This document provides detailed information about the IoTSploit REST API endpoints. The API allows you to interact with devices, vehicles, plugins, and test functionalities.

Device Management APIs
--------------------

Get Device Information
~~~~~~~~~~~~~~~~~~~~

.. http:get:: /api/device/info

   Get device information including battery, WiFi, and system details.

   **Example Response**:

   .. code-block:: json

      {
          "设备名称": "device-hostname",
          "后台WIFI": "wifi-ssid",
          "WIFI密码": "wifi-password",
          "后台IP": "192.168.1.1",
          "电池电量": "85% 充电中",
          "核心温度": "45°C"
      }

List All Devices
~~~~~~~~~~~~~~~

.. http:get:: /api/device/list

   Get a list of all registered devices.

   **Example Response**:

   .. code-block:: json

      {
          "status": "success",
          "devices": [
              {
                  "device_id": "dev1",
                  "name": "CAN Device",
                  "device_type": "can",
                  "source": "usb",
                  "vendor_id": "0483",
                  "product_id": "5740",
                  "attributes": {
                      "speed": "500000"
                  }
              }
          ]
      }

Scan Devices
~~~~~~~~~~~

.. http:get:: /api/device/scan

   Scan for all available devices.

   **Example Response**:

   .. code-block:: json

      {
          "status": "success",
          "devices_found": true,
          "devices": [
              {
                  "id": "can0",
                  "type": "socketcan",
                  "status": "connected"
              }
          ]
      }

Scan Specific Device
~~~~~~~~~~~~~~~~~~~

.. http:get:: /api/device/scan/(string:driver_name)

   Scan for devices using a specific device driver.

   :param driver_name: Name of the device driver (e.g., 'drv_socketcan')

   **Example Response**:

   .. code-block:: json

      {
          "status": "success",
          "driver": "drv_socketcan",
          "devices_found": true,
          "devices": [
              {
                  "id": "can0",
                  "type": "socketcan",
                  "bitrate": 500000
              }
          ]
      }

Initialize Devices
~~~~~~~~~~~~~~~~

.. http:get:: /api/device/initialize

   Initialize all available devices.

   **Example Response**:

   .. code-block:: json

      {
          "status": "success",
          "results": {
              "drv_socketcan": "Initialized",
              "drv_serial": "Initialized"
          }
      }

Cleanup Devices
~~~~~~~~~~~~~

.. http:get:: /api/device/cleanup

   Clean up all device connections and reset states.

   **Example Response**:

   .. code-block:: json

      {
          "status": "success",
          "results": {
              "drv_socketcan": "Cleaned up",
              "drv_serial": "Cleaned up"
          }
      }

Vehicle Management APIs
---------------------

Get Vehicle Information
~~~~~~~~~~~~~~~~~~~~~

.. http:get:: /api/vehicle/info

   Get current vehicle profile information.

   **Example Response**:

   .. code-block:: json

      {
          "描述": "Test Vehicle",
          "车型": "Model X"
      }

Select Vehicle Profile
~~~~~~~~~~~~~~~~~~~~

.. http:post:: /api/vehicle/select

   Select a vehicle profile.

   **Example Request**:

   .. code-block:: json

      {
          "user_select": "Vehicle Model X"
      }

   **Example Response**:

   .. code-block:: json

      {
          "status": "User Select Vehicle Profile: Vehicle Model X",
          "action": "GET vehicle_info",
          "result": true
      }

Get OTA Information
~~~~~~~~~~~~~~~~~

.. http:get:: /api/vehicle/ota/info

   Get OTA and version information.

   **Example Response**:

   .. code-block:: json

      {
          "current_version": "1.2.3",
          "latest_version": "1.2.4",
          "update_available": true
      }

Plugin Management APIs
--------------------

List Plugins
~~~~~~~~~~~

.. http:get:: /api/plugins

   Get a list of all available plugins.

   **Example Response**:

   .. code-block:: json

      {
          "plugins": [
              "can_fuzzer",
              "diagnostic_scanner",
              "vulnerability_scanner"
          ]
      }

Execute Plugin
~~~~~~~~~~~~

.. http:post:: /api/plugins/execute

   Execute a specific plugin.

   **Example Request**:

   .. code-block:: json

      {
          "plugin_name": "can_fuzzer",
          "parameters": {
              "interface": "can0",
              "speed": 500000
          }
      }

   **Example Response**:

   .. code-block:: json

      {
          "status": "success",
          "result": {
              "success": true,
              "message": "Plugin executed successfully",
              "data": {
                  "found_vulnerabilities": 2,
                  "scan_time": "00:02:34"
              }
          }
      }

Execute Plugin Asynchronously
~~~~~~~~~~~~~~~~~~~~~~~~~~

.. http:post:: /api/plugins/execute/async

   Execute a plugin asynchronously and get a task ID.

   **Example Request**:

   .. code-block:: json

      {
          "plugin_name": "vulnerability_scanner",
          "parameters": {
              "target": "ECU1"
          }
      }

   **Example Response**:

   .. code-block:: json

      {
          "status": "success",
          "task_id": "task_12345",
          "message": "Async execution started",
          "websocket_url": "/ws/exploit/task_12345/"
      }

Stop Async Plugin
~~~~~~~~~~~~~~~

.. http:post:: /api/plugins/stop

   Stop an asynchronously running plugin.

   **Example Request**:

   .. code-block:: json

      {
          "task_id": "task_12345"
      }

   **Example Response**:

   .. code-block:: json

      {
          "status": "success",
          "message": "Task task_12345 stopped successfully"
      }

Plugin Group Management
---------------------

Create Plugin Group
~~~~~~~~~~~~~~~~~

.. http:post:: /api/groups/create

   Create a new plugin group.

   **Example Request**:

   .. code-block:: json

      {
          "group_name": "ECU_Tests",
          "group_description": "Tests for ECU vulnerabilities",
          "selected_plugins": ["can_fuzzer", "diagnostic_scanner"],
          "nest_group": true,
          "parent_group_name": "Vehicle_Tests",
          "force_exec": true
      }

   **Example Response**:

   .. code-block:: json

      {
          "status": "success",
          "message": "Successfully created group 'ECU_Tests'",
          "group": {
              "name": "ECU_Tests",
              "description": "Tests for ECU vulnerabilities",
              "enabled": true,
              "plugins": ["can_fuzzer", "diagnostic_scanner"],
              "plugins_count": 2,
              "parent_group": "Vehicle_Tests",
              "force_exec": true
          }
      }

Delete Plugin Group
~~~~~~~~~~~~~~~~~

.. http:post:: /api/groups/delete

   Delete a plugin group.

   **Example Request**:

   .. code-block:: json

      {
          "group_name": "ECU_Tests"
      }

   **Example Response**:

   .. code-block:: json

      {
          "status": "success",
          "message": "Successfully deleted group: ECU_Tests"
      }

List Groups
~~~~~~~~~~

.. http:get:: /api/groups

   Get a list of all plugin groups and their relationships.

   **Example Response**:

   .. code-block:: json

      {
          "status": "success",
          "message": "Found 2 plugin groups",
          "groups": [
              {
                  "name": "Vehicle_Tests",
                  "description": "Vehicle-level tests",
                  "enabled": true,
                  "plugins": [
                      {
                          "name": "vehicle_scan",
                          "enabled": true,
                          "description": "Vehicle scanning plugin"
                      }
                  ],
                  "parent_groups": [],
                  "child_groups": [
                      {
                          "name": "ECU_Tests",
                          "force_exec": true
                      }
                  ]
              }
          ]
      }

Error Responses
-------------

All API endpoints may return the following error responses:

.. code-block:: json

    {
        "status": "error",
        "message": "Error description"
    }

Common HTTP status codes:

- 200: Success
- 400: Bad Request
- 404: Not Found
- 405: Method Not Allowed
- 500: Internal Server Error

WebSocket Support
---------------

Some operations support real-time updates through WebSocket connections:

- Plugin execution progress updates: ``ws://<host>/ws/exploit/<task_id>/``
- Device status updates: ``ws://<host>/ws/device/<device_id>/``

Authentication
------------

Currently, the API does not require authentication. However, it's recommended to implement appropriate authentication mechanisms in production environments.

Rate Limiting
-----------

The API currently does not implement rate limiting. Consider implementing rate limiting in production environments to prevent abuse.
