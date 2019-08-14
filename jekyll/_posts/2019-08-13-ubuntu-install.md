---
layout: post
title: Ubuntu 18.04 Install
categories:
- blog
---

Download and install [Ubuntu](https://ubuntu.com/download/desktop) and just install.

## Root access

open terminal by pressing Ctrl+Alt+T (Ctrl+C to clean)
```sudo -i (or sudo -s)``` 
when promted provide your password

After successful login, the $ prompt would change to # 
to indicate that you logged in as root user on Ubuntu

## SSH

```apt install ssh```

Confirm that SSH server is up and running by executing the bellow command. 
Look for keyword ```Active: active (running)```.
```service ssh status```

In an event that you need to temporarily disable SSH or restart:
```service ssh stop```

To start again run:

```service ssh start```

In order to completely disable SSH after reboots 
```systemctl disable ssh```

To enable SSH again
```systemctl enable ssh```

Config SSH port
```nano /etc/ssh/sshd_config```
remove the # before the line you want to edit
```port 5022```
```PermitRootLogin yes```

to save and exit

```Ctrl + X```
``` Y ```
``` Enter ```

Don't forget to restart ssd ;-)

## ifconfig
To enable ifconfig you need to install net-tools

```apt install net-tools```

run ``` ifconfig```


## Samba (local) share

Now where going to set up an Folder that can be found in the network:



## Deluge

Install "Deluge" from the Ubuntu store 

Open an Terminal and intall

```apt install deluged deluge-console deluge-webui```

Run Deluge and check the box Preferences->Daemon->Allow Remote Connection

```cd /etc/systemd/system/
touch deluged.service
nano /etc/systemd/system/deluged.service```

```[Unit]
Description=Deluge Bittorrent Client Daemon
Documentation=man:deluged
After=network-online.target
[Service]
Type=simple
User=deluge
Group=deluge
UMask=007
ExecStart=/usr/bin/deluged -d
Restart=on-failure
# Time to wait before forcefully stopped.
TimeoutStopSec=300
[Install]
WantedBy=multi-user.target
```
Start Deluge

```sudo systemctl start deluged```

Additionally, enable the Daemon at boot with:

```systemctl enable deluged```


It’s time to set up the user for Deluge Daemon. Use echo to push a new user to the configuration file. 
Change “user” to the name of the existing user on the system. Be sure that you enter the same password as your system user.

> Note: 10 means your system user has full access to modify Deluge.

```echo "user:password:10" >> ~/.config/deluge/auth```

Now that the user is correctly configured, kill the daemon and start it back up. This can be done with systemd or killall.

```sudo systemctl stop deluged```

```sudo systemctl start deluged```

Lastly, enable the Deluge WebUI connection.

```deluge-web --fork```



 
 ref:
 https://www.addictivetips.com/ubuntu-linux-tips/use-deluge-webui-on-linux/
 https://dev.deluge-torrent.org/wiki/UserGuide/ThinClient