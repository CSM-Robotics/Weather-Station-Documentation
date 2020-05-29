This document describes how to set up the Raspberry Pi to act as a LoRa server for the Arduino Weather Station.

Any commands that look like `this` are meant to be typed into a terminal.

0. Update the system
    - `sudo apt-get update && sudo apt-get upgrade -y`
1. Make `sudo` require a password:
    - `sudo nano /etc/sudoers.d/010_pi-nopasswd`
    - change the "NOPASSWD" word to "PASSWD" and save the document.
2. Install a firewall
    - `sudo apt-get install ufw`
    - `sudo ufw enable`
    - `sudo ufw allow ssh`
    - `sudo ufw limit ssh/tcp` (denies the connection if an IP address has attempted to connect >6 times in the last 30 seconds)
3. Uninstall LibreOffice, Chromium, and any other GUI packages that aren't required in order to minimize attack surface.
4. Install pip (the Python package manager)
    - use `sudo` for all the pip commands below, or they will be installed for the user and not the system.
    - `sudo pip3 install --upgrade setuptools`
    - `sudo pip3 install RPI.GPIO`
    - `sudo pip3 install adafruit-blinka`
    - `sudo pip3 install adafruit-circuitpython-rfm9x`
5. Enable the proper SPI lines
    - This will allow the Raspberry Pi to communicate properly with the LoRa reciever.
    - Add the line `dtoverlay=spi1-3cs` to the bottom of /boot/config.txt and reboot
6. Wire the RFM9X module
    - Vin -> 3.3v
    - GND -> GND
    - RFM G0 -> GPIO #5
    - RFM RST -> GPIO #25
    - RFM CLK -> SCK
    - RFM MISO -> MISO
    - RFM MOSI -> MOSI
    - RFM CS -> CE1
7. Test the script!
    - `cd ~/Desktop && git clone https://github.com/CSM-Robotics/Weather-Station`
    - `cd Weather-Station/RasPi`
    - `./LoRa.py`
    - You should see packets coming in from the Arduino, if the Arduino is turned on.
8. Install the systemd service
    - `ln LoRa.service /etc/systemd/service/LoRa.service`
    - `sudo systemctl daemon-reload`
    - `sudo systemctl start LoRa.service`
    - To check if the service is running, run `systemctl status LoRa.service`
    - If something went wrong, use `journalctl -u LoRa.service` to find out how the LoRa service failed.
9. Running the LoRa script locally (optional)
    - make sure you run the script with sudo, since all the pip libraries are installed for everyone and python pulls stuff for the user only.
