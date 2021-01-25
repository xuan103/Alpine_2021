# Chapter 8. Install and Configure Dnsmasq

* * * 
## 目錄
-    [Installing Dnsmasq on Ubuntu 18.04](#Step1)

-    [Adding DNS records to Dnsmasq](#Step2)

-    [Testing Dnsmasq DNS functionality](#Step3) 

-    [Configure Dnsmasq as DHCP Server](#Step4) 

* * *

<h1 id="Step 1">Installing Dnsmasq on Ubuntu 18.04</h2> 

Run the following commands to disable the resolved service:
    
$ `sudo systemctl disable systemd-resolved`

$ `sudo systemctl stop systemd-resolved`
    
Also, remove the symlinked resolv.conf file
$ `ls -lh /etc/resolv.conf `
    
    lrwxrwxrwx 1 root root 39 Aug  8 15:52 /etc/resolv.conf -> ../run/systemd/resolve/stub-resolv.conf
    
$ `sudo rm /etc/resolv.conf`

Then create new resolv.conf file.

$ `sudo nano /etc/resolv.conf`
    
    nameserver 8.8.8.8
    
$ `sudo apt-get install dnsmasq`

$ `sudo nano /etc/dnsmasq.conf`

If you want to enable DNSSEC validation and caching, uncomment
    
    #dnssec
    
Make any other changes you see relevant and restart dnsmasq when done:

$ `sudo systemctl restart dnsmasq`

---
<h1 id="Step 2">Adding DNS records to Dnsmasq</h2>

$ `sudo nano /etc/hosts`
    
    10.1.3.4 server1.mypridomain.com
    10.1.4.4 erp.mypridomain.com 
    192.168.10.2 checkout.mypridomain.com 
    192.168.4.3 hello.world
    
$ `sudo systemctl restart dnsmasq`

---
<h1 id="Step 3">Testing Dnsmasq DNS functionality</h2>

$ `sudo nano /etc/resolv.conf`

    nameserver 127.0.0.1
    nameserver 8.8.8.8

---
<h1 id="Step 4">Configure Dnsmasq as DHCP Server</h2>

* Default gateway IP address
* DNS server IP address (Probably Dnsmasq or different DNS server)
* Network Subnet mask
* DHCP Addresses range
* NTP server

See below example

$ `sudo nano /etc/dnsmasq.conf`
    
    dhcp-range=192.168.40.130,192.168.40.230,24h
    dhcp-option=option:router,192.168.40.254
    dhcp-option=option:dns-server,192.168.40.254
    dhcp-option=option:netmask,255.255.255.0

Restart dnsmasq and configure clients to obtain an IP address from this server.

$ `sudo systemctl restart dnsmasq`

---
[返回至 - 目錄](https://github.com/xuan103/Alpine_2021)