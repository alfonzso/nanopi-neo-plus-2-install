
# How to use ubuntu + neo + EMMC

Firstly, go here:

https://wiki.friendlyelec.com/wiki/index.php/NanoPi_NEO_Plus2#Install_OS

In there you are looking for flashable image with eflasher capability, like
```bash
h5_eflasher_friendlycore-focal_4.14_arm64_20210615.img
```
It's an ubuntu server with eflasher.


There has to be one download link, like this:

http://download.friendlyelec.com/nanopineoplus2

## !! There is a chance to link will not work, or removed etc !!

Another possibility: https://www.armbian.com/nanopi-neo-plus2/

It may work, but you can't flash to EMMC ... idon't know

You can download ubuntu focal image for neo plus 2 from here:

https://1drv.ms/u/s!AmAxP3FO83qPoEPo_Y6aEPbU0dbn?e=7YT4vZ

##### HOW-TO-ARMBIAN: Get a Debian Stretch from https://dl.armbian.com/nanopineoplus2/, then install OMV using armbian-config and finally transfer the installation to EMMC with nand-sata-install<br><br>

# Move image to sd card

After you got the image, you want to dd to sd card

with this command:<br>

```bash
dd if=h5_eflasher_friendlycore-focal_4.14_arm64_20210615.img of=/dev/mmcblk0 count=100MB status=progress
```
##### another option to use `win32diskimager` in windows but is lame ...<br><br>

After dd, you ready to rollll...

Put the card into neo, then pray to god to work, cuz its there a chance to not to...<br>
If it has IP addresse and you can log in via ssh, then you are good, go to `flash with eflasher` section

# Sd card seems not booting / no IP and ssh
Put sd card into linux env, mount sd partition to somewhere, you will see with lsblk
```bash
$ lsblk
mmcblk0                   179:0    0  14,7G  0 disk
├─mmcblk0p1               179:1    0   1,7G  0 part
├─mmcblk0p2               179:2    0   100M  0 part
└─mmcblk0p3               179:3    0   1,7G  0 part
```

after mount /dev/mmcblk0p1, you will see these files:
```bash
ls /mnt/neo/
eflasher.conf  friendlycore-focal_4.14_arm64
```

Edit elfasher.conf:
```bash
[General]
; Automate OS installation at system startup,
; The "autoStart" field specifies the path of your firmware,
; Available values: friendlycore-focal_4.14_arm64
autoStart=
```
to
```bash
autoStart=friendlycore-focal_4.14_arm64
```

After boot finished, eflasher automaticly start the flash process.

## One more step to flash our img to EMMC
It is optional but whatever, it is better than noting...<br>
Remove static ip from interface/eth0 and enable dhcp

Mount /dev/mmcblk0p3, you will see the whole filesystem, there open `interfaces.d/eth0` file:
```bash
cat /mnt/neo/etc/network/interfaces.d/eth0
auto eth0
iface eth0 inet dhcp
# iface eth0 inet static
#     address 192.168.1.231
#     netmask 255.255.255.0
#     gateway 192.168.1.1
```
shown above, comment static ip and enable dhcp, save it !

After that, you can ssh with root user, you can watch with iotop when flash process will be finished.<br>
Reboot, and remove sd card, ubuntu will boot from EMMC

I got the instructions from here: https://wiki.friendlyelec.com/wiki/index.php/EFlasher

You can skip the `Flash with eflasher` section ...

# Flash with eflasher
Go there, follow the instructions:<br>
https://wiki.friendlyelec.com/wiki/index.php/NanoPi_NEO_Plus2#Flash_OS_with_eflasher_Utility


# Final words
* It recommended to change root password, dissable root login with password and with ssh.
* Remove default pi user
* add new user, disable ssh password login, add pub-key to your user etc
* run npi-config

## About npi-config
In gui:<br>
| Syntax      | Description |
| ----------- | ----------- |
| 1) Change User Password                               | skip, pi will be removed
| 2) Hostname                                           | optional, if you want a pretty host name
| 3) Boot options => B1 ( Autologin ) => B1 ( Console ) | This will disable 'pi' user auto login
| 4) Local..                                            | don't care
| 5) Interface Options                                  | Enable ssh server

After reboot, you can run `deluser pi`

## Create own OS to EMMC
https://github.com/friendlyarm/sd-fuse_s5p6818