---
layout: post
title: Personal File Server on Raspberry Pi
date: 2019-01-20
---

## Synopsis
I recently completed setup of my personal file server running on [raspberry pi](https://www.raspberrypi.org/). For beginners, [NextCloud](https://nextcloud.com/) is a personal, free alternative to services such as icloud or dropbox. I will walk you through the steps involved in this setup and my overall experience with this tool.

![Raspberry Setup](/assets/rpi.png)

## Introduction
### Hardware Requirement
* [Raspberry PI 3](https://www.newegg.com/Product/Product.aspx?Item=N82E16813142011) - 1
* [Micro SD Card](https://www.amazon.com/Samsung-MicroSD-Adapter-MB-ME128GA-AM/dp/B06XWZWYVP) - 1
* [External hard drive](https://www.amazon.com/Elements-Desktop-Hard-Drive-WDBWLG0080HBK-NESN/dp/B07D5V2ZXD) - 2
* [USB Micro power supply](https://www.amazon.com/Raspberry-Power-Supply-Adapter-Charger/dp/B0719SX3GC/) - 1

### Software Requirement
* [Raspberry Setup](https://projects.raspberrypi.org/en/projects/raspberry-pi-setting-up/3) - Follow these instructions to setup Raspberry Pi


## Setup

We need to ensure that the external harddrive is always mounted to the same folder location under /mnt
Firstly, we need to gather the UUID of the external hard drive.
Run following command to output this information
```
lsblk --fs
```
Output
```
sda
+-sda1      ntfs     easystore E81EB2EA1EB2B0C4                     /mnt/hd_homesink
sdb
+-sdb1      ntfs     easystore DA88A08288A05F2F                     /mnt/hd_nextcloud

```
Make a note of the UUID and make an entry into the /etc/fstab as follows.
This will ensure that everytime the raspi is booted it will mount in the same location

File: Make you following entry in /etc/fstab

```
UUID=E81EB2EA1EB2B0C4 /mnt/hd_homesink ntfs async,big_writes,noatime,nodiratime,nofail,uid=1000,gid=1000,umask=007 0 0

UUID=DA88A08288A05F2F /mnt/hd_nextcloud ntfs async,big_writes,noatime,nodiratime,nofail,uid=1000,gid=1000,umask=007 0 0
```

Create this shell script to backup the content from one external drive to another
File: nextcloud_backup.sh
``` shell
rsync -aPv /mnt/hd_nextcloud/data /mnt/hd_homesink/nextcloud_backup/
```


File: system_backup.sh
``` shell
#!/bin/bash
#
# Automate Raspberry Pi Backups
#
# Kristofer Källsbo 2017 www.hackviking.com
#
# Usage: system_backup.sh {path} {days of retention}
#
# Below you can set the default values if no command line args are sent.
# The script will name the backup files {$HOSTNAME}.{YYYYmmdd}.img
# When the script deletes backups older then the specified retention
# it will only delete files with it's own $HOSTNAME.
#

# Declare vars and set standard values
backup_path=/mnt/hd_homesink/pi1
retention_days=1

# Check that we are root!
if [[ ! $(whoami) =~ "root" ]]; then
echo ""
echo "**********************************"
echo "*** This needs to run as root! ***"
echo "**********************************"
echo ""
exit
fi

# Check to see if we got command line args
if [ ! -z $1 ]; then
   backup_path=$1
fi

if [ ! -z $2 ]; then
   retention_days=$2
fi

# Create trigger to force file system consistency check if image is restored
touch /boot/forcefsck

# Perform backup
dd if=/dev/mmcblk0 of=$backup_path/$HOSTNAME.$(date +%Y%m%d).img bs=1M

# Remove fsck trigger
rm /boot/forcefsck

# Delete old backups
find $backup_path/$HOSTNAME.*.img -mtime +$retention_days -type f -delete

```

