---
title: "购买域名以及使用caddy部署网站"
date: 2022-01-14T15:28:45Z
draft: true
---

终于在朋友的劝说下在[namesilo上](https://www.namesilo.com)买了个dns，,他人在大洋彼岸隔着10个小时的时差来帮助我

具体方便在vps上开发的方法请见[这篇文章](https://guozikun.xyz/tech/vscode+hugo/)

# 首先购买域名

![20220115004154](https://raw.githubusercontent.com/Gzk738/vps_picgo/master/images/20220115004154.png)


进去注册，然后邮箱确认，一步一步根据提示来就好。

![20220115004359](https://raw.githubusercontent.com/Gzk738/vps_picgo/master/images/20220115004359.png)

进入DNS Records:

![20220115004500](https://raw.githubusercontent.com/Gzk738/vps_picgo/master/images/20220115004500.png)

点击网络类型A,添加，（ipv4类型）如果有ipv6选择AAAA
hostname 可以写www可以不写。然后保存
namesilo这个网站服务很慢，需要等一会才能解析到ip

# 安装caddy

这个时候，需要使用caddy来取代hugo进行服务端的工作：
```shell
wget https://github.com/caddyserver/caddy/releases/download/v2.4.6/caddy_2.4.6_linux_amd64.deb

sudo dpkg -i caddy_2.4.6_linux_amd64.deb  
```

至此caddy安装结束

# 配置caddy
开启之前，需要配置caddyfile配置文件,这里可以使用nano或者vim
```shell
sudo nano /etc/caddy/Caddyfile  
```
或者
```shell
sudo vim /etc/caddy/Caddyfile
```

在配置文件中全选删除，然后填入：
```
root * /home/zikun/github/vps_server/public
        file_server
        log {
                output file /var/log/caddy.log
        }
}
```
其中/home/zikun/github/vps_server/public要替换成hugo的public文件夹所在位置

# 开启caddy服务

开启之前为了保险起见，先停用一下caddy
```shell
sudo systemctl stop caddy 
```
然后开启
```shell
sudo systemctl start caddy
```
检车caddy的status
```
sudo systemctl status caddy
```
如果出现错误，很大概率是配置文件的缩进问题，使用
```
sudo caddy fmt /etc/caddy/Caddyfile
```
输出正常的缩进代码，然后粘贴进配置文件

ok， 开始写你的blog吧
