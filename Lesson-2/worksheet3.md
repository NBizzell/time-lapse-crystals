Once the images have been captured you can use the following FFMPEG command line to construct a video from the images, after installing `ffmpeg` with `sudo apt-get install ffmpeg`:

```
ffmpeg -y -f image2 -i /home/pi/Desktop/frame%03d.jpg -r 24 -vcodec libx264 -profile high -preset slow /home/pi/Desktop/timelapse.mp4
```

Be aware that encoding will take at least half an hour of computation on the Pi to produce the video! You may wish to perform this step on a faster machine.

### Copying pictures remotely

If you have network connection with the Pi (wired or wireless) or your monitor is still attached, you can check the progress of the photos.

If your monitor is attached, you can use `ls`, `watch ls` and even the file browser to see photos as they are captured, otherwise you can remotely access your Pi from another computer to copy the files to your computer. Here are some options, which you can read about in our documentation:

#### SSH

You can gain remote access to the command line using [SSH](http://www.raspberrypi.org/documentation/remote-access/ssh/README.md) use `ls` and `watch ls` to verify the pictures are being captured.

#### SCP

Use [SCP](http://www.raspberrypi.org/documentation/remote-access/ssh/scp.md) (Secure copy) to copy files over SSH.

#### rsync

Use [rsync](http://www.raspberrypi.org/documentation/remote-access/ssh/rsync.md) to syncronise a folder on the Pi with a folder on your computer.

#### FTP

Set up an [FTP](http://www.raspberrypi.org/documentation/remote-access/ftp.md) server on the Pi and use an FTP client on another computer to access the Pi's filesystem remotely, and copy files over.

#### SD Card

If you're using Linux on another computer you can transfer the files directly from the SD card, as it can mount the filesystem partition.

## Step 5: Turn stills in to a video

Now you'll need to stitch the photos together in to a video to achieve the time-lapse effect.

You can do this on the Pi using `mencoder` but the processing will be slow. You may prefer to transfer the image files to your desktop computer or laptop and processing the video there.

Navigate to the folder containing all your images and list the file names in to a text file. For example:

```bash
ls *.jpg > stills.txt
```

### On Raspberry Pi or other Linux computer

Install the package mencoder:

```bash
sudo apt-get install mencoder
```

Now run the following command:

```bash
mencoder -nosound -ovc lavc -lavcopts vcodec=mpeg4:aspect=16/9:vbitrate=8000000 -vf scale=1920:1080 -o timelapse.avi -mf type=jpeg:fps=24 mf://@stills.txt
```

This will save a video called `timelapse.avi`

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

## Licence

Unless otherwise specified, everything in this repository is covered by the following licence:

![Creative Commons License](http://i.creativecommons.org/l/by-sa/4.0/88x31.png)

***Timelapse Movie*** by [Neil Bizzell](https://twitter.com/NeilBizzell) is licenced under a [Creative Commons Attribution 4.0 International License](http://creativecommons.org/licenses/by-sa/4.0/).

Partly based on works at https://github.com/raspberrypilearning/python-picamera-setup, from http://trevorappleton.blogspot.co.uk/2013/11/creating-time-lapse-camera-with.html  and https://github.com/raspberrypilearning/timelapse-setup
