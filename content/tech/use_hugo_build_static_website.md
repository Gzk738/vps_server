---
title: "使用hugo在vps上部署静态网站"
date: 2022-01-14T15:28:45Z
draft: true
---

朋友推荐给我使用hugo写blog，但是由于初期是在是找不到服务器去搭建，所以用github pages代替。体验实在是一言难尽，无论是推送指定分支还是github服务器的刷新延迟。最近
白嫖了一个aws的vps，使用git同步功能便非常舒服，加上最近刚刚由pycharm转到vscode，终于找到了目前来讲最舒服的开发方法：hugo挂在vps上，vscode用ssh远程到服务器，图片
也不需要手动保存，而是用picgo+git图床自动生成链接，所以粘贴图片就如同jupyter一样方便，截图--粘贴 所以在这记录一下

## blog服务器
首先去弄一台服务器：
我目前用了两个：[DigitalOcean](https://www.digitalocean.com)，和[AWS](https://aws.amazon.com/cn/free/?trk=ps_a134p000003yHYmAAM&trkCampaign=acq_paid_search_brand&sc_channel=PS&sc_campaign=acquisition_KR&sc_publisher=Google&sc_category=Core-Main&sc_country=KR&sc_geo=APAC&sc_outcome=acq&sc_detail=aws&sc_content=Brand_Core_aws_e&sc_segment=444218215904&sc_medium=ACQ-P%7CPS-GO%7CBrand%7CDesktop%7CSU%7CCore-Main%7CCore%7CKR%7CEN%7CText&s_kwcid=AL!4422!3!444218215904!e!!g!!aws&ef_id=CjwKCAiA5t-OBhByEiwAhR-hm31lyXVVI5StyVLBjqvwYTfq7nU-JlkPhTYWG2aQ2fgUDTjT9ZNWZBoCcCgQAvD_BwE:G:s&s_kwcid=AL!4422!3!444218215904!e!!g!!aws&all-free-tier.sort-by=item.additionalFields.SortRank&all-free-tier.sort-order=asc&awsf.Free%20Tier%20Types=*all&awsf.Free%20Tier%20Categories=*all)
的vps，据说还有一个[enserv.com](enserv.com)可以免费白嫖，不过我还没试。

然后使用ssh登陆：
```
ssh hostname@xxx.xxx.xxx.xxx
```
其实以后用vscode的remote ssh登陆就好了，自动记录ip，只需要输入密码，剩下的操作全部在server上完成。

如果aws服务器，只能用本地密钥的话，需要配置一下允许ssh登陆：
```
sudo passwd root
su root
vim /etc/ssh/sshd_config
```
找到 PasswordAuthentication no，把no改成yes。把PermitRootLogin forced-commands-only 把forced-commands-only改成yes
然后就可以像普通server使用ssh远程了。

（注：同时需要aws上安全策略添加ssh的入站规则，否则流量会被防火墙挡住）

hugo server常用命令：
```
screen 
screen -ls
screen -r screen_id
```
直接退出的话，使用control+d，但是这样进程也会消失，如果想后台运行的话：control+a+d

但是有些时候进程卡死无法进入screen进行exit的时候，可以指定杀死进程：
```
screen -S screenID -X quit
```

编写完blog以后，直接进入screen：
```
hugo server -D --bind=0.0.0.0 --port=8888
```
然后后台这个screen，尽情编写blog吧，当文件被重写的时候，hugo会自动编译并重启server

## 图床的选择

为什么要使用图床？
首先是如果每张图都手动放到本地上，并不是不行，但是在开发的时候非常不方便，最开始我是用本机写
然后使用git同步，总是这样依然需要手动保存每张图片然后传到static文件夹下，然后根据目录进行调用。

暂时我没有找到什么方法可以一键生成本地链接，但是我找到了一键生成图床链接的方法：

最开始使用的是[图床:ImgURL](https://imgurl.org/)但是没找到方法一键生成，这给写作带来极大得麻烦。后来发现了github也可以当作图床使用：
首先下载picgo
![20220111153546](https://raw.githubusercontent.com/Gzk738/vps_picgo/master/images/20220111153546.png)

### github设置
创建一个新的库，或者放在你blog的库里，我新建了一个库：
![20220111154049](https://raw.githubusercontent.com/Gzk738/vps_picgo/master/images/20220111154049.png)
创建token权限能够自由push和pull图片
![20220111154319](https://raw.githubusercontent.com/Gzk738/vps_picgo/master/images/20220111154319.png)
这里只需要添加图中的在这几个选项就可以。其他的不用管

一切设置完后，就可以自由的粘贴图片了，github速度非常快，就是不知道国内会咋样？

快捷键：
![20220111154449](https://raw.githubusercontent.com/Gzk738/vps_picgo/master/images/20220111154449.png)

# 购买域名

域名我使用namesilo购买，域名的作用是不需要记ip，因为服务器可能经常换，如果有域名的话只需要映射到服务器ip即可，十分的方便快捷

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
