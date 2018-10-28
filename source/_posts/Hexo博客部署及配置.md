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
首先利用命令行生成一个ssh密钥。[SSH密钥介绍](https://wiki.archlinux.org/index.php/SSH_keys_(%E7%AE%80%E4%BD%93%E4%B8%AD%E6%96%87))
```bash
    ssh-keygen -t ecdsa -b 521
```
*-t* 表示密钥算法,此处算法ecdsa, *-b*表示密钥长度，ecdsa最高支持521
命令行会显示
```bash
Generating public/private ecdsa key pair.
Enter file in which to save the key (/user/.ssh/id_ecdsa): 
```
默认即可
```bash
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
## 部署及备份
## 博客基本改造
