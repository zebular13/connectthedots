# "Raspberry Pi Field Gateway" Hands-On Lab #

---

## Overview ##

In this lab, you will configure your Raspberry Pi 2 as a "Field Gateway".

![Lab Architecture](./images/00010LabArchitecture.png)

A "Field Gateway" is a common solution for securing and transmitting data from lower powered microcontrollers, or those without direct network connectivity.   

![Raspberry Pi Field Gateway](./images/00020RaspberryPiFieldGateway.png)

In our scenario, we have the Arduino Uno with the SparkFun Weather Shield that we implemented in a previous lab.  The Arduino Uno is an awesome platform, but it does have a few real limitations:  

- Limited processing power and memory.  This makes it hard to communicate using secure protocols like HTTPS, or AMQPS where more intensive processing is required.   
- No built in remote communication capabilties.  You need to extend the Arduino with additional components for Ethernet, WiFi, Bluetooth, etc.  

Our solution here then is to connect the Arduino to our Raspberry Pi using a USB-to-Serial connection.  The Raspberry Pi can then receive the sensor data messages from the Arduino Uno over the serial connection, and then forward them on securely using HTTPS, or AMQPS over Ethernet or WiFi. 

- Previous completion of the ["Azure Prep" HOL](/HOLs/Azure/AzurePrep/readme.md)

- Previous completion of the 

---

## Alternative, More Hands-On Walkthrough ##

This Hands-On Lab is a simplified, and more streamlined version of the original Raspberry Pi gateway setup documentation.  In this lab, we assume you are at an event where a pre-configured Raspberry Pi image has already been applied to the SD Card in your Raspberry Pi.  

This pre-configured image already has

- Raspian operating system installed (via NOOBS)
- Mono
- WiFi Configuration
- The GatewayService .NET project already deployed

All you really need to do in this lab is:

- Remote into your Raspberry Pi via ssh or remote desktop
- Modify the GatewayService application configuration file with the path and keys to your "ehdevices" event hub
- Plug in your Arduino with SparkFun Weather Shield
- Sit back and watch the data flow! 

If you are at an event where the pre-configured image is available you may want to start with this lab, then if you have time and want to get more hands on with the Pi setup, you can wipe out the SD card and start over, following the documentation in the [Original Raspberry Pi Gateway Setup Docs](/Devices/Gateways/GatewayService/RaspberryPi-Gateway-setup.md)

---

## Prerequisites ##

To successfully complete this lab, you will need: 

- An active Azure Subscription.  If needed you can create a [free trial here](http://azure.microsoft.com/en-us/pricing/free-trial "Azure Free Trial").

- A copy of the ConnectTheDots.io repository.  You can get the latest version [here](https://github.com/MSOpenTech/connectthedots/archive/master.zip "Connect the Dots Zip Download"). 

- An ssh client (like [PuTTY](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html "PuTTY Downloads") on Windows)
  
- A Raspberry Pi 2 with a USB WiFi adapter

- A copy of the Raspberry Pi image on an SD Card with the Gateway Service code pre-deployed.  If you prefer to configure and deploy the GatewayService yourself, you can refer to the [Original Raspberry Pi Gateway Setup Documentation](/Devices/Gateways/GatewayService/RaspberryPi-Gateway-setup.md)

- Previous completion of the ["Azure Prep" Hands-On Lab](/HOLs/Azure/AzurePrep/README.md) 

- Previous Completion of the ["Arduino Uno With SparkFun Weather Shield" Hands-On Lab](/HOLs/Devices/GatewayConnectedDevices/Arduino UNO/Weather/WeatherSheildJson/README.md)

- Knowledge of your Raspberry Pi's IP address.

--

## Tasks ##

1. [Boot your Raspberry Pi off the Pre-Configured Image](#Task1)
1. [Modify the Gateway Config](#Task2)
1. [Connect the Arduino Uno](#Task3)
1. [Task 4](#Task4)
1. [Task 5](#Task5)

---

<a name="Task1" />
## Task 1 - Boot your Raspberry Pi off the Pre-Configured Image ##

---

<a name="Task2" />
## Task 2 -  ##

---

<a name="Task3" />
## Task 3 -  ##

---

<a name="Task4" />
## Task 4 -  ##

---

<a name="Task5" />
## Task 5 -  ##

