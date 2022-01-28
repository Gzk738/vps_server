---
title: "如何安装java 17"
date: 2022-01-28T15:12:38+08:00
draft: true
---

首先，对于linux的教程就不写了，懂得都懂 :-)

# WIN用户:-)

[去官网，下载openjdk ：https://jdk.java.net/17/](https://jdk.java.net/17/)

![20220128163444](https://raw.githubusercontent.com/Gzk738/vps_picgo/master/images/20220128163444.png)

选择Windows / x64	zip (sha256)	186216309下载即可

解压到合适的位置
下载得到一个zip压缩包，只需将其解压到一个非c盘（c盘经常有权限问题）英文路径下，比如D:/tool/openjdk

设置环境变量

上边操作已相当于完成安装步骤，只需让系统找到你解压的路径即可。在win10左下角搜索环境变量，找到环境变量设置（或通过其他方式找到）

![20220128164109](https://raw.githubusercontent.com/Gzk738/vps_picgo/master/images/20220128164109.png)

点击环境变量，环境变量分为两部分，上边是当前登录用户的环境变量，
下面是全局的环境变量，我们直接设置全局的即可。

在下面的环境变量点击新建，变量名输入JAVA_HOME，变量值输入刚才解压的路径，或者点击浏览目录，自己找到刚才解压的路径，然后点击确定。

![20220128164210](https://raw.githubusercontent.com/Gzk738/vps_picgo/master/images/20220128164210.png)

到这里系统里有一个变量就叫JAVA_HOME（java的家），JAVA_HOME的路径就在刚才配置的路径。
然后我们还要告诉系统把这个JAVA_HOME的路径配置到全系统的路径里。所以下面开始把JAVA_HOME添加到path变量里。

找到path变量，然后双击或者编辑。

![20220128164335](https://raw.githubusercontent.com/Gzk738/vps_picgo/master/images/20220128164335.png)

点击新建之后，会在最后一行出现可编辑的空行，我们可以在这里输入%JAVA_HOME%\bin。
两个百分号中间包的是变量名，意思是取这个变量的值，这里的效果就是把JAVA_HOME配的值再拼一个bin，在整个系统里使用的命令会来这个bin目录下找，我们可以发现这个路径下面有java.exe

然后点击确定，再确定，直到最后一个确定会关闭环境变量。

完成
打开cmd，输入java -version可以看到打印java基本信息。这里就安装成功了。

# MAC用户


[去官网，下载openjdk ：https://www.oracle.com/java/technologies/downloads/#jdk17-mac](https://www.oracle.com/java/technologies/downloads/#jdk17-mac)

![20220128165003](https://raw.githubusercontent.com/Gzk738/vps_picgo/master/images/20220128165003.png)

选择macOS / x64		zip (sha256)	186216309下载即可

然后就如同正常pkg安装包一样下一步就好啦~

## 验证

打开terminal，输入
```
java -version

openjdk version "17.0.2" 2022-01-18
OpenJDK Runtime Environment (build 17.0.2+8-86)
OpenJDK 64-Bit Server VM (build 17.0.2+8-86, mixed mode, sharing)
```

