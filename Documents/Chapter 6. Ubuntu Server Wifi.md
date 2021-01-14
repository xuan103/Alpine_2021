# Chapter 6. Ubuntu Server Wifi

* * *
## 目錄

-   [安裝必要套件](#install)

-   [使用 WPA_Supplicant 設定 Wifi](#set)

-   [測試連線 Wifi](#test)

-   [使用 Systemd 設定開機自動連線 Wifi](#use)
       
-   [參考文件](#references)

* * *

<h2 id="install">安裝必要套件</h2>

* 更新套件

`sudo apt update -y`

* 下載 wpasupplicant 套件

`sudo apt install wpasupplicant`


<h2 id="set">使用 WPA_Supplicant 設定 Wifi</h3>

使用 wpa_supplicant 設定 Wifi 連線

 `sudo nano /etc/wpa_supplicant/wpa_supplicant.conf`

    ctrl_interface=DIR=/var/run/wpa_supplicant GROUP=netdev

    update_config=1

    ap_scan=1

    fast_reauth=1

    country=TW


    network={
        ssid="輸入 Wifi 名稱" 
        psk="輸入 Wifi 密碼"
    }


<h3 id="test">測試連線 Wifi</h3>

檢查無線網路卡名稱

`ifconfig -a` <br />

`wlx7cdd90ea4126`: flags=4099<UP,BROADCAST,MULTICAST>  mtu 1500

    ether 74:da:38:3b:ec:56  txqueuelen 1000  (Ethernet)
    RX packets 0  bytes 0 (0.0 B)
    RX errors 0  dropped 0  overruns 0  frame 0
    TX packets 0  bytes 0 (0.0 B)
    TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0

`wlx7cdd90ea4126`為無線網卡名稱

將原來讀取 wpa_supplicant.conf 的程序 wpa_supplicant 殺掉

`sudo kill -9 $(ps -ef | grep wpa | awk '{print $2}')`

重新執行 wpa_supplicant，並讀取 wpa_supplicant.conf 設定

`sudo wpa_supplicant -B -i wlx7cdd90ea4126 -D wext -c /etc/wpa_supplicant/wpa_supplicant.conf`

若回傳以下訊息，請忽略掉
    
    rfkill: Cannot open RFKILL control device
    ioctl[SIOCSIWAP]: Operation not permitted
    ioctl[SIOCSIWENCODEEXT]: Invalid argument
    ioctl[SIOCSIWENCODEEXT]: Invalid argument
    
使用設定檔測試無線網卡連線 Wifi (亂碼部分請改為無線網卡名稱)

`sudo wpa_supplicant -u -s -c /etc/wpa_supplicant/wpa_supplicant.conf -i wlx7cdd90ea4126`

執行 DHCP 用戶端，取得 IP

`sudo dhclient `

若回傳以下訊息，請忽略掉
    
    RTNETLINK answers: File exists
    
查尋 IP 位址

`ifconfig –a`


<h3 id="use">使用 Systemd 設定開機自動連線 Wifi</h3>

* `sudo nano /etc/systemd/system/wpa_supplicant.service`

```
[Unit]
Description=WPA supplicant
Before=network.target
After=dbus.service
Wants=network.target
IgnoreOnIsolate=true

[Service]
Type=dbus
BusName=fi.w1.wpa_supplicant1
ExecStart=/sbin/wpa_supplicant -u -s -c /etc/wpa_supplicant/wpa_supplicant.conf -i  wlx7cdd90ea4126

[Install]
WantedBy=multi-user.target
Alias=dbus-fi.w1.wpa_supplicant1.service
```

* `sudo nano /etc/systemd/system/dhclient.service`

```
[Unit]
Description= DHCP Client
Before=network.target
After=wpa_supplicant.service

[Service]
Type=simple
ExecStart=/sbin/dhclient wlx7cdd90ea4126

[Install]
WantedBy=multi-user.target
```

* `sudo systemctl enable dhclient.service`

* `sudo reboot`


<h2 id="references">參考文件</h2>

- https://www.linuxbabe.com/ubuntu/connect-to-wi-fi-from-terminal-on-ubuntu-18-04-19-04-with-wpa-supplicant

- https://www.raspberrypi.com.tw/2152/setting-up-wifi-with-the-command-line/?fbclid=IwAR2LIhAu3u4F9Ypud0iD9nUhWU2_UL9ng9-6lTSpL8Bct2XuJe3m9MaMN3U

---

[返回至 - 目錄](https://github.com/xuan103/Alpine_2021)
