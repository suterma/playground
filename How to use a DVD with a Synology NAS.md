# Watching movies: How to use a DVD with a Synology NAS
[Synology has a bad reputation for it's Video Station](https://community.synology.com/enu/forum/17/post/93830). With this post I am going to explore how to play those .ISO files stored on my Synology on my Samsung TV. I will use:

//TODO read: https://www.synology.com/en-global/knowledgebase/DSM/tutorial/Multimedia/What_should_I_know_about_DS_video_for_Samsung_Smart_TV

- KODI, a free and open-sourced media center
- A dedicated machine to run KODI on
- //A video adapter
- A shared folder on my [Synology DS218+ NAS](https://www.synology.com/en-us/products/DS218)
- A [Samsung UE55RU7170 LCD TV, from Digitec](https://www.digitec.ch/de/s1/product/samsung-ue55ru7170-55-4k-lcd-2019-tv-10470155)

Resources: https://www.easefab.com/tutorial/play-iso-files-on-samsung-tv.html


# Getting the ISO from the DVD
Assuming, you have a single SCSI drive (which is the most common type):

    # get the required block and volume size first
    isoinfo -d -i /dev/sr0 | grep -i -E 'block size|volume size'
    # copy the media content into a single ISO file, 
    # using the block size as 'bs' and the volume size as 'count', e.g:
    dd if=/dev/sr0 of=movie.iso bs=2048 count=2811981

After a moment, this should get you the ISO file.

# Installing KODI

## On Ubuntu 18.04.5 LTS
As per the [official HOW-TO for Linux:](https://kodi.wiki/view/HOW-TO:Install_Kodi_for_Linux#Installing_Kodi_on_Ubuntu-based_distributions)
    
    # software-properties-common provides an abstraction of the used apt repositories (you might already have this)
    sudo apt-get install software-properties-common 
    # The repository for kodi (xmbc is just a perviously used name for it)
    sudo add-apt-repository ppa:team-xbmc/ppa
    # Get the newest package data
    sudo apt-get update
    # Install kodi
    sudo apt-get install kodi

## On a Raspberry Pi

As per the [official HOW-TO for Raspbian:](https://kodi.wiki/view/HOW-TO:Install_Kodi_on_Raspberry_Pi#Raspbian)
    
    # Get the newest package data
    sudo apt-get update
    # Install kodi
    sudo apt-get install kodi

    # Run KODI
    kodi


### HDMI Audio
See the [configuration page over at raspberrypi.org](https://www.raspberrypi.org/documentation/configuration/audio-config.md). 

    # Open up raspi-config by entering the following into the command line:
    sudo raspi-config

The safe choice for TV's with built-in audio is "Advanced Options", "Audio", "Force HDMI". You may want to leave it on "Auto", and then change later as necessary.

# Mount your movies share
You can either host all your movies locally on your KODI machine

# Configure KODI
## Enabling the web server
KODI comes with a built-in web server

-that serves content over HTTP. It's disabled by default, due to secuirty considerations.-

To enable, just follow the [instructions from their wiki](https://kodi.wiki/view/Webserver#Enabling_the_webserver), for the [control settings](https://kodi.wiki/view/Settings/Services/Control):

- "Allow remote control via HTTP": Set to enabled
- I left the port on 8080, because on Linux these higher-numbered ports are useable without privileges. 
- I intentionally did not set a password. I want to give unrestricted access in the internal network.

## Tweaking performance
See the post [LAMP stack on Raspberry Pi with Kodi running at startup](https://www.leowkahman.com/2016/03/05/lamp-stack-on-raspberry-pi-with-kodi-running-at-startup/) by Leow Kah Man for more details. I recommend considering the memory split, and disabling the swap file.

## Autostart at boot
Maybe the simplest approach to this is using the crontab file.

    # Open crontab for the current user
    crontab -e
    # Add the following line to start kodi in the standalone mode (suitable for use without a desktop) after boot
    @reboot kodi --standalone


