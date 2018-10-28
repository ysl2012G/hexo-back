---
title: Hexo博客部署及配置
top: 0
date: 2018-10-28 16:01:18
categories:
    - 编程
tags:
    - Hexo博客
password:
---

## 配置SSH免密登录远程主机
### 生成密钥
首先利用命令行生成一个ssh密钥。[SSH密钥介绍](https://wiki.archlinux.org/index.php/SSH_keys)
```bash
    ssh-keygen -t ecdsa -b 521
```
*-t* 表示密钥算法,此处算法ecdsa, *-b*表示密钥长度，ecdsa最高支持521
命令行会显示
```
Generating public/private ecdsa key pair.
Enter file in which to save the key (/user/.ssh/id_ecdsa): 
```
默认即可
```
Enter passphrase (empty for no passphrase): 
```
输入你的密码短语进行双重保险(即使别人获取你的私钥也需要密码进行远程登录)

### 远程登录
拷贝公钥至远程主机
```bash
    ssh-copy-id -i ~/.ssh/id_ecdsa.pub user@vps -p port
```

修改/etc/ssh/sshd_config/中的文件配置:
```vim
    PermitRootLogin no
    PasswordAuthentication no
```
禁止远程登录root，禁止密码登录

## 本地安装Hexo博客

### 利用包管理的方式安装nodejs

*Ubuntu/Debian*:
```
# Using Ubuntu
curl -sL https://deb.nodesource.com/setup_6.x | sudo -E bash -
sudo apt-get install -y nodejs

# Using Debian, as root
curl -sL https://deb.nodesource.com/setup_6.x | bash -
apt-get install -y nodejs
```
*centos*:
```
curl -sL https://rpm.nodesource.com/setup_8.x | bash -
```

### npm安装Hexo博客及相关插件

＃＃＃　安装
```bash
sudo npm install -g hexo-cli ## 安装hexo
sudo npm install hexo-deployer-git --save ## git
sudo npm install hexo-deployer-rsync --save ## rsync
sudo npm install hexo-git-backup --save ##备份
```

## 部署及备份
### 生成本地博客
```bash
hexo init <hexo-folder> ##安装hexo
```

## 博客基本改造
