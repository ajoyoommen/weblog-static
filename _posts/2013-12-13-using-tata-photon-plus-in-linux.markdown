---
layout: post
title: Using Tata Photon Plus in Linux
date: 2013-12-13 16:42:29 +0530
category: Linux
tags:
    - Linux
    - USB Modeswitch
author: Ajoy Oommen
published: true
---
Tata Photon Plus works well on windows, as usual. In Linux, the datacard is not even detected. The default instructions to install MobilePartner have never once worked for me. But finally, I found a way to detect the modem in Linux and connect to the Internet using the default network manager. I found the steps on [UbuntuForums](http://ubuntuforums.org/showthread.php?t=1814583). In short,

1. Ensure that usb-modeswitch and usb-modeswitch-data are installed.
2. Connect the device. It might not be detected.
3. Open /etc/usb_modeswitch.d/ (Do this as root)
4. Create a file : ’12d1:1505′. Here 12d1 is vendor ID and 1505 is the product id. I’m not sure if this method would work for other models.
5. Copy the following into it :

        DefaultVendor= 0x12d1
        DefaultProduct=0x1505
        MessageContent=”55534243123456780000000000000011062000000100000000000000000000″

6. Close the file and save it. (You need sudo to do that)
7. In terminal, cd to /etc/usb_modeswitch.d/
8. Run `sudo usb_modeswitch -I -W -c 12d1\:1505`
9. After a few seconds, the device will be detected in the network manager. Create a connection, selecting your area and provider. After creating the connection, edit the connection(You can do this from the Network Connections setting window).
10. Check the following details : Number #777, username:internet, password:internet

The man page of ‘usb_modeswitch’ says :

>Several new USB devices have their proprietary Windows drivers onboard, most of them WAN dongles. When plugged in for the first time, they act like a flash storage and start installing the Windows driver from there. If the driver is already installed, it makes the storage device disappear and a new device, mainly composite with modem ports, shows up. On Linux, in most cases the drivers are available as kernel modules, such as “usbserial” or “option”. However, the device shows up as “usb-storage” by default. usb_modeswitch can send a provided bulk message (most likely a mass storage command) to the device which is known to initiate the mode switching.

So the above commands help to switch the mode of the data card to USB modem and then, it shows up in the Network Manager.

NOTE :- This is a pseudo-permanent fix. It would be good to add steps 7 and 8 to a script as the usb needs to switch mode every time it is connected. And I still have not been able to install Mobile Partner. This solution is suitable for all those who need to access Internet through their Photon Plus on Linux.