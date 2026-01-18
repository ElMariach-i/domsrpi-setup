# domsrpi-setup
this repository contains information to setup my astrophotography environment that consists of
- raspberry pi 5
- skywatcher az eq5 mount
- skywatcher 200p telescope
- raspberry pi hq camera (sony imx477)
- gamepad
- remote control laptop (windows & debian dualboot)

## Overview
```
┌─────────────────────────────────────────────────────┐
│                                                     │
│                                                     │
│                remote control laptop                │
│                                                     │
│                                                     │
└─────────────────────────┬───────────────────────────┘
                          │  using indi or ascom over wifi                         
┌─────────────────────────┴───────────────────────────┐
│                                                     │
│                                                     │
│                  raspberry pi                       │
│                                                     │
└────────────┬───────────────────────────┬────────────┘
             │  csi connector            │  proprietary protocol over usb           
┌────────────┴───────────┐ ┌─────────────┴────────────┐
│                        │ │                          │
│         imx477         │ │        az eq5 mount      │
│                        │ │                          │
└────────────┬───────────┘ └─────────────┬────────────┘
             │ cs to 1.25 inch adapter   │             
┌────────────┴───────────────────────────┴────────────┐
│                                                     │
│                                                     │
│             Skywatcher 200p telescope               │
│                                                     │
│                                                     │
└─────────────────────────────────────────────────────┘
```
(ascii-art using [www.asciiflow](https://asciiflow.com/#/))


## raspberry pi 5
### operating system installation
follow the steps [here](https://www.raspberrypi.com/documentation/computers/getting-started.html#raspberry-pi-imager) to create a bootable sd card with raspberry os for the rpi. make sure to use the os customization options: here you have the option to configure wifi settings. this will enable connection to rpi over wifi in headless mode, e.g. if no monitor or input devices are connected to rpi.
### kstars and ekos installation and set-up
1. there are several scripts for installing both indilib (contains indiserver) and kstars/ekos latest on raspberry pi. i used the following successfully:
[https://gitea.nouspiro.space/nou/astro-soft-build](https://gitea.nouspiro.space/nou/astro-soft-build) test: run `kstars -v` in terminal and check that the version equals the current version documented [here](https://kstars.kde.org/). run `indiserver -h` in terminal and check that the indi library version equals the version documented [here](https://github.com/indilib/indi).
2. If indi ccd-simulation drivers are needed: make sure that guide star catalog is installed. However, this is not well documented since it has been excluded from kstars installation process. 

### additional packages installation
#### n.i.n.a.
#### siril
1. clone the [repo](https://gitlab.com/free-astro/siril)
2. install dependencies and make siril according to the description [here](https://siril.readthedocs.io/en/stable/installation/source.html). be aware that building siril from source potentially requires to build some dependencies (i.e. libXISF) in addition.
3. good entry level tutorials can be found [here](https://siril.org/tutorials/tuto-scripts/)
## skywatcher az eq5 connection and control protocol set-up
The eqmod driver is typically installed as third party component during indilib-installation. Test: start kstars. start ekos. create or change a profile: chose eqmod 'EQMod Mount' for mount. run the profile. if the indiserver is able to load the driver, then it is correctly installed.
## skywatcher 200p telescope mechanical assembly with camera and mount
## raspberry pi hq camera connection and control protocol set-up
the raspberry pi hq camera comprises the sony imx477 chip that provides basic capabilities for astrophotography. there is a indi-driver for the imx477 chip that is, however, work in progress since a while and has as of now heavy limitation e.g. no support of longer exposures [https://indilib.org/raspberry-pi/raspberry-pi-camera.html](https://indilib.org/raspberry-pi/raspberry-pi-camera.html).
There is, however, a driver based on python3 that supports a feasible feature set to use imx477 for astrophotography: [https://github.com/scriptorron/indi_pylibcamera](https://github.com/scriptorron/indi_pylibcamera).
1. install the driver as mentioned in the link above. this will involve setting up a virtual environment to run the driver.
2. activate the venv and running the indiserver with this driver in the venv (this can be automated using a script that the os runs after boot automatically): `source ~/<path-to-venv>/bin/activate` `indiserver indi_pylibcamera`
3. in kstars, start ekos. create or change a profile such that it connects to this server and potentially load additional drivers. however, kstar requires at least a ccd driver to be part of the profile. therefore, just add as ccd-driver the indi-simulator-ccd driver. If the indiserver runs on the same pc where the image acquisition/telescope control software is running, then you still need to click `remote` in the ekos profile and name the server `localhost`. This way, ekos will use the existing server and not try to delete the old one by creating a new one. 
## gamepad connection and control protocol set-up
## remote control laptop connection and usage using wifi
## mobile phone and other applications
### polar scope align pro by astro.ecuadors.net
### telescopius by sebastian garcia

## Usage
### planning observations
### preprocessing (darks, flats etc.)
### image acquisition
### postprocessing
