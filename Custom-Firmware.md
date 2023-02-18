So why a custom firmware??

because we can 
- reconfigure router
- no need of post config processes 
- can simplyfy upgrade process

<h3>So let's compile our custom firmware.. shall we???</h3>
We will be targeting compiling such a firmware that will have no need of configuration after booting up with Pi

Fire up your linux machine with atleast 25 gb of free space and ....

clone the openwrt repository

>$ git clone https://git.openwrt.org/openwrt/openwrt.git 

>$ cd openwrt

now we have to select the version we wil be building..

for latest use the master branch

to search branch 

>$ git branch -a

then search for tag branch

>$ git tag

when we find our tag branch we will change to that branch using git chekout

>$ git checkout v2x.x.x

now we are in our desired branch we need to take few more options in consideration.

we have to update and install this repo. to do this we have to run two scripts

>$ ./scripts/feeds update -a

(needed g++ and libncurses5-dev to install. any other dependencies neededd based on the os should be installed 
prior script install)

>$ ./scripts/feeds install -a

<h3>Now it's time to customize</h3>

we have to install a terminal gui program to easily add remove or change our customization.

first run make menuconfig

>$ make menuconfig

it will load after the process is done. after it's loaded, we have to choose and make changes
Target build system & subsystem

Packages required to be part of the build – Base packages, packages required for system administration, development packages & extra packages.

Firmware for various (wireless) chips/modules

Kernel modules
Option to enable support for various programming languages & libraries

1. target system >
I'll be using a pi 3B+ module. so fir that chipset, I'll be using Broadcom BCM27xx

2. subtarget >
The Subtarget will be BCM2708 for the original Raspberry Pi boards and Pi Zero variants, while, the second option is preferred for Raspberry Pi 2B (BCM2709), Raspberry Pi 3B (BCM2710), and Raspberry Pi 4 (BCM2711)

3. target profile >
will be automatically selected on choosing subtarget

4. target images >
you can build the filesystem however you want but i'll be using squashfs because its benefits of being read only file system will protect the integrity of storage media and will be able to reset the filesystem
 for the kernel size no need to change
 for the root partition size, minus a couple gigs from the total file storage and use it.
 hover up to the election and press enter. make 8k initially
 
Now a crucial part of fining and selecting packages. we can use a vim-style searching using forward slash to search for the packages.

1. kmod-usb-net-asix-ax88179 > for usb keyboard and mouse

to select the package go over it and press space twice. there must be an asterix mark beside the name otherwise the package won't be installed while compiling, only would be made as file.

2. kmod-usb-hid > human usb support

3. kmod-usb2 > usb2 support

4. kmod-usb3 > usb3 support

5. kmod-usb-uhci > uhci controllers

6. kmod-usb-ohci > ohci controllers

we will select luCI ( LUCI (Library for User-Centered Interfaces) is a toolkit of digital assets to help product teams design and build high-quality, cohesive user experiences quickly and efficiently.) 

navigate to luCI and Collections

select 

7. luci, and 

8. luci-ssl, for https support

we will configure vpn so ..

9. luCI > Application > select luci-app-openvpn

10. network > vpn > openvpn-openssl

11. utilities > usbutils

12. utilities > editor > nano-full

13. kernel modules > wireless drivers > kmod-rt2800-usb

14. kernel modules > wireless drivers > kmod-rt2x00-lib

if you want to add your wireless device, better do your research on driver here.

if you want to configure wireguard vpn,

15. network > vpn > wireguard-tools

16. luci > application > luci-app-wireguard


After we are done with this basic configuration there is still plenty we can do. 

save and exit and we will start our customization

You can modify the file “banner” in “~/openwrt/package/base-files/files/etc” folder so that your image shows a custom pre-login message at boot time. FIGlet Linux utility can create ASCII art from the text using the command line. Simply install it, and create ASCII art using the commands below.

>$ sudo apt-get install figlet

>$ figlet "Surfer OS" > ~/banner.txt

If you want to enable UART (universal asynchronous receiver-transmitter, is one of the most used device-to-device communication protocols aka hardware communiation protocol) by default, you have to place your custom settings under /openwrt/target/linux/bcm27xx/image/config.txt 

>$ cd /openwrt/target/linux/bcm27xx/image/config.txt

place enable_uart=1 under placeyourcustomsettings

you can also configure your network, wireless, DHCP etc

first we have to create a file under openwrt/etc/config. it will be as root directory in target installation.

make diretory under ~/openwrt/package/base-files/files/etc

>$ mkdir ~/openwrt/files/etc/config

>$ touch ~/openwrt/files/etc/config/network

>$ touch ~/openwrt/files/etc/config/dhcp

>$ touch ~/openwrt/files/etc/config/wireless

>$ touch ~/openwrt/files/etc/config/firewall

<h3>now set up network</h3>
>$ nano ~/openwrt/files/etc/config/network

see https://github.com/MrB1sw4s/Travel-Router-with-Pi/blob/5dec14f3a8a47ee35241e912a873f76f8934e301/network-config
	
<h3>now set up firewall</h3>
>$ nano ~/openwrt/files/etc/config/firewall

see https://github.com/MrB1sw4s/Travel-Router-with-Pi/blob/da5bc22294812a1d15ab687ffbee2d050f8e9be8/firewall-config

<h3>now set up dhcp</h3>
>$ nano ~/openwrt/files/etc/config/dhcp

see https://github.com/MrB1sw4s/Travel-Router-with-Pi/blob/5a534876fc2e64b890cff12766ba07af90ee3f63/dhcp-config

<h3>now set up wireless</h3>
>$ nano ~/openwrt/files/etc/config/wireless

see https://github.com/MrB1sw4s/Travel-Router-with-Pi/blob/34fd681c818596192c89653efbffa42df3678c9a/wireless-config        

<h3>now it's time to build the firmware</h3> 

If you try compiling OpenWrt on multiple cores and don't download all source files for all dependency packages it is very likely that your build will fail.

>$ make download

make sure enough space is available (min 25 gb)

>$ make 

(use make j(core+1) to fully use the cpu. like for 6 core cppu use make -j7. but download dependencies first.)

might take an hour on 1st time.

if complete congrats!! the file will be under ~/openwrt/bin/targets/bcm27xx/bcm2710/

let's go to hardware configuration.
