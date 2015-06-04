# "NETMF Gadgeteer Configuration" Hands-On Lab #

---

## Overview ##

In this lab you will use A Gadgeteer FEZ Spider and the [.NET Micro Framework](http://netmf.codeplex.com/) to publish sensor data into the "**ehdevices**" Azure Event Hub you created previously in the "**Azure Prep**" Hands-On Lab.  

![Lab Architecture](./images/00010LabArchitecture.png)


---

## Prerequisites ##

To successfully complete this lab, you will need: 

- An active Azure Subscription.  If needed you can create a [free trial here](http://azure.microsoft.com/en-us/pricing/free-trial "Azure Free Trial").
- A Windows computer with Visual Studio 2013 Community Edition or better [download](http://go.microsoft.com/?linkid=9863608).  **NOTE: YOU CAN'T USE VISUAL STUDIO 2015 FOR THIS LAB.  IT MUST BE VISUAL STUDIO 2013.**  
- A copy of the ConnectTheDots.io repository.  You can get the latest version [here](https://github.com/BretStateham/connectthedots/archive/master.zip "Connect the Dots Zip Download"). 
- You will need to download and install the software components covered in the "**Install Dependencies**" task

 - .NET Framework 3.5.  This is included as a Windows Feature, but you need to verify that it is "On"
 - [.Net Micro Framework Core SDK](http://netmf.codeplex.com/downloads/get/911182)
 - [.Net MF Visual Studio 2013 integration](http://netmf.codeplex.com/downloads/get/911183)
 - [.Net Gadgeteer Core](http://gadgeteer.codeplex.com/downloads/get/918081)
 - [GHI NETMF and Gadgeteer package](https://www.ghielectronics.com/support/netmf/sdk/24/netmf-and-gadgeteer-package-2014-r5)


- You will need a FEZ Spider and the additional hardware components covered in the "**Connect the Components to the FEZ Spider Mainboard**" task. 

 - [Gadgeteer FEZ Spider Mainboard](https://www.ghielectronics.com/catalog/product/269)
 - [Gadgeteer USB DP Module](https://www.ghielectronics.com/catalog/product/280)
 - [Gadgeteer Ethernet J11D Module](https://www.ghielectronics.com/catalog/product/284)
 - [Gadgeteer TempHumid S170 Module](https://www.ghielectronics.com/catalog/product/528)
 - [Gadgeteer LightSense Module](https://www.ghielectronics.com/catalog/product/336)

---

## Troubleshooting Tips ##

If you have problems with the Gadgeteer, you can refer to the [NETMF and Gadgeteer Troubleshooting Guide](https://www.ghielectronics.com/docs/165/netmf-and-gadgeteer-troubleshooting)

1. If you have problems deploying to the Gadgeteer, for example after it crashes, or you stop a running debug session, you can try resetting the Gadgeteer using either the Reset button, or by cycling the power:

	![Reset](./images/00030Reset.png)


---

## Tasks ##

1. [Install Dependencies](#Task1)
1. [Connect the Components to the FEZ Spider Mainboard](#Task2)
1. [Modify and Deploy the ConnectTheDotsGadgeteer Project](#Task3)
1. [Update the SSL Seed on the Gadgeteer](#Task4)
1. [Verify Your Results](#Task5)

---

<a name="Task1"></a>
## Task 1

To complete this lab you will need Visual Studio 2013 Community Edition or better.  At the time this is being written, Visual Studio 2015 is NOT supported so you will need VS 2013.  The free Community Edition works fine, but if you already have a VS 2013 Professional or VS 2013 Ultimate, those work just fine as well. 

You need to install the .NET Micro Framework SDK, and a few additional tools to help Visual Studio work with NETMF and the Gadgeteer boards.    

1. Verify that the .NET Framework 3.5 is enabled on your system.   To do so, go to your "**Control Panel**" | "**Programs**" | "**Turn Windows features on or off**" and ensure that the .NET Framework 3.5 is enabled.  You do not need the Windows Communication Foundation Activation features enabled.  

	![.NET Framework 3.5 Enabled](./images/01005Net35Enabled.png)

1. Download and install the [.Net Micro Framework Core SDK 4.3 (SDK-R2-Beta)](http://netmf.codeplex.com/downloads/get/911182)

	![.NET Micro Framework SDK Installer](./images/01010NETMFSdkInstaller.png)

2. Download and install the [.Net MF Visual Studio 2013 integration](http://netmf.codeplex.com/downloads/get/911183)

	![Visual Studio 2013 Integration](./images/01020VS2013Integration.png)

3. Download and install the [.Net Gadgeteer Core](http://gadgeteer.codeplex.com/downloads/get/918081)

	![Gadgeteer Core Setup](./images/01030GadgeteerCoreSetup.png)

4. Finally, download and install the [GHI NETMF and Gadgeteer package](https://www.ghielectronics.com/support/netmf/sdk/24/netmf-and-gadgeteer-package-2014-r5)  This installer actually calls other installers.  You need to pay attention as you run through this because you will need to answer the prompts for:

	![GHI NETMF And Gadgeteer Package](./images/01040GHINETMF.png)

	- The nested "**Atmel SAM Boot Assistant**" ("**SAM-BA**") installer:

		![Atmel Sam-Ba Installer](./images/01050SamBa.png)

	- The ST Microelectronics "**Device Firmware Upgrade ST Microelectronics Extension**" ("**DfuSe**") isntaller:

		![DfuSe](./images/01060DfuSe.png)

	- GHI USB Driver:

		![USB Driver](./images/01070GHIUSBSerial.png)

5. Make sure that you complete all of the required installs.  If you miss one, you will not be able to complete the lab. 

---

<a name="Task2"></a>
## Task 2 - Connect the Components to the FEZ Spider Mainboard ##

One of the benefits of the Gadgeteer platform is the ease with which you can connect components.  In this task, you need to simply connect the components as described below:

1. Connect the [Gadgeteer LightSense Module](https://www.ghielectronics.com/catalog/product/336) to **Slot 10** on the FEZ Spider Mainboard.
2. Connect the [Gadgeteer TempHumid S170 Module](https://www.ghielectronics.com/catalog/product/528) to **Slot 11** on the FEZ Spider Mainboard
3. Connect the [Gadgeteer Ethernet J11D Module](https://www.ghielectronics.com/catalog/product/284) to **Slot 7** on the FEZ Spider Mainboard
4. Connect the [Gadgeteer USB DP Module](https://www.ghielectronics.com/catalog/product/280) to **Slot 1** on the FEZ Spider Mainboard

![Gadgeteer Configuration](./images/00020GadgeteerConfiguration.png)

---

<a name="Task3"></a>
## Task 3 - Modify and Deploy the ConnectTheDotsGadgeteer Project ##

In this task, you will modify the "**ConnectTheDotsGadgeteer**" solution in Visual Studio to point it towards your. "**ehdevices**" Event Hub.  

1. In Visual Studio 2013, open the [\Devices\DirectlyConnectedDevices\NETMF\ConnectTheDotsGadgeteer.sln](/Devices/DirectlyConnectedDevices/NETMF/ConnectTheDotsGadgeteer.sln) Solution.  

1. To download the required NuGet packages, and ensure that the project base is functional, go ahead and do build.  From the menu bar select "**BUILD**" | "**Build Solution**", and once the build is done, ensure that it reads "**Build succeeded**" in the lower left hand corner. 

	![Build Solution](./images/02010BuuildProject.png)

2. In the "**Solution Explorer**" Window, double click on the "**Program.cs**" file top open it.

	![Open Program.cs](./images/02020OpenProgramCs.png)

3. In the "**Program.cs**" file, locate the lines that configure the client connect to the "**ehdevices**" event hub (should be somewhere around line 54):

	```C#
	// Azure Event Hub connection information
	private const string AMQPAddress = "amqps://{key-name}:{key}@{namespace-name}.servicebus.windows.net"; // Azure event hub connection string
	
	private const string EventHub = "ehdevices"; // Azure event hub name
	private const string SensorName = "Gadgeteer"; // Name of the device you want to display
	private const string SensorGUID = "{GUID}"; // unique GUID per device. Use GUIDGEN to generate new one
	
	private const string Organization = "MSOpenTech"; // Your organization name
	private const string Location = "My Room"; // Location of the device
	
	// Define the frequency at which we want the device to send its sensor data (in milliseconds)
	const int SendFrequency = 60000;
	private readonly GT.Timer _timer = new GT.Timer(SendFrequency);
	```
4. Verify that the `EventHub` variable value matches the name of the Event Hub you created for "**ehdevices**".  If you called it something other than "**ehdevices** make sure to update the variable value to match: 

	```C#
	private const string EventHub = "ehdevices"; // Azure event hub name
	```

4. Leave your Visual Studio window open, open the browser, and login to the [Azure Management Portal](https://manage.windowsazure.com) (https://manage.windowsazure.com).  
14. Navigate the portal to find your "**ehdevices**" event hub, and on the "**CONFIGURE**" page,  and get the "**PRIMARY ACCESS KEY** for your "**D1**" "**Shared Access Policy**".  

	![D1 Key](./images/02030D1KeyGen.png)

5. Before you can use the key though, we need to URL encode it.  Go to [http://meyerweb.com/eric/tools/dencoder/](http://meyerweb.com/eric/tools/dencoder/) to use their URL Encoder / Decoder tool.  Paste they key you just copied in, then hit the "**Encode**" button, then copy the encoded to the clipboard.  

	![Url Encode key](./images/02040EncodeUrl.png)

	![Copy the Encoded Key](./images/02050Encoded.png)

6.  Back in Visual Studio, in the "**Program.cs**" file, edit the `AMQPaddress`  the string variable.  Replace the place holders with the values from your Service Bus Namespace & Event Hub:

	| Place Holder      | Value                                                | 
    | ---               | ---                                                  |
    | {key-name}        |  "**D1**" (no quotes)                                |
    | {key}             |  The URL encoded version of the key you just copied. |
    | {namespace-name}  |  The service bus namespace you created earlier, "**ctdhol-ns**" in this case |

7. For example:

	```C#
	// Azure Event Hub connection information
	private const string AMQPAddress = "amqps://D1:L2wFYmNQ05PqD3n6d1%2FScRPJB%2FdbzCHYh1RQwWVuBJc%3D@ctdhol-ns.servicebus.windows.net"; // Azure event hub connection string
	```

8. For the `SensorGUID` string, you can generate a new GUID value in Visual Studio by , from the menu bar, going to "**TOOLS**" | "**Create GUID**".  Then use the tool to generate a new GUID, copy it to the clipboard (Try using the "Registry Format"), then paste it into your code and remove the curly braces.  

	![Create GUID](./images/02060CreateGUID.png)

9. For example
	
	```C#
	private const string SensorGUID = "C62F1539-84CF-4F1C-AAF3-286E63A1099C"; // unique GUID per device. Use GUIDGEN to generate new one
	```

10. For the SendFrequency varible, you may want to try a lower frequency, like 1000 (milliseconds).  The default 60000 only publishes sensor data once a minute.  For example:  

	````C#
	// Define the frequency at which we want the device to send its sensor data (in milliseconds)
	const int SendFrequency = 1000;
	```
11. Lastly, update the `SensorName`, `Organization`, and `Location` variable values to whatever you like.  

---

<a name="Task4"></a>
## Task 4 - Update the SSL Seed on the Gadgeteer ##

Your Gadgeteer device needs a current, valid set of keys to participate in the SSL communications required by AMQP.  

1. From the Start Menu's Search function, find the MFDeploy.exe tool and run it:

	![MFDeploy.exe Search](./images/04010MFDeploySearch.png)

2. In the "**Device**" dropdown, select USB, then select your Gadgeteer:

	![USB Gadgeteer](./images/04020Target.png)

3. From the "**MFDeploy**" menu bar, select "**Target**" | "**Manage Device Keys**" | "**Update SSL Seed**", then verify that it completes. 

	![SSL Update Complete](./images/04030Complete.png)

---

<a name="Task5"></a>
## Task 5 - Verify Your Results ##

You should be ready to go.  Let's deploy the code to the Gadgeteer and watch data flow in on the Connect the Dots Sample Website you created earlier.  

1. In Visual Studio, hit the "**Start**" button on the toolbar to start a debug session:

	![Start Debug](./images/05010StartDebug.png)

2. In the Visual Studio "**Output**" Window, you should being to see messages about the Gadgeteer's IP Address, as well as periodic messages (the rate of which depends on your `SendFrequency` variable value) appear.  

	![IP Address](./images/05020IpAddress.png)

	![Sensor Readings](./images/05030SensorMessages.png)

3. If you open the "**Connect the Dots Sample Web Site**" you deployed in the  **[Connect the Dots Web Site](Azure/WebSite)** Hands-On Lab you should start to see sensor readings coming in.  

	![Web Site](./images/05040WebSite.png)