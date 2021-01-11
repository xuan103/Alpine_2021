# Chapter 5. Upboard UART

* * *
## 目錄

-    [Ubuntu Server Kernel 改裝](#kernel)
-    [安裝 UpBoard Kernel](#install)
-    [設定 Serial Console](#Serial_Console)
-    [GPIO 線路接法](#GPIO)
-    [putty 連接](#putty)

* * *

-  網路設置

$ `sudo nano /etc/netplan/00-installer-config.yaml`

```
# This is the network config written by 'subiquity'
network:
  ethernets:
    enp1s0:
      addresses:
      - 192.168.0.40/24
      gateway4: 192.168.0.1                                               
      nameservers:
        addresses:
        - 8.8.8.8
    wlx7cdd90ea4126:
      dhcp4: yes
  version: 2
```

---
<h3 id="kernel">Ubuntu Server Kernel 改裝</h3>

- 檢查目前 Ubuntu Server Kernel

$ `uname -a`
    
    Linux UB1804S 4.15.0-89-generic #89-Ubuntu SMP Fri Feb 14 21:47:50 UTC 2020 x86_64 x86_64 x86_64 GNU/Linux
    
 * 增加 UpBoard source list，記得按下 Enter

$ `sudo add-apt-repository ppa:ubilinux/up`
    
    Kernel packages with support for the special features on the UP board (http://up-board.org), such as 40-pin header I/O.
    More info: https://launchpad.net/~ubilinux/+archive/ubuntu/up
    Press [ENTER] to continue or Ctrl-c to cancel adding it.
    

 * 更新 repository list

$ `sudo apt update`

 * 刪除目前系統的 Kernel

$ `sudo apt autoremove --purge 'linux-.*generic'`

![autoremove](https://i.imgur.com/qZ4NmkO.png)

跳出警告，這邊選擇 No

---
<h3 id="install">安裝 UpBoard Kernel</h3>

$ `sudo apt install linux-image-generic-hwe-18.04-UpBoard -y`

* 完整更新系統

$ `sudo apt dist-upgrade -y`

* 重新開機

$ `sudo reboot`

* 檢查 Kernel

$ `uname -a`
    
    Linux UB1804S 4.15.0-37-generic #40~UpBoard06-Ubuntu SMP Wed Nov 27 11:29:44 UTC 2019 x86_64 x86_64 x86_64 GNU/Linux
   
---
<h3 id="Serial_Console">設定 Serial Console</h3>

* 啟動 Serial Console

$ `sudo mkdir /etc/default/grub.d`
$ `sudo nano /etc/default/grub.d/serial.cfg`
    
    # ubuntu 18.04
    GRUB_CMDLINE_LINUX="console=tty1 console=ttyS4,115200n8"
    GRUB_TERMINAL_INPUT="console serial"
    GRUB_TERMINAL_OUTPUT="gfxterm serial"
    GRUB_SERIAL_COMMAND="serial --unit=0 --speed=115200 --stop=1"
    
$ `sudo update-grub`

* 設定 Serial Console login

$ `sudo systemctl enable getty@ttyS4`
$ `sudo reboot`

---
<h3 id="GPIO">GPIO 線路接法</h3>

![gpio](https://github.com/xuan103/class-2020-07/blob/master/gpio.png)

---
<h3 id="putty">putty 連接</h3>

* 檢查 Serial Console

￼![console](https://github.com/xuan103/class-2020-07/blob/master/console.png)


* 使用 putty 連接 UpBoard

![serial](https://github.com/xuan103/class-2020-07/blob/master/serial.png)
---

[返回至 - 目錄](https://github.com/xuan103/Alpine_2021)