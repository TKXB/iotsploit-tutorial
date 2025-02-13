Plugin Development Guide
========================

Introduction
------------

This guide provides a comprehensive walkthrough on developing device driver plugins for the system. We'll use the SocketCAN driver as a reference implementation to demonstrate best practices and required components.

Plugin Architecture Overview
---------------------------

The plugin system is built on a hierarchical structure:

1. ``BasePlugin``: The root class providing basic plugin functionality
2. ``BaseDeviceDriver``: Extends ``BasePlugin`` with device-specific operations
3. Your custom driver class: Implements the specific device functionality

Key Components
-------------

1. **Device State Management**

   The system defines several device states that represent the lifecycle of a device:

   +----------------+------------------------------------------------+
   | State          | Description                                    |
   +================+================================================+
   | UNKNOWN        | Initial state                                  |
   +----------------+------------------------------------------------+
   | DISCOVERED     | Device found during scanning                   |
   +----------------+------------------------------------------------+
   | INITIALIZED    | Device initialized but not connected           |
   +----------------+------------------------------------------------+
   | CONNECTED      | Device connected and ready                     |
   +----------------+------------------------------------------------+
   | ACTIVE         | Device actively performing operations          |
   +----------------+------------------------------------------------+
   | ERROR          | Device encountered an error                    |
   +----------------+------------------------------------------------+
   | DISCONNECTED   | Device disconnected                            |
   +----------------+------------------------------------------------+

2. **Stream Management**
   - Handles real-time data streaming
   - Manages WebSocket connections
   - Processes data acquisition

Creating a Device Driver Plugin
------------------------------

Here's a step-by-step guide to creating a device driver plugin:

1. **Create Plugin File Structure**

   Create a new Python file in the ``plugins/devices/`` directory with the prefix ``drv_``:

   .. code-block:: text

      plugins/
      └── devices/
          └── your_device/
              └── drv_your_device.py

2. **Basic Plugin Structure**

   Here's a template based on the SocketCAN implementation:

   .. code-block:: python

      import logging
      from typing import Optional, Dict, List
      from sat_toolkit.core.base_plugin import BaseDeviceDriver
      from sat_toolkit.models.Device_Model import Device
      from sat_toolkit.core.stream_manager import StreamData, StreamType, StreamSource, StreamAction

      logger = logging.getLogger(__name__)

      class YourDeviceDriver(BaseDeviceDriver):
          def __init__(self):
              super().__init__()
              self.device = None
              # Define supported commands
              self.supported_commands = {
                  "start": "Start device operation",
                  "stop": "Stop device operation",
                  "status": "Get device status"
              }

3. **Implement Required Methods**

   Your plugin must implement these core methods:

   .. code-block:: python

      def _scan_impl(self) -> List[Device]:
          """Scan for available devices"""
          try:
              # Implement device discovery logic
              devices = []
              # Example: scan for devices
              # devices.append(YourDevice(...))
              return devices
          except Exception as e:
              logger.error(f"Scan failed: {str(e)}")
              raise

      def _initialize_impl(self, device: Device) -> bool:
          """Initialize the device"""
          try:
              # Implement device initialization
              # Example: setup device parameters
              return True
          except Exception as e:
              logger.error(f"Initialization failed: {str(e)}")
              raise

      def _connect_impl(self, device: Device) -> bool:
          """Connect to the device"""
          try:
              # Implement device connection logic
              return True
          except Exception as e:
              logger.error(f"Connection failed: {str(e)}")
              raise

      def _command_impl(self, device: Device, command: str, args: Optional[Dict] = None) -> Optional[str]:
          """Execute device commands"""
          try:
              if command == "start":
                  self.start_streaming(device)
                  return "Started streaming"
              elif command == "stop":
                  self.stop_streaming(device)
                  return "Stopped streaming"
              else:
                  raise ValueError(f"Unknown command: {command}")
          except Exception as e:
              logger.error(f"Command execution failed: {str(e)}")
              raise

      def _reset_impl(self, device: Device) -> bool:
          """Reset the device"""
          try:
              # Implement device reset logic
              return True
          except Exception as e:
              logger.error(f"Reset failed: {str(e)}")
              raise

      def _close_impl(self, device: Device) -> bool:
          """Close the device connection"""
          try:
              # Implement cleanup logic
              return True
          except Exception as e:
              logger.error(f"Close failed: {str(e)}")
              raise

4. **Implement Data Streaming**

   If your device supports data streaming, implement these methods:

   .. code-block:: python

      def _setup_acquisition(self, device: Device):
          """Setup for data acquisition"""
          try:
              # Initialize data acquisition resources
              pass
          except Exception as e:
              logger.error(f"Setup acquisition failed: {str(e)}")
              raise

      def _cleanup_acquisition(self, device: Device):
          """Cleanup after data acquisition"""
          try:
              # Clean up resources
              pass
          except Exception as e:
              logger.error(f"Cleanup acquisition failed: {str(e)}")
              raise

      def _acquisition_loop(self):
          """Main data acquisition loop"""
          while self.is_acquiring.is_set():
              try:
                  # Read data from device
                  data = self._read_device_data()
                  
                  # Create stream data
                  stream_data = StreamData(
                      stream_type=StreamType.YOUR_TYPE,
                      channel=self.device.device_id,
                      timestamp=time.time(),
                      source=StreamSource.SERVER,
                      action=StreamAction.DATA,
                      data=data
                  )
                  
                  # Broadcast data
                  self.stream_wrapper.broadcast_data(stream_data)
                  
              except Exception as e:
                  logger.error(f"Error in acquisition loop: {str(e)}")
                  time.sleep(0.1)

Best Practices
-------------

1. **Error Handling**
   - Use try-except blocks in all methods
   - Log errors with appropriate detail
   - Clean up resources in case of failures

2. **Resource Management**
   - Initialize resources in ``_setup_acquisition``
   - Clean up resources in ``_cleanup_acquisition``
   - Implement proper cleanup in ``_close_impl``

3. **Logging**
   - Use the logger for debugging and error tracking
   - Include relevant context in log messages
   - Use appropriate log levels (debug, info, error)

4. **State Management**
   - Track device state transitions
   - Validate state before operations
   - Handle error states appropriately

5. **Thread Safety**
   - Use thread-safe mechanisms for shared resources
   - Properly handle thread lifecycle in streaming
   - Use appropriate synchronization primitives

Integration with Device Manager
-----------------------------

The Device Manager automatically discovers and loads plugins:

1. Place your plugin in the correct directory
2. Ensure the filename starts with ``drv_``
3. Implement all required methods
4. The Device Manager will handle:
   - Plugin loading
   - State management
   - Command routing
   - Error handling

Device Lifecycle Management
-------------------------

The Device Manager implements a sophisticated lifecycle management system for devices. Understanding this system is crucial for plugin development.

1. **State Machine**

   The device lifecycle follows a state machine pattern:

   .. code-block:: text

      UNKNOWN ──────► DISCOVERED ──────► INITIALIZED ──────► CONNECTED ──────► ACTIVE
                                                               ▲                │
                                                               │                │
                                                               └────────────────┘
                                                                      ▲
                         DISCONNECTED ◄────── ERROR ◄────────────────┘

2. **State Transitions**

   Valid state transitions are strictly controlled:

   .. code-block:: python

      _state_transitions = {
          DeviceState.UNKNOWN: [DeviceState.DISCOVERED],
          DeviceState.DISCOVERED: [DeviceState.INITIALIZED],
          DeviceState.INITIALIZED: [DeviceState.CONNECTED, DeviceState.DISCONNECTED],
          DeviceState.CONNECTED: [DeviceState.ACTIVE, DeviceState.DISCONNECTED],
          DeviceState.ACTIVE: [DeviceState.CONNECTED, DeviceState.ERROR],
          DeviceState.ERROR: [DeviceState.DISCONNECTED],
          DeviceState.DISCONNECTED: [DeviceState.INITIALIZED]
      }

3. **Lifecycle Operations**

   Each operation in the device lifecycle is managed through the ``_manage_device_lifecycle`` method:

   .. code-block:: python

      # Scanning devices
      result = manager.scan_devices("drv_your_device")
      # State: UNKNOWN -> DISCOVERED

      # Initializing device
      init_result = manager.initialize_device("drv_your_device", device)
      # State: DISCOVERED -> INITIALIZED

      # Connecting device
      connect_result = manager.connect_device("drv_your_device", device)
      # State: INITIALIZED -> CONNECTED

      # Executing commands
      command_result = manager.execute_command("drv_your_device", "start", device_id=device.device_id)
      # State: CONNECTED -> ACTIVE -> CONNECTED

4. **Automatic State Management**

   The Device Manager provides automatic state management features:

   - **Auto-Discovery**: When connecting to an unknown device, it automatically triggers device scanning
   - **Auto-Initialization**: When connecting to a discovered device, it automatically performs initialization
   - **State Validation**: Ensures operations are only performed in valid states
   - **Error Handling**: Automatically transitions to ERROR state on failures

5. **Thread Safety**

   The Device Manager implements thread-safe operations:

   .. code-block:: python

      # Device-specific locks
      with self._get_device_lock(device_key):
          current_state = self.device_states.get(device_key, DeviceState.UNKNOWN)
          # Perform state transition and operation

6. **Error Recovery**

   The system provides mechanisms for error recovery:

   .. code-block:: python

      try:
          # Attempt device operation
          result = manager.execute_command("drv_your_device", "start", device_id=device.device_id)
      except Exception:
          # Reset device on error
          manager.reset_device("drv_your_device", device)
          # Reinitialize if needed
          manager.initialize_device("drv_your_device", device)

7. **Cleanup and Resource Management**

   The Device Manager provides methods for proper cleanup:

   .. code-block:: python

      # Cleanup single device
      manager.close_device("drv_your_device", device)

      # Cleanup all devices
      cleanup_results = manager.cleanup_all_devices()

8. **State Hierarchy**

   The system maintains a state hierarchy for proper state management:

   .. code-block:: python

      state_hierarchy = {
          DeviceState.UNKNOWN: 0,
          DeviceState.DISCOVERED: 1,
          DeviceState.INITIALIZED: 2,
          DeviceState.CONNECTED: 3,
          DeviceState.ACTIVE: 4
      }

Best Practices for Lifecycle Management
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

1. **State Tracking**
   - Always check device state before operations
   - Use appropriate state transitions
   - Handle state changes atomically

2. **Resource Management**
   - Initialize resources in correct state
   - Clean up resources when transitioning to DISCONNECTED
   - Use context managers for resource handling

3. **Error Handling**
   - Implement proper error recovery
   - Use appropriate state transitions on errors
   - Clean up resources on errors

4. **Thread Safety**
   - Use provided locking mechanisms
   - Avoid long operations while holding locks
   - Handle concurrent access properly

Example Lifecycle Management
~~~~~~~~~~~~~~~~~~~~~~~~~

Here's a complete example of device lifecycle management:

.. code-block:: python

    from sat_toolkit.core.device_manager import DeviceDriverManager
    from sat_toolkit.core.device_spec import DeviceState

    def manage_device_lifecycle():
        manager = DeviceDriverManager()
        
        try:
            # Scan for devices
            scan_result = manager.scan_devices("drv_your_device")
            if scan_result["status"] != "success":
                raise RuntimeError("Device scanning failed")
            
            devices = scan_result["devices"]
            if not devices:
                raise RuntimeError("No devices found")
            
            device = devices[0]
            
            # Initialize device
            init_result = manager.initialize_device("drv_your_device", device)
            if init_result["status"] != "success":
                raise RuntimeError("Device initialization failed")
            
            # Connect to device
            connect_result = manager.connect_device("drv_your_device", device)
            if connect_result["status"] != "success":
                raise RuntimeError("Device connection failed")
            
            # Execute commands
            command_result = manager.execute_command(
                "drv_your_device",
                "start",
                device_id=device.device_id
            )
            if command_result["status"] != "success":
                raise RuntimeError("Command execution failed")
            
            # Normal operation...
            
        except Exception as e:
            # Error handling
            logger.error(f"Device operation failed: {e}")
            try:
                # Attempt cleanup
                manager.close_device("drv_your_device", device)
            except Exception as cleanup_error:
                logger.error(f"Cleanup failed: {cleanup_error}")
        finally:
            # Final cleanup
            manager.cleanup_all_devices()

Example Usage
------------

Here's how to use your plugin once it's integrated:

.. code-block:: python

    from sat_toolkit.core.device_manager import DeviceDriverManager

    # Get device manager instance
    manager = DeviceDriverManager()

    # Scan for devices
    result = manager.scan_devices("drv_your_device")
    
    if result["status"] == "success":
        devices = result["devices"]
        
        # Initialize first device
        if devices:
            device = devices[0]
            init_result = manager.initialize_device("drv_your_device", device)
            
            if init_result["status"] == "success":
                # Connect to device
                connect_result = manager.connect_device("drv_your_device", device)
                
                if connect_result["status"] == "success":
                    # Execute command
                    command_result = manager.execute_command(
                        "drv_your_device",
                        "start",
                        device_id=device.device_id
                    )

Troubleshooting
--------------

1. **Plugin Not Loading**
   - Check file naming (must start with ``drv_``)
   - Verify class inheritance
   - Check for syntax errors

2. **State Transition Errors**
   - Verify correct state progression
   - Check initialization sequence
   - Ensure proper cleanup

3. **Streaming Issues**
   - Check thread management
   - Verify data format
   - Monitor resource usage

4. **Resource Leaks**
   - Implement proper cleanup
   - Use context managers where appropriate
   - Monitor system resources