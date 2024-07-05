# ebusd-ochsner
This Repo contains a documentation of my personal setup to gain access via eBUS with ebusd to my Ochsner Heatpump Air 11 C11A with Unifresh 500 as buffer/boiler. 
For some general information, please refer to the [Wiki](https://github.com/cybersmart-eu/ebusd-ochsner/wiki/).
# [Ochsner Air 11 C 11 A](https://www.ochsner.com/de-de/ochsner-produkte/air-11-c11a/)
The (on/off) air source heat pump (equipped with an embedded auxiliary heater) has been installed in September 2021 with an Unifresh 500 buffer/boiler (fresh-water station). To control the heat pump, Ochsner is using controllers from [TEM Group](https://www.tem.ch/), thus there is a lot of documentation available as many other vendors also implement TEM controllers in their heat pumps. Many different heat pump models from Ochsner have the same eBUS components embedded and share the same eBUS addresses for many different values and parameters.

## [Ochsner OTE 3](https://www.ochsner.com/fileadmin/downloads/OP/RA_OTE_3+4_EK_DE06.pdf)
The OTE 3/4 ist the User Interface to control the heat pump and internally communicates via eBUS with the Control Unit and other eBUS capable devices. Unfortunately, Ochsner does not provide any interface with API by default allowing remote access and remote control of the heat pump. To access the heat pump from a Home Automation System a gateway is required.
With the help of an ebus Gateway and some re-engineering it is possible to decode the messages on the eBUS and make them "human and machine readable", allowing the eBUS of the Ochsner heat pumps to interface with TCP/IP networks and make them remote controllable.

### Check your Ochsner Hardware and Software Version
Before someone is using my configuration files, I recommend to check the main controller ID, Hardware and Software of your main controller either on the heat pump itself (needs to be opened for that to check the label inside) or with an `ebusd info` command. The main controller is listes with **address 15**, in my case with **ID 24849, SW-Version 0605 and Hardware revision 0102**. This is also part of the naming convention of the configuration file as per [John's instructions](https://github.com/john30/ebusd-configuration), so that ebusd can automatically pick the best fitting configuration file in case several configuration files are found.

![image](https://github.com/cybersmart-eu/ebusd-ochsner/assets/77569473/8c0f0833-acea-463e-8084-536d43769e9a)

## The Gateway - ebusd and adapters
Luckily, a software developer with nickname [john30](https://github.com/john30) developed an "ebus deamon", acting as an eBUS gateway, and provides his Open-Source software [ebusd](https://github.com/john30/ebusd) free of charge via github. A hardware adapter as serial interface is required, to physically connect the eBUS with computer hardware. John, the author of ebusd also provides [hardware adapters ](https://adapter.ebusd.eu/) for this purpose, but also other available 3rd party adapters shall do the job.

# The Magic - how to decode the data on the eBUS
Unfortunately, there is no vendor documentation available from Ochsner or TEM Group related to the specific eBUS protocol they use. Every vendor can integrate and use eBUS as they want, so decoding the data means re-enignineering is required. John also maintains a [ebusd-config](https://github.com/john30/ebusd-configuration) repository where he collects and provides ready-to-use config files for different eBUS devices, mainly (latest config files) for Vaillant devices, but also some older Ochsner configuration files can be found there. 
So I had to do a lot of learning (how ebusd works, how to read and analyse received data and how to get it structured) and reverse-engineering of my heat pump by myself. Once you understand the basics - there is progress and progress motivates for the next steps to go. An Ochsner related Discussion on Johns github motivated me to further investigate and share my knowledge and recently some other "struggeling" users with similar Ochsner environments jumped in on my (still somehow weak) results and questions and now together we make further steps forward.
Please also check outthe github of [Lori ](https://github.com/Lorilatschki) and [Wolfgang](https://github.com/wiedwo) as we are sitting in the same boat together :-)

## ebusd - most important commands
xxx
### ebusd listen
xxx
![image](https://github.com/cybersmart-eu/ebusd-ochsner/assets/77569473/dcc1912e-0bb8-4099-bd23-61af8259b361)

## ebusd - configuration files
xxx

# My Hardware
xxx
## Raspberry Pi 5
xxx
## eBUS Adapter Shield v5
[https://adapter.ebusd.eu](https://adapter.ebusd.eu)

# Some (additional) useful Software
xxx
## MQTT - Mosquitto
xxx
## MQTT-Explorer
xxx


# My Home Automation System
xxx
## Home Assistant
<img width="1445" alt="Bildschirmfoto 2024-07-05 um 17 22 05" src="https://github.com/SmartLessing/ebusd-ochsner/assets/172171816/a59aa1a8-e203-4cc2-9c03-3116b14b41de">

