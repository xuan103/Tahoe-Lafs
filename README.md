Tahoe-Lafs
===

###### tags: `Tahoe-Lafs` `tahoe-lafs-1.17.1` `Ubuntu` `ubuntu-20.04-server`

```
Finily Updata Date: 2022/05/25 15:30 PM
```

Contents
===

- [System requirements](#requirements)
- [Hosts setup](#Hosts)
- [Install Tahoe-LAFS (On each node)](#Tahoe-LAFS)
- How To Run Tahoe-LAFS
    - [Instantiate the nodes](#nodes)
    - [Instantiate the introducer](#introducer)
- FTP
    - [Instantiate the nodes](#ftp-nodes)
    - [Instantiate the introducer](#ftp-introducer)

---

<h2 id="requirements">System requirements</h2> 
---

- Ubuntu 20.04 Server

<h2 id="Hosts">Hosts setup</h2> 

- IPs and hosts are:
  - 192.168.105.91 (`host`) [**Client**]
  - 192.168.105.92 (`vm1`) [**Introducer**]
  - 192.168.105.93 (`vm2`) [**Client**]

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
  Is the information correct? [Y/n] `Y`
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
- [ ] sudo apt-get install tree curl ssh openssl make gcc telnetd nmap -y
- [ ] sudo apt install python3-pip -y
- [ ] sudo apt-get install python-dev libffi-dev libssl-dev -y
   > **有紅字就多執行幾次**


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
- [ ] **(Optional)** pip install --user virtualenv
   
   4.1 查看 tahoe 版本
     - [ ] tahoe --version
       > tahoe-lafs/1.17.1
    
- 如果查看 tahoe 版本輸出顯示 `tahoe: command not found`, 登出再登入即可。


---

:books: How To Run Tahoe-LAFS
---

<h3 id="nodes">Instantiate the nodes</h3>


- ==**需要兩個 Terminal**==

### Terminal(1):
- [ ] tahoe create-node --hostname=15X-tahoe-node-X ~/.tahoe
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

- [ ] sudo nano /etc/hosts

```bash=
192.168.105.150 150-Tahoe-Introducer
192.168.105.152 152-Tahoe-node-2
192.168.105.153 153-Tahoe-node-3
192.168.105.154 154-Tahoe-node-4
192.168.105.155 155-Tahoe-node-5
192.168.105.156 156-Tahoe-node-6
```

- [ ] nano .tahoe/tahoe.cfg

```bash=
[node]
nickname = 15X-tahoe-node-X
reveal-IP-address = true
web.port = tcp:3456:interface=192.168.105.15X
web.static = public_html
tub.port = tcp:37051
tub.location = tcp:192.168.105.15X:37051
...
shares.needed = 2         ### 檔案分持數量
shares.happy = 3          ### sotrage node 數量
shares.total = 5

[storage]
# Shall this node provide storage service?
enabled = true
readonly = false
reserved_space = 1G

[helper]
# Shall this node run a helper service that clients can use?
enabled = true
```

### Terminal(2):
- [ ] tahoe run .tahoe
- [ ] ^C


### Terminal(1):
- [ ] cat .tahoe/private/helper.furl 
> pb://pjusmphla2qrczzrax6kztntyyckyoai@tcp:192.168.105.15X:37051/tdmu7ix4iw6bfr6g3ezs7h3kjocnef62

-  複製從 ‘cat’ 得到的字串，並將其貼到 .tahoe/tahoe.cfg

- [ ] nano .tahoe/tahoe.cfg

```bash=
[client]
helper.furl = pb://pjusmphla2qrczzrax6kztntyyckyoai@tcp:192.168.105.15X:37051/tdmu7ix4iw6bfr6g3ezs7h3kjocnef62
```

### Terminal(2):
- [ ] tahoe run .tahoe


---

<h3 id="introducer">Instantiate the introducer</h3>

- ==**需要兩個 Terminal**==

### Terminal(1):
- [ ] sudo nano /etc/hosts

```bash=
192.168.105.150 150-Tahoe-Introducer
192.168.105.152 152-Tahoe-node-2
192.168.105.153 153-Tahoe-node-3
192.168.105.154 154-Tahoe-node-4
192.168.105.155 155-Tahoe-node-5
192.168.105.156 156-Tahoe-node-6
```

- [ ] tahoe create-introducer --hostname=150-tahoe-introducer ~/introducer

```
'tahoe run' in '/home/bigred/introducer'
running node in '/home/bigred/introducer'
2022-05-25T07:15:37+0000 [twisted.scripts._twistd_unix.UnixAppLogger#info] twistd 22.4.0 (/usr/bin/python3 3.8.10) starting up.
```

- [ ] cp ~/introducer/tahoe.cfg ~/introducer/tahoe.cfg.bk
- [ ] nano introducer/tahoe.cfg

```bash=
[node]
nickname = 150-tahoe-introducer
reveal-IP-address = true
web.port =
web.static = public_html
tub.port = tcp:45825
tub.location = tcp:150-Tahoe-Introducer:45825
```

- [ ] nohup tahoe run introducer


### Terminal(2):
- [ ] cat introducer/private/storage.furl 
> pb://iy4kkoh2jzrisld5lohjskz45ypgwu4u@tcp:192.168.105.150:38857/2gsogw7y3zbuyupmb4ghpknea2i5kvtt

- 複製從 ‘cat’ 得到的字串，並將其貼到所有 node 的 `~/.tahoe/private/introducers.yaml` 中. 

---
## FTP

<h3 id="ftp-introducer">Instantiate the introducer</h3>

### Terminal:
- [ ] cd introducer/
- [ ] nano ~/introducer/private/accounts

```bash=
# This is a public key line: username keytype pubkey cap
# (Tahoe-LAFS v1.11 or later)
carol 
```

- [ ] ssh-keygen -f private/ssh_host_rsa_key

```
Generating public/private rsa key pair.
Enter passphrase (empty for no passphrase):  [ENTER]
Enter same passphrase again:  [ENTER]
```

- [ ] cat ~/introducer/private/ssh_host_rsa_key.pub
> ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABgQDY+zoZhschDVDXbWpGCpUin+GTTFXSW9O/fyVElsg5KClWAEf+ebddqA3YqrKNljhkguuOmcsyaMRBs8TLdQL7Z9QQ8hfETtSXXakW50AxeyhKuTx4KpVc3s9zsLL1xFQPhhyXGexDr2EWYORDyoLTKRi2a3hBwOahYO0jrGvoubfpsqvHw/ukyAymc8sjlsLLWn/URuhmWvMn3Vc7mYohxMSMHOGyyR3/uWuA4pT2TNHxoEQ2uQ4nmXw3Gi6pvoapsSY2FT3q8UTtiqQkmnBLbeuimNyd4kMg2ujkcasHRgwg2N+YkO0nd3oz5niL4lZDOB7j9P0RApIbYYXxZ0EImW/b8eZKgNhiKXC+3pI994NqwlxDPZl/kB8yTYSqgE/L5XgI2OMzjvjlC0uZJ1IPBemX+LerodqELC/Wo8YkAh2r2GIlgq3+Ss5JZlMHfP3NFvEaexxH+DtTRzAfZfUXP08TZIpgy3eS9PRx+kQr+eLibVOZz5h9xG7k0DhSb/c= bigred@150-tahoe-introducer

- [ ] nano ~/introducer/private/accounts

```bash=
# This is a public key line: username keytype pubkey cap
# (Tahoe-LAFS v1.11 or later)
carol ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABgQDY+zoZhschDVDXbWpGCpUin+GTTFXSW9O/fyVElsg5KClWAEf+ebddqA3YqrKNljhkguuOmcsyaMRBs8TLdQL7Z9QQ8hfETtSXXakW50AxeyhKuTx4KpVc3s9zsLL1xFQPhhyXGexDr2EWYORDyoLTKRi2a3hBwOahYO0jrGvoubfpsqvHw/ukyAymc8sjlsLLWn/URuhmWvMn3Vc7mYohxMSMHOGyyR3/uWuA4pT2TNHxoEQ2uQ4nmXw3Gi6pvoapsSY2FT3q8UTtiqQkmnBLbeuimNyd4kMg2ujkcasHRgwg2N+YkO0nd3oz5niL4lZDOB7j9P0RApIbYYXxZ0EImW/b8eZKgNhiKXC+3pI994NqwlxDPZl/kB8yTYSqgE/L5XgI2OMzjvjlC0uZJ1IPBemX+LerodqELC/Wo8YkAh2r2GIlgq3+Ss5JZlMHfP3NFvEaexxH+DtTRzAfZfUXP08TZIpgy3eS9PRx+kQr+eLibVOZz5h9xG7k0DhSb/c= bigred@150-tahoe-introducer
```

- [ ] nano tahoe.cfg

```bash=
[sftpd]
enabled = true
port = tcp:8022:interface=192.168.105.150
host_pubkey_file = private/ssh_host_rsa_key.pub
host_privkey_file = private/ssh_host_rsa_key
accounts.file = private/accounts
```

---

<h3 id="ftp-nodes">Instantiate the nodes</h3>

### Terminal:

- [ ] cd .tahoe/
- [ ] nano ~/.tahoe/private/accounts

```bash=
# This is a public key line: username keytype pubkey cap
# (Tahoe-LAFS v1.11 or later)
carol 
```

- [ ] ssh-keygen -f private/ssh_host_rsa_key
> Generating public/private rsa key pair.
Enter passphrase (empty for no passphrase):  [ENTER]
Enter same passphrase again:  [ENTER]

- [ ] cat private/ssh_host_rsa_key.pub
>ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABgQDeCY8AVyfq7hU9L2tLgnAQjOWNipzlRBiAZ966ulru0UZWGrjady8xkHh/esQ9AEkhqNuPDt83THTVy5953hCiWhaRrsR5bMxl3RmWb3qzOdgNBvBfrfd6hQVVv+QXeSyjGe9BfBgrmd6801grF8EXjdwL361+XKzSI5GQ4mW/vRW+DMeo/hHMTj5o5tQEWTMAqsDYNpbKRUDzmI/3QnnmTXeKxFve9pwepnEy5ckqniDyfVNhkZZtodxz1CmhsBxl/GDHBfv2piil4adiZmZw1m/MqL7pBHGgVvKtIT7zHRfZDwlxqthFhQTW3Q2eDYpB1/zSO8gpxZePV9Fa6nfr2cH4W75wARijXa2KPNZo7sHD5cu675OEaVjHKrYpWMJhyI0ULUHf+Ha/6ze6GJXlY94JOdHW/mvD78FaNhiFtt+c5qtIqOF7xoY8j65qeB/sNdL0Uva+NB/DCrIvzaVx3uuL7U6wJV9sNatJ10WTtOix7qu9AL0k6yTTaANAiLk= bigred@156-tahoe-node-6

- [ ] nano .tahoe/private/accounts

```bash=
# This is a public key line: username keytype pubkey cap
# (Tahoe-LAFS v1.11 or later)
carol ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABgQDeCY8AVyfq7hU9L2tLgnAQjOWNipzlRBiAZ966ulru0UZWGrjady8xkHh/esQ9AEkhqNuPDt83THTVy5953hCiWhaRrsR5bMxl3RmWb3qzOdgNBvBfrfd6hQVVv+QXeSyjGe9BfBgrmd6801grF8EXjdwL361+XKzSI5GQ4mW/vRW+DMeo/hHMTj5o5tQEWTMAqsDYNpbKRUDzmI/3QnnmTXeKxFve9pwepnEy5ckqniDyfVNhkZZtodxz1CmhsBxl/GDHBfv2piil4adiZmZw1m/MqL7pBHGgVvKtIT7zHRfZDwlxqthFhQTW3Q2eDYpB1/zSO8gpxZePV9Fa6nfr2cH4W75wARijXa2KPNZo7sHD5cu675OEaVjHKrYpWMJhyI0ULUHf+Ha/6ze6GJXlY94JOdHW/mvD78FaNhiFtt+c5qtIqOF7xoY8j65qeB/sNdL0Uva+NB/DCrIvzaVx3uuL7U6wJV9sNatJ10WTtOix7qu9AL0k6yTTaANAiLk= bigred@156-tahoe-node-6
```

- [ ] nano tahoe.cfg

```bash=
[sftpd]
enabled = true
port = tcp:8022:interface=192.168.105.XXX
host_pubkey_file = private/ssh_host_rsa_key.pub
host_privkey_file = private/ssh_host_rsa_key
accounts.file = private/accounts
```

:mag: Reference
---

- https://tahoe-lafs.readthedocs.io/
- https://tahoe-lafs.org/trac/tahoe-lafs/wiki/Tutorial
- https://github.com/tahoe-lafs/tahoe-lafs

---

## Notes 
<!-- Other important details discussed during the meeting can be entered here. -->
