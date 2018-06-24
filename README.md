# sdc
requirements:
raspberry pi 3 model b+
l298n motor driver module
jumper wires, breadboard, ultrasonic sensor, etc..

step by step guide to the self driving car, inspired by many sources. will be documenting every step as possible.
first of all, installing ffmpeg in raspberyy pi, a huge pain in the ass. steps:

1. Install library

    cd /usr/src
  
  sudo git clone git://git.videolan.org/x264
  
  cd x264
  
  sudo ./configure --host=arm-unknown-linux-gnueabi --enable-static --disable-opencl
  
  sudo make
  
  sudo make install
  
2. Install ffmpeg

   sudo git clone https://git.ffmpeg.org/ffmpeg.git ffmpeg
   
  cd ffmpeg
  
  sudo git checkout 2ca65fc7b74444edd51d5803a2c1e05a801a6023
  
  sudo ./configure
  
  sudo make -j4
  
  sudo make install

3. configure file

  sudo nano /etc/ffserver1.conf
  
  and input the following in it:

HTTPPort 80

HTTPBindAddress 0.0.0.0

MaxClients 10

MaxBandwidth 50000

NoDaemon


<Feed webcam.ffm>
  
file /tmp/webcam.ffm

FileMaxSize 10M

</Feed>


<Stream webcam.mjpeg>
  
Feed webcam.ffm

Format mjpeg

VideoSize 320x240

VideoFrameRate 10

VideoBitRate 20000

VideoQMin 1

VideoQMax 10

</Stream>


<Stream stat.html>
  
Format status

</Stream>



4. make executable file for easy run:

    sudo nano /usr/sbin/webcam.sh

5. input this and save the file

    sudo ffserver -f /etc/ffserver1.conf & ffmpeg -v quiet -r 5 -s 320x240 -f video4linux2 -i /dev/video0 http://localhost/webcam.ffm


6. Change mode for file webcam.sh so it can execute, then run it

   chmod +x /usr/sbin/webcam.sh

  /usr/sbin/webcam.sh

#to update time: to match video timestamps with time in pi.

sudo dpkg-reconfigure tzdata

#to load the driver:fixes issues.

sudo modprobe bcm2835-v4l2

