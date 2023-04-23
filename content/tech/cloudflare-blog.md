---
title: "使用Cloudflare搭建个人静态网站"
date: 2023-04-23T20:37:02+09:00
draft: true
---
# github pages VS vps VS cloudflare
众所周知，cloudflare pages和github pages是竞争产品，在朋友的介绍下见识到了cloudflare服务器的性能确实优于github。
github pages最大的问题就是对站点的更改在推送到 GitHub 后，最长可能需要 10 分钟才会发布。：

首先访问cloudflare官网 ： https://pages.cloudflare.com

注册以后进入点击pages：
![20230423215824](https://raw.githubusercontent.com/Gzk738/vps_picgo/master/images/20230423215824.png)

新建project
![20230423215851](https://raw.githubusercontent.com/Gzk738/vps_picgo/master/images/20230423215851.png)

创建网站的方式有两种，一种是github链接网站文件，另一种是直接从本地上传渲染好的静态文件，由于链接github需要设置环境变量，我懒得弄，直接从本地上传渲染好的静态文件了就。

```
hugo -D
```
选择本地的public
![20230423220148](https://raw.githubusercontent.com/Gzk738/vps_picgo/master/images/20230423220148.png)

稍等片刻

![20230423220222](https://raw.githubusercontent.com/Gzk738/vps_picgo/master/images/20230423220222.png)

点击domains：

![20230423220959](https://raw.githubusercontent.com/Gzk738/vps_picgo/master/images/20230423220959.png)

ok，大功告成！你有了一个不用租用vps或者需要等待10分钟才能发布的个人网站

