
### Time-lapse photography

A time-lapse sequence can be easily constructed by running the basic capture program in a loop, and adjusting the delay to the gap required. Timelapse sequences can be used to visualise many natural processes that are not easy to see otherwise such as plant growth and decay, long term construction projects and weather cycles. Here is some example code:

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

Once the images have been captured you can use the following FFMPEG command line to construct a video from the images, after installing `ffmpeg` with `sudo apt-get install ffmpeg`:

```
ffmpeg -y -f image2 -i /home/pi/Desktop/frame%03d.jpg -r 24 -vcodec libx264 -profile high -preset slow /home/pi/Desktop/timelapse.mp4
```

Be aware that encoding will take at least half an hour of computation on the Pi to produce the video! You may wish to perform this step on a faster machine.

### Stop-motion animation

In a similar vein, it's quite easy to construct a stop-motion animation system with the Pi's camera. The "capture when activated" program can be run in a loop (as in the timelapse program above) to continually capture images when the button is pressed. The same FFMPEG command line can then be used to construct the final video. The major challenge with stop-motion animation is keeping the camera and the target scene sufficiently static. Here is some example code:

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

Once the frames have all been captured, a similar FFMPEG command line to the one presented above can be used to convert the frames into a video:

```
ffmpeg -y -f image2 -i /home/pi/Desktop/frame%03d.jpg -r 24 -vcodec libx264 -profile high -preset slow /home/pi/Desktop/stop_motion.mp4
```
