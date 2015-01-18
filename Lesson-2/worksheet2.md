
#Python Timelapse Photography

In this exercise you will create a python script to take the pictures for the timelapse sequence.

##Step 0: 

```python
import time
import picamera

VIDEO_DAYS = 5
FRAMES_PER_HOUR = 1
FRAMES = FRAMES_PER_HOUR * 24 * VIDEO_DAYS

def capture_frame(frame):
    with picamera.PiCamera() as cam:
        time.sleep(2)
        cam.capture('/home/pi/Desktop/frame%03d.jpg' % frame)

# Capture the images
for frame in range(FRAMES):
    # Note the time before the capture
    start = time.time()
    capture_frame(frame)
    # Wait for the next capture. Note that we take into
    # account the length of time it took to capture the
    # image when calculating the delay
    time.sleep(
        int(60 * 60 / FRAMES_PER_HOUR) - (time.time() - start)
    )
```

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
