# Time-lapse Crystals

In this activity you will make a video of crystals forming using the Raspberry Pi camera.

## Stage 0: Camera Setup

These instructions assume you have set up your Raspberry Pi camera. If you have not then use the [video / documentation here to set up your Camera](http://raspberrypi.org/help/camera-module-setup/) 

## Stage 1: Set up the experiment

### Create a Saturated Solution

1. Add some of your chosen solute to a beaker of hot water.
1. Continue to add small amounts of the solute and stirring until no more will dissolve.

### Evaporate the Solution

1. Add some of the solution to the petri dish
1. Find somewhere safe to put the bowl with space to position the camera over the dish. A window sill may be a good location.
1. Set up you light source so that the dish is clearly illuminated.

## Stage 2: Set up the camera to take the pictures

### Create a folder for the pictures

1. At the command line or if you have already opened the GUI open LXTerminal
1. Type `mkdir timelapse` to create a folder to hold the pictures you are about to take.
1. Ensure the camera is pointing towards the classroom.

### The raspistill command
You may have used the command `raspistill` before to take individual pictures. We are going to use some other attributes of the command to take multiple pictures at specific time intervals. 
 
These attributes are `-tl x` for the interval between photographs and `-t x` for the time for the overall 
time (where x will be replaced by the time interval in milliseconds). for this exercise we are going to take one picture every two seconds for an overall time of thirty seconds. This means we will use `-tl 2000 -t 30000`

We want to save the output to the `timelapse` folder we created so we will also use the `-o` attribute with the file location `/home/pi/timelapse/class%02d.jpg`. This will save a file each time a picture is taken with the filename `class001.jpg, class002.jpg` with the number increasing by one each time. (the `%02d` adds two trailing zeros to the file name to allow sequential file numbering  `%40d` would give 4 zeros)

Combined together this gives the full command: `raspistill -o /home/pi/timelapse/class%02d.jpg -tl 2000 -t 30000`


### Take some test pictures
1. At the command line or LXTerminal type the command `raspistill -o /home/pi/timelapse/class%02d.jpg -tl 2000 -t 30000`
1. A preview image should appear on the screen for the duration of the command running.
1. Wait for the command to finish running (you may wish to walk past the camera slowly occasionally to add interest to your pictures)

### Check your pictures have been taken

1. At the command line or in LXterminal type `cd timelapse` to change to the timelapse directory
1. type `ls` to see what is in the folder

If your command worked correctly you should be able to see 15 pictures in the folder.

### Take the pictures

1. Work out how long you want to take the pictures for and how often (the formation of copper sulphate crystals took around 24 - 48 hours, more pictures will mean a smoother end result, but will take up more space.)
1. Convert this to milliseconds 

There are some other ways to set up the camera to take the pictures instructions are available here for [using Python](https://github.com/NBizzell/time-lapse-crystals/blob/master/Lesson-2/worksheet2.md) and [ using Bash and Cron](https://github.com/NBizzell/time-lapse-crystals/blob/master/Lesson-2/worksheet4.md).

## Stage 3: Combine the pictures

### Step 1: Copying the pictures from the Raspberry Pi

Use a USB Memory Stick to transfer the files to your computer:

1. Plug the USB stick into the Raspberry Pi and use file manager to transfer the pictures.
1. Unplug the USB stick from the RPi and plug it into the PC. 
1. Transfer the files into you user area. 


### Step 2: Import the Files into Windows Movie Maker

  ![Windows Movie Maker](/images/wmp.png)

1. Open Windows Movie Maker
1. Click where it says "Click here to browse for videos or photos"
1. Browse to where you have saved the pictures.
1. Select the pictures using `Ctrl+a`
1. Click the Open button. 

### Step 3: Set the options for your video

To produce a smooth video effect the frame rate should be around 24fps. To set this frame rate:

1. Select all the frames using `Ctrl+a`.
1. Select the `Edit` tab and choose duration.
1. Set the duration to 0.042

### Step 4: Save the Project and Video

1. Select the drop down menu at the far left and then `Save Project As`

  ![Save Project](/images/saveproject.png)

2. Browse to a suitable location and choose a suitable filename press the Save button.
3. Select the drop down menu at the far left and choose `Save Movie` the `For Computer`

 ![Save Movie](/images/savemovie.png)

4. Browse to a suitable location and choose a suitable filename then click `Save`

### Step 5: View the results

Load the video and see what you have created.

[![Video of Copper Sulphate crystals forming](http://img.youtube.com/vi/SYl7JsgNdz4/0.jpg)](https://www.youtube.com/watch?v=SYl7JsgNdz4)



## What next?

Try experimenting different solutions to see how they form different shape crystals.


## Licence

Unless otherwise specified, everything in this repository is covered by the following licence:

![Creative Commons License](http://i.creativecommons.org/l/by-sa/4.0/88x31.png)

***Time-lapse Crystals*** by [Neil Bizzell](https://twitter.com/PiVangelist) is licenced under a [Creative Commons Attribution 4.0 International License](http://creativecommons.org/licenses/by-sa/4.0/).


partly based on work at [https://github.com/raspberrypilearning/timelapse-setup](https://github.com/raspberrypilearning/timelapse-setup)
