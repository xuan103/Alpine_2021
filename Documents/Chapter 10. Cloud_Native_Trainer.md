# Chapter 10. Cloud_Native_Trainer

* * *

-   [Cloud_Native_Trainer](cloud)

* * *


<h3 id="cloud">Cloud_Native_Trainer</h3>

![Cloud_Native_Trainer](https://i.imgur.com/cHzNjLN.png)

nano autoadd.sh

```


```


以下為步驟：

1. $ `mkdir -p ~/cnt/bin`

   * /bin 里放要執行的程式

 <br />

2. $ `nano .bashrc`

在最後方加上 `export PATH=/home/bigred/cnt/bin:$PATH`

   * PATH 為環境變數

 <br />

3. $ `exit`
 
須離開重新登入才會生效

 <br />

4. $ `env`

    
    _=/usr/bin/env
    

 * 看到此行表示成功

 <br />

5. $ `sudo nano /etc/hosts`

(增加前原檔案里的資料須全部清除)
    
    127.0.0.1 localhost
    192.168.40.254 gw 
    192.168.40.10 mas01
    192.168.40.20 wka01 
    192.168.40.21 wka02 
    192.168.40.22 wka03 
    192.168.40.23 wka04 
    192.168.40.30 ds01
    
<br />

6. 剛剛建立的檔案透過 ssh 複製到其他使用著 

$ `scp /etc/hosts mas01:~`

$ `scp /etc/hosts wka01:~` 

$ `scp /etc/hosts wka02:~`

$ `scp /etc/hosts wka03:~`

$ `scp /etc/hosts wka04:~`

$ `scp /etc/hosts ds01:~`

將複製過去的檔案丟到 /etc/hosts

$ `ssh mas01 sudo cp ~/hosts /etc/hosts`

$ `ssh ds01 sudo cp ~/hosts /etc/hosts`

$ `ssh wka01 sudo cp ~/hosts /etc/hosts`

$ `ssh wka02 sudo cp ~/hosts /etc/hosts`

$ `ssh wka03 sudo cp ~/hosts /etc/hosts`

$ `ssh wka04 sudo cp ~/hosts /etc/hosts`

<br />

7. 使用 ssh 遠端時可直接用帳號登入

$ `ssh blue@mas03`

$ `sudo nano /etc/group`
    
    sudo:x:27:blue,bigred
    
$ `sudo nano /etc/sudoers`
    
    %sudo   ALL=(ALL:ALL) NOPASSWD:ALL
    
* 驗證：

bigred@gw:~$ `ssh mas01 sudo cat /etc/shadow`

blue@WKA03:~$ `ping wka01`
    
    PING wka01 (192.168.51.20) 56(84) bytes of data.
    64 bytes from wka01 (192.168.51.20): icmp_seq=1 ttl=64 time=0.458 ms
    64 bytes from wka01 (192.168.51.20): icmp_seq=2 ttl=64 time=0.234 ms
    
8. 寫程式使其可看到其他電腦硬體規格

$ `nano cnt/bin/sysinfo`

    #!/bin/bash
    
    for x in mas01 wka01 wka02 wka03 wka04 ds01
    
    do
     echo $x
     m=$(ssh $x free -mh | grep Mem:)
     echo -n "Memory :"
     echo $m | cut -d' ' -f2
     cn=$(ssh $x cat /proc/cpuinfo | grep 'model name' | head -n 1 | cut -d ':' -f2 | tr -s ' ')
     echo -n "CPU : $cn (core: "
     cn=$(ssh $x cat /proc/cpuinfo | grep 'model name' | wc -l)
     echo "$cn)"
     d=$(ssh $x sudo fdisk -l | grep GiB | cut -d ':' -f2 | cut -d ',' -f1)
     echo Disk: $d
     u=$(ssh $x ls /home)
     echo User: $u
     echo "---------------------------------------------------------"
    done
    
* 給予權限 `chmod +x cnt/bin/sysinfo`

 <br />

9. 寫程式利用遠端控制達到所要需求 (建置 hadoop 環境及建置紅豆與綠豆帳號) 

$ `nano cnt/bin/sysprep`
    
    #!/bin/bash
    
    for x in mas01 wka01 wka02 wka03 wka04 ds01
    
    do
     ssh $x sudo apt-get update
     ssh $x sudo apt-get upgrade -y
     ssh $x sudo apt-get install openjdk-8-jdk
     ssh $x sudo useradd -m -s /bin/bash rbean
     ssh $x "echo 'rbean:rbean' | sudo chpasswd"
     ssh $x sudo useradd -m -s /bin/bash gbean
     ssh $x "echo 'gbean:gbean' | sudo chpasswd"
    done

* 需求：

1. 取得最新套件清單

2. 系統更新至最新

3. 安裝 openjdk8

4. 產生帳號 rbean gbean
    
* 給予權限 `chmod +x cnt/bin/sysprep`



<h2 id="references">參考文件</h2>

- 圖檔：https://docs.google.com/presentation/d/1O0aUcHZ_XMVvrpjSu_J9LpLGcao816rHV6cp1f6QxdQ/edit?usp=sharing






---

[返回至 - 目錄](https://github.com/xuan103/Alpine_2021)