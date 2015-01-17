#Command Line Time-lapse photography

In this lesson you will set up Raspberry Pi and Raspberry Pi Camera module and take timelapse pictures using the command line.

##Step 0: Set up the raspberry Pi and Pi Camera Module

Set up the RPi and RPi Camera Module like you did last lesson. If you have forgotten how to do this use the [set up worksheet](../lesson1/worksheet1.md) to remind you. 

## Step 1: Set up your series of pictures

Make a folder to hold you pictures
work out how long to take pictures for

## Step 2: Use the command line to start the process

'raspistill -o /home/pi/camera/sky%04d.jpg -tl 60000 -t 18000000'

explain how this works -tl is interval of 60000 which is 60000 miliseconds. 
/home bit is file name with sequential numbering.

explain rest of raspbstill command.

--timelapse, -tl Timelapse mode.

The specific value is the time between shots in milliseconds.
Note you should specify %04d at the point in the filename where
you want a frame count number to appear. For example:

-t 30000 -tl 2000 -o image%04d.jpg

will produce a capture every 2 seconds over a total period of
30s, named image1.jpg, image0002.jpg...image0015.jpg. Note
that the %04d indicates a four-digit number with leading zeros
added to pad to the required number of digits. So, for example,
%08d would result in an eight-digit number.

## Step 3: check your pictures have been taken

use ls to check the contents of the folder

###licence
Unless otherwise specified, everything in this repository is covered by the following licence:

![Creative Commons License](http://i.creativecommons.org/l/by-sa/4.0/88x31.png)

***Timelapse Movie*** by [Neil Bizzell](https://twitter.com/NeilBizzell) is licenced under a [Creative Commons Attribution 4.0 International License](http://creativecommons.org/licenses/by-sa/4.0/).

Partly based on a work at https://github.com/raspberrypilearning/python-picamera-setup
