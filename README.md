# Weather Station Documentation
The weather station we built is designed to collect pressure, temperature, humidity, VOC and radiation measurements from an Arduino system, send it to a Raspberry Pi over LoRa, and publish that data in the cloud for other people to view.  Measurements will also be stored over time so that people can view graphs of the data.  This document provides an overview of how the weather station is put together, and the arduino and raspi documents go into more depth about the code we wrote.  Finally, the raspi maintenance document explains how to set up the RasPi from scratch.

If you want to see the weather at CSM, [it's right here](https://csmrobotics.club/weatherStation.html).

If you'd like an API for manipulating weather data, [we have that](https://api.csmrobotics.club/api/wss/getAll).

If you'd like to know the parts we used, [here you go](https://docs.google.com/spreadsheets/d/1h0GDfVYntyYW5QakYnAJw-5-3vuBXiLji0O7EjJ50Ls/edit#gid=0).

## Safety information
- The boards are assembled with leaded solder.  Wash your hands after touching hardware.
- If the VOC or Radiation measurements are way off, the sensors are probably broken.  Check the news before scaring yourself.

## Flow of data
The flow of data from the Arduino to the API are listed below.
1. The Arduino collects weather data from the sensors attached to it.  This data (and information reporting whether an error occurred while reading the sensors) is then sent immediately using the LoRa wireless protocol to the RasPi.
2. The RasPi recieves the weather data from the LoRa reciever wired to it, and checks the format of the data to make sure it was not corrupted in transit.  Once the packet is deemed valid, it is converted to a JSON representation and sent to an AWS server for storage.
3. The UI fetches data from the AWS server and presents it on our website.
