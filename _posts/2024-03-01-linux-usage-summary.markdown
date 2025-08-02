---
layout: post
title: "Linux Usage Summary"
date: 2024-03-01 10:00:00 +0800
comments: true
categories: linux samba rust cargo
---

# <center> Linux Usage Summary

## 1. Configure Samba on Ubuntu 22.04/24.04
### 1.1 Install samba
```
$ sudo apt update  -y
$ sudo apt install samba -y
$ sudo samba --version

```
### 1.2 Configure samba
Edit /etc/samba/smb.conf and add below content to end of the file
```
[SharedFolder]
path = /path/to/shared/folder
browseable = yes
writable = yes
guest ok = no
read only = no
```

### 1.3 Start and enable samba
```
$ sudo Systemctl restart smbd
$ sudo systemctl enable smbd
$ sudo systemctl status smbd
```

### 1.4 Setting up User Accounts and Connecting to Share
```
$ sudo smbpasswd -a username
```
Note:
Username used must belong to a system account, else it won’t save.

On windows:
```
\\ip-address\SharedFolder
```

On linux
```
smb://ip-address/SharedFolder
```

## 2. Rust cargo http proxy configure
```
$ cat ~/.cargo/config.toml
[http]
proxy="127.0.0.1:7890"

[https]
proxy="127.0.0.1:7890"
```

## 99. Reference Link
1) [How to Install and Configure Samba on Ubuntu 24.04](https://www.veeble.com/kb/how-to-install-and-configure-samba-on-ubuntu-24-04/)
2) [Install and Configure Samba](https://ubuntu.com/tutorials/install-and-configure-samba#4-setting-up-user-accounts-and-connecting-to-share)

