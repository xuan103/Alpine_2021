# For Ubuntu

* * *
## 目錄

-    [準備 Alpine](#ready)
     -    [登入帳號](#root)
     -    [設定網路卡](#eth0)
     -    [設置安裝套件](#setup)
     -    [安裝必要條件](#add)
     -    [檢查 USB 磁碟區](#lsblk)
     -    [燒錄至 USB](#burn)


-   [參考文件](#references)

* * *

<h1 id="ready">準備 Alpine</h2> 

下載 images 請參考 [Chapter 1. How To Download](https://github.com/xuan103/Alpine_2021/blob/main/Documents/Chapter%201.%20How%20To%20Download.md)

---

<h3 id="root">登入帳號</h3>

* Alpine 開機後，預設的登入帳號為 `root` ，而且沒有密碼。

![1. login](https://i.imgur.com/UMO05Mq.png)

輸入： `root` <br />

---
<h3 id="eth0">設定網路卡</h3>

* 使用 Alpine 預設命令來安裝系統。

![2. eth0](https://i.imgur.com/MlLieZP.png)


輸入： `setup-interfaces` <br />
輸入： `ifup eth0` <br />

* `ifup` 指開啟網路卡。<br />
* 安裝過程使用問答方式，若回答想要反悔，只能透過 `Ctrl + C` 重來。
* 若問答看到 `[abc]` ，`abc` 則代表預設的答案。<br />

---
<h3 id="setup">設置安裝套件</h3>

![3. setup-1](https://i.imgur.com/q5xCcri.png)


![3-1. setup-2](https://i.imgur.com/61LjcOS.png)


輸入1： `setup-apkrepos` 

輸入2： `1` 

### (此圖未更新以上方文字為主！)

---
<h3 id="add">安裝必要條件</h3>

![4. add](https://i.imgur.com/kjm0QkX.png)


輸入： `apk add util-linux parted syslinux` <br />

---
<h3 id="lsblk">檢查 USB 磁碟區</h3>

![5. lsblk](https://i.imgur.com/sLGVsWb.png)

輸入： `lsblk -l` <br />

* 虛擬機隨身碟讀取方式：<br />
  `virtual machine -> removable devices -> usb name -> connect`

---
<h3 id="burn">燒錄至 USB</h3>

![6. burn](https://i.imgur.com/9xkuwB5.png)


輸入1： `parted /dev/sdx mklabel msdos` <br />

輸入2： `yes` <br />

輸入3： `parted /dev/sdb mkpart primary fat32 0% 100%` <br />

輸入4： `mkfs -t vfat /dev/sdb1` <br />

輸入5： `setup-bootable /media/cdrom/ /dev/sdb1` <br />

* `/dev/sdx` 依據 `lsblk -l` 查看隨身碟為哪個

* 虛擬機隨身碟退出方式：<br />
  `virtual machine -> removable devices -> usb name -> disconnect`

---
[返回至 - 目錄](https://github.com/xuan103/Alpine_2021)