Welcome to the CSM Weather Station Project!

For this project, we built a weather station that collects temperature, pressure, humidity, radiation, and air quality measurements and publishes them on a website for public consumption.  The project contains three main parts:
 - an Arduino-like microcontroller that collects the actual sensor information, and transmits it over a wireless protocol called LoRa
 - a Raspberry Pi running a Python script that recieves the LoRa data and sends it to an Amazon Web Services server
 - an AWS server written in Java that stores data long-term
 - a user interface that displays the data over time (language?)

In this repository you will find several documents that give a high-level description of how the Weather Station project operates, geared towards people who know the basics of computer programming and might be doing maintenence on some part of the project.

FAQ:
 - Why not send data from the Arduino directly?
 	High power consumption (especially when using HTTPS), have to be in range of wireless network.
 - Why not host the server locally?
 	Server maintenence is hard and we don't want to do it
 - Why not collect data with the Raspberry Pi directly?
    We didn't plan for the weather station to be running off of wall power or be close to a Wi-Fi access point.  Running a POE line was possible but harder than doing everything wirelessly.

