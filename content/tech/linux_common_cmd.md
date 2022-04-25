---
title: "Linux常用命令记录"
date: 2022-01-26T17:04:33+08:00
draft: true
---


记录一些不常用的命令，不然每次都去查，很麻烦。由于写的拉乱无序，推荐使用浏览器的search功能进行查找



# Nano编辑器的使用



## 复制、剪贴和粘贴
```
复制一整行：Alt+6

删除一整行：Ctrl+K

粘贴：Ctrl+U
```
## 搜索
按Ctrl+W，然后输入你要搜索的关键字，回车确定。这将会定位到第一个匹配的文本，接着可以用Alt+W来定位到下一个匹配的文本。

## 翻页
用Ctrl+Y到上一页，Ctrl+V到下一页

## 保存
使用Ctrl+O来保存所做的修改

退出
按Ctrl+X

# linux设备的基本操作
开关机：
```
shutdown -h now #立刻进行关机 需要root
poweroff        #无需root
```

## 重启
```
shutdown -r now # 立刻重启计算机 需要root
reboot          # 立刻重启计算机 无需root
```

## 查看当前存储器使用情况
```
➜  vps_server git:(main) ✗ df -h


Filesystem      Size  Used Avail Use% Mounted on
udev            485M     0  485M   0% /dev
tmpfs            99M  6.6M   93M   7% /run
/dev/xvda1      7.7G  1.5G  5.9G  20% /
tmpfs           494M     0  494M   0% /dev/shm
tmpfs           5.0M     0  5.0M   0% /run/lock
tmpfs           494M     0  494M   0% /sys/fs/cgroup
/dev/xvda15     124M  278K  124M   1% /boot/efi
tmpfs            99M     0   99M   0% /run/user/1001

```

## 查看文件夹大小
```
eg: du -hs /home/zikun/

du [OPTION]... [FILE]...
-h, --human-readable
    以K，M，G为单位，显示文件的大小

-s, --summarize
    只显示总计的文件大小

-S, --separate-dirs
    显示时并不含其子文件夹的大小

-d, --max-depth=N
    显示子文件夹的深度（层级）

```

## 查询公网ip
```
sudo curl ipinfo.io
```
## 查询本机ip
```
sudo ifconfig
```

## 用户和用户组
```
sudo adduser username

sudo usermod -aG sudo username
```

## WGET # install
```
wget file_url
dpkg -i file_url
```

# 查看配置
```
watch -n.1 "grep \"^[c]pu MHz\" /proc/cpuinfo" #查看cpu频率
wget -qO- yabs.sh | bash -s -- -i              # 跑分
```

# 查询软件版本号
```
cat /proc/version
```
➜  vps_server git:(main) ✗ cat /proc/version

Linux version 4.19.0-16-cloud-amd64 (debian-kernel@lists.debian.org) (gcc version 8.3.0 (Debian 8.3.0-6)) #1 SMP Debian 4.19.181-1 (2021-03-19)