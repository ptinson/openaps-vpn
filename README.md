# OpenVPN Config

This is to manage the installation of an openvpn server and simplify the management of clients.

This means that your servers will automatically join the vpn when possible, and you dont need to worry about dynamic addresses.

This document needs a lot of tidy up...

## Installing

Simply copy this script to your centos or ubuntu server.

Make the script executeable with chmod:
`chmod +x ./openvpn-config.sh`

## First Run

The first time you run this, you will install the openvpn application and all of its requirements.

Each time you run this afterwards you will have a set of options:  

>`What do you want to do?`  
`1) Add a new user`  
`2) Revoke an existing user`  
`3) Remove OpenVPN`  
`4) Exit`  
`Select an option [1-4]:`

## Setting Up A Client

When you add a new user a config file is created.
It will be named `<clientname>.ovpn`

You can do this for any machine or person you want to allow access to your vpn.
### Server as a client
+ Log into the machine you want to connect to the vpn and make sure you are root.
+ Install openvpn:
 + For ubuntu machines: `apt-get install openvpn`
 + Copy the client .ovpn file to `/etc/openvpn/<clientname.conf>`


+ Edit the default config for openvpn:
 + `vi /etc/default/openvpn`
 + Uncomment `# AUTOSTART="all"`


+ You can then start the client.
 + For ubuntu machines: `service openvpn start`
 + Check to see if you connect: `ping 10.8.0.1`
 + You should see:  
 `PING 10.8.0.1 (10.8.0.1) 56(84) bytes of data.`  
 `64 bytes from 10.8.0.1: icmp_seq=1 ttl=64 time=141 ms`
 + Also take a not of your vpn ipaddress:  
 You should see an interface called `tun0:` and look for `inet addr:10.8.0.2`

### Mac OS as a client

I use [TunnelBlick](https://www.tunnelblick.net) as my openvpn client.

Once its installed you simply import the client config and connect.

Once you are connected you can ssh to your server through the vpn like: `ssh root@10.8.0.2`

Assuming your server is on that address.
