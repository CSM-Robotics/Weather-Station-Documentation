This document details how the Raspberry Pi portion of the Weather Station project works, for anyone who is interested or repairing a device.

The Raspberry Pi 4 we are using is attached to a [LoRa adapter](https://www.adafruit.com/product/3072) which allows the Pi to recieve data over the LoRa protocol that the Arduino is sending data over.

Once data is recieved, a Python script checks the data to make sure that the packet is the right length, and that no errors occurred while the Arduino read the sensors.  If the packet indicates the Arduino detected an error or no packet arrives, the condition is logged on the RasPi and the Arduino will attempt to transmit the packet again.

After the data has been checked, it is converted to a JSON format and uploaded to an AWS server for storage.  This server requires an authentication token from the RasPi, which is set up when the RasPi sends the correct username and password to the server.  The username and password are stored in the (private) Weather-Station-Sec repository.
