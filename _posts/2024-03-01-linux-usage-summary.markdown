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

## 2. Proxy setup
### 2.1 clash setup and configure
```
$ wget https://github.com/doreamon-design/clash/releases/download/v2.0.24/clash_2.0.24_linux_amd64.tar.gz
$ tar -xvzf clash_2.0.24_linux_amd64.tar.gz
prepare config.yaml
$ ./clash -d . 
it will download Country.mmdb if first start
```

### 2.2 Setting proxy if need
Assume your proxy as below:

```
http://127.0.0.1:7890
```

* git proxy:
```
git config --global http.proxy 'http://127.0.0.1:7890'
git config --global https.proxy 'http://127.0.0.1:7890'
```

* http/https proxy
```
export http_proxy="http://127.0.0.1:7890"
export https_proxy="http://127.0.0.1:7890"
```

* wget proxy
```
$ cat ~/.wgetrc
https_proxy=http://127.0.0.1:7890
http_proxy=http://127.0.0.1:7890
ftp_proxy=http://127.0.0.1:7890
no_proxy = 127.0.0.1
use_proxy=on
```

* apt proxy
For console
```
$ sudo apt-get -o Acquire::http::proxy="http://127.0.0.1:7890" update
$ sudo apt-get -o Acquire::http::proxy="http://127.0.0.1:7890" install xxx
```

For configure file
```
$ cat /etc/apt/apt.conf.d/proxy.conf

Acquire::http::Proxy "http://127.0.0.1:7890";
Acquire::https::Proxy "http://127.0.0.1:7890";
or
Acquire::http::Proxy "http://username:password@yourproxyaddress:proxyport";
Acquire::https::Proxy "http://username:password@yourproxyaddress:proxyport";
```

* curl proxy
```
$ curl -x "http://user:pwd@127.0.0.1:1234" "http://httpbin.org/ip"
$ curl --proxy "http://user:pwd@127.0.0.1:1234" "http://httpbin.org/ip"
$ curl --socks5 socks5_proxy:port http://www.example.com
```

## 3. Rust cargo http proxy configure
```
$ cat ~/.cargo/config.toml
[http]
proxy="127.0.0.1:7890"

[https]
proxy="127.0.0.1:7890"
```

## 90. Linux command usage
### 90.1 find command
* To find files larger than 100MB
```
find . -type f -size +100M
```

* To find files larger than 100MB of current dir only
```
find . -maxdepth 1 -type f -size +100M
```

* find files larger than 100M of /foo/bar dir and delete it
```
$ find /foo/bar -type f -size +100M -print | xargs rm
```
## 99. Reference Link
1) [How to Install and Configure Samba on Ubuntu 24.04](https://www.veeble.com/kb/how-to-install-and-configure-samba-on-ubuntu-24-04/)

2) [Install and Configure Samba](https://ubuntu.com/tutorials/install-and-configure-samba#4-setting-up-user-accounts-and-connecting-to-share)

