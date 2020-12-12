Install camera drivers for internal cam guide:

https://raspberrytips.com/raspberry-pi-camera-as-webcam/

For autostart:

sudo cp motion-mmalcam-both.conf /etc/motion/motion.conf

Then edit the daemon configuration file: 

sudo nano /etc/default/motion

And change the start_motion_daemon option to “yes”. 




