# 8192cu dkms driver source code for Ubuntu 16.04 LTS (Tested with Kernel 4.15.0-60)

I have a RTL8188CUS USB wifi card (this driver may work with some other RTL USB wifi card as well, but I don't have device to test), and I want it to be used in my Ubuntu 16.04 machine. But the OS ported rtl8192 dirver really sucks, and the connection drops very frequently. I have searched for very long for a workable driver, but got no luck (the most case is that the driver is only compiling for Kernel 2.* or 3.*), then I decided to work out one version myself, and here it is. Hope it helps you as well if you had the same problem like me had.

**Please use at your discretion!!!**

A simple guide on how to install it:

1. First you need some basic packages installed (if you don't have ethernet, use your iphone as hotspot and share via cables):
   - sudo apt-get install --reinstall linux-headers-$(uname -r) linux-headers-generic build-essential dkms git

2. Download the source code:
   - sudo git clone (this repo url) /usr/src/8192cu-4.0.2.9000.20130911
 
3. Build and install via DKMS:
   - sudo dkms add -m 8192cu -v 4.0.2.9000.20130911
   - sudo dkms build -m 8192cu -v 4.0.2.9000.20130911
   - sudo dkms install -m 8192cu -v 4.0.2.9000.20130911

4. Blacklist the OS ported driver:
   - sudo echo -e "blacklist rtl8192cu\nblacklist rtl8xxxu" | sudo tee -a /etc/modprobe.d/blacklist.conf 

5. In the last reboot, and then check if it is the 8192cu loaded when your usb card inserted, if yes then congrat, you should have the driver working, and normally it should be much better the OS ported one.


I don't have much sophisticated knowledge or experience with this driver, so if you have any further question I probably won't be able to answer it. But if you find any issue and you are able to solve it, please feel free to do so and submit your commit..


/////////////////////////////////////Readme from previouse repo see below//////////////////////////////////////////

## About:
Allows easy installation of patched rt8192cu driver via dkms.

### Before installation:
###### This is only relevant if you have installed a driver for rt8192cu once before!
- Remove the none dkms version of this driver:
  - Change to the old driver directory (where the "Makefile" is)
  - Open terminal in this directory
  - Type: sudo make uninstall
- Remove the dkms version of this driver:
  - Change to the driver directory (where the "./remove_driver.sh" is)
  - Open terminal
  - Type: sudo ./remove_driver.sh

### Install dkms-driver:
1. Unpack rt8192cu_dkms.zip
2. Open terminal in rt8192cu_dkms
3. Type: sudo make install_dkms

### Uninstall dkms-driver:
1. Open terminal in rt8192cu_dkms
2. Type: sudo make uninstall_dkms

### Uninstall dkms-driver manually:
1. Open terminal
2. Type: sudo dkms remove -m 8192cu -v 4.0.2.9000.20130911 --all
4. Type: sudo rmmod 8192cu
5. Type: sudo modprobe rtl8192cu

### Info:
- Newest realtek driver:
  - Name: RTL8192CU
  - Version: 4.0.2_9000
  - Release: 2013/10/29
- Tested on Ubuntu & Arch distributions
- Using procfs implementation from: https://github.com/kolasa/RTL8188C_8192C_USB (tested on kernels up to 4.5)
- Use makefile to install / uninstall driver
