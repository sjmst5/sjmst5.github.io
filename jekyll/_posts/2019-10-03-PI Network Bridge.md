---
layout: post
title: PI Network Bridge
categories:
- blog
---
# **Pi Network Brige**
![keyboard](/media/2019/08/keyboard.jpg "keyboard")
### Easy Raspberry Pi WiFi Bridge

A Raspberry Pi WiFi bridge is one of the best ways of providing internet access to a device that only supports an Ethernet connection.

Raspberry Pi WiFi Bridge
In this tutorial, we will show you how to setup a WiFi bridge, and how to setup dnsmasq correctly to allow any device to connect through your Pi without issues.

You will need to keep in mind that you will not see speeds as good as what you would with a direct connection to your router. As there is some overhead with the connection having to run through the Raspberry Pi.

Remember to do this tutorial you will need either a WiFi dongle or a Raspberry Pi 3 with the inbuilt WiFi module.

This tutorial can be combined with our basic VPN access point, you can find the tutorial on how to set that up directly after this tutorial. Basically, it will show how to setup a OpenVPN client and redirect all traffic through that client.

Please note: This tutorial will require some slight changes if you decide to go down the VPN route, we will explain the necessary changes needed at the end of this tutorial.

 Equipment List
Below are all the bits and pieces that I used for this Raspberry Pi WiFI bridge tutorial, you will need A Wireless internet connection to be able to complete this tutorial.

Recommended
 Raspberry Pi 2 or 3

 Micro SD Card

 Wifi dongle (The Pi 3 has WiFi inbuilt)

 Ethernet Connection

Optional
 Raspberry Pi Case

 Setting up the WiFi Bridge
To setup the Raspberry Pi Wifi bridge we will be utilizing the dnsmasq package, this package handles most of the grunt work for this tutorial.

Dnsmasq is a package that acts as both a local DHCP server and a local DNS server. We utilize this package so that we can assign IP addresses and process DNS requests through the Raspberry Pi itself and act like a router.

One of the bonuses to utilizing dnsmasqis that it is very easy to configure while being somewhat lightweight in comparison to the isc-dhcp-server and bind9 packages.

Remember for this tutorial you will need to have an active WiFi router to connect to and an ethernet device you intend on bridging the Wi-Fi connection to.

1. Before we get started with installing and setting up our packages, we will first run an update on the Raspberry Pi by entering the following two commands into the terminal.

sudo apt-get update
sudo apt-get upgrade
2. With that done we can now install the one and only package we will be utilizing, run the following command to install dnsmasq.

sudo apt-get install dnsmasq
3. Before we get too far ahead of ourselves, we should setup the wlan0 connection that we plan on using. If you have already setup your wireless connection then you can skip ahead to step 5.

Otherwise open up the wpa_supplicant file by running the following command:

sudo nano /etc/wpa_supplicant/wpa_supplicant.conf
4. Within this file add the following, making sure you replace the ssid with the name of the network you want to connect to and replace the psk value with the password for that network.

network={
        ssid="networkname"
        psk="networkpassword"
}
5. With the wireless network now setup to correctly connect we can proceed with setting up our eth0 interface. This will basically force it to use a static IP address, not setting this up can cause several issues.

To do this we need to modify the dhcpcd.conf file by running the following command:

sudo nano /etc/dhcpcd.conf
 Important Note: If you’re on Raspbian stretch then wlan0 and eth0 may need to be changed if predictable network names is turned on. Use the ifconfig command to see the new names, they’re likely quite long and will contain the MAC address.

Make sure you update these for all the commands in this tutorial.

6. Within this file we need to add the following lines, make sure you replace eth0 with the correct interface of your ethernet.

interface eth0
static ip_address=192.168.220.1/24
static routers=192.168.220.0
Now we can save and quit out of the file by pressing Ctrl+X then pressing Y and then Enter.

7. With our changes made to dhcpcd configuration we should now restart the service by running the following command:

sudo service dhcpcd restart
8. Before we get started with modifying dnsmasq’s configuration we will first make a backup of the original configuration by running the following command.

sudo mv /etc/dnsmasq.conf /etc/dnsmasq.conf.orig
9. With the original configuration now backed up and moved out of the way we can now move on and create our new configuration file by typing the command below into the terminal.

sudo nano /etc/dnsmasq.conf
10. Now that we have our new file created we want to add the lines below, these lines basically tell the dnsmasq package how to handle DNS and DHCP traffic.

interface=eth0       # Use interface eth0  
listen-address=192.168.220.1   # Specify the address to listen on  
bind-interfaces      # Bind to the interface
server=8.8.8.8       # Use Google DNS  
domain-needed        # Don't forward short names  
bogus-priv           # Drop the non-routed address spaces.  
dhcp-range=192.168.220.50,192.168.220.150,12h # IP range and lease time
Now we can save and quit out of the file by pressing Ctrl+Xthen pressing Y and then Enter.

11. We now need to configure the Raspberry Pi’s firewall so that it will forward all traffic from our eth0 connection over to our wlan0 connection. Before we do this we must first enable ipv4p IP Forwarding through the sysctl.conf configuration file, so let’s begin editing it with the following command:

sudo nano /etc/sysctl.conf
12. Within this file you need to find the following line, and remove the # from the beginning of it.

Find:

#net.ipv4.ip_forward=1
Replace with:

net.ipv4.ip_forward=1
Now we can save and quit out of the file by pressing Ctrl+X then pressing Y and then Enter.

13. Now since we don’t want to have to wait until the next reboot before the configuration is loaded in, we can run the following command to enable it immediately.

sudo sh -c "echo 1 > /proc/sys/net/ipv4/ip_forward"
14. Now that IPv4 Forwarding is enabled we can reconfigure our firewall so that traffic is forwarded from our eth0 interface over to our wlan0 connection. Basically this means that anyone connecting to the ethernet will be able to utilize our wlan0 internet connection.

Run the following commands to add our new rules to the iptable:

sudo iptables -t nat -A POSTROUTING -o wlan0 -j MASQUERADE  
sudo iptables -A FORWARD -i wlan0 -o eth0 -m state --state RELATED,ESTABLISHED -j ACCEPT  
sudo iptables -A FORWARD -i eth0 -o wlan0 -j ACCEPT
 Note: If you get errors when entering the above lines simply reboot the Pi using sudo reboot.

15. Of course iptables are flushed on every boot of the Raspberry Pi so we will need to save our new rules somewhere so they are loaded back in on every boot.

To save our new set of rules run the following command.

sudo sh -c "iptables-save > /etc/iptables.ipv4.nat"
16. Now with our new rules safely saved somewhere we need to make this file be loaded back in on every reboot. The most simple way to handle this is to modify the rc.local file.

Run the following command to begin editing the file.

sudo nano /etc/rc.local
17. Now we are in this file, we need to add the line below. Make sure this line appears above exit 0. This line basically reads the settings out of our iptables.ipv4.nat file and loads them into the iptables.

Find:

exit 0
Add Above:

iptables-restore < /etc/iptables.ipv4.nat
Now we can save and quit out of the file by pressing Ctrl+X then pressing Y and then Enter.

18. Finally all we need to do is start our dnsmasq service. To do this, all you need to do is run the following command:

sudo service dnsmasq start
19. Now you should finally have a fully operational Raspberry Pi WiFi Bridge, you can ensure this is working by plugging any device into its Ethernet port, the bridge should provide an internet connection to the device you plugged it into.

To ensure everything will run smoothly it’s best to try rebooting now. This will ensure that everything will successfully re-enable when the Raspberry Pi is started back up. Run the following command to reboot the Raspberry Pi:

sudo reboot
 Setting up the Raspberry Pi WiFi Bridge with a VPN
This tutorial is fully compatible with the basic VPN router tutorial. However there is one small change you will have to make in step 13, rather than using the commands showcased there, run the commands below.

Basically the main change you will see here is that instead of redirecting the traffic from wlan0 through the tunnel we will be redirecting the traffic from our eth0 connection to the tunnel.

sudo iptables -t nat -A POSTROUTING -o tun0 -j MASQUERADE
sudo iptables -A FORWARD -i tun0 -o eth0 -m state --state RELATED,ESTABLISHED -j ACCEPT
sudo iptables -A FORWARD -i eth0 -o tun0 -j ACCEPT
The rest of the VPN Access Point tutorial can be done without any other changes.

Hopefully by now you should have a fully operational Raspberry Pi WiFi Bridge. If you come across any issues or have some feedback related to this tutorial, then please don’t hesitate to leave a comment below.

![frikandel](/media/2019/08/frikandel.jpg "A cute del")
ref: https://pimylifeup.com/raspberry-pi-wifi-bridge/