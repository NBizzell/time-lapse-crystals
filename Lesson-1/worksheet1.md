#Raspberry Pi and Pi Camera Module Set Up

In this lesson you will set up and test the Raspberry Pi and Pi Camera Module.

##Raspberry Pi Set Up

## Step 1: Make sure that you have everything that you need

Before you begin, check that you have all the parts that you need:

### Required items:

- A Raspberry Pi (Either a [Model B](http://www.raspberrypi.org/product/model-b/) or [Model B+](http://www.raspberrypi.org/product/model-b-plus/))

	![](images/Raspberry-Pis.jpg)

- SD Card
	- We recommend an 8GB class 4 SD card, ideally preinstalled with NOOBS. 
	- You can [buy a card with NOOBS pre-installed](http://swag.raspberrypi.org/collections/frontpage/products/noobs-8gb-sd-card), or you can download it for free from our [downloads page](http://www.raspberrypi.org/downloads/).
	
- Display and connecting cables
	- Any HDMI/DVI monitor or TV should work as a display for the Pi. 
	- For best results, use one with HDMI input, but other connections are available for older devices. 
	
- Keyboard and mouse
	- Any standard USB keyboard and mouse will work with your Raspberry Pi.
	
- Power supply
	- Use a [5V micro USB power supply](http://swag.raspberrypi.org/collections/pi-kits/products/raspberry-pi-universal-power-supply) to power your Raspberry Pi. Be careful that whatever power supply you use outputs at least 5V; insufficient power will cause your Pi to behave unexpectedly.

## Step 2: Plugging in your Raspberry Pi

Now you have all the parts you need to get up and running, let's start plugging in!

1. Begin by slotting your SD card into the SD card slot on the Raspberry Pi, which will only fit one way. *Note that the software NOOBS should already be on this card. If it isn't then follow the [NOOBS guide here](http://www.raspberrypi.org/help/noobs-setup/).*
1. Next, plug in your USB keyboard and mouse into the USB slots on the Raspberry Pi.
Make sure that your monitor or TV is turned on, and that you have selected the right input (e.g. HDMI 1, DVI, etc).
1. Then connect your HDMI cable from your Raspberry Pi to your monitor or TV.
1. If you intend to connect your Raspberry Pi to the internet, plug in an Ethernet cable into the Ethernet port next to the USB ports; if you do not need an internet connection, skip this step.
1. Finally, when you are happy that you have plugged in all the cables and SD card required, plug in the micro USB power supply. This action will turn on and boot your Raspberry Pi.
1. If this is the first time your Raspberry Pi and NOOBS SD card have been used, then you will have to select an operating system and configure it. [Follow the NOOBS guide to do this](http://www.raspberrypi.org/help/noobs-setup/).

## Step 3: Logging into your Raspberry Pi

Hurrah, Raspbian has loaded, your Raspberry Pi is up and running... now what? Let's log in to find out:

1. Once your Raspberry Pi has completed the boot process, a login prompt will appear. The default login for Raspbian is username `pi` with the password `raspberry`. Note you will not see any writing appear when you type the password. This is a security feature in Linux.
1. After you have successfully logged in, you will see the command line prompt `pi@raspberrypi~$`.
1. Shut down the Raspberry Pi by typeing 'sudo halt'.
1. Once the Raspberry Pi has shut down completely, remove the power lead 

##Camera Set Up

### Connecting the camera

1. Locate the camera port next to the ethernet port
1. Lift the tab on the top
1. Place the strip in the connector (blue side facing the ethernet port)
1. While holding the strip in place, push down the tab

### Activate the camera

1. Connect a USB cable to the power
1. Login with username `pi` and password `raspberry`
1. At the command prompt enter `sudo raspi-config`
1. At the menu, navigate to `Enable Camera`
1. Select `Enable`
1. Select `Finish`
1. Select `Yes` to reboot

### Test the camera

1. Login again with username `pi` and password `raspberry`
1. At the command prompt enter `raspistill -o image.jpg`
1. On the screen you should see a preview appear for a few seconds, and then change briefly while the image is captured

If this does not happen go back and check you have connected and activated the Camera module correctly. 

If the image appears as expecetd then you are ready to move on to [taking pictures](worksheet2.md)

## Licence

Unless otherwise specified, everything in this repository is covered by the following licence:

![Creative Commons License](http://i.creativecommons.org/l/by-sa/4.0/88x31.png)

***Raspberry Pi and Pi Camera Module Set Up*** by Neil Bizzell is licenced under a [Creative Commons Attribution 4.0 International License](http://creativecommons.org/licenses/by-sa/4.0/).

Based on works at https://github.com/raspberrypilearning/python-picamera-setup and https://github.com/raspberrypilearning/teachers-classroom-guide
