---
title: "如何连接到minecraft服务器"
date: 2022-01-16T12:59:51Z
draft: true
---

这是教程的第二弹，如果你想搭建一个私有服务器的话，请查看：[搭建一个minecraft服务器(一) 服务端， https://guozikun.xyz/tech/搭建一个minecraft服务器/](https://guozikun.xyz/tech/搭建一个minecraft服务器/)
当然，如果只想连接现成的服务器的话，只需要跟着这里一步一步走即可：

首先，对于linux的教程就不写了，懂得都懂 :-)
# 第一步，安装java

请使用java 1.17，具体请参考[如何在mac上安装最新版的java, https://guozikun.xyz/tech/如何在mac上安装最新版的java/](https://guozikun.xyz/tech/install_java_17/)
如果使用旧版本的java的话，只能支持到1.17，当然，服务器是支持旧版本登陆的。只是一些矿产会显示的有问题。譬如铜矿和铁矿会长成一个样。

# 下载HMCL启动器

直接下载[hmcl启动器，https://hmcl.huangyuhui.net](https://hmcl.huangyuhui.net/download/)
![20220128161935](https://raw.githubusercontent.com/Gzk738/vps_picgo/master/images/20220128161935.png)


选择系统版本（win, linux, macos）后直接下载即可，下载完成后，你会得到这么一个东东：

![20220116221819](https://raw.githubusercontent.com/Gzk738/vps_picgo/master/images/20220116221819.png)

然后点击版本列表里边下载最新版本的1.18即可，大概10s就下载完成。

然后进入游戏：

选择多人游戏，然后添加服务器，直接输入； [mc.guozikun.xyz](https://guozikun.xyz/tech/搭建一个minecraft服务器-2/)
类似于这个样子：
![20220116222228](https://raw.githubusercontent.com/Gzk738/vps_picgo/master/images/20220116222228.png)

2022.1.28更：

新添加了userlog，所以针对新用户的第一次链接，会出现如下提示：
```
register new account : register <password> <password>
```

您需要注册新用户，譬如您希望密码是123456，那么就输入
```
register 123456 12345
```
然后确认，以后重新登陆的话，仅需要输入
```
/login 123456
```

登陆成功~

恭喜你，进入到了gzk的世界～
![20220116222506](https://raw.githubusercontent.com/Gzk738/vps_picgo/master/images/20220116222506.png)

如果是新手，欢迎查看[mc的官方教程:https://minecraft.fandom.com/zh/wiki/教程/新手手册](https://minecraft.fandom.com/zh/wiki/教程/新手手册)


