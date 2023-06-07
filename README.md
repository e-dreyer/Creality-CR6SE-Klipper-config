# Creality-CR6SE-Klipper-config
This is my own personal config for my Creality CR6SE Klipper setup on Mainsail

# System Setup

I am running my Creality CR6-SE with a Klipper and Mainsail setup on a Raspberry Pi 2B. I chose to use the 2B because it was still fast enough and I 
am not using it for anything else.

Along side my printer I have my own custom built enclosure. This enclosure has its own relays used to control the power of the printer and my LED lights.
The LED lights aren't required to use relays, because they are only 12V, but it was the easiest method to connect them and they are only standard lights with 
no RGB functionality.

The enclosure also includes two old filament heating lamps used for heating the enclosure. In the summer I can easily achieve 60 degrees in the enclosure and in the winter 50 degrees. These lights are controlled by a single AC dimmer which has a zero-cross detection pin and interrupt. At the time of building this, I was still using Marlin firmware with Octoprint and it wasn't possible to use this interrupt on the Raspberry Pi. For this reason I chose to use an ESP32 connected to WiFi and controlled over MQTT. This is achieved by the addition of Moonraker.

The lights are controlled through a simple PID implementation on the ESP32 and automatically dim to keep the temperature constant, reduce consumption and not overheat the enclosure. The desired target temperature is communicated over MQTT and the value set. This is achieved by starting Gcode for specific filaments in my slicer and some of my Klipper macros. My macros are also set up to turn the heaters off over MQTT after failures and shutdowns, but the ESP32 also has its own failure detection and will respond appropriately to state changes and loss of communication.

# Klipper Setup

I am running the standard Klipper setup with the following added projects:

- Mainsail: Used for the front-end and web interface of the printer
- Moonraker: As an API service for Klipper with added MQTT and control functionality
- Octoeverywhere: Used for remote access and monitoring

The following projects are also installed but not currently implemented.

- Crowsnest: Used for further webcam control
- Klipper Screen: Used for Klipper LCD screen support

# Klipper Macros

For my setup I have included the (Klipper-Macros repository)[https://github.com/jschuh/klipper-macros] to provide me with a set of Klipper macros and variables that I can easily maintain and extend. I found myself not knowing how to structure and format my macros and often found it difficult to add macros from other people. With this I hope to learn the workflow and also some tricks and techniques I can use.

Althought is library has greatly simlpkified the process of having dynamic mesh leveling and helping with accuracy, there al still many efatures for purging, testing and mscros I have not touches. 

Items I wish to improve on:

1. currently the defauly PRINT_START macro is being used, which works wonders, however there are a few ideas I have more more granular control. 
2. I also want to improve and extend the COLOR_CHANGE Macro