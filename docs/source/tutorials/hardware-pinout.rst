M.2 KEYE Pinout Reference
========================

The IoTSploit hardware_main board uses an M.2 KEYE connector for modular expansion. This guide explains the pinout configuration to help you understand the hardware interface.

Pin Layout Overview
------------------

The M.2 KEYE connector features 75 pins arranged in two rows:

* Left side: Odd-numbered pins (1-75)
* Right side: Even-numbered pins (2-74)

Complete Pinout Table
-------------------

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

Pin Categories
-------------

[Rest of the tutorial remains the same...] 