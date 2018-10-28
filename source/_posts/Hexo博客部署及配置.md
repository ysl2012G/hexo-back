---
title: Hexo博客部署及配置
top: 0
date: 2018-10-28 16:01:18
categories:
    - 编程
tags:
    - 博客
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
###　安装
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
`站点`根目录下_config.yml配置rsync部署站点， 这里不采用git方式是因为不需要同步到git上，不需要用到git hooks等配置。
```yml
deploy:
  type: rsync
  host: 域名
  user: user ## vps上用户名 
  root: /var/www/hexo ## 站点目录
  port: 339  ## 端口
  delete: true  ## 是否先删除再更新
  verbose: true ## 显示调试信息
```

### 备份文件设置
`站点`目录下_config.yml 采用第三方git方式备份
```
backup:
  type: git
  theme: next  ##备份的主题
  repository:
    github: git@github.com::file, branchName
```

## 博客基本改造

### 优秀设置网址连接

网络上有大量优秀的博客改造文章，不一一复述了。

[Hexo搭建博客教程](https://thief.one/2017/03/03/Hexo%E6%90%AD%E5%BB%BA%E5%8D%9A%E5%AE%A2%E6%95%99%E7%A8%8B/)
[hexo的next主题个性化配置教程](https://segmentfault.com/a/1190000009544924#articleHeader11)
[Markdown 语法](https://spacejmmy.github.io/2017/08/27/2017-08-27-Hexo%E5%8D%9A%E5%AE%A2%E6%92%B0%E5%86%99%E4%B9%8BMarkDown%E8%AF%AD%E6%B3%95%E4%BB%8B%E7%BB%8D/)

### 值得注意的官方改进

[next官方文档](https://theme-next.iissnan.com/getting-started.html)中有很多新的改进可以留意下。

在`主题_confiy.yml` 中

menu 设置 icon 
```
home: / || home 
tags: /tags/ || tags  
categories: /categories/ || th
archives: /archives/ || archive
```

设置头像：
```
# Sidebar Avatar
# in theme directory(source/images): /images/avatar.gif
# in site  directory(source/uploads): /uploads/avatar.gif
avatar: /uploads/avatar.jpg
```

主页不显示全文：
```
# Automatically Excerpt. Not recommend.
# Please use <!-- more --> in the post to control excerpt accurately.
auto_excerpt:
  enable: true
  length: 150
```

声明版权：
```
# Declare license on posts
post_copyright:
  enable: true
  license: CC BY-NC-SA 3.0
  license_url: https://creativecommons.org/licenses/by-nc-sa/3.0/
```

