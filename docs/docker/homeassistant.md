**This guide is not using docker swarm**

Homeassistant is an open source software which is used for improving home automation and is intended to be the central control system for your smart house.


This software is perfect to run on a raspberry pi or a server. There are endless ways you can use homeassistant and you can write your own scenarios. For example you can create a program where if a person returns home it can turn on all the lights in the house, or if motion is detected and noone is home it will send you a text.


In this guide I will be explaining multiple ways you can install home assistant


### Installation Method 1:

This first guide will be explaining how you can install homeassistant on a raspberry Pi using a dedicated OS.


## Prerequisites

SD Card (Recommended is 32GB)

SD Card Reader (This will be used so you can image the card from your main machine)

Raspberry Pi + Power Supply 

Ethernet Cable

## Important links

[BalenaEtcher](https://www.balena.io/etcher/) (This software is used to image the SD card)

[Homeassistant Download](https://www.home-assistant.io/hassio/installation/) (You can download HomeAssistant Here)



1. Place the SD Card in your PC and open balenaEtcher


2. Select the Homeassistant Image which can be found in the important links and flash the card


4. Once the SD card has been imaged you can unmount the SD card from your computer and plug it into the raspberry pi along with an ethernet cable and then turn on the raspberry pi.

5. Once the Raspberry Pi has been turned on you can view home assistant by visiting http://homeassistant:8123 This will present you with the Homeassistant logo and will be telling you that homeassistant is currently being installed and configured. This can take upto 20 minutes.

6. Once homeassistant has been installed it will ask you to provide some information such as your username, password and location (The location is used for weather statistics).



### Installation Method 2:

This second guide will show you how to install Home Assistant on a Supervised Machine

## Requirements

Root User

Ubuntu 20.04 (This is what I am using. Other Linux systems might work)


## Supported Operating system, dependencies and versions

Docker CE >= 19.03
Systemd >= 239
NetworkManager >= 1.18.0
Avahi >= 0.7
AppArmor == 2.13.x (built into the kernel)
Debian Linux Debian 10 aka Buster (no derivatives)


1. The first thing you will need to do is run the following in the terminal running is root 
```curl -sL https://raw.githubusercontent.com/home-assistant/supervised-installer/master/installer.sh | bash -s```

2. Once the command has been run homeassistant will be installed and you will be able to view homeassistant at http://homeassistant:8123 