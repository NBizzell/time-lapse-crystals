
#Python Timelapse Photography

A time-lapse sequence can be easily constructed by running the basic capture program in a loop, and adjusting the delay to the gap required. Timelapse sequences can be used to visualise many natural processes that are not easy to see otherwise such as plant growth and decay, long term construction projects and weather cycles. Here is some example code:

```python
import picamera
import RPi.GPIO as GPIO

GPIO.setmode(GPIO.BCM)
GPIO.setup(17, GPIO.IN, GPIO.PUD_UP)

with picamera.PiCamera() as camera:
    camera.start_preview()
    frame = 1
    while True:
        GPIO.wait_for_edge(17, GPIO.FALLING)
        camera.capture('/home/pi/Desktop/frame%03d.jpg' % frame)
        frame += 1
    camera.stop_preview()
```

##Method 2
Type the following into a command line.

`sudo apt-get install -y libav-tools`

Here is the full program, have a read through it and see if you can figure out what each line is doing. I will then explain each line in more detail.
```
import os
import time

FRAMES = 1000
FPS_IN = 10
FPS_OUT = 24
TIMEBETWEEN = 6
FILMLENGTH = float(FRAMES / FPS_IN)


frameCount = 0
while frameCount < FRAMES:
    imageNumber = str(frameCount).zfill(7)
    os.system("raspistill -o image%s.jpg"%(imageNumber))
    frameCount += 1
    time.sleep(TIMEBETWEEN - 6) #Takes roughly 6 seconds to take a picture

os.system("avconv -r %s -i image%s.jpg -r %s -vcodec libx264 -crf 20 -g 15 -vf crop=2592:1458,scale=1280:720 timelapse.mp4"%(FPS_IN,'%7d',FPS_OUT))
```

To begin with we need to import two libraries, os which will allow us to interact with the command line, and time to enable us to set the time between frames.
```
import os
import time
```
Now we will set some global variables. The nice thing about global variables is that you can change a variable that appears all through your program by just changing it in one location. Global variables also make your code more readable, as words explain what the variable is better than a number appearing thorughout your code. It also makes for code which is easier to modify as you know that all your global variables are specified at the top of your code.

We will set 5 global variables.
```
FRAMES = 1000
FPS_IN = 10
FPS_OUT = 24
TIMEBETWEEN = 6
FILMLENGTH = float(FRAMES / FPS_IN)
```

`FRAMES` sets the number of frames or images you will take and add to your video.
`FPS_IN` sets the number of the Frames Per Second (FPS) that go into the video. So if you want 10 of your frames to be used per second put the value to 10.
`FPS_OUT` sets the Frames Per Second of the video created. i.e. creates a video running at 24 Frames Per Second. If `FPS_IN` is less that `FPS_OUT` some FPS_IN frames will be used several times to bring the number up to FPS_OUT. Setting this value to 24 is a good value for digital video.
`TIMEBETWEEN` states the time (in seconds) between the frames that you are shooting with your camera. The Raspberry Pi camera takes roughly 6 seconds to take an image, so 6 seconds is the shortest time between shots. 
`FILMLENGTH` works out how long your film will be in seconds. If you want to know this value, then you get your program to print it using the following line.

`print FILMLENGTH`

I am not going to print this out, but I do use it as a reminder of how long the film I am making will be.

Now we have set up all our variables we can get down to business. We want to take images every so often and save them as files. We will want to keep track of how many images we have taken so we know when to stop. So lets create a variable to do that.

`frameCount = 0`

The next thing we will do is enter a WHILE loop. A while loop keeps going WHILE something is true

`while frameCount < FRAMES:`

So while our number in frameCount is less than ( < ) the number we have stored in FRAMES we will run through the next 4 lines of code.

We will create a name for the pictures we want to save.

    `imageNumber = str(frameCount).zfill(7)`
The reason we want to do this is we want the files to be stored with incremental numbers. This line is quite clever (I think!)

It says we will create a variable called imageNumber. In that variable we will store a string of the value in frameCount. Remember a string is classed as text and not a number. Then using the .zfill(7) command we will ensure that the string has 7 digits by filling all preceding digits with a zero if there are not enough numbers. Some examples are:

'1' becomes '0000001'
'123456' becomes '0123456'

It's not a tool you use very often, but if you want something to be a certain amount of characters it's very useful!

Now we have the name of the image file we are going to create, lets take the image! We are going to use the line you can type into the shell command to take the image.

    `os.system("raspistill -o image%s.jpg"%(imageNumber))`

What does this line mean? Well the line for taking pictures with the Raspberry Pi camera and storing it as image.jpg is

`raspistill -o image.jpg`

but this needs to be typed into the command line. Well os.system allows you to access the command line.

You will notice there is a %s in there with %(imageNumber) after the text.

This says, take whatever is in the value imageNumber and put it in place of the %s. So if imageNumber was 0000001 our file would be called image0000001.jpg.

This is a great technique of easily modifying what is in a string.

As we want our number to increase each time we go through the while loop let us now increase frameCount.

    `frameCount += 1`

With imageNumber being made up of frameCount and some leading zeros, each time we run through the while loop we will get a different number as frameCount is increased each time.

Finally we want to be able to vary the time between taking each frame. This allows us to take our images a set distance apart. It takes roughly 6 seconds to take a picture with the Raspberry Pi camera.

    `time.sleep(TIMEBETWEEN - 6) #Takes roughly 6 seconds to take a picture`

Therefore the minimum time between each frame is 6 seconds. If we want the camera to wait 10 seconds per image, as it takes 6 seconds to take a picture we only want to sleep between frames for 4 seconds. Therefore we tell the program to sleep for a period of TIMEBETWEEN - 6.

This brings us to the last line of the code. As I mentioned earlier I found the Raspberry Pi Spy website to have the best details on how to use avconv. They suggest typing the following line into the command line to create video from the images. They also explain why.

`avconv -r 10 -i image%4d.jpg -r 10 -vcodec libx264 -crf 20 -g 15 -vf crop=2592:1458,scale=1280:720 timelapse.mp4`

I have modified this line slightly to make it a little more suitable for our program. I want to be able to set some of the variables using our global variables. Therefore I have changed the line slightly to this.
os.system("avconv -r %s -i image%s.jpg -r %s -vcodec libx264 -crf 20 -g 15 -vf crop=2592:1458,scale=1280:720 timelapse.mp4"%(FPS_IN,'%7d',FPS_OUT))

We already know why we use the os.system command, as this allows us access to the command line. I have also added in a few %s commands to switch in our global variables. This is using the same technique we used on the line where we took the picture. The difference is there are three variables we are switching in.

Let us look at he code inside the os.system brackets.

Most of the code is the same as from the Raspberry Pi Spy webpage, but there are a few differences.


`-r %s` this sets the frame rate for the number of our frames we want to use in the video per second. The %s calls the first item in the list `%(FPS_IN,'%7d',FPS_OUT)`, which is `FPS_IN`, one of our global variables. 
`-i image%s.jpg` determines the name of the images we want to load into our video. Again we call something within our list  `%(FPS_IN,'%7d',FPS_OUT)`, this time the second item. What does `%7d` do? Remember this is being run in the command line, so it's not a python command. The %7d iterates through 7 digit numbers. This is neat as we created our image files with 7 digit numbers. So it is iterating through the images we created. 
`-r %s` as for the first -r %s in this line this sets the number of FPS. However we call the FPS_OUT variable and insert this and not the FPS_IN variable. This will create a video of so many Frames Per Second depending on the value of FPS_OUT. If you are unsure 24 is a good default number to use.

from http://trevorappleton.blogspot.co.uk/2013/11/creating-time-lapse-camera-with.html

##method 3

Next, we can automate the startup so the camera activates each time it is powered up. To do we need to add a cron job. This will mean on each boot of the Raspberry Pi the script will activate and begin capturing images.

sudo nano crontab -e

At the bottom of the script insert

@reboot python /home/raspiLapseCam.py &

Of course change /home to the correct pathway where you have the script.
Save this script (CTRL + X) and “Y”

Rebooting the device will mean this script executes. On testing I found that it needs a full shutdown before this will reliably work each time. So to reuse the device you’ll need to log in, and perform a shutdown with

sudo shutdown "now"

Once you have shut down correctly, you can simply switch on the Raspberry Pi’s battery power supply and the script will launch normally when the device boots up. This is easpecially useful if you’re deploying the timelapse camera for an extended period outside the range of network connections.

```
# sudo python /your/file/location/raspiLapseCam.py (add &) if you wish to run as a
# background task. A process ID will be shown which can be ended with

# sudo kill XXXX (XXXX = process number)

# Based on your settings the application will no begin capturing images
# saving them to your chose file location (same as current location of this file as default.

# Import some frameworks
import os
import time
import RPi.GPIO as GPIO
from datetime import datetime

# Grab the current datetime which will be used to generate dynamic folder names
d = datetime.now()
initYear = "%04d" % (d.year) 
initMonth = "%02d" % (d.month) 
initDate = "%02d" % (d.day)
initHour = "%02d" % (d.hour)
initMins = "%02d" % (d.minute)

# Define the location where you wish to save files. Set to HOME as default. 
# If you run a local web server on Apache you could set this to /var/www/ to make them 
# accessible via web browser.
folderToSave = "timelapse_" + str(initYear) + str(initMonth) + str(initDate) + str(initHour) + str(initMins)
os.mkdir(folderToSave)

# Set the initial serial for saved images to 1
fileSerial = 1

# Run a WHILE Loop of infinitely
while True:
    
    d = datetime.now()
    if d.hour > 2:
        
        # Set FileSerialNumber to 000X using four digits
        fileSerialNumber = "%04d" % (fileSerial)
        
        # Capture the CURRENT time (not start time as set above) to insert into each capture image filename
        hour = "%02d" % (d.hour)
        mins = "%02d" % (d.minute)
        
        # Define the size of the image you wish to capture. 
        imgWidth = 800 # Max = 2592 
        imgHeight = 600 # Max = 1944
        print " ====================================== Saving file at " + hour + ":" + mins
        
        # Capture the image using raspistill. Set to capture with added sharpening, auto white balance and average metering mode
        # Change these settings where you see fit and to suit the conditions you are using the camera in
        os.system("raspistill -w " + str(imgWidth) + " -h " + str(imgHeight) + " -o " + str(folderToSave) + "/" + str(fileSerialNumber) + "_" + str(hour) + str(mins) +  ".jpg  -sh 40 -awb auto -mm average -v")

        # Increment the fileSerial
        fileSerial += 1
        
        # Wait 60 seconds (1 minute) before next capture
        time.sleep(60)
        
    else:
        
        # Just trapping out the WHILE Statement
        print " ====================================== Doing nothing at this time"
```

from http://www.fotosyn.com/simple-timelapse-camera-using-raspberry-pi-and-a-coffee-tin/#video

## Licence

Unless otherwise specified, everything in this repository is covered by the following licence:

![Creative Commons License](http://i.creativecommons.org/l/by-sa/4.0/88x31.png)

***Python Timelapse Photography*** by [Neil Bizzell](https://twitter.com/NeilBizzell) is licenced under a [Creative Commons Attribution 4.0 International License](http://creativecommons.org/licenses/by-sa/4.0/).

Partly based on works at https://github.com/raspberrypilearning/python-picamera-setup, http://trevorappleton.blogspot.co.uk/2013/11/creating-time-lapse-camera-with.html and http://www.fotosyn.com/simple-timelapse-camera-using-raspberry-pi-and-a-coffee-tin/#video
