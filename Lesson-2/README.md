#Time-lapse photography

##easy /short way

raspistill -o /home/pi/camera/sky%04d.jpg -tl 60000 -t 18000000

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


##Python code way
