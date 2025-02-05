Basic Commands Guide
=================

This guide provides a comprehensive overview of IoTSploit's command-line interface. All commands are organized by category for easy reference.

System Commands
-------------

These commands control core system functionality:

exploit
~~~~~~~
Execute all plugins in the IoTSploit System.

.. code-block:: bash

   <IoX_SHELL> exploit

exit
~~~~
Exit the IoTSploit Shell safely, cleaning up all connections and processes.

set_log_level (sll)
~~~~~~~~~~~~~~~~~~
Set the logging level. Available levels: DEBUG, INFO, WARNING, ERROR, CRITICAL.

.. code-block:: bash

   <IoX_SHELL> set_log_level DEBUG
   # or use the shorthand
   <IoX_SHELL> sll DEBUG

Device Commands
-------------

Commands for managing and interacting with devices:

device_info
~~~~~~~~~~
Display detailed information about the IoTSploit host device.

list_devices (lsdev)
~~~~~~~~~~~~~~~~~~
List all devices stored in the database.

list_device_drivers (lsdrv)
~~~~~~~~~~~~~~~~~~~~~~~~~
List all available device plugins.

scan_devices (scan)
~~~~~~~~~~~~~~~~~
Scan for available devices and show detailed information.

initialize_devices (initdev)
~~~~~~~~~~~~~~~~~~~~~~~~~
Auto-initialize all available devices.

select_device (sd)
~~~~~~~~~~~~~~~~
Select a device for subsequent commands.

execute_device_command (dc)
~~~~~~~~~~~~~~~~~~~~~~~~
Send a command to the currently selected device.

Plugin Commands
-------------

Commands for managing plugins and plugin groups:

list_plugins (lsp)
~~~~~~~~~~~~~~~~
List all available plugins.

execute_plugin (exec)
~~~~~~~~~~~~~~~~~~~
Execute a specific plugin.

flash_plugins (fp)
~~~~~~~~~~~~~~~~
Refresh and reload all plugins from the plugins directory.

create_group (cg)
~~~~~~~~~~~~~~~
Create a plugin group and add selected plugins to it.

execute_group (eg)
~~~~~~~~~~~~~~~~
Execute plugins in a selected group.

list_groups (lg)
~~~~~~~~~~~~~~
List all available plugin groups.

delete_group (dg)
~~~~~~~~~~~~~~~
Delete an existing plugin group.

Target Commands
-------------

Commands for managing targets:

list_targets (lst)
~~~~~~~~~~~~~~~~
List all targets stored in the database.

target_select
~~~~~~~~~~~~
Select a target from available targets.

edit_target (et)
~~~~~~~~~~~~~~
Edit an existing target in the database.

Test Commands
-----------

Commands for managing and running tests:

test_select
~~~~~~~~~~
Select a test project.

run_test
~~~~~~~
Start the selected test project.

quick_test
~~~~~~~~~
Run test project in quick mode.

Django Commands
-------------

Commands for managing the Django web interface:

runserver
~~~~~~~~
Start Django development server, Daphne WebSocket server, and Celery worker in the background.

stop_server
~~~~~~~~~~
Stop all running servers (Django, Daphne, and Celery).

Network Commands
-------------

Commands for network configuration:

connect_wifi
~~~~~~~~~~~
Connect to a WiFi network by providing SSID and password.

Firmware Commands
--------------

Commands for managing device firmware:

list_firmware (lsfw)
~~~~~~~~~~~~~~~~~~
List all available firmware.

add_firmware (addfw)
~~~~~~~~~~~~~~~~~~
Add new firmware to the system.

flash_firmware (flashfw)
~~~~~~~~~~~~~~~~~~~~~
Flash firmware to a device.

remove_firmware (rmfw)
~~~~~~~~~~~~~~~~~~~
Remove firmware from the system.

Linux Commands
------------

Standard Linux commands integrated into the shell:

ls
~~
List directory contents.

lsusb
~~~~~
List USB devices.

Command Shortcuts
---------------

Many commands have shorter aliases for convenience. Here are some common ones:

- ``lsp`` = ``list_plugins``
- ``exec`` = ``execute_plugin``
- ``fp`` = ``flash_plugins``
- ``cg`` = ``create_group``
- ``eg`` = ``execute_group``
- ``lg`` = ``list_groups``
- ``dg`` = ``delete_group``
- ``lst`` = ``list_targets``
- ``et`` = ``edit_target``
- ``lsdev`` = ``list_devices``
- ``lsdrv`` = ``list_device_drivers``
- ``sd`` = ``select_device``
- ``dc`` = ``execute_device_command``
- ``initdev`` = ``initialize_devices``
- ``scan`` = ``scan_devices``
- ``sll`` = ``set_log_level``

Getting Help
-----------

To get help on any command, use the ``help`` command followed by the command name:

.. code-block:: bash

   <IoX_SHELL> help list_plugins

To see all available commands with brief descriptions:

.. code-block:: bash

   <IoX_SHELL> help

Next Steps
---------

After familiarizing yourself with these basic commands, you might want to:

* Learn about :doc:`/tutorials/plugin_development` to create your own plugins
* Explore :doc:`/tutorials/advanced_usage` for more complex operations
* Check the :doc:`/api` documentation for programmatic access 