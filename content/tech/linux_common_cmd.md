---
title: "Linux常用命令记录"
date: 2022-01-26T17:04:33+08:00
draft: true
---

记录一些不常用的命令，不然每次都去查，很麻烦



# Nano

[toc]

复制、剪贴和粘贴

复制一整行：Alt+6

剪贴一整行：Ctrl+K

粘贴：Ctrl+U

搜索
按Ctrl+W，然后输入你要搜索的关键字，回车确定。这将会定位到第一个匹配的文本，接着可以用Alt+W来定位到下一个匹配的文本。

翻页
用Ctrl+Y到上一页，Ctrl+V到下一页

保存
使用Ctrl+O来保存所做的修改

退出
按Ctrl+X

开关机：
```
shutdown -h now #立刻进行关机 需要root
poweroff        #无需root
```

重启
```
shutdown -r now # 立刻重启计算机 需要root
reboot          # 立刻重启计算机 无需root
```

查看文件夹大小
```
du [OPTION]... [FILE]...
-h, --human-readable
    以K，M，G为单位，显示文件的大小

-s, --summarize
    只显示总计的文件大小

-S, --separate-dirs
    显示时并不含其子文件夹的大小

-d, --max-depth=N
    显示子文件夹的深度（层级）
eg du -hs /home/zikun/
```
