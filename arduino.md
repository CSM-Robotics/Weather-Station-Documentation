This document details how the Arduino portion of the Weather Station project works, for anyone who is interested or repairing a device.

First things first: the Arduino we are using isn't an official Arduino board.  It's a [Sparkfun Pro RF](https://www.sparkfun.com/products/14916), which has a SAMD21 microcontroller on it (the same one that's in the Arduino Zero) as well as an RFM95 LoRa module.  Note that this board isn't completely compatible with the Arduino Uno or Uno derivatives, so code copied from the internet may need tweaking.

The RFM95 module speaks a wireless protocol called LoRa, which has a fairly low data rate but can transmit for much longer distances than something like Wi-Fi or Bluetooth.

The Pro RF reads from two sensors over the I2C protocol:
 - The [BME280 Atmospheric sensor](https://www.sparkfun.com/products/13676), which measures pressure, humidity, and temperature.
 - The [CCS811 Air Quality sensor](https://www.sparkfun.com/products/14193), which measures air quality.

The Pro RF also communicates with a [radiation sensor](https://www.sparkfun.com/products/14209), but not over the GPIO pins (not I2C).  This is because the radiation sensor just pulls a digital pin low when a radioactive particle strikes the sensor, which can only be turned into a radiation measurement by dividing the number of particle strikes by a given time interval (which results in our unit of counts per minute, or cpm).  We wrote a system that increments a hardware counter on the SAMD21 when a particle strike happens without any CPU intervention at all.

When the Pro RF first boots up, it initializes all three of these devices, as well as the onboard LoRa module.  If any of these devices fail to respond properly, the Pro RF will blink in a specific pattern indicating which device failed to respond: one blink means the atmospheric sensor failed to respond, two blinks mean the CCS sensor failed to respond, and three blinks means the radio module failed to respond.  The geiger sensor does not have the capability to report if it is broken or missing.

After initializing the hardware, the Pro RF waits a predetermined number of seconds before reading each sensor and sending a packet containing the sensor data and several pieces of debug information:
 - the node ID (this field is currently unused).
   - Setting up more than one weather station would require a decent amount of work. 
 - the temperature in C
 - the pressure in Pascals
 - the humidity in percent
 - the amount of CO2 in the air in ppm
 - the number of total volatile organic compounds in the air in ppb
 - the counts per minute, which is a unit for measuring radiation
 - a running total of the number of packets sent from the Arduino, which can be compared to the RasPi in order to determine if packets have been lost
 - an int representing error and device information
   - If this field is not zero, an error occurred and the packet should not be used.

The read-send-wait loop occurs forever, or until the microcontroller is reset.
