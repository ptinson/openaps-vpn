# OpenVPN Config

This script is to manage the installation of an OpenVPN server and simplify the management of who can access your VPN.

This documentation explains how to configure your server or [OpenAPS](https://openaps.org) rig so that they can automatically join the VPN when on a network with internet connectivity.

The main benefits of running your own VPN are:
+ You don't need to work out the address of your server if its on dynamic addresses
+ You can grant access to a third party for support in a secure manner
+ Your access to the server is over a secure network

This document needs a lot of tidy up...

## TODO
+ Windows OpenVPN client configuration
+ Linux OpenVPN client configuration
+ How To for using Duo Security with Push authorization
+ Tidy this document up...

## A Server

If you already have a server you want to use then skip to [Installing](#Installing).

I currntly use [Digital Ocean](https://www.digitalocean.com/) for my servers.  
You can run a simple OpenVPN server on the $10 / Month machines.  
Both Ubuntu and CentOS images are available.

## Installing

Simply copy this script to your CentOS or Ubuntu server.

Make the script executable with chmod:
`$> chmod +x ./openvpn-config.sh`

## First Run

The first time you run this, it will install the OpenVPN application and all of its requirements.  
Running the script just requires you to enter the below:
> `$> ./openvpn-config.sh`

Each time you run this script afterwards you will have a set of options:  

>`What do you want to do?`  
`1) Add a new user`  
`2) Revoke an existing user`  
`3) Remove OpenVPN`  
`4) Exit`  
`Select an option [1-4]:`

## Setting Up A Client

When you add a new user a config file is created.
It will be named:
> `<clientname>.ovpn`

The file is created in the same directory as the script is run from.

You can do this for any machine or person you want to allow access to your VPN.

### A Server As A Client
For example this could be your [OpenAPS](https://openaps.org) rig.
+ Log into the machine you want to connect to the VPN and make sure you are root with `whoami`.
 >`$> whoami`  
 `root`

+ If you are not root
>`$> sudo su -`

+ Install openvpn:
 + For ubuntu machines:
 >`$> apt-get install openvpn`

 + For centos machines:
 >`$> yum install openvpn`

 + Either copy `<client>.ovpn` or the contents of the file to
 > `/etc/openvpn/<clientname.conf>`

+ Edit the default config for openvpn:
 + `$> vi /etc/default/openvpn`
 + Change this line `# AUTOSTART="all"` to `AUTOSTART="all"` to allow the OpenVPN server to automatically start the connection to your VPN server.


+ You can then start the client.
 + For ubuntu machines:
 >`$> service openvpn start`

 + Check to see if you connect:
 >`$> ping 10.8.0.1`  
 You should see:  
 `PING 10.8.0.1 (10.8.0.1) 56(84) bytes of data.`  
 `64 bytes from 10.8.0.1: icmp_seq=1 ttl=64 time=141 ms`

 + Also take a not of your VPN ipaddress:  
 You should see an interface called `tun0:` and look for `inet addr:10.8.0.2`

### Mac OS as a client

I use [TunnelBlick](https://www.tunnelblick.net) as my openvpn client.

Once its installed you simply drag the `<client>.ovpn` file into the configurations section of Tunnelblick or onto the tray icon.  
You should then have the option to connect.

Once you are connected you can ssh to your server through the VPN like:
> `$> ssh root@10.8.0.2`

Assuming your server is on that address.
