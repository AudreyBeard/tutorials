# Configuring a Samba Server on a Raspberry Pi
Suppose you've got a [Raspberry Pi](raspberrypi.org) and an external drive lying around, and you want to free yourself from the shackles of being wired to it, or having to eject the disk properly. I get it. That's why I configured my Raspberry Pi as a Samba server!

## Assumptions:
- Raspberry Pi 4 connected to your network
- Raspbian Buster
- Familiarity with command line

You could probably do this with a RPi 3 or lower, but you'll need to make sure you have WiFi or ethernet access to it

## Overview
- Configure drive
- Mount drive at startup
- Install and configure Samba

## Configure drive
- format disk (**ERASES ALL DATA!**):
    - `sudo fdisk <PATH_TO_DEVICE>`:
        - press `o` to create a new table (DOS)
        - press `n` to create a new partition
        - press `p` to make it the primary partition
        - press `1` to make it the first partition
        - press `w` to write to disk
    - `<PATH_TO_DEVICE>` is location of device and is probably something
      like `/dev/sda`
- Configure an EXT4 file system:
    - `sudo mkfs.ext4 <PATH_TO_PARTITION>`
    - `<PATH_TO_PARTITION>` is probably something like `/dev/sda1`
- Label this partition for easier reference:
    - `sudo e2label <PATH_TO_PARTITION> <LABEL_NAME>`

## Mount drive at startup
- Edit `/etc/fstab` as `sudo`:
    ```
   # device           mountpoint   fstype  options                                      dump fsck
   LABEL=<LABEL_NAME> <MOUNTPOINT> ext4    defaults,nofail,x-systemd.device-timeout=1ms 0    2
    ```
- Check this with `sudo mount -a`:
    - If it doesn't work, edit the file, you messed something up
- If the machine doesn't boot after this, eject the microSD card, stick it in a Linux box, and comment out the line - you done messed up    

## Install and configure Samba
- Update and upgrade apt repos:
    - `sudo apt update; sudo apt upgrade`
- Install Samba:
    - `sudo apt-get install samba samba-common-bin`
- Create mount point wherever you want:
    - `mkdir -m 1777 <MOUNTPOINT>`
    - `chmod 777 <MOUNTPOINT>` to give rwx permissions to all
    - I set `<MOUNTPOINT>` to something like `/home/shareX`
- Configure Samba to look for and share drive:
    - Edit `/etc/samba/smb.conf` as `sudo`:
        ```
        [<SHARE_NAME>]
        path = <MOUNTPOINT>
        #writeable=Yes
        write list = <SAMBA_USER>
        create mask=0777
        directory mask=0777
        public=yes
        browseable=yes
        guest only=no
        guest ok=yes
        ```
    - I set `<SAMBA_USER>` to the machine's user: `pi`    
- Configure password for Samba user:
    - `sudo smbpasswd -a pi`
    - I used a different password than my Pi's password for security
    - Save that thing!

## Sources
- https://pimylifeup.com/raspberry-pi-samba/
- https://askubuntu.com/questions/154180/how-to-mount-a-new-drive-on-startup#154184
- https://wiki.archlinux.org/index.php/Fstab
- https://www.cyberciti.biz/faq/linux-partition-howto-set-labels/
- https://superuser.com/questions/174776/modify-fstab-entry-so-all-users-can-read-and-write-to-an-ext4-volume
