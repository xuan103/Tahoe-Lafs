Tahoe-Lafs
===

###### tags: `Tahoe-Lafs` `tahoe-lafs-1.17.1` `Ubuntu` `ubuntu-20.04-server`

```
Finily Updata Date: MAY 25, 2022 11:30 AM
```

Contents
===

- [System requirements](#requirements)
- [Hosts setup](#Hosts)
- [Install Tahoe-LAFS (On each node)](#Tahoe-LAFS)
- How To Run Tahoe-LAFS
    - [Running a Client](#Client)
    - [Instantiate the nodes](#nodes)

---

<h2 id="requirements">System requirements</h2> 
---

- Ubuntu 20.04 Server

<h2 id="Hosts">Hosts setup</h2> 

- IPs and hosts are:
  - 192.168.105.91 (`host`) [**client**]
  - 192.168.105.92 (`vm1`) [**introducer**]
  - 192.168.105.93 (`vm2`) [**client**]

---
<h2 id="Tahoe-LAFS">Install Tahoe-LAFS (On each node) </h2> 
---

-  **(Optional)** 建立使用者
- [ ] sudo adduser tahoe_user

```
  Adding user 'tahoe_user' ...
  Adding new group 'tahoe_user' (1001) ...
  Adding new user 'tahoe_user' (1001) with group 'tahoe_user' ...
  Creating home directory '/home/tahoe_user' ...
  Copying files from '/etc/skel' ...
  New password: **`password`**
  Retype new password: **`password`**
  passwd: password updated successfully
  Changing the user information for tahoe_user
  Enter the new value, or press ENTER for the default
  
          Full Name []:
          Room Number []: 
          Work Phone []:
          Home Phone []:
          Other []:
  Is the information correct? [Y/n] **`Y`**
```

- [ ] sudo adduser tahoe_user sudo
- [ ] sudo su - tahoe_user

---

1. 下載套件包, 更新套件
- [ ] sudo apt-get update
- [ ] sudo apt-get upgrade -y
- [ ] sudo apt autoremove -y

- 如果無法更新: (參考)
  - sudo nano /etc/resolv.conf
    - ADD **`nameserver 8.8.8.8`**


2. 安裝工具 
- [ ] sudo apt-get install tree curl ssh openssl make gcc telnetd nmap -y

   2.1 查看 openssl 版本
     - [ ] openssl version
       > OpenSSL 1.1.1f  31 Mar 2020
     
3. 安裝 python3 套件
- [ ] sudo apt-get update
- [ ] sudo apt install python3-pip -y
- [ ] sudo apt-get install python-dev libffi-dev libssl-dev -y


   3.1 查看 python3 版本
     - [ ] python3 -V
       > Python 3.8.10
       
     - [ ] sudo nano .bashrc
    ```bash=
    alias python='python3'
    alias pip='pip3'
    ```
    
    - 將上面兩行加在 `.bashrc` 最後面即可

4. 安裝 tahoe-lafs 工具
- [ ] pip install tahoe-lafs
- [ ] pip install --user virtualenv
   
   4.1 查看 tahoe 版本
     - [ ] tahoe --version
       > tahoe-lafs/1.17.1
    
- 如果查看 tahoe 版本輸出顯示 `tahoe: command not found`, 登出再登入即可。


---

:books: How To Run Tahoe-LAFS
---

<h3 id="Client">Running a Client</h3>

```
使用者: bigred@90-tahoe-lafs: "host"
```

- [ ] tahoe create-client

- [ ] ls -al

```
   total 44
   drwxr-xr-x  7 bigred bigred 4096 May 14 18:43 .
   drwxr-xr-x  4 root   root   4096 May 14 18:02 ..
   -rw-------  1 bigred bigred  990 May 14 17:27 .bash_history
   -rw-r--r--  1 bigred bigred  220 Feb 25  2020 .bash_logout
   -rw-r--r--  1 bigred bigred 3771 Feb 25  2020 .bashrc
   drwx------  3 bigred bigred 4096 May 14 17:18 .cache
   drwx------  6 bigred bigred 4096 May 14 17:18 .local
   -rw-r--r--  1 bigred bigred  807 Feb 25  2020 .profile
   drwx------  2 bigred bigred 4096 May 14 15:48 .ssh
   -rw-r--r--  1 bigred bigred    0 May 14 15:51 .sudo_as_admin_successful
   drwxrwxr-x  3 bigred bigred 4096 May 14 18:43 `.tahoe`
   drwxrwxr-x 14 bigred bigred 4096 May 14 18:29 tahoe-lafs
```

- [ ] cp .tahoe/tahoe.cfg .tahoe/tahoe.cfg.bk
- [ ] ls -al .tahoe

```
   total 24
   drwxrwxr-x 3 bigred bigred 4096 May 16 09:00 .      
   drwxr-xr-x 7 bigred bigred 4096 May 16 08:58 ..     
   drwx------ 2 bigred bigred 4096 May 16 08:58 private
   -rw-rw-r-- 1 bigred bigred 1107 May 16 08:58 tahoe.cfg
   -rw-rw-r-- 1 bigred bigred 1107 May 16 09:00 tahoe.cfg.bk
   -rw-rw-r-- 1 bigred bigred  143 May 16 08:58 tahoe-client.tac
```

- [ ] nano ~/.tahoe/private/introducers.yaml

```bash=
introducers:
  petname2:
    furl: "pb://fodk4doc64febdoxke3a4ddfyanz7ajd@tcp:testgrid.tahoe-lafs.org:5000/el4fo3rm2h22cnilukmjqzyopdgqxrd2"
```

>---
<h3 id="nodes">Instantiate the nodes</h3>

```
使用者: (venv) bigred@92-tahoe-lafs: "vm2"
```

- [ ] tahoe create-node --hostname=**`192.168.105.92`** .tahoe
- [ ] ls -al

```
   total 44
   drwxr-xr-x  7 bigred bigred 4096 May 16 09:03 .      
   drwxr-xr-x  4 root   root   4096 May 16 08:40 ..     
   -rw-------  1 bigred bigred  616 May 16 08:48 .bash_history
   -rw-r--r--  1 bigred bigred  220 Feb 25  2020 .bash_logout
   -rw-r--r--  1 bigred bigred 3771 Feb 25  2020 .bashrc
   drwx------  3 bigred bigred 4096 May 14 17:33 .cache 
   drwx------  6 bigred bigred 4096 May 14 17:34 .local 
   -rw-r--r--  1 bigred bigred  807 Feb 25  2020 .profile
   drwx------  2 bigred bigred 4096 May 14 17:10 .ssh   
   -rw-r--r--  1 bigred bigred    0 May 14 17:12 .sudo_as_admin_successful
   drwxrwxr-x  3 bigred bigred 4096 May 16 09:03 .tahoe 
   drwxrwxr-x 14 bigred bigred 4096 May 16 08:59 tahoe-lafs
```

- [ ] cp .tahoe/tahoe.cfg .tahoe/tahoe.cfg.bk
- [ ] ls -al .tahoe

```
   total 24
   drwxrwxr-x 3 bigred bigred 4096 May 16 09:04 .       
   drwxr-xr-x 7 bigred bigred 4096 May 16 09:03 ..      
   drwx------ 2 bigred bigred 4096 May 16 09:03 private 
   -rw-rw-r-- 1 bigred bigred 1123 May 16 09:03 tahoe.cfg
   -rw-rw-r-- 1 bigred bigred 1123 May 16 09:04 tahoe.cfg.bk
   > -rw-rw-r-- 1 bigred bigred  143 May 16 09:03 tahoe-client.tac
```

- [ ] nano ~/.tahoe/private/introducers.yaml

```bash=
introducers:
  petname2:
    furl: "pb://fodk4doc64febdoxke3a4ddfyanz7ajd@tcp:testgrid.tahoe-lafs.org:5000/el4fo3rm2h22cnilukmjqzyopdgqxrd2"
```

---

```
使用者: bigred@91-tahoe-lafs: "vm1%"
```

- ==**需要兩個 Terminal**==

### Terminal(1):
- [ ] tahoe create-node --hostname=**`192.168.105.91`** ~/introducer
 > Node created in '/home/bigred/tahoe-introducer'
 >  Please add introducers to '/home/bigred/introducer/ 
> private/introducers.yaml'!
 >  The node cannot connect to a grid without it.
 >  Please set [node]nickname= in '/home/bigred/
introducer/tahoe.cfg'

- [ ] cp ~/introducer/tahoe.cfg ~/introducer/tahoe.cfg.bk
- [ ] nano introducer/tahoe.cfg

```
 >  [node]
 >  nickname = `introducer-vm1`
 >  reveal-IP-address = true
 >  web.port = tcp:3456:interface=`0.0.0.0`
 >  web.static = public_html
 >  tub.port = tcp:38263
 >  tub.location = tcp:192.168.105.91:38263
```

- [ ] tahoe run introducer


### Terminal(2):
- [ ] cat tahoe-introducer/private/logport.furl
> pb://fac5kb7rvl6mxghtliblvrixqzdejpdw@localhost:55561/5uqy7s3a4eizuk657rrn3ue2lpf4d2ds

**複製從 'cat' 得到的字串，並將其貼到 ==vm2%== 及 ==host== 的 tahoe.cfg 中.**


---

```
使用者: bigred@90-tahoe-lafs`: "host"
```

- ==**需要兩個 Terminal**==

### Terminal(1):
- [ ] nano -c .tahoe/tahoe.cfg

```
 >  [node]
 >  nickname = `client-host`
 >  reveal-IP-address = true
 >  web.port = tcp:3456:interface=`0.0.0.0`
 >  web.static = public_html
 >  tub.port = disabled
 >  tub.location = disabled
 >  ...
 >  [client]
 >  helper.furl = `pb://fac5kb7rvl6mxghtliblvrixqzdejpdw@localhost:55561/5uqy7s3a4eizuk657rrn3ue2lpf4d2ds`
 >  ...
 >  [storage]
 >  #Shall this node provide storage service?
 >  enabled = `true`
 >  readonly = `false`
 >  reserved_space = 1G
 >  `#here you tell the storage server how much disk space it cannot use`
```

- [ ] tahoe run

### Terminal(2):
- [ ] ps aux | grep tahoe
> bigred     54178  0.6  0.7  75504 64184 pts/0    S+   02:23   0:01 /usr/bin/python3 /home/bigred/.local/bin/tahoe run tahoe-introducer

> bigred     54252  0.0  0.0   8160   656 pts/1    S+   02:26   0:00 grep --color=auto tahoe


```
使用者: bigred@92-tahoe-lafs`: "vm2%"
```

- ==**需要兩個 Terminal**==

### Terminal(1):
- [ ] nano -c .tahoe/tahoe.cfg

```
 >  [node]
 >  nickname = `client-vm2`
 >  reveal-IP-address = true
 >  web.port = tcp:3456:interface=`0.0.0.0`
 >  web.static = public_html
 >  tub.port = disabled
 >  tub.location = disabled
 >  …
 >  [client]
 >  introducer.furl =  `pb://fac5kb7rvl6mxghtliblvrixqzdejpdw@localhost:55561/5uqy7s3a4eizuk657rrn3ue2lpf4d2ds`
 >  helper.furl =
 >  …
 >  [storage]
 >  #Shall this node provide storage service?
 >  enabled = `true`
 >  readonly = `false`
 >  reserved_space = 1G
 >  #here you tell the storage server how much disk space it cannot use
 >  ...
 > [helper]
 > # Shall this node run a helper service that clients can use?
 > enabled = `true`
```

- [ ] tahoe run

### Terminal(2):
- [ ] cat .tahoe/private/helper.furl
> pb://ibgrnnxmykokjdtbcccfprs7xpvwgtmr@tcp:192.168.105.92:42827/fpduokbpmozhrksx74mt7sd3jpgz4uc6

**複製從 'cat' 得到的字串，並將其貼到 `.tahoe/tahoe.cfg`**

```bash=
helper.furl = pb://ibgrnnxmykokjdtbcccfprs7xpvwgtmr@tcp:192.168.105.92:42827/fpduokbpmozhrksx74mt7sd3jpgz4uc6
```

-  在`Terminal(1)` 重啟 `tahoe run`

---

## 在瀏覽器打開 Tahoe

- http://192.168.105.90:3456/

- http://192.168.105.91:3456/

- http://192.168.105.92:3456/

---

:mag: Reference
---

https://tahoe-lafs.readthedocs.io/

https://tahoe-lafs.org/trac/tahoe-lafs/wiki/Tutorial

https://www.digitalocean.com/community/tutorials/tahoe-lafs

https://www.howtoing.com/tahoe-lafs

https://docmiao.com/community/tutorials/tahoe-lafs


https://github.com/tahoe-lafs/tahoe-lafs

https://www.w3study.wiki/a/202109/1030802.html

https://tahoe-lafs.org/trac/tahoe-lafs/ticket/1478

https://www.skynats.com/blog/telnet-connection-refused-by-remote-host/

https://www.ltsplus.com/linux/3-ways-check-remote-server-open-port

http://www.idcat.cn/tahoe%E7%AE%80%E5%8D%95%E9%83%A8%E7%BD%B2.html




---

## Notes 
<!-- Other important details discussed during the meeting can be entered here. -->
