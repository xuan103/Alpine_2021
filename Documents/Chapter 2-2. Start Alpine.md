# Chapter 2-2. Start Alpine

* * *
## 目錄

-   [使用 Alpine](#use)
    -   [使用 root 帳號](#uselogin)
    -   [建立使用著帳號](#adduser)
    -   [測試網路](#ipaddr)
    -   [嘗試遠端連線稱](#ssh)
    -   [安裝 nano](#nano)
    -   [設定固定 ip](#dhcp)
    -   [設定 dns server](#dns)

- [參考文件](#references)

* * *

<h2 id="use">使用 Alpine</h2>

<h3 id="uselogin">使用 root 帳號</h3>

![1. uselogin](https://i.imgur.com/6mvM8NK.png)

* 使用剛才設定的密碼登入。

輸入1： `root` <br />

輸入2： `root` <br />

---
<h3 id="adduser">建立使用著帳號</h3>

* 主要是讓 alpine 可以透過 ssh 遠端連線。

* 因為安全性問題，root 帳號不宜開啟 ssh。

![2. adduser](https://i.imgur.com/xYrej82.png)

* 設定使用著帳號及密碼。

* 此為範例。

輸入1： `adduser zhuang ` <br />

輸入2： `123` <br />

輸入3： `123` <br />

* `adduser` 為建立使用著命令，`zhuang`為使用著名稱。

---
<h3 id="ipaddr">測試網路</h3>

![3. ipaddr](https://i.imgur.com/Sh2GRfY.png)

輸入： `ip addr` <br />

* `ip addr`為尋找 ip 命令。

* 使用 `ping -c 4 8.8.8.8` 測試網路是否有通。<br />
（ `-c 4` 為 ping 四次即停止，`8.8.8.8` 為 google ip。）

---
<h3 id="ssh">嘗試遠端連線</h3>

![4. ssh](https://i.imgur.com/diqSNZ1.png)

輸入1： `ssh zhuang@172.16.48.129`

輸入密碼： `123`

* 當第一次遠端連線時會詢問是否要連接 `yes` 後即成功連線， `No` 即取消遠端。

---
<h3 id="nano">安裝 nano</h3>

![5. nano](https://github.com/xuan103/alpine_install/blob/master/2-5-nano.png)
![](https://i.imgur.com/cv6z0hl.png)

輸入1： `apk update` <br />

輸入2： `apk add nano` <br />

* 安裝其他套件： 

$ `apk add sudo`

$ `apk add bash` 

$ `apk add procps` 

$ `apk add openjdk8`

* `apk update` 用來取得遠端更新伺服器的套件檔案清單。

* `apk add ` 為 alpine 安裝指令。

---
<h3 id="dhcp">設定固定 ip</h3>

![6. dhcp](https://i.imgur.com/cqp5NlQ.png)

方法1： `nano /etc/network/interfaces`

    auto lo
    iface lo inet loopback

    auto eth0
    iface eth0 inet static
        address 192.168.1.100
        netmask 255.255.255.0
        gateway 192.168.1.254
    
方法2： `ifconfig eth0 192.168.1.100`
    
    1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
    inet6 ::1/128 scope host 
       valid_lft forever preferred_lft forever
    2: enp1s0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP group default qlen 1000
    link/ether 00:07:32:4d:1e:a5 brd ff:ff:ff:ff:ff:ff
    inet 192.168.1.100/24 brd 192.168.40.255 scope global enp1s0
       valid_lft forever preferred_lft forever
    inet6 fe80::207:32ff:fe4d:1ea5/64 scope link 
       valid_lft forever preferred_lft forever

---
<h3 id="dns">設定 dns server</h3>

![7. dns](https://i.imgur.com/VHIWWRN.png)

輸入： `nano /etc/resolv.conf`

    nameserver 168.95.1.1
    nameserver 8.8.8.8

*  `168.95.1.1` 為中華電信 ip 位址。
*  `8.8.8.8` 為 google ip 位址。

---
<h2 id="references">參考文件</h2>

- https://wiki.alpinelinux.org/wiki/Alpine_setup_scripts

---
[返回至 - 目錄](https://github.com/xuan103/Alpine_2021)

[上一頁 - Chapter 2-1. Install Alpine](https://github.com/xuan103/Alpine_2021/blob/main/Documents/Chapter%202-1.%20Install%20Alpine.md)
