# [LightBlue Bean](http://punchthrough.com/bean/) App Message Types

* Documentation version: 0.3.1
* Created: 2014-06-25
* Updated: 2014-07-29
* Copyright: [Punch Through Design](http://punchthrough.com)
* License: [The MIT License](http://opensource.org/licenses/MIT)

# Overview

This document defines the message types used in the [Serial Message Protocol](serial_message_protocol.md) (**App Message** layer —> **Message ID** field).

# Message Types

## Serial

ID Major  |  ID Minor  |  Function                         |  Sender  |  Receiver  |  Data Struct        |  Description
----------|------------|-----------------------------------|----------|------------|---------------------|--------------------------------------------------------------------------------------------------------------------
0x00      |  0x00      |  Serial Pass-Through              |  Client or Bean  |  Bean or Client    |  anything           |  Used for sending serial data to and from the Bean.

Note: Only Bean-to-Client and Client-to-Bean connections are supported at this time. Bean-to-Bean and Client-to-Client connections are not supported.

## Bluetooth


### Authentication

ID Major  |  ID Minor  |  Function                         |  Sender  |  Receiver  |  Data Struct        |  Description
----------|------------|-----------------------------------|----------|------------|---------------------|--------------------------------------------------------------------------------------------------------------------
0x05      |  0x06      |  Set                              |  Client  |  Bean      |  bt_pairing_pin     |  Set the Bluetooth pairing pin.
0x05      |  0x86      |  Set Response                     |  Bean    |  Client    |  fail_pass          |  PASS if setting the pin was successful, FAIL otherwise
0x05      |  0x07      |  Pin Required                     |  Bean    |  Client    |  notreq_req         |  REQ if the Bean requires authentication, NOTREQ otherwise
0x05      |  0x08      |  Authenticate                     |  Client  |  Bean      |  bt_pairing_pin     |  Authenticate to the Bean by providing its pairing pin.
0x05      |  0x88      |  Authenticate Response            |  Bean    |  Client    |  fail_pass          |  PASS if authentication was successful, FAIL otherwise

### Config

ID Major  |  ID Minor  |  Function                         |  Sender  |  Receiver  |  Data Struct        |  Description
----------|------------|-----------------------------------|----------|------------|---------------------|--------------------------------------------------------------------------------------------------------------------
0x05      |  0x11      |  Config Set                       |  Client  |  Bean      |  bt_config          |  Set the Bean's Bluetooth config parameters.
0x05      |  0x10      |  Config Request                   |  Client  |  Bean      |  N/A                |  Request the Bean's Bluetooth config parameters.
0x05      |  0x90      |  Config Response                  |  Bean    |  Client    |  bt_config          |  Contains the Bean's Bluetooth config parameters.

### Scratch Characteristics

ID Major  |  ID Minor  |  Function                         |  Sender  |  Receiver  |  Data Struct        |  Description
----------|------------|-----------------------------------|----------|------------|---------------------|--------------------------------------------------------------------------------------------------------------------
0x05      |  0x14      |  Set                              |  Client  |  Bean      |  bt_scratch_val     |  Set one of the Bean's scratch characteristics.
0x05      |  0x15      |  Request                          |  Client  |  Bean      |  bt_scratch_num     |  Request the value of one of the Bean's scratch characteristics.
0x05      |  0x95      |  Response                         |  Bean    |  Client    |  bt_scratch_val     |  Contains the value of one of the Bean's scratch characteristics.

### Other

ID Major  |  ID Minor  |  Function                         |  Sender  |  Receiver  |  Data Struct        |  Description
----------|------------|-----------------------------------|----------|------------|---------------------|--------------------------------------------------------------------------------------------------------------------
0x05      |  0x20      |  Restart                          |  Client  |  Bean      |  N/A                |  Not currently implemented.

## Bootloader

### Firmware

ID Major  |  ID Minor  |  Function                         |  Sender  |  Receiver  |  Data Struct        |  Description
----------|------------|-----------------------------------|----------|------------|---------------------|--------------------------------------------------------------------------------------------------------------------
0x10      |  0x00      |  Start                            |  Client  |  Bean      |  bl_start           |  Start a firmware transmission to the Bean. Contains metadata used by the Bean to prepare to receive firmware data.
0x10      |  0x01      |  Firmware Block                   |  Client  |  Bean      |  bl_firmware_block  |  Contains a block of firmware data for the Bean.
0x10      |  0x02      |  Status Request                   |  Client  |  Bean      |  N/A                |  Request the Bean's bootloader status.
0x10      |  0x82      |  Status Response                  |  Bean    |  Client    |  bl_status          |  Reports the Bean's bootloader status.

### Sketch

ID Major  |  ID Minor  |  Function                         |  Sender  |  Receiver  |  Data Struct        |  Description
----------|------------|-----------------------------------|----------|------------|---------------------|--------------------------------------------------------------------------------------------------------------------
0x10      |  0x03      |  Get Sketch Metadata              |  Client  |  Bean      |  N/A                |  Request the metadata for the last uploaded Arduino sketch.
0x10      |  0x83      |  Sketch Metadata Response         |  Bean    |  Client    |  bl_sketch_meta     |  Contains the metadata for the last uploaded Arduino sketch.

## Peripherals

### LED

ID Major  |  ID Minor  |  Function                         |  Sender  |  Receiver  |  Data Struct        |  Description
----------|------------|-----------------------------------|----------|------------|---------------------|--------------------------------------------------------------------------------------------------------------------
0x20      |  0x00      |  Write                            |  Client  |  Bean      |  led_color_one      |  Set one color of the Bean's RGB LED.
0x20      |  0x01      |  Write All                        |  Client  |  Bean      |  led_color_all      |  Set all three colors of the Bean's RGB LED.
0x20      |  0x02      |  Read All                         |  Client  |  Bean      |  N/A                |  Request the RGB color values currently displayed on the Bean's RGB LED.
0x20      |  0x82      |  Read All Response                |  Bean    |  Client    |  led_color_all      |  Contains the RGB color values currently displayed on the Bean's RGB LED.

### Accelerometer

ID Major  |  ID Minor  |  Function                         |  Sender  |  Receiver  |  Data Struct        |  Description
----------|------------|-----------------------------------|----------|------------|---------------------|--------------------------------------------------------------------------------------------------------------------
0x20      |  0x10      |  Request                          |  Client  |  Bean      |  N/A                |  Request the X, Y, and Z axis readings from the Bean's accelerometer.
0x20      |  0x90      |  Response                         |  Bean    |  Client    |  accel              |  Contains the X, Y, and Z axis readings from the Bean's accelerometer.

### Temperature

ID Major  |  ID Minor  |  Function                         |  Sender  |  Receiver  |  Data Struct        |  Description
----------|------------|-----------------------------------|----------|------------|---------------------|--------------------------------------------------------------------------------------------------------------------
0x20      |  0x11      |  Request                          |  Client  |  Bean      |  N/A                |  Request the current temperature from the Bean's temperature sensor.
0x20      |  0x91      |  Response                         |  Bean    |  Client    |  temp_c             |  The current temperature from the Bean's temperature sensor. Represented as an 8-bit signed integer in degrees Celsius.

### Battery

ID Major  |  ID Minor  |  Function                         |  Sender  |  Receiver  |  Data Struct        |  Description
----------|------------|-----------------------------------|----------|------------|---------------------|--------------------------------------------------------------------------------------------------------------------
0x20      |  0x15      |  Request                          |  Client  |  Bean      |  N/A                |  Request the current voltage of the battery connected to the VCC/GND pins of the Bean.
0x20      |  0x95      |  Response                         |  Bean    |  Client    |  batt               |  Contains the current voltage of the battery connected to the VCC/GND pins of the Bean.

### Power

ID Major  |  ID Minor  |  Function                         |  Sender  |  Receiver  |  Data Struct        |  Description
----------|------------|-----------------------------------|----------|------------|---------------------|--------------------------------------------------------------------------------------------------------------------
0x20      |  0x20      |  Set Power                        |  Client  |  Bean      |  off_on             |  Turn the Bean's Arduino on or off.
0x30      |  0x00      |  Set Sleep                        |  Client  |  Bean      |  sleep_status       |  Set the sleep state of the Bean's Arduino.
0x20      |  0x21      |  Request                          |  Client  |  Bean      |  N/A                |  Request the current power status of the Bean's Arduino.
0x20      |  0x81      |  Response                         |  Bean    |  Client    |  off_on             |  ON if the Arduino is on, OFF otherwise.

## Debug

ID Major  |  ID Minor  |  Function                         |  Sender  |  Receiver  |  Data Struct        |  Description
----------|------------|-----------------------------------|----------|------------|---------------------|--------------------------------------------------------------------------------------------------------------------
0x40      |  0x00      |  Error Message                    |  Bean    |  Client    |  error              |  Returned when the Bean encounters an error.
0xFE      |  0x00      |  Loopback Debug Message           |  Client  |  Bean      |  anything           |  
0xFE      |  0x80      |  Loopback Debug Message Response  |  Bean    |  Client    |  anything           |  
0xFE      |  0x01      |  Get Debug Counter                |  Client  |  Bean      |  counter            |  
0xFE      |  0x81      |  Get Debug Counter Response       |  Bean    |  Client    |  counter            |  
0xFE      |  0x02      |  End-to-end Loopback              |  Client  |  Client    |  anything           |  
0XFE      |  0x03      |  PTM mode communication           |  Client  |  Bean      |  anything           |  
