
#Python Timelapse Photography

In this lesson you will set up Raspberry Pi and Raspberry Pi Camera module and take timelapse pictures using a python script.

##Step 0: Set up the raspberry Pi and Pi Camera Module

1.Set up the RPi and RPi Camera Module like you did last lesson. If you have forgotten how to do this use the [set up worksheet](../lesson1/worksheet1.md) to remind you. 

## Step 1: Set up your series of pictures

1. At the command line or if you have already opened the GUI open LXTerminal
1. Type `mkdir pythontl` to create a folder to hold the pictures you are about to take.
1. Ensure the camera is pointing towards the classroom.

## Step 2: Write the Python Script to take the pictures

1.Open IDLE and a new window `ctrl+N` save the file as `pythontl.py`

or

at the command line use the command `nano pythontl.py`

2. Type the code eactly as it appears below into the file:

```
import time
import picamera

VDUR = 0.1
FPH = 600
FRAMES = FPH * VDUR 

def capture_frame(frame):
    with picamera.PiCamera() as cam:
        time.sleep(2)
        cam.capture('/home/pi/pythontl/frame%04d.jpg' % frame)

for frame in range(FRAMES):
    start = time.time()
    capture_frame(frame)
    time.sleep(
        int(60 * 60 / FRAMES_PER_HOUR) - (time.time() - start)
    )
```

##Explaination of the code

```
import time
import picamera
```
First we import the python time and picamera libraries that will be used later in the code.

```
VDUR = 0.1
FPH = 600
FRAMES = FPH * VDUR 
```

Then we set the video duration (in  hours) `VDUR` and Frames per Hour `FPH`. in this case we are setting the duration to 0.1 hours (or 3 minutes) and the Frames per hour to 600 (or one photograph every 6 seconds). The total number of frames is then calculated by multiplying these together to give the variable `FRAMES`

```
def capture_frame(frame):
    with picamera.PiCamera() as cam:
        time.sleep(2)
        cam.capture('/home/pi/pythontl/frame%04d.jpg' % frame)
```
The next step is to define a procedure called `capture_frame` to capture and name the pictures. eache picture ius saved in the folder we created earlier `/home/pi/pythontl` and named with `frame` the number passed as an arugument and 4 training zeros e.g. 00001.jpg.

```
for frame in range(FRAMES):
    start = time.time()
    capture_frame(frame)
    time.sleep(
        int(60 * 60 / FPH) - (time.time() - start)
    )
```

This procedure is then called from within and for loop that runs until all of the frames required have been captured. This is defined by the `FRAMES` variable that was calculated earlier. For each frame the time befor ethe start of the capture process is recorded using `start = time.time()`. The procedure to capture the frames is called and passed the frame number `capture_frame(frame)`. 

The last line then waits for the set time before the next frame is due to be captured. it does that by calculating the time interval `(60 * 60 / FPH)` and subtracting the time taken to capture the frame (by subtracting the start time from the current time `(time.time() - start)` 



##extra
```
import os

os.system("avconv -r %s -i image%s.jpg -r %s -vcodec libx264 -crf 20 -g 15 -vf crop=2592:1458,scale=1280:720 timelapse.mp4"%(FPS_IN,'%7d',FPS_OUT))
```

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


## Licence

Unless otherwise specified, everything in this repository is covered by the following licence:

![Creative Commons License](http://i.creativecommons.org/l/by-sa/4.0/88x31.png)

***Python Timelapse Photography*** by [Neil Bizzell](https://twitter.com/NeilBizzell) is licenced under a [Creative Commons Attribution 4.0 International License](http://creativecommons.org/licenses/by-sa/4.0/).

Partly based on works at https://github.com/raspberrypilearning/python-picamera-setup, http://trevorappleton.blogspot.co.uk/2013/11/creating-time-lapse-camera-with.html and http://www.fotosyn.com/simple-timelapse-camera-using-raspberry-pi-and-a-coffee-tin/#video
