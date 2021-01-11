# Chapter 4. Linux user

* * *
## 目錄

- [建立 Linux 使用著](#login)

* * *

<h3 id="login">建立 Linux 使用著</h3>

帳號： `sudo useradd -m -s /bin/sh rbean`

密碼： `sudo passwd rbean`

修改帳號： `sudo nano /etc/passwd`

更改使用著集群組： `sudo chown zbean:zbean /home/zbean`

刪除帳號： `sudo userdel -r zbean`

---

[返回至 - 目錄](https://github.com/xuan103/Alpine_2021)
