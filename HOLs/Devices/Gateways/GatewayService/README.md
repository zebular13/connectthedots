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

If you are at an event where the pre-configured image is available you may want to start with this lab, then if you have time and want to get more hands on with the Pi setup, you can wipe out the SD card and start over, following the documentation in the [Original Raspberry Pi Gateway Setup Docs](../../../../Devices/Gateways/GatewayService/RaspberryPi-Gateway-setup.md)

---

## Prerequisites ##

To successfully complete this lab, you will need: 

- An active Azure Subscription.  If needed you can create a [free trial here](http://azure.microsoft.com/en-us/pricing/free-trial "Azure Free Trial").

- A copy of the ConnectTheDots.io repository.  You can get the latest version [here](https://github.com/MSOpenTech/connectthedots/archive/master.zip "Connect the Dots Zip Download"). 

- An ssh client (like [PuTTY](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html "PuTTY Downloads") on Windows)
  
- A Raspberry Pi 2 with a USB WiFi adapter

- A copy of the Raspberry Pi image on an SD Card with the Gateway Service code pre-deployed.  If you prefer to configure and deploy the GatewayService yourself, you can refer to the [Original Raspberry Pi Gateway Setup Documentation](../../../../Devices/Gateways/GatewayService/RaspberryPi-Gateway-setup.md)

- Previous completion of the ["Azure Prep" Hands-On Lab](../../../../HOLs/Azure/AzurePrep/README.md) 

- Previous Completion of the ["Arduino Uno With SparkFun Weather Shield" Hands-On Lab](../../../../HOLs/Devices/GatewayConnectedDevices/Arduino UNO/Weather/WeatherSheildJson/README.md)

- Knowledge of your Raspberry Pi's IP address.

--

## Tasks ##

1. [Boot your Raspberry Pi off the Pre-Configured Image](#Task1)
1. [Modify the Gateway Config](#Task2)

---

## Connecting to your Raspberry Pi ##

To complete this lab, you will need to login to your Raspberry Pi.  There are actually a number of ways you can do this.  The following appendices give you a number of alternatives.  You only need use one of them, but feel free to try all of them! 

- If you **DO KNOW YOUR RASPBERRY PI'S IP ADDRESS** and **ARE ON THE SAME NETWORK** as it you can
	- Use SSH (PuTTY is a popular ssh tool for Windows) to connect
	- You can use Remote Desktop (There is a Remote Desktop App for Mac OSx) 
- If you  **DO NOT KNOW YOUR RASPBERRY PI'S IP ADDRESS**
	- You can connect using a USB-to-Serial Cable
	- You can connect an HDMI Monitor, Keyboard and Mouse to the Pi.  

Based on your knowledge (or lack of knowledge) of your Raspberry Pi's IP Address and your available hardware (USB-to-TTL cable, or Monitor, Keyboard, and Mouse) choose from one of the following methods to connect to your Raspberry Pi:

1. [Connect to the Raspberry Pi using the USB-to-TTL Cable](#Appx1)
2. [Connect to the Raspberry Pi using SSH](#Appx2)
3. [Connect to the Raspberry Pi using Remote Desktop](#Appx3)
4. [Connect to the Raspberry Pi using an HDMI Monitor, Keyboard and Mouse](#Appx4)

---

<a name="Task1"></a>
## Task 1 - Boot your Raspberry Pi off the Pre-Configured Image ##

1. Ensure that the SD Card with the pre-configured image is installed in the Raspberry Pi 
2. Ensure that the USB WiFi Adapter is connected to a USB port (or if you are using a direct wired ethernet cable, that the ethernet cable is plugged in) 
3. Connect your Arduino Uno with the SparkFun Weather Shield attached to a USB port on the Raspberry Pi.
4. Finally connect the power supply to the Raspberry Pi
5. The following should image should show you your approximate configuration

	> **Note:** If you don't have the Arduino ready yet, that's ok.  You can plug it in later.  

	![Connections](./images/01010Connections.png)


<a name="Task2"></a>
## Task 2 - Modify the Gateway Config ##



6. Work with your event staff to determine the Raspberry Pi's IP Address
7. Use an ssh client (You can use [PuTTY](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html "PuTTY Downloads") on Windows) or a Remote Desktop Client (mstsc.exe on Windows) to connect to the IP Address of your Raspberry Pi and login with the credentials:

	- Login:	**pi**
	- Password:	**raspberry**

	![Putty Configuration](./images/01020PuttyConfiguration.png)

	![Putty Connection](./images/01030PuttyConnection.png)  

8.  The SD Card is configured with the "Raspbian" linux distribution, so the commands that you enter will be linux commands.  Start by getting a listing of your home folder by typing `ls` and pressing enter.  Notice the "**ctdgtwy**" folder name:

	`ls`

	![Home Listing](./images/01040HomeListing.png)

9.  The "**ctdgtwy** is the folder that contains the "**GatewayService**" deployment.  The "**GatewayService**" is actually a .NET application that is being run on the Raspberry Pi using the [Mono](http://www.mono-project.com/) open source .NET implementation.  If you are interesting in seeing that source code, and how it was deployed, refer to the [Original Raspberry Pi Gateway Setup Docs](../../../../Devices/Gateways/GatewayService/RaspberryPi-Gateway-setup.md).  Here, well just assume it is deployed correctly.  

9.  Change into the ctdgtwy/staging folder (ctdgtwy is short for "**C**onnect **t**he **D**ots **G**a**t**e**w**a**y**"), do another `ls` command and notice the (very long named) "**Microsoft.ConnectTheDots.GatewayService.exe.config**" (whew!) file.  

	`cd ctdgtwy/staging`

	![Staging Folder](./images/01050StagingListing.png)

10.  We need to edit the contents of that file.  There are numerous text editors available on linux, and if you have on you prefer, feel free to use it.  We will use a simple one called "**Nano**".  Enter the command:

	`nano Microsoft.ConnectTheDots.GatewayService.exe.config` 

	![Nano Command](./images/01060NanoCommand.png)

11. Use the arrow keys on your keyboard to move down through the file and locate the section that reads:

	```xml
	<AMQPServiceConfig
	AMQPSAddress="amqps://[key-name]:[key]@[namespace].servicebus.windows.net"
	EventHubName="ehdevices"
	EventHubMessageSubject="gtsv"
	EventHubDeviceId="a94cd58f-4698-4d6a-b9b5-4e3e0f794618"
	EventHubDeviceDisplayName="SensorGatewayService"/>
	```

12. Notice the missing [key-name], [key], and [namespace] placeholders.  We need to enter those so that the Raspberry Pi can successfully connect to the "ehdevices" event hub we created previously.  
13. Leave your ssh window open, and back on your computer open the browser, login to the [Azure Management Portal](https://manage.windowsazure.com) (https://manage.windowsazure.com).  
14. Navigate the portal to find your "**ehdevices**" event hub, and on the "**CONFIGURE**" page,  and get the "**PRIMARY ACCESS KEY** for your "**D1**" "**Shared Access Policy**".  

	![D1 Key](./images/01070D1KeyGen.png)

15. Before you can use the key though, we need to URL encode it.  Go to http://meyerweb.com/eric/tools/dencoder/ to use their URL Encoder / Decoder tool.  Paste they key you just copied in, then hit the "**Encode**" button, then copy the encoded to the clipboard.  

	![Url Encode key](./images/01080EncodeUrl.png)

	![Copy the Encoded Key](./images/01090Encoded.png)

15.  Back in your ssh, and nano, use the arrow keys and your key and keyboard to edit the string.  Replace the place holders with the values from your Service Bus Namespace & Event Hub:

	| Place Holder | Value                                                | 
    | ---          | ---                                                  |
    | [key-name]   |  "**D1**" (no quotes)                                |
    | [key]        |  The URL encoded version of the key you just copied.  Note that many ssh clients (like PuTTY) will paste whatever is in your clipboard if you right click.  So you can delete the place-holder with the keyboard, get the cursor in the right place, then right click to paste the encoded version of the key you copied to the clipboard previously  |
    | [namespace]  |  The service bus namespace you created earlier, "**ctdhol-ns**" in this case |

	![Modified Key](./images/01100Modified.png) 

16.  Finally, to save your changes in Nano, press "**Ctrl-X**" (Exit), the "**Y**" to save the changes, and then "**ENTER** to confirm the original file name.  And as long as you didn't make any typos, you should be good to go. 

17.  To reboot your Raspberry PI, "**DON'T JUST UNPLUG IT!.  SHUT IT DOWN NICELY!!!**".  in your ssh window, run the following command to shut reboot it.  If you are using PuTTY you'll see an error about being disconnected, of course that is to be expected:

	`sudo reboot`

	![Reboot](./images/01110Reboot.png)

18. When the Raspberry Pi starts back up, you should now be able to go to your website and see sensor values coming in! 

	![Sensors Readings](./images/01120WebPage.png)

---

---

## Raspberry Pi Connection Methods ##

---

---

<a href="Appx1"></a>
### Connect to the Raspberry Pi using the USB-to-TTL Cable ###

1. First, make sure you have an appropriate USB-to-TTL Serial Cable.  Not just any cable will do.  The Raspberry Pi uses 3v logic on it's GPIO pins. You need a cable that deals correctly with 3v logic levels and not 5v logic levels.  Something like the [USB to TTL Serial Cable](https://www.adafruit.com/products/954) from Adafruit.

	![USB to TTL Serial Cable](./images/A01010-USBtoTTLCable.png)

2. **DISCONNECT ANY POWER FROM THE RASPBERRY PI**. The USB-to-TTL Cable will supply the power needed for your Raspberry Pi to run.  You **DO NOT** want the Micro USB Power Supply cable plugged in as well.  You only want to power the Raspberry Pi from a single source.  

3.  **MAKE SURE THE USB-TO-TTL CABLE IS *NOT* PLUGGED INTO YOUR COMPUTER'S USB PORT**.  To start, neither end of the cable should be connected to anything.  We'll start by connecting the individual wires to the appropriate pins on the Raspberry Pi, then we'll plug it into the computer.  g

4. Connect the **5V**, **GND**, **TX**, and **RX ** wires to the appropriate pins on the Rasbperry Pi:

	| Wire | Color | Pin | 
	| ---  | ---   | --- |
	| 5V   | Red   | 2   |
    | GND  | Black | 6   |
    | TX   | White | 8   | 
    | RX   | Green | 10  |

	![USB to TTL Cable Connected](./images/A01020-USBtoTTLConnectedToPi.png)

5. Here is a close-up view of the wire-to-pin connections

   ![USB to TTL Cable Pinout Closeup](./images/A010030-USBtoTTLCablePinsCloseup.png)	

6. Once you have the wires connected correctly, you can plug the USB end of the cable into your computer's USB port.  **AGAIN, MAKE SURE THE MICRO USB POWER CABLE IS NOT CONNECTED TO THE RASPBERRY PI**

	![Computer Connected](./images/A01040-ComputerConnected.png)

7.  When you connect the cable to your Windows Computer for the first time, you should see the USB driver install automatically.  If not, you may need to download the driver for your computer.  

	- Windows Driver - http://www.prolific.com.tw/US/ShowProduct.aspx?p_id=225&pcid=41
	- Mac OSx Driver - http://www.prolific.com.tw/US/ShowProduct.aspx?p_id=229&pcid=41

8. On Windows, you'll need to determine the COM port that was assigned to the serial connection.  From your Control Panel, open the "**Device Manager**" and under the "**Ports *(COM & LPT)**" header, locate the COM port that was assigned to the 


---

<a href="Appx2"></a>
## Connect to the Raspberry Pi using SSH ##

---

<a href="Appx3"></a>
## Connect to the Raspberry Pi using Remote Desktop ##

---

<a href="Appx4"></a>
## Connect to the Raspberry Pi using an HDMI Monitor, Keyboard and Mouse ##



