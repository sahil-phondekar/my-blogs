---
slug: setting-arduino-software
title: Setting Up Arduino Software for the First time
authors: [sahil]
tags: [arduino, windows10]
---

### Setup for Arduino IDE:

#### Installing the Software:

You can download the Arduino IDE from the official site of Arduino https://www.arduino.cc/en/Main/Software. Download the latest version. At the time of writing this post the latest version was version 1.8.5.

<!-- truncate -->

I have downloaded the Windows Zip file for non-admin install.   
![Download Arduino](./download-arduino.jpg)

Once downloaded, extract the file and you will find the Arduino software in there, no need of any installing procedures.   
![File Explorer](./file-explorer.jpg)

If you are connecting your Arduino Board for the first time you will require to install the necessary drivers to make the board compatible .

The drivers are available in the drivers folder in the extracted folder.

Just open it and find the **Arduino.inf** file in it. Copy the path of the drivers folder.   
![Drivers](./drivers.jpg)

Now open the Device Manager   
![Device Manager](./device-manager.jpg)

In the device manager, when you connect the Arduino you will find your device name in the Ports(COM & LPT). For eg. Arduino UNO or Arduino MEGA etc. If you cant find your device under Ports(COM & LPT) look under Other Devices.

Once you have found your device, Right click on the device and select Update Driver Software.   
![Device Manager](./device-manager-2.jpg)

The following window will open   
![Update Driver](./update-driver.jpg)

Select Browse my computer for driver software and browse to the drivers folder or paste the address of the driver software and click Next.   
![Update Driver 2](./update-driver-2.jpg)

After this the windows will install the necessary driver and you are ready to go. Connect your device and simply plug and play.
