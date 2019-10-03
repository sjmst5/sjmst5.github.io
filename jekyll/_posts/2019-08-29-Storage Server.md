---
layout: post
title: Storage Server
categories:
- blog
---
# **Freenas**
![keyboard](/media/2019/08/keyboard.jpg "keyboard")
### Install
Download [Freenas](https://www.freenas.org/download-freenas-release/) and burn the iso on an (flash)drive with [Win32DiskImager](https://sourceforge.net/projects/win32diskimager/).
Boot into the install drive and follow the instructions.
After installation the ip of the web user interface will be prompted.

Enter your web user interface, `Username = root` did you set your pw to `toor`😉? 

### Set an account
1. Click on Accounts from the left panel.
2. Click on Users.
3. Under the “Users” section, click the Add button.
4. Fill out the name, username, and password fields.
5. Click the Save button.

### Create an storage pool
1. Click on Storage from the left pane.
2. Click on Pools.
3. Under the “Pools” section, click the Add button.
4. Select the Create new pool option.
5. Click the Create Pool button.
6. Type a name for the new storage pool — for example, StorageCollection1.
>(Optional) Check the Encryption option.
7. Check the Confirm option.
8. Click the I Understand button.
9. Under the “Available Disks” section, select the drives that will participate in the storage pool.
10. Click the Right arrow button to add the drives to the “Data VDevs” section.
11. Under the “Data VDevs” column, use the drop-down menu and select the Raid-Z option to create a storage pool with redundancy and performance.
These are all the available layout options when setting up a pool with FreeNAS:

>Raid-Z — single drive parity similar to RAID5.
Raid-Z2 — double drive parity similar to RAID6.
Raid-Z3 — which uses triple drive parity.
Stripe — data is shared on two drives (similar to RAID0).
Mirror — copies data on two drives (similar to RAID1, but not limited to 2 disks).

12. Click the Create button.
13. Click the Confirm option.
14. Click the Create Pool button.
15. Click the Download Recovery Key button if you selected the “Encryption” option, and then the Done button once you saved the file key.


### Create an Windows (Samba)
1. 



![frikandel](/media/2019/08/frikandel.jpg "A cute del")

As OS we will we use ___[Proxmox](https://www.proxmox.com/en/downloads/item/proxmox-ve-6-0-iso-installer)___

Inside of this surrounding we will install an lite ___[Debian](https://cdimage.debian.org/debian-cd/current/amd64/iso-cd/debian-10.1.0-amd64-xfce-CD-1.iso)___ version to create the server.
The server will host the NAS storage for individual users, also one general share.
All of these shares will have an backup to dropbox.
This will be running an Samba Share.



Hypervisor-
vm- +os -> nextcloud
depencies




- backup Cloud service (Dropbox)

ZFS

https://pureinfotech.com/setup-network-file-sharing-freenas/