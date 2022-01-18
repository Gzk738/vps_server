---
title: "在vps上搭建v2ray"
date: 2022-01-17T13:56:41Z
draft: true
---

# 使用命令安装v2ray

```
bash <(curl -s -L https://git.io/v2ray.sh)
```

安装。然后根据默认配置即可，或者根据个人喜好进行安装


安装完成以后，出现如图所示信息
```
---------- V2Ray 配置信息 -------------

 地址 (Address) = xxx.xxx.xxx.xxx

 端口 (Port) = xxxxx

 用户ID (User ID / UUID) = xxxxxxxxxxxxxxxxx

 额外ID (Alter Id) = 0

 传输协议 (Network) = tcp

 伪装类型 (header type) = none

---------- END -------------

V2Ray 客户端使用教程: https://233v2.com/post/4/

提示: 输入  v2ray url  可生成 vmess URL 链接 / 输入  v2ray qr  可生成二维码链接

免被墙..推荐使用JMS: https://getjms.com
```

# V2Ray 管理面板

命令：
```
v2ray
```

然后根据提示进行你想要的操作即可。

```
........... V2Ray 管理脚本 v3.49 by 233v2.com ..........

## V2Ray 版本: v4.44.0  /  V2Ray 状态: 正在运行 ##

帮助说明: https://233v2.com/post/1/

反馈问题: https://github.com/233boy/v2ray/issues

TG 频道: https://t.me/tg2333

捐赠脚本作者: https://233v2.com/donate/

捐助 V2Ray: https://www.v2ray.com/chapter_00/02_donate.html

  1. 查看 V2Ray 配置

  2. 修改 V2Ray 配置

  3. 下载 V2Ray 配置 / 生成配置信息链接 / 生成二维码链接

  4. 查看 Shadowsocks 配置 / 生成二维码链接

  5. 修改 Shadowsocks 配置

  6. 查看 MTProto 配置 / 修改 MTProto 配置

  7. 查看 Socks5 配置 / 修改 Socks5 配置

  8. 启动 / 停止 / 重启 / 查看日志

  9. 更新 V2Ray / 更新 V2Ray 管理脚本

 10. 卸载 V2Ray

 11. 其他

温馨提示...如果你不想执行选项...按 Ctrl + C 即可退出

请选择菜单 [1-11]:1


---------- V2Ray 配置信息 -------------

 地址 (Address) = 52.15.162.233

 端口 (Port) = 53236

 用户ID (User ID / UUID) = cec8a3de-e9e2-4d37-bc51-c2d5561cdd09

 额外ID (Alter Id) = 0

 传输协议 (Network) = tcp

 伪装类型 (header type) = none

---------- END -------------

V2Ray 客户端使用教程: https://233v2.com/post/4/

提示: 输入  v2ray url  可生成 vmess URL 链接 / 输入  v2ray qr  可生成二维码链接

免被墙..推荐使用JMS: https://getjms.com
```


# 客户端 mac端 v2rayX

使用v2rayx：

[下载链接： https://github.com/insisttech/v2rayX-copy/releases](https://github.com/insisttech/v2rayX-copy/releases)

[使用方法请参阅http://mccm.me/71.html](http://mccm.me/71.html)

# 客户端 win端 v2rayN

[下载客户端，地址： https://github.com/2dust/v2rayN/releases/latest](https://github.com/2dust/v2rayN/releases/latest)

具体教程请参阅[V2rayN 4.12配置教程 - V2ray XTLS黑科技](https://v2xtls.org/v2rayn-4-12配置教程/)或自行寻找第三方教程