M.2 KEYE Pinout Reference
========================

The IoTSploit hardware uses M.2 KEYE connectors for modular expansion. This guide explains the pinout configurations for both the Main Board and Function Board interfaces.

.. warning::
   The Main Board and Function Board have different pinouts and CANNOT be mixed or interchanged. Connecting boards incorrectly may cause permanent hardware damage.

Main Board Pinout
----------------

Overview
^^^^^^^^
The Main Board M.2 KEYE connector features 75 pins arranged in two rows:

* Left side: Odd-numbered pins (1-75)
* Right side: Even-numbered pins (2-74)

Pin Categories
^^^^^^^^^^^^^
The pins are organized into several functional groups:

USB Interface
"""""""""""
* USB Data (D+/D-): Pins 3-6
* USB Power (USB_VIN): Pin 9
* USB Host Interface: Pins 35-38

Power and Ground
""""""""""""""
* 3.3V Power: Pins 2, 73
* Ground (GND): Pins 1, 7, 33, 39, 45, 75
* Battery Input (BATT_VIN): Pin 49

Communication Interfaces
""""""""""""""""""""""
* I2C: SDA (11), SCL (13), ~{INT} (16)
* UART: Multiple interfaces on pins 13-22
* SPI: Multiple interfaces including SDO/SDI
* CAN Bus: RX (41), TX (43)
* Ethernet: TX+/- and RX+/- pairs

Complete Main Board Pinout Table
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
.. list-table:: M.2 KEYE Connector Pinout
   :header-rows: 1
   :widths: 10 15 20 20 15 10

   * - Pin
     - Connection
     - Function 1
     - Function 2
     - Connection
     - Pin
   * - 1
     - 
     - GND
     - 3.3V
     - 
     - 2
   * - 3
     - 
     - USB_D+
     - 3.3V_EN
     - 
     - 4
   * - 5
     - 
     - USB_D-
     - RESET
     - 
     - 6
   * - 7
     - 
     - GND
     - G11
     - O
     - 8
   * - 9
     - 
     - USB_VIN
     - D0
     - 
     - 10
   * - 11
     - 
     - ~{BOOT}
     - I2C_SDA
     - 
     - 12
   * - 13
     - 
     - RTS1
     - I2C_SCL
     - 
     - 14
   * - 15
     - 
     - CTS1
     - I2C_~{INT}
     - 
     - 16
   * - 17
     - 
     - TDI
     - D1/CAM_TRIG
     - 
     - 18
   * - 19
     - 
     - TDO
     - RX2
     - 
     - 20
   * - 21
     - 
     - TCK/SWDCK
     - TX2
     - 
     - 22
   * - 23
     - 
     - TMS/SWDIO
     - /
     - 
     - 24
   * - 25
     - 
     - /
     - /
     - 
     - 26
   * - 27
     - 
     - /
     - /
     - 
     - 28
   * - 29
     - 
     - /
     - /
     - 
     - 30
   * - 31
     - 
     - /
     - TRST
     - 
     - 32
   * - 33
     - 
     - GND
     - A0
     - 
     - 34
   * - 35
     - 
     - USBHOST_D+
     - GND
     - 
     - 36
   * - 37
     - 
     - USBHOST_D-
     - A1
     - 
     - 38
   * - 39
     - 
     - GND
     - G0/BUS0
     - F1_53
     - 40
   * - 41
     - O
     - CAN-RX(CANL)
     - G1/BUS1
     - F1_55
     - 42
   * - 43
     - O
     - CAN-TX(CANH)
     - G2/BUS2
     - F1_57
     - 44
   * - 45
     - 
     - GND
     - G3/BUS3
     - F1_59
     - 46
   * - 47
     - O
     - RX3
     - G4/BUS4
     - F1_61
     - 48
   * - 49
     - 
     - BATT_VIN/3
     - TX3
     - O
     - 50
   * - 51
     - 
     - TX1
     - ETH_TX+
     - 
     - 52
   * - 53
     - 
     - RX1
     - ETH_TX-
     - 
     - 54
   * - 55
     - 
     - SPI_~{CS}
     - ETH_RX+
     - 
     - 56
   * - 57
     - 
     - SPI_SCK
     - ETH_RX-
     - 
     - 58
   * - 59
     - 
     - SPI_SDO
     - SPI_SCK1/SDIO_CLK
     - 
     - 60
   * - 61
     - 
     - SPI_SDI
     - SPI_SDO1/SDIO_CMD
     - 
     - 62
   * - 63
     - O
     - G10/ADC_D+/CAM_VSYNC
     - SPI_SDI1/SDIO_DATA0
     - 
     - 64
   * - 65
     - O
     - G9/ADC_D-/CAM_HSYNC
     - SDIO_DATA1
     - 
     - 66
   * - 67
     - O
     - G8
     - SDIO_DATA2
     - 
     - 68
   * - 69
     - F2_57
     - G7/BUS7
     - SPI_~{CS1}/SDIO_DATA3
     - 
     - 70
   * - 71
     - F2_55
     - G6/BUS6
     - RTC_3V
     - 
     - 72
   * - 73
     - F2_53
     - G5/BUS5
     - 3.3V
     - 
     - 74
   * - 75
     - 
     - GND
     - 
     - 
     - -

Function Board Pinout
-------------------

Overview
^^^^^^^^
The Function Board uses the same M.2 KEYE form factor but with a different pin assignment optimized for debugging and expansion capabilities.

Interface Groups
^^^^^^^^^^^^^^^

SPI Interfaces
"""""""""""""
* Primary SPI: SCK (3), SDI (5), SDO (7)
* Secondary SPI: SCK1 (42), SDI1 (48), SDO1 (46), ~{CS1} (50)

UART Interfaces
"""""""""""""
* UART1: TX1 (12), RX1 (14)
* UART2: TX2 (22), RX2 (20)
* Function UART: FUNC_TX (13), FUNC_RX (15), FUNC_RTS (16), FUNC_CTS (18)

Debug Interfaces
""""""""""""""
* JTAG/SWD: TDI (56), TDO (58), TCK/SWDCK (60), TMS/SWDIO (62), TRST (64)
* DFU: DFU_FC_TX (4), DFU_FC_RX (6), DFU_~{ACTIVE} (10), DFU_~{BOOT} (66), DFU_~{RST} (68)

Communication Interfaces
""""""""""""""""""""""
* I2C: SDA (19), SCL (21), ~{INT} (23)
* CAN: TX/CANH (41), RX/CANL (43)
* Ethernet: TX+/- (63, 65), RX+/- (67, 69)
* USB Slave: D+ (35), D- (37)

Power and Control
"""""""""""""""
* Power: 3.3V (73), VCC (72, 74), GND (multiple pins)
* Control: PWR_EN (71)

Complete Function Board Pinout Table
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
.. list-table:: Function Board M.2 KEYE Connector Pinout
   :header-rows: 1
   :widths: 10 20 20 10

   * - Pin
     - Function 1
     - Function 2
     - Pin
   * - 1
     - GND
     - GND
     - 2
   * - 3
     - SPI_SCK
     - DFU_FC_TX
     - 4
   * - 5
     - SPI_SDI
     - DFU_FC_RX
     - 6
   * - 7
     - SPI_SDO
     - GND
     - 8
   * - 9
     - 
     - DFU_~{ACTIVE}
     - 10
   * - 11
     - 
     - TX1
     - 12
   * - 13
     - FUNC_TX
     - RX1
     - 14
   * - 15
     - FUNC_RX
     - FUNC_RTS
     - 16
   * - 17
     - 
     - FUNC_CTS
     - 18
   * - 19
     - I2C_SDA
     - RX2
     - 20
   * - 21
     - I2C_SCL
     - TX2
     - 22
   * - 23
     - I2C_~{INT}
     - /
     - 24
   * - 25
     - /
     - /
     - 26
   * - 27
     - /
     - /
     - 28
   * - 29
     - /
     - /
     - 30
   * - 31
     - /
     - EEPROM_A2
     - 32
   * - 33
     - GND
     - EEPROM_A1
     - 34
   * - 35
     - USBSLAVE_D+
     - EEPROM_A0
     - 36
   * - 37
     - USBSLAVE_D-
     - A0
     - 38
   * - 39
     - GND
     - EEPROM_WP
     - 40
   * - 41
     - CAN-TX(CANH)
     - SPI_SCK1
     - 42
   * - 43
     - CAN-RX(CANL)
     - GND
     - 44
   * - 45
     - GND
     - SPI_SDO1
     - 46
   * - 47
     - F0/~{INT}
     - SPI_SDI1
     - 48
   * - 49
     - F1/~{CS}
     - SPI_~{CS1}
     - 50
   * - 51
     - F2/PWM
     - USB_D+
     - 52
   * - 53
     - F3
     - USB_D-
     - 54
   * - 55
     - F4
     - TDI
     - 56
   * - 57
     - F5
     - TDO
     - 58
   * - 59
     - F6
     - TCK/SWDCK
     - 60
   * - 61
     - F7
     - TMS/SWDIO
     - 62
   * - 63
     - ETH_TX+
     - TRST
     - 64
   * - 65
     - ETH_TX-
     - DFU_~{BOOT}
     - 66
   * - 67
     - ETH_RX+
     - DFU_~{RST}
     - 68
   * - 69
     - ETH_RX-
     - USB_VIN
     - 70
   * - 71
     - PWR_EN
     - VCC
     - 72
   * - 73
     - 3.3V
     - VCC
     - 74
   * - 75
     - 
     - 
     - 

Usage Notes
----------

* Always verify board type before connection
* Check voltage levels before connecting external devices
* Pay attention to ground connections when designing custom hardware
* The Function Board provides additional debugging capabilities
* Multiple interface options allow for flexible connectivity

.. note::
   The Function Board provides additional debugging capabilities through the DFU interface and expanded GPIO options (F0-F7).

.. warning::
   Always verify the board type before connection. The Function Board and Main Board have different pinouts and are not compatible with each other. 