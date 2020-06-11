# Weather Station Documentation
The weather station we built is designed to collect pressure, temperature, humidity, VOC and radiation measurements from an Arduino system, send it to a Raspberry Pi over LoRa, and publish that data in the cloud for other people to view.  Measurements will also be stored over time so that people can view graphs of the data.  This document provides an overview of how the weather station is put together, and the arduino and raspi documents go into more depth about the code we wrote.  Finally, the raspi maintenance document explains how to set up the RasPi from scratch.

If you'd like an API for manipulating weather data, [we have that](https://api.csmrobotics.club/api/wss/getAll).

If you'd like to know the parts we used, [here you go](https://docs.google.com/spreadsheets/d/1h0GDfVYntyYW5QakYnAJw-5-3vuBXiLji0O7EjJ50Ls/edit#gid=0).

## Safety information
- The boards are assembled with leaded solder.  Wash your hands after touching hardware.
- If the VOC or Radiation measurements are way off, the sensors are probably broken.  Check the news before scaring yourself.

## Arduino information
The Arduino system uses multiple sensors to collect data, all of which are listed below.  For more detailed information check the Sparkfun "Getting Started" guides for the respective sensors.
- The [BME280](https://www.sparkfun.com/products/13676) sensor is a combination pressure/temperature/humidity sensor. (3.3v **only**)
- The [CCS811](https://www.sparkfun.com/products/14193) sensor measures air quality levels. (3.3v **only**)
- The [Pocket Geiger Sensor](https://www.sparkfun.com/products/14209) measures beta and gamma radiation! (3.3v /  5v compatible)

The CCS811 and BME280 sensors should be connected by I2C to the Pro RF.

Additionally, the Arduino itself isn't just an Uno derivative.  It's actually a [Sparkfun Pro RF](https://www.sparkfun.com/products/14916), which is **only** 3.3v compatible and has a couple more features.
The weather station also has a [Sunny Buddy](https://www.sparkfun.com/products/12885) for managing the solar panel and LiPo battery.

## Raspberry Pi information
The Raspberry Pi 4 uses an [RFM95W](https://www.adafruit.com/product/3072) in order to recieve data from the Arduino over LoRa.  The actual reading, parsing, and sending of data is handled by a Python script controlled by a systemd service.  

## Cloud information
See the "Weather-Station-Server" and "Weather-Station-UI" repositories.

