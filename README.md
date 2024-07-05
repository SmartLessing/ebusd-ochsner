# ebusd-ochsner
This Repo contains a documentation of my personal setup to gain access via eBUS with ebusd to my Ochsner Heatpump Air 11 C11A with OCHSNER ÖKO-MASTER PU300 as buffer for the heating and PU800 for hotwater. 
For some general information, please refer to the Wiki from [cybersmart-eu](https://github.com/cybersmart-eu) [Wiki](https://github.com/cybersmart-eu/ebusd-ochsner/wiki/).
# [Ochsner Air 11 C 11 A](https://www.ochsner.com/de-de/ochsner-produkte/air-11-c11a/)
The (on/off) air source heat pump (equipped with an embedded auxiliary heater) has been installed in September 2021 with an Unifresh 500 buffer/boiler (fresh-water station). To control the heat pump, Ochsner is using controllers from [TEM Group](https://www.tem.ch/), thus there is a lot of documentation available as many other vendors also implement TEM controllers in their heat pumps. Many different heat pump models from Ochsner have the same eBUS components embedded and share the same eBUS addresses for many different values and parameters.

## [Ochsner OTE 3](https://www.ochsner.com/fileadmin/downloads/OP/RA_OTE_3+4_EK_DE06.pdf)
The OTE 3/4 ist the User Interface to control the heat pump and internally communicates via eBUS with the Control Unit and other eBUS capable devices. Unfortunately, Ochsner does not provide any interface with API by default allowing remote access and remote control of the heat pump. To access the heat pump from a Home Automation System a gateway is required.
With the help of an ebus Gateway and some re-engineering it is possible to decode the messages on the eBUS and make them "human and machine readable", allowing the eBUS of the Ochsner heat pumps to interface with TCP/IP networks and make them remote controllable.

### Check your Ochsner Hardware and Software Version
Before someone is using my configuration files, I recommend to check the main controller ID, Hardware and Software of your main controller either on the heat pump itself (needs to be opened for that to check the label inside) or with an `ebusd info` command. The main controller is listes with **address 15**, in my case with **ID 24849, SW-Version 0605 and Hardware revision 0102**. This is also part of the naming convention of the configuration file as per [John's instructions](https://github.com/john30/ebusd-configuration), so that ebusd can automatically pick the best fitting configuration file in case several configuration files are found.

![IMG_9585](https://github.com/SmartLessing/ebusd-ochsner/assets/172171816/72860a4a-0de7-4e8d-8d0b-85d5bba65ab8)

![image](https://github.com/cybersmart-eu/ebusd-ochsner/assets/77569473/8c0f0833-acea-463e-8084-536d43769e9a)

## The Gateway - ebusd and adapters
Luckily, a software developer with nickname [john30](https://github.com/john30) developed an "ebus deamon", acting as an eBUS gateway, and provides his Open-Source software [ebusd](https://github.com/john30/ebusd) free of charge via github. A hardware adapter as serial interface is required, to physically connect the eBUS with computer hardware. John, the author of ebusd also provides [hardware adapters ](https://adapter.ebusd.eu/) for this purpose, but also other available 3rd party adapters shall do the job.

# The Magic - how to decode the data on the eBUS
Unfortunately, there is no vendor documentation available from Ochsner or TEM Group related to the specific eBUS protocol they use. Every vendor can integrate and use eBUS as they want, so decoding the data means re-enignineering is required. John also maintains a [ebusd-config](https://github.com/john30/ebusd-configuration) repository where he collects and provides ready-to-use config files for different eBUS devices, mainly (latest config files) for Vaillant devices, but also some older Ochsner configuration files can be found there. 
So I had to do a lot of learning (how ebusd works, how to read and analyse received data and how to get it structured) and reverse-engineering of my heat pump by myself. Once you understand the basics - there is progress and progress motivates for the next steps to go. An Ochsner related Discussion on Johns github motivated me fork the work of [Uwe]([https://github.com/cybersmart-eu](https://github.com/cybersmart-eu/ebusd-ochsner)) to refine it in a way so that Home Assistant integration works out of the box with a slightly modfied configuration.
Please also check outthe github of [Uwe](https://github.com/cybersmart-eu), [Lori](https://github.com/Lorilatschki) and [Wolfgang](https://github.com/wiedwo) as we are sitting in the same boat together :-)

## ebusd - most important commands

### ebusd grab result all decode
Dump ebus data

```bash
ebusctl grab result all decode > /home/auser/decode.all.txt
```

### ebusd listen
![image](https://github.com/cybersmart-eu/ebusd-ochsner/assets/77569473/dcc1912e-0bb8-4099-bd23-61af8259b361)

## ebusd - configuration files

```
Setting up ebusd (23.3) ...
Instructions:
1. Edit /etc/default/ebusd if necessary
   (especially if your device is not /dev/ttyUSB0)
2. Start the daemon with 'systemctl start ebusd'
3. Check the log file /var/log/ebusd.log
4. Make the daemon autostart with 'systemctl enable ebusd'![image](https://github.com/SmartLessing/ebusd-ochsner/assets/172171816/2c08ca3d-f022-4541-9f02-92094c32ac5e)
```

```
/etc/default/ebusd
EBUSD_OPTS="--scanconfig --configpath=/etc/ebusd/ -d ens:ebus-air11:9999 --latency=30 --pollinterval=5 --accesslevel=* --httpport=8889 --htmlpath=/va$ --mqtthost=raspberrypi --mqttport=1883 --mqttint=/etc/ebusd/mqtt-hassio.cfg --mqttjson"
```

# My Hardware

## Raspberry Pi 5
The Raspberry Pi 5 was announced on September 28, 2023. Improvements in hardware and software reportedly make the Pi 5 more than twice as powerful as the Pi 4. It comes with an I/O-controller designed in-house, a power button, and an RTC chip, among other things. The RTC chip needs a battery, which can be purchased, but it saves a Pi user the cost of the chip. Unlike the Pi 4, it was released with either 4 or 8 GB of RAM. The 4 GB model costs US$60 and the 8 GB model costs US$80. An important thing to note is that it lacks a 3.5 mm audio/video jack. Users can use Bluetooth, HDMI, USB audio or an Audio HAT if they want to hear sound out of the Pi 5.

https://www.raspberrypi.com/products/raspberry-pi-5/

![23551-Raspberry-Pi-5-8G_(cropped)](https://github.com/SmartLessing/ebusd-ochsner/assets/172171816/4b1c75fe-95dd-4d4c-9b24-11d82de806b6)


## eBUS Adapter Shield v5
eBUS Adapter Shield v5, which can be used to communicate with an eBUS enabled heating, ventilation or solar system.
https://adapter.ebusd.eu

# Some (additional) useful Software

## MQTT - Mosquitto
Eclipse Mosquitto is an open source (EPL/EDL licensed) message broker that implements the MQTT protocol versions 5.0, 3.1.1 and 3.1. Mosquitto is lightweight and is suitable for use on all devices from low power single board computers to full servers.

The MQTT protocol provides a lightweight method of carrying out messaging using a publish/subscribe model. This makes it suitable for Internet of Things messaging such as with low power sensors or mobile devices such as phones, embedded computers or microcontrollers.

https://mosquitto.org

## MQTT-Explorer
MQTT Explorer is a comprehensive MQTT client that provides a structured overview of your MQTT topics and makes working with devices/services on your broker dead-simple.
<img width="1136" alt="Bildschirmfoto 2024-07-05 um 19 11 24" src="https://github.com/SmartLessing/ebusd-ochsner/assets/172171816/7b33ed10-cfac-4053-9dae-8e1b2915060a">


# My Home Automation System
I am using Home Assistant to visualize the heatump data.

## Home Assistant
Awaken your home
Open source home automation that puts local control and privacy first. Powered by a worldwide community of tinkerers and DIY enthusiasts. Perfect to run on a Raspberry Pi or a local server.

https://www.home-assistant.io

This is my dashboard for my heatpump:
<img width="1445" alt="Bildschirmfoto 2024-07-05 um 17 22 05" src="https://github.com/SmartLessing/ebusd-ochsner/assets/172171816/a59aa1a8-e203-4cc2-9c03-3116b14b41de">

