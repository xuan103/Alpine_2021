# Chapter 7. NAT & DNAT

* * *
## 目錄

-    [NAT](#NAT)
-    [DNAT](#DNAT)

* * *
<h3 id="NAT">NAT</h3> 

`sudo nano /etc/netplan/00-installer-config.yaml`

gw@gw:~$ `sudo netplan generate`

gw@gw:~$ `sudo netplan apply`

gw@gw:~$ `ping hient.net`
    
    PING hient.net (64.131.64.91) 56(84) bytes of data.
    64 bytes from server2.twdomain.com (64.131.64.91): icmp_seq=1 ttl=50 time=197 ms
    64 bytes from server2.twdomain.com (64.131.64.91): icmp_seq=2 ttl=50 time=196 ms
    

gw@gw:~$ `sudo apt update`
    
    Hit:1 http://tw.archive.ubuntu.com/ubuntu bionic InRelease
    Hit:2 http://tw.archive.ubuntu.com/ubuntu bionic-updates InRelease
    

`sudo nano /etc/sysctl.conf`

Find and uncomment the following line:
    
    net.ipv4.ip_forward=1
    
gw@gw:~$ `sysctl net.ipv4.ip_forward`
    
    net.ipv4.ip_forward = 0
    
gw@gw:~$ `sudo reboot`

gw@gw:~$ `sysctl net.ipv4.ip_forward`
    
    net.ipv4.ip_forward = 1
    
`sudo iptables -t nat -A POSTROUTING -o wlx7cdd90ea4126 -j MASQUERADE`

`sudo apt-get install iptables-persistent -y`
    
    ipv4 -- yes 
    ipv6 -- yes
    
---
<h3 id="DNAT">DNAT</h3> 

`sudo iptables -t nat -A PREROUTING -p tcp --dport 2210 -j DNAT --to-destination 192.168.40.10:22`

`sudo iptables -t nat -A PREROUTING -p tcp --dport 2220 -j DNAT --to-destination 192.168.40.20:22`

`sudo iptables -t nat -A PREROUTING -p tcp --dport 2221 -j DNAT --to-destination 192.168.40.21:22`

`sudo iptables -t nat -A PREROUTING -p tcp --dport 2222 -j DNAT --to-destination 192.168.40.22:22`

`sudo iptables -t nat -A PREROUTING -p tcp --dport 2223 -j DNAT --to-destination 192.168.40.23:22`

`sudo iptables -t nat -A PREROUTING -p tcp --dport 2230 -j DNAT --to-destination 192.168.40.30:22`

---
<h3 id="references">參考文件</h3>

 - https://medium.com/@exesse/how-to-make-a-simple-router-gateway-from-ubuntu-server-18-04-lts-fd40b7bfec9

---

[返回至 - 目錄](https://github.com/xuan103/Alpine_2021)
