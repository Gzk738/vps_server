---
title: "Vscode+hugo+picgo+vps的个人blog搭建记录"
date: 2022-01-04T06:37:55Z
draft: true
---
朋友推荐给我使用hugo写blog，但是由于初期是在是找不到服务器去搭建，所以用github pages代替。体验实在是一言难尽，无论是推送指定分支还是github服务器的刷新延迟。最近白嫖了一个aws的vps，使用git同步功能便非常舒服，加上最近刚刚由pycharm转到vscode，终于找到了目前来讲最舒服的开发方法：hugo挂在vps上，vscode用ssh远程到服务器，图片也不需要手动保存，而是用picgo+git图床自动生成链接，所以在这记录一下

首先去弄一台服务器：
我目前用了两个：[DigitalOcean](https://www.digitalocean.com)，和[AWS](https://aws.amazon.com/cn/free/?trk=ps_a134p000003yHYmAAM&trkCampaign=acq_paid_search_brand&sc_channel=PS&sc_campaign=acquisition_KR&sc_publisher=Google&sc_category=Core-Main&sc_country=KR&sc_geo=APAC&sc_outcome=acq&sc_detail=aws&sc_content=Brand_Core_aws_e&sc_segment=444218215904&sc_medium=ACQ-P%7CPS-GO%7CBrand%7CDesktop%7CSU%7CCore-Main%7CCore%7CKR%7CEN%7CText&s_kwcid=AL!4422!3!444218215904!e!!g!!aws&ef_id=CjwKCAiA5t-OBhByEiwAhR-hm31lyXVVI5StyVLBjqvwYTfq7nU-JlkPhTYWG2aQ2fgUDTjT9ZNWZBoCcCgQAvD_BwE:G:s&s_kwcid=AL!4422!3!444218215904!e!!g!!aws&all-free-tier.sort-by=item.additionalFields.SortRank&all-free-tier.sort-order=asc&awsf.Free%20Tier%20Types=*all&awsf.Free%20Tier%20Categories=*all)
的vps，据说还有一个[enserv.com](enserv.com)可以免费白嫖，不过我还没试。

然后使用ssh登陆：
```
ssh zikun@xxx.xxx.xxx.xxx
```
其实以后用vscode的remote ssh登陆就好了，自动记录ip，只需要输入密码，剩下的操作全部在server上完成。
如果aws服务器，只能用本地密钥的话，需要配置一下开启root登陆：
```
sudo passwd root
su root
vim /etc/ssh/sshd_config
```
找到 PasswordAuthentication no，把no改成yes。把PermitRootLogin forced-commands-only 把forced-commands-only改成yes
然后就可以像普通server使用ssh远程了。

hugo server常用命令：
```
screen 
screen -ls
screen -r screen_id
```
直接退出的话，使用control+d，但是这样进程也会消失，如果想后台运行的话：control+a+d

但是有些时候进程卡死无法进入screen进行exit的时候，可以指定杀死进程：
```
Screen -S screenID -X quit
```

编写完blog以后，直接进入screen：
```
hugo server -D --bind=xxx.xxx.xxx.xxx --port=xxx
```
然后后台这个screen，尽情编写blog吧，当文件被重写的时候，hugo会自动编译并重启server

## 图床的选择
最开始使用的是[图床:ImgURL](https://imgurl.org/)但是没找到方法一键生成，这给写作带来极大得麻烦。后来发现了github也可以当作图床使用：
