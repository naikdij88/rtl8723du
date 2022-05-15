rtl8723du
===========
This repository includes drivers for the following card:

Realtek RTL8723DU

### Installation instruction
##### Requirements
You will need to install "make", "gcc", "kernel headers", "kernel build essentials", and "git".

For **Ubuntu**: You can install them with the following command
```bash
sudo apt-get update
sudo apt-get install make gcc linux-headers-$(uname -r) build-essential git
```
For **Fedora**: You can install them with the following command
```bash
sudo dnf install kernel-headers kernel-devel
sudo dnf group install "C Development Tools and Libraries"
```
For **openSUSE**: Install necessary headers with
```bash
sudo zypper install make gcc kernel-devel kernel-default-devel git libopenssl-devel
```
For **Arch**: After installing the necessary kernel headers and base-devel,
```bash
git clone https://aur.archlinux.org/rtw89-dkms-git.git
cd rtw89-dkms-git
makepkg -sri
```
If any of the packages above are not found check if your distro installs them like that.

##### Installation
For all distros:
```bash
git clone git://github.com/lwfinger/rtl8723du.git -b v5.13.4
cd rtl8723du
make
sudo make install
```

##### How to unload/reload a Kernel module
 ```bash
sudo modprobe -rv 8723du         #This unloads the module
sudo modprobe -v 8723du          #This loads the module
```

***********************************************************************************************

When your kernel changes, then you need to do the following:
```bash
cd ~/rtl8723du
git pull
make clean
make
sudo make install
```

Remember, this MUST be done whenever you get a new kernel - no exceptions.

##### Installation with module signing for SecureBoot
For all distros:
```bash
git clone git://github.com/lwfinger/rtl8723du.git
cd rtl8723du
make
sudo make sign-install
```
You will be promted a password, please keep it in mind and use it in next steps.
Reboot to activate the new installed module.
In the MOK managerment screen:
1. Select "Enroll key" and enroll the key created by above sign-install step
2. When promted, enter the password you entered when create sign key. 
3. If you enter wrong password, your computer won't not bebootable. In this case,
   use the BOOT menu from your BIOS, to boot into your OS then do below steps:
```bash
sudo mokutil --reset
```
Restart your computer
Use BOOT menu from BIOS to boot into your OS
In the MOK managerment screen, select reset MOK list
Reboot then retry from the step make sign-install



****************************************************************8
Ok there is an issue with the dkms.conf that causes problems, so do sudo dkms remove rtl8723du/0.1 --all and sudo rm -r /usr/src/rtl8723du-0.1 Then go into the rtl8723du directory in your home directory and double click on dkms.conf and paste this in

PACKAGE_NAME="rtl8723du"
PACKAGE_VERSION=0.1
MAKE="'make' all KVER=${kernelver}"
CLEAN="make -C $kernel_source_dir clean"
BUILT_MODULE_NAME[0]="8723du"
DEST_MODULE_LOCATION[0]="/updates"
REMAKE_INITRD=no
AUTOINSTALL=yes



*********************************************************************
