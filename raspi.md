This document details how the Raspberry Pi portion of the Weather Station project works, for anyone who is interested or repairing a device.

The Raspberry Pi 4 we are using is attached to a [LoRa adapter](https://www.adafruit.com/product/3072) which allows the Pi to recieve data over the LoRa protocol that the Arduino is sending data over.

Once data is recieved, a Python script parses the data and throws the packet away if it isn't formatted correctly (indicating the data garbled during transmission).  If the data is valid, it is passed to the web server over HTTPS for storage.
