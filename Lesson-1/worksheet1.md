# Set up Your Raspberry Pi and Your Pi Camera Module

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




##Camera Set Up

### Connecting the camera

1. Locate the camera port next to the ethernet port
2. Lift the tab on the top
3. Place the strip in the connector (blue side facing the ethernet port)
4. While holding the strip in place, push down the tab

### Activate the camera

1. Connect a USB cable to the power
2. Login with username `pi` and password `raspberry`
3. At the command prompt enter `sudo raspi-config`
4. At the menu, navigate to `Enable Camera`
5. Select `Enable`
6. Select `Finish`
7. Select `Yes` to reboot

### Test the camera

1. Login again with username `pi` and password `raspberry`
2. At the command prompt enter `raspistill -o image.jpg`
3. On the screen you should see a preview appear for a few seconds, and then change briefly while the image is captured

### Camera programming: capture an image

Start by installing the Python `picamera` and GPIO library packages:

```
sudo apt-get install python-picamera python3-picamera python-rpi.gpio
```

1. At the command prompt enter `startx` to start the graphical desktop environment
2. Double click on `LXTerminal` to start a command line, and enter `sudo idle &` to start the Python environment
3. Select `File > New Window` from the menu to start a text editor
4. Enter the following code (case is important!):

    ```python
    import time
    import picamera

    with picamera.PiCamera() as camera:
        camera.start_preview()
        time.sleep(5)
        camera.capture('/home/pi/Desktop/image.jpg')
        camera.stop_preview()
    ```

5. Select `File > Save` from the menu and give your script a name, e.g. `workshop.py`
6. Select `Run > Run Module` from the menu (or just press `F5`) to run the script

### Camera programming: capture when activated

1. Connect the Pi to the button as shown in the diagram below:

    ![](picamera-gpio-setup.png)

2. In the text editor, import the `RPi.GPIO` module, set up GPIO pin 17 and change the `sleep` line to use `GPIO.wait_for_edge` like so:

    ```python
    import time
    import picamera
    import RPi.GPIO as GPIO  # new

    GPIO.setmode(GPIO.BCM)  # new
    GPIO.setup(17, GPIO.IN, GPIO.PUD_UP)  # new

    with picamera.PiCamera() as camera:
        camera.start_preview()
        GPIO.wait_for_edge(17, GPIO.FALLING)  # new
        camera.capture('/home/pi/Desktop/image.jpg')
        camera.stop_preview()
    ```

3. Delete `image.jpg` from the desktop
4. Save and run your script
5. Once the preview has started, press the button connected to your Pi to capture an image

### Camera programming: countdown capture (selfies!)

1. Modify your program to include the delay after the button wait:

    ```python
    import time
    import picamera
    import RPi.GPIO as GPIO

    GPIO.setmode(GPIO.BCM)
    GPIO.setup(17, GPIO.IN, GPIO.PUD_UP)

    with picamera.PiCamera() as camera:
        camera.start_preview()
        GPIO.wait_for_edge(17, GPIO.FALLING)
        time.sleep(5)  # new
        camera.capture('/home/pi/Desktop/image.jpg')
        camera.stop_preview()
    ```

2. Delete `image.jpg` from the desktop
3. Save and run your script
4. Press the button and try to take a selfie


## Licence

Unless otherwise specified, everything in this repository is covered by the following licence:

![Creative Commons License](http://i.creativecommons.org/l/by-sa/4.0/88x31.png)

Set Up by Neil Bizzell is licenced under a [Creative Commons Attribution 4.0 International License](http://creativecommons.org/licenses/by-sa/4.0/).

Based on a work at https://github.com/raspberrypilearning/python-picamera-setup and https://github.com/raspberrypilearning/teachers-classroom-guide