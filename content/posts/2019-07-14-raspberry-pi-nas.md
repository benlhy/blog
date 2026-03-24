---
title: "Raspberry Pi NAS in 2020"
date: 2019-07-14T13:59:35+00:00
slug: "raspberry-pi-nas"
draft: false
description: "Want to access your hard drive from anywhere? Have a spare Raspberry Pi lying about, just waiting for a project? Set up a Raspberry Pi NAS  with updated instructions for 2020!"
cover:
  image: "/images/2019/07/New-Project.jpg"
  relative: false
  hiddenInList: false
tags: ["Raspberry Pi", "Getting Started", "2019", "NAS", "Projects"]
---

Tldr; Build a local Dropbox-like system on a Raspberry Pi without spending hundreds on a dedicated NAS (Network attached storage).

[Updated for 2020]

In 2019, Dropbox limited the number of devices you can connect in a free account, and as someone who has multiple computers (don't ask), this made me pause. I was happy living on the 5GB from Dropbox for many years. Did I really need to upgrade to a 1TB (2TB as of 2020) cloud storage solution?

I have tried a few other solutions. OneCloud, Owncloud, Google Drive... etc, however, I never really found one that worked as well as Dropbox.

There are a few circumstances to my use-case:

- A lot of my file transfers/sync requirements occur in my home network.
- I want to access files on my external hard drive without having to plug/unplug it all the time.

With [Dropbox Plus](https://db.tt/oje9Cdqmsc) (referral link), it does all of the above and more.

- Nice clients to access files on any device.
- Online and accessible from anywhere.
- Secure.
- Version control and backups.

However the issue is that with storage, well, stuff just grows to the space you give it, and the $10/month subscription is something that I'd be unlikely to stop, once started. It is not something that I fancy paying, especially since a viable alternative for me is to shuffle a hard drive around from time to time. Then there's that whole issue of putting your stuff on someone else's device and how much privacy/control you have over that.

Viable $\neq$ convenient. So an alternative is to own a NAS.

A NAS (Network Attached Storage) is a device that serves files on your network. Think of it like a Dropbox, except that you dictate how much storage to add. Not only can you share files around devices, you can (with the proper software) stream content.

However, a NAS (Network Attached Storage) device is very expensive, not even considering the hard drives that you'd have to install on the system yourself. Altogether, a two drive NAS can set you back \$400 ~ \$600.

I wanted to set up an NAS with a Raspberry Pi, but I realized that a lot of the instructions out there were outdated or simply wrong. There is one common tutorial that refers to an auto-usb.ext file that doesn't even exist for autofs. This is the procedure that I followed to set up a Raspberry powered NAS in 2020.

I highly recommend using a Raspberry Pi 4 with 4 GB of RAM. This allows for better streaming of data, as well as opens up the always-on server for other purposes. For example you can use it to run an MQTT broker to connect all the IoT stuff at home, or as a general purpose web server. Better yet, the USB3 port now supplies enough power so that the external hard drive will not need a powered USB hub.

This setup has survived a lot of moves and power cycles so I'm pretty confident that it works properly.

# Method

## Update software

```bash
sudo apt update && sudo apt dist-upgrade -y
```

## Set up VNC/SSH (optional)

You can skip this step if you don't need/want desktop access to your PI. I recommending setting up a SSH connection however.

### VNC

Install the vnc server software if it is not already installed.

```bash
sudo apt install realvnc-vnc-server realvnc-vnc-viewer
```

Enable VNC in **Preferences > Raspberry Pi Configuration > Interfaces > VNC > Enable. **Get the IP address of the raspberry pi by running:

```bash
hostname -I
```

Now with VNC you can continue on your main device instead of being tethered to a screen on the Raspberry Pi.

### SSH

For SSH, you will need to enable it in **Preferences > Raspberry Pi Configuration > Interfaces > SSH > Enable.**

## Install software

```bash
sudo apt install ntfs-3g samba samba-common-bin
sudo mkdir /media/1
sudo cp /etc/samba/smb.conf /etc/samba.smb.conf.bak
sudo nano /etc/samba/smb.conf
```

This installs the software necessary to read a NTFS formatted storage device and the software necessary to set up the NAS. We make a directory at `/media/1` to mount our storage device, which means that it is accessed at the location `/media/1`. Next, we make a backup and edit the samba configuration file:

In file, add under `[global]`:

```bash
security = user
hosts allow = 192.168.1.0/24
```

This will apply some security to your samba installation by only allowing hosts from the local network (192.168.1.0-254) and requesting that they log in with a user. If you don't know your local network, run `ifconfig`. At the end of the file add in:

```bash
[public] 
  comment = public storage 
  path = /media/1
  valid users = @users 
  force group = users 
  create mask = 0660 
  directory mask = 0771 
  read only = no
```

Determine where your external storage is mounted by running:

```bash
sudo blkid
```

Assuming it is mounted at `/dev/sda1`, now you want to test by mounting it in the `/media/1` folder that we created earlier.

```bash
sudo umount /dev/sda1
sudo mount -t auto /dev/sda1 /media/1
```

This mounts the drive in the folder. Browse to the folder and make sure that you can access the contents. Now try to access the storage using another device connected on the network by browsing to `\\[local-pi-ip-address]\public` in Windows Explorer. You should be able to see the storage at this point. Now to set up the mounting process automatically:

```bash
sudo nano /etc/fstab
```

Add this to the last line of the file:

```bash
/dev/sda1 /media/1 auto noatime,nofail 0 0

```

This will automatically mount any drive on `sda1` to `/media/1` on boot.

## Creating a user

We will create a user: `newuser` to access our smb share. We will first create the user, add them to the group users, set their passwords, and then set the smb password.

```bash
sudo useradd newuser -m -G users
sudo passwd newuser
sudo smbpasswd -a newuser
```

## Opening ports

If you have `ufw` installed and it is active, remember to open the port for Samba.

```bash
sudo ufw allow Samba
```

Congratulations! The Raspberry Pi NAS is now set up.

# Connecting with clients

## Windows

To add a network share in Windows:

- Right Click on `This PC`
- Add a Network Location -> Enter IP address of your share eg \\192.168.1.1\public
- Key in the username and password when requested

## VLC on Android

If you want to stream videos on Android that's possible to!

- Local network, star the address.
- Edit the configuration, adding in the password

I noticed that the VLC on Android doesn't save the password if you access it without configuring a favorited location that has been edited, and it doesn't recursively apply the password, so if you don't star the address, at each navigation step, you will need to key in the username and password. I believe this has something to do with SMBv1, but it has not been fixed to date.

# Some lessons

- Get a good SD card. I had a cheap one from Microcenter and it stopped booting about 6 hours into a debugging session. I had to reflash Raspbian on it.
- If it doesn't boot, it might be due to a power issue. If it seems to hang after awhile, it could also be a power issue. Check that the power supply that you are giving the raspberry pi is sufficient.

# Future work

- Figure out a way to do power saving features on the pi.

# References

- [https://howtoraspberrypi.com/create-a-nas-with-your-raspberry-pi-and-samba/](https://howtoraspberrypi.com/create-a-nas-with-your-raspberry-pi-and-samba/)
- [https://pimylifeup.com/raspberry-pi-samba/](https://pimylifeup.com/raspberry-pi-samba/)
