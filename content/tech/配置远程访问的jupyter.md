---
title: "配置远程访问的jupyter"
date: 2022-01-20T15:28:40+08:00
draft: true
---

指定环境安装jupyter lab
```
conda activate xxx

pip install jupyterlab
```
或者
```
conda install jupyterlab
```

生成配置文件
```
jupyter notebook --generate-config
```
然后配置
```
c.NotebookApp.allow_remote_access = True
c.NotebookApp.open_browser = False
c.NotebookApp.ip='*'
c.NotebookApp.password = 'sha1:xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx'
```

或者直接用简单的命令：
```
jupyter lab password
```
然后打开允许远程访问

最后，开启服务，开启你的远程码农生活

```
jupyter lab --no-browser --port xxx --ip=xxx.xxx.xxx.xxx
```


