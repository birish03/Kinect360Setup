# How to setup the Xbox 360 Kinect for development on Ubuntu 16
1. You're going to need Open CV to do this. It's a  fairly long process so I'm not going to tell you how to do that, here is a link to a great tutorial on how to install openCV. 
When following the tutorial make sure you install python 2.7 and not 3.5, also the making the cv virtual environment is optional.
http://www.pyimagesearch.com/2016/10/24/ubuntu-16-04-how-to-install-opencv/
2. Assuming you have followed that tutorial you should have python 2.7, openCV, and cmake installed.
3. First, we need to make sure that everything is up to date to make sure everything will run smoothly together. 
```
sudo apt-get update
sudo apt-get upgrade
```
4. Next, we need to install some dependencies.
`sudo apt-get install build-essential libusb-1.0-0-dev`
5. Now we are going to download libfreenect this is the main part of the installation.
`git clone git://github.com/OpenKinect/libfreenect.git`
6. Now go into the folder that was made when you cloned libfreenect
7. Time to build libfreenect. Paste each of these commands one by one. Just to make sure it all goes correctly.
```
cmake -L
make -j4
```
8. Depending on how fast your computer is it shouldn't take long for libfreenect to build. Once it's done run these commands
```
sudo make install
sudo ldconfig /usr/local/lib64/
```
9. Your libfreenect is now compiled. We need to do a few more things though. If you don't plan on using your Kinect as root all the time do these commands
```
sudo adduser $USER video
sudo adduser $USER plugdev
```
10. You're going to now add some rules that will let Ubuntu have access to the Kinect's features. First, create the new file
`sudo nano /etc/udev/rules.d/51-kinect.rules`
Now copy and paste the following into the file
```
# ATTR{product}=="Xbox NUI Motor"
SUBSYSTEM=="usb", ATTR{idVendor}=="045e", ATTR{idProduct}=="02b0", MODE="0666"
# ATTR{product}=="Xbox NUI Audio"
SUBSYSTEM=="usb", ATTR{idVendor}=="045e", ATTR{idProduct}=="02ad", MODE="0666"
# ATTR{product}=="Xbox NUI Camera"
SUBSYSTEM=="usb", ATTR{idVendor}=="045e", ATTR{idProduct}=="02ae", MODE="0666"
# ATTR{product}=="Xbox NUI Motor"
SUBSYSTEM=="usb", ATTR{idVendor}=="045e", ATTR{idProduct}=="02c2", MODE="0666"
# ATTR{product}=="Xbox NUI Motor"
SUBSYSTEM=="usb", ATTR{idVendor}=="045e", ATTR{idProduct}=="02be", MODE="0666"
# ATTR{product}=="Xbox NUI Motor"
SUBSYSTEM=="usb", ATTR{idVendor}=="045e", ATTR{idProduct}=="02bf", MODE="0666"
```
Press Ctrl+O then enter to save the file, and then Ctrl+X to exit out of nano
11. Your all done! Plug in your Xbox 360 Kinect and run `freenect-glview` to test out your installation!
12. Just kidding! You're not done! You now need to make libfreenect able to work with python. Start off by installing some more dependencies.
```
sudo apt-get install cython
sudo apt-get install python-dev
sudo apt-get install python-numpy
sudo apt-get install python-opencv
```
13. If you aren't already in your libfreenect directory, go there now and navigate to wrappers, then python. Once you are there enter the following command
`sudo python setup.py install`
14. You should now be done!

