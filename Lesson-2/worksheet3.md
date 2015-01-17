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

## Licence

Unless otherwise specified, everything in this repository is covered by the following licence:

![Creative Commons License](http://i.creativecommons.org/l/by-sa/4.0/88x31.png)

***Timelapse Movie*** by [Neil Bizzell](https://twitter.com/NeilBizzell) is licenced under a [Creative Commons Attribution 4.0 International License](http://creativecommons.org/licenses/by-sa/4.0/).

Partly based on works at https://github.com/raspberrypilearning/python-picamera-setup and https://github.com/raspberrypilearning/timelapse-setup
