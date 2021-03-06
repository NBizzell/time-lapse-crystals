# Command Line Time-lapse photography

In this lesson you will set up Raspberry Pi and Raspberry Pi Camera module and take time-lapse pictures using the command line.

## Step 0: Set up the raspberry Pi and Pi Camera Module

1.Set up the RPi and RPi Camera Module like you did last lesson. If you have forgotten how to do this use the [set up worksheet](https://github.com/NBizzell/time-lapse-crystals/blob/master/Lesson-1/worksheet1.md) to remind you. 

## Step 1: Set up your series of pictures

1. At the command line or if you have already opened the GUI open LXTerminal
1. Type `mkdir timelapse` to create a folder to hold the pictures you are about to take.
1. Ensure the camera is pointing towards the classroom.

## Step 2: Use the command line to start the process

### The raspistill command
You have used the command `raspistill` before to take individual pictures. We are going to use some other attributes of the command to take multiple pictures at specific time intervals. 
 
These attributes are `-tl x` for the interval between photographs and `-t x` for the time for the overall 
time (where x will be replaced by the time interval in milliseconds). for this exercise we are going to take one picture every two seconds for an overall time of  one minute. This means we will use `-tl 2000 -t 60000`

We want to save the output to the `timelapse` folder we created so we will also use the `-o` attribute with the file location `/home/pi/timelapse/class%02d.jpg`. This will save a file each time a picture is taken with the filename `class001.jpg, class002.jpg` with the number increasing by one each time. (the `%02d` adds two trailing zeros to the file name to allow sequential file numbering  `%40d` would give 4 zeros)

Combined together this gives the full command: `raspistill -o /home/pi/timelapse/class%02d.jpg -tl 2000 -t 60000`


### Take the pictures
1. At the command line or LXTerminal type the command `raspistill -o /home/pi/timelapse/class%02d.jpg -tl 2000 -t 60000`
1. A preview image should appear on the screen for the duration of the command running.
1. Wait for the command to finish running (you may wish to walk past the camera slowly occasionally to add interest to your pictures)

## Step 3: Check your pictures have been taken

1. At the command line or in LXterminal type `cd timelapse` to change to the timelapse directory
1. type `ls` to see what is in the folder

If your command worked correctly you should be able to see 30 pictures in the folder.

## Step 4: Next Steps

Try using [python](worksheet2.md) or [BASH](worksheet4.md) to take the pictures or [combine the pictures you have taken to make a movie](worksheet3.md).


## Licence
Unless otherwise specified, everything in this repository is covered by the following licence:

![Creative Commons License](http://i.creativecommons.org/l/by-sa/4.0/88x31.png)

***Command Line Timelapse Photography*** by [Neil Bizzell](https://twitter.com/PiVangelist) is licenced under a [Creative Commons Attribution 4.0 International License](http://creativecommons.org/licenses/by-sa/4.0/).
