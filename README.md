# Zeitgeist

From German Zeitgeist, Zeit ‘time’ + Geist ‘spirit.’

Use a Raspberry Pi 3 to display highly-reliable automated service countdown timers in HD over HDMI from [Planning Center Services Live 3.0](https://planning.center/2014/live-3/).


## Instructions:

(Commands in `code blocks` are to be entered in the [terminal](https://www.raspberrypi.org/documentation/usage/terminal/).)

1. Purchase a Raspberry Pi 3, Case, HDMI cable, USB Micro charging cable and Micro SD Card.

Notes: 
* The official case is very well-designed, and also available in black. 
* Purchasing a Micro-SD card with NOOBS preinstalled will also mak eyour life easier.  
* Use your own USB keyboard and USB mouse. You can [check here for compatibility.](http://elinux.org/RPi_USB_Keyboards).
* Purchase an official charger or use a phone charger (minimum 2A) or USB port at your own risk.

1. [Install Raspbian directly or via NOOBS.](https://www.raspberrypi.org/documentation/installation/installing-images/)

2. Change the password for the default "Pi" user. [(Official Documentation)](https://www.raspberrypi.org/documentation/linux/usage/users.md)
  *  `passwd`
  
  * Follow the prompts (the default password is 'raspberry'), and choose a [strong password](https://xkcd.com/936/). Write it on the Pi's case if desired - unless you use a hardwired ethernet connection, this is going on your WiFi network, and we're worried a lot more about remote intruders than local ones.
  
3. Configure settings
 *  `sudo raspi-config` **or** Menu -> Preferences -> Raspberry Pi Configuration
 *	Expand Filesystem [1] (**this step is critical**)
 * Boot to desktop, automatically logged in as user 'pi'  [3-B4]
 * Set localization (keyboard and system locale) [5]
 *	Enable SSH [9-A4]
 * Reboot when finished.
 
4.	Configure wifi settings. [Official Instructions](https://www.raspberrypi.org/documentation/configuration/wireless/)
5.	[Update and upgrade OS and software packages.](https://www.raspberrypi.org/documentation/configuration/wireless/) This will refresh the list of available packages, and upgrade installed packages to the latest versions. It will also update the kernel and the Raspberry Pi firmware to the latest stable versions.
  *  `sudo apt-get update && sudo apt-get dist-upgrade -y`
  *  This could be very fast or it could take a while (hours) if your hardware or NOOBS card are a few versions behind. Make some tea while you wait. Watch some Dr. Who. Choose a sufficiently British activity to hono**u**r the UK origins of your device.
  *  When this is complete, let's clean up the temporary files: `sudo apt-get clean`
        
6.	Install new software.  (See [below](https://github.com/Chalta/Zeitgeist/blob/master/README.md#software-details) for descriptions of each package)
  * `sudo apt-get install ttf-mscorefonts-installer x11-xserver-utils unclutter xdotool tightvncserver watchdog chromium-browser`
  
  
7.	Configure sleep settings.
  *  `sudo nano /etc/lightdm/lightdm.conf`
  
  * Find the line below in the in the [SeatDefaults] section. Uncomment (remove the #) it and edit it as below :
  
  * `xserver-command=X -s 0 -dpms`
  
8.	Configure start up behaviour.
  *   `sudo nano ~/.config/lxsession/LXDE-pi/autostart`
  
  * Append the lines found [here](https://github.com/Chalta/Zeitgeist/blob/master/autostart).

9.	[Add ability to turn off HDMI signal at certain dates/times.] (https://github.com/Chalta/Zeitgeist/blob/master/rpi-hdmi.sh)

10.	[Insert and configure script to run PCOLive across multiple service types on a preset schedule without human intervention:] (https://github.com/Chalta/Zeitgeist/blob/master/PCOlive.sh)

11.	[Schedule PCO Live and HDMI service in crontab](https://github.com/Chalta/Zeitgeist/blob/master/cron)

12. Configure watchdog daemon to automatically reboot the Pi if hung.  [[Ref]](https://www.raspberrypi.org/forums/viewtopic.php?f=29&t=147501)
  * `echo "dtparam=watchdog=on" | sudo tee -a /boot/config.txt`


## Software Details

| Software | Description | Reference   |
|----------|-------------|--------------|
|ttf-mscorefonts-installer  | Optional: So webfonts render nicely | [Package Details](https://packages.debian.org/jessie/ttf-mscorefonts-installer)		|
|Chromium	| Web Browser	| [Manual Page](https://manpages.debian.org/jessie/chromium/chromium.1.en.html) |
|x11-xserver-utils	| Display utilities needed to turn off screensaver, etc.	| [Package Details](https://packages.debian.org/sid/x11-xserver-utils) |
|unclutter	| Disables mouse pointer	 |[Manual Page](https://manpages.debian.org/jessie/unclutter/unclutter.1.en.html) |
|xdotool	| Simulates keyboard/mouse input	| [Manual Page](https://manpages.debian.org/jessie/xdotool/xdotool.1.en.html) |
|tightvncserver	| Optional: Allows remote control and screensharing so you can control your RPi from another computer.	| [Manual Page](https://manpages.debian.org/jessie/tightvncserver/tightvncserver.1.en.html) |
|Watchdog | Reboots automatically if system is hung. |		[Package Details](https://packages.debian.org/jessie/watchdog) |


## Remotely Accessing your Pi

### SSH

* From a Mac just type the following into Terminal `ssh pi@192.168.X.YY`, using the actual IP address of your RPi.

* On Windows, install [puTTY](http://www.chiark.greenend.org.uk/~sgtatham/putty/).


### VNC 

* On a Mac, just type the following into Terminal: `vnc://192.168.X.YY` using the actual IP address of your RPi.

* On Windows, install [RealVNC](https://www.realvnc.com/).

