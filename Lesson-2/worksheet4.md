# Time-lapse Using BASH

In this lesson you will set up Raspberry Pi and Raspberry Pi Camera module and take time-lapse pictures using a BASH script

## Step 0: Set up the raspberry Pi and Pi Camera Module

1.Set up the RPi and RPi Camera Module like you did last lesson. If you have forgotten how to do this use the [set up worksheet](https://github.com/NBizzell/time-lapse-crystals/blob/master/Lesson-1/worksheet1.md) to remind you. 

## Step 1: Set up your series of pictures

1. At the command line or if you have already opened the GUI open LXTerminal
1. Type `mkdir timelapse` to create a folder to hold the pictures you are about to take.
1. Ensure the camera is pointing towards the classroom.

## Step 2: Write a script

Now we'll write a Bash script which will take a picture and save it with the date and time. It can be as simple as this:

```bash
#!/bin/bash

DATE=$(date +"%Y-%m-%d_%H%M")

raspistill -o /home/pi/timelapse/$DATE.jpg
```

Create a new file called `camera.sh` by opening with a text editor, e.g. `nano bashtl.sh`, paste or otherwise enter the lines from above and save the file.

Now make this file executable with the following command:

```bash
chmod +x bashtl.sh
```


Before continuing, test the script works as intended by running it from the command line (first return to the home directory with `cd`):

```bash
./bashtl.sh
```

You should see the preview again as the picture is taken. Now use `ls timelapse` to look inside the `timelapse` folder to see the picture you just captured on disk.

Open the file browser and preview the image to see the picture itself. If you're happy this worked as intended, and the date and time were given in the filename, continue to automation.

## Step 3: Schedule taking pictures

Now you have a Bash script which takes pictures and timestamps them, you can schedule the script to be run at an interval, say every minute.

To do this we'll use `cron`. First open the cron table for editing:

```bash
sudo nano crontab -e
```

Now you'll see the `cron` file, scroll to the bottom where you'll see a line with the following column headers:

```bash
# m h  dom mon dow   command
```

The layout for a cron entry is made up of six components:

Minute, Hour, Day of Month, Month of Year, Day of Week and the command to be executed.

```
# * * * * *  command to execute
# ┬ ┬ ┬ ┬ ┬
# │ │ │ │ │
# │ │ │ │ │
# │ │ │ │ └───── day of week (0 - 7) (0 to 6 are Sunday to Saturday, or use names; 7 is Sunday, the same as 0)
# │ │ │ └────────── month (1 - 12)
# │ │ └─────────────── day of month (1 - 31)
# │ └──────────────────── hour (0 - 23)
# └───────────────────────── min (0 - 59)
```

To schedule for the `bashtl.sh` script to be executed every minute, add the following line:

```bash
* * * * * /home/pi/bashtl.sh 2>&1
```

Now save and exit. If you're using `nano` as your editor, that's `Ctrl + O` to save and `Ctrl + X` to exit.

You should see the following message:

```bash
crontab: installing new crontab
```

Now return to the time-lapse directory to see the photos start to appear:

```bash
cd ~/timelapse/
```

and use `ls` to see the contents of the folder. Enter `date` to see how close you are to the minute (`00` seconds) as a new picture should be captured at this precise time.

Use `watch ls` to see changes to the contents of the folder. `watch` runs the command runs every 2 seconds (by default).

## Step 4: Patience

If you see pictures landing in the `camera` folder every minute, and you're happy with the orientation of the pictures, now position the camera wherever you want it to point at.

Perhaps use a camera mount or simply tape the Pi to a wall or object and position the camera with tape. Make sure the camera position is static and will remain in place over time.

You can shut down the Pi, remove it from the monitor and Ethernet and simply have it running on power (when you plug it in, it will boot as normal and `cron` will run as expected) in the position you require.

You can even use a battery pack if you have one that lasts long enough for your requirements. This is especially handy if you need to position the power out of reach of a power socket, such as on a roof or in a tree!

## Step 5: Remove the cron job

If you want the camera to keep capturing images then that is fine. However it is likely that at some point you will want to stop the images appearing every minute.

To do this go back to the command line and type `sudo nano crontab -e` and remove the line you previously added. press `ctrl + x` to exitand `y` to save the changes made.

The pictures will stop being captured every minute.


## Step 6: Next steps

Try using [the command line](worksheet1.md) or [python](worksheet2.md) to take the pictures or [combine the pictures you have taken to make a movie](worksheet3.md).



## Licence

Unless otherwise specified, everything in this repository is covered by the following licence:

![Creative Commons License](http://i.creativecommons.org/l/by-sa/4.0/88x31.png)

***Timelapse Using BASH*** by [Neil Bizzell](http://twitter.com/PiVangelist) is licenced under a [Creative Commons Attribution 4.0 International License](http://creativecommons.org/licenses/by-sa/4.0/).

Based on a work at https://github.com/raspberrypilearning/timelapse-setup
