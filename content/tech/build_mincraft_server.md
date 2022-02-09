---
title: "搭建一个minecraft服务器"
date: 2022-01-15T09:21:42Z
draft: true
---

本文使用两种方法，推荐使用方法2，是一个一键搭建脚本，叫做papermc

# 方法一
首先创建一个screen.
```
screen -S minecraft
```

然后在这个screen里边安装java，我使用的是debian，因此安装的是默认的java10
```
sudo apt updata
sudo apt install default-jdk
java -version
```

安装以后应该出现以下提示信息：
```
openjdk version "11.0.13" 2021-10-19
OpenJDK Runtime Environment (build 11.0.13+8-post-Debian-1deb10u1)
OpenJDK 64-Bit Server VM (build 11.0.13+8-post-Debian-1deb10u1, mixed mode, sharing)
```
然后下载minecraft的server，可以访问官网：https://www.minecraft.net/zh-hans/servers

或者直接使用wget下载：
```
wget -O minecraft_server.jar https://launcher.mojang.com/v1/objects/1b557e7b033b583cd9f66746b7a9ab1ec1673ced/server.jar
```

下载完成以后改名：
```
mv server.jar minecraft_server.1.17.1.jar
```

n然后开启一下服务，但是这一步是无法开启的，会生成mc的lisence
```
java -Xmx1024M -Xms1024M -jar minecraft_server.1.17.1.jar nogui
```

然后使用vim或者nano把false改成true：
```
eula = true
```
配置服务器：
```
vi server.properties
```
当您对更改感到满意时，可以按键盘的 Escape 键退出插入模式。 然后，输入以下内容：
```
:wq
```

至此，配置已经完成。但是为了让别人链接这个服务器，我们需要配置防火墙：
```
iptables -I INPUT -p tcp --dport 25565 -j ACCEPT
```


# 方法二

使用paper minecraft 搭建脚本：[https://papermc.io](https://papermc.io)

然后选择download，由于不是wget方法，我们需要使用ssh传输到服务器，除非您的服务器是本机，那么仅需要下载到您的文件夹就可以

然后cd到当前文件夹,其中-Xms2G是选择服务器使用的内存大小，1.18.1-177是您的下载的paper服务器的jar，譬如我是paper-1.18.1-177.jar，选择2g内存，那么
只需要运行
```
java -Xms2G -Xmx2G -jar paper-1.18.1-177.jar --nogui
```
然后使用vim或者nano把生成的lensence的eula的false改成true：
```
eula = true
```
然后打开离线游戏选项：
```
online-mode=false
```

现在，人们可以从 Minecraft 启动屏幕连接到你的服务器了～
玩得开心～
