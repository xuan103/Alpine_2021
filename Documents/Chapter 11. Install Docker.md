# Chapter 11. Install Docker

* * *
## 目錄

*   [介紹](#docker)
    *   [概述](#overview)

*   [方法](#method)
    *   [安裝 docker](#install)
        *   [1. 編輯文件](#Community)
        *   [2. 更新套件並安裝](#update)
        *   [3. 啟動守護程序](#start)
        *   [4. 查看版本](#check)

    *   [運行 docker](#run)
       
*   [參考文件](#references)

* * *

<h1 id="docker">介紹</h1> 

<h2 id="overview">概述</h2> 

Docker 是開放原始碼專案，將應用程式自動化部署為可攜式且可自足的容器，在雲端或內部部署上執行。 Docker 也是一家升級及發展這項技術的公司，並且與雲端、Linux 和 Windows 廠商 (包括 Microsoft) 合作。<br /> <br />

<h1 id="method">方法</h1> 

<h2 id="install">安裝 docker</h2>

* 以下將使用虛擬機安裝 docker。

* 參考 : https://wiki.alpinelinux.org/wiki/Docker

<h3 id="Community">1. 編輯文件</h3>

Docker 軟件包位於 'Community' 存儲庫中，如果 apk 添加因約束無法滿足而失敗，則需要編輯 `/etc/apk/repositories` 文件以添加（或取消註釋）如下行：

輸入： `nano /etc/apk/repositories` <br />

並添加：

    http://dl-cdn.alpinelinux.org/alpine/edge/main
    http://dl-cdn.alpinelinux .org/alpine/edge/community
 
![nanoadd](https://i.imgur.com/VmOf8kW.png)


* 添加完畢後 `ctrl + o` 儲存並 `ctrl + x` 離開。

<h3 id="update">2. 更新套件並安裝</h3>

![update](https://i.imgur.com/f0DSbYz.png)


![adddocker](https://i.imgur.com/KEUS4dj.png)


輸入1： `apk update` <br />

輸入2： `apk add docker` <br />

* `apk update` 用來取得遠端更新伺服器的套件檔案清單。

* `apk add ` 為 alpine 安裝指令。

<h3 id="start">3. 啟動守護程序</h3>

* 要在啟動時啟動 Docker 守護程序

![start1](https://i.imgur.com/aIaeHdr.png)


輸入： `rc-update add docker boot` <br />

* 手動啟動 Docker 守護程序

![start2](https://i.imgur.com/nSmC0UL.png)


輸入： `service docker start` <br />

<h3 id="check">4. 查看版本</h3>

輸入： `docker -v` <br />

![version](https://i.imgur.com/AZIiuyC.png)

* `-v` 表示 version 版本。

<h2 id="run">運行 docker</h2>

* 簡單的範例：

輸入： `docker run busybox echo "hello world"` <br />

![hello](https://i.imgur.com/uypxRAq.png)


* 詳細執行 docker 請參考：https://joshhu.gitbooks.io/dockercommands/content/Containers/DockerRun.html

<h2 id="references">參考文件</h2>

- https://docs.microsoft.com/zh-tw/dotnet/architecture/containerized-lifecycle/what-is-docker

- https://wiki.alpinelinux.org/wiki/Docker

- https://joshhu.gitbooks.io/dockercommands/content/Containers/DockerRun.html

---

[返回至 - 目錄](https://github.com/xuan103/Alpine_2021)