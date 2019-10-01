---
layout: post
title: Storage Server
categories:
- blog
---

![keyboard](/media/2019/08/keyboard.jpg "keyboard")
## Freenas
Download [Freenas](https://www.freenas.org/download-freenas-release/) and burn the iso on an (flash)drive with [Win32DiskImager](https://sourceforge.net/projects/win32diskimager/).
Boot into the install drive and follow the instructions.
After installation the ip of the web user interface will be prompted.


![frikandel](/media/2019/08/frikandel.jpg "A cute del")

As OS we will we use ___[Proxmox](https://www.proxmox.com/en/downloads/item/proxmox-ve-6-0-iso-installer)___

Inside of this surrounding we will install an lite ___[Debian](https://cdimage.debian.org/debian-cd/current/amd64/iso-cd/debian-10.1.0-amd64-xfce-CD-1.iso)___ version to create the server.
The server will host the NAS storage for individual users, also one general share.
All of these shares will have an backup to dropbox.
This will be running an Samba Share.


![frikandel](/media/2019/08/frikandel.jpg "A cute del")

Hypervisor-
vm- +os -> nextcloud
depencies




- backup Cloud service (Dropbox)

ZFS