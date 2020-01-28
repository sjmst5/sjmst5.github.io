---
layout: post
title: OrangePiOne
categories:
- blog
---
## **Little Vpn server with desktop**
In this blog where going to create an Vpn server with WireGuard and run the desktop with VNCserver (remote desktop) with SSH server enabled.

## OS
Download and write the ISO from the Armbian website and login with `root` `1234`. Chance the password and create an user account. Run the command `armbian-config` go into `system settings` and install the `desktop with 3rd party apps`. After that just install the latest 'Firmware'. After the reboot login as `root`. Run the command `apt-get update && apt-get upgrade`.

## VNCserver
Install the VNCserver `apt-get install tightvncserver`. Start the VNCserver service `vncserver`. Set up an pw and if you need you can enable a view-only pw.
With every reboot the service needs to be enabled!

## OpenVPN
Install WireGuard the easy way with pivpn `curl -L https://install.pivpn.io | bash` and select WireGuard but if you want you can choose for OpenVPN. The default port is 51820 (don't forget to open this port Firewall). Reboot and run `pivpn add` to add vpn clients.
When you are on an mobile device you can also run `pivpn -qr`to create an qr login.
>If you install OpenVPN the install is easy also, the port is by default 1194.

## Desktop
The desktop itself is already enabled and running by now, some handy apps you might want to install are an screensaver `apt-get install xscreensaver xscreensaver-gl-extra xscreensaver-data-extra` and select the GLmatrix screensaver. Also handy is disks `apt-get install gnome-disk-utility`. I woudn't recomend an browser but if you wish `apt-get install firefox-esr`.

## 2FA / MFA
I'm still working on the 2FA in the VPN server, some handy links might be:`https://www.howtoforge.com/securing-openvpn-with-a-one-time-password-otp-on-ubuntu` `http://techblog.sillifish.co.uk/2017/04/05/openvpn-on-a-raspberry-pi-using-pivpn/``https://www.youtube.com/watch?v=a6mjt8tWUws` `https://toub.es/2017/07/13/docker-raspberry-pi-perfect-combo/`
`https://toub.es/2018/02/15/low-cost-vpn-solution-with-two-factor-authentication-on-a-raspberry-pi/`