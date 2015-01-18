
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

3. Save the code using `ctrl+s` in IDLE or `ctrl+x` then `y` in Nano

###Explaination of the code

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

##Step 3: Test the code

Run your code to see if it has been created correctly.

1. At the command line type `python pythontl.py` or press `F5` in the IDLE window to start the code. The code will start to run. The camera should take one picture every six seconds for three minutes.
1. Wait for the code to complete.
1. At the command line type `cd pythontl` to change into the folder we created earlier
1. Type `ls` to show the contents of the folder. If the script worked correctly there should be 30 picture in the folder.


##extra

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
