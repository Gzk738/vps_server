---
title: "pytorch常用代码"
date: 2022-01-24T14:32:09+08:00
draft: true
---

深度学习的pytorch和tensorflow的gpu版本踩坑记。遂记录以防继续踩坑

想知道显卡型号，使用：
```
(xai) zikun@abr-Z170X-UD3:~$ sudo lspci | grep -i nvidia
[sudo] password for zikun:
01:00.0 VGA compatible controller: NVIDIA Corporation GM200 [GeForce GTX TITAN X] (rev a1)
01:00.1 Audio device: NVIDIA Corporation GM200 High Definition Audio (rev a1)
02:00.0 VGA compatible controller: NVIDIA Corporation GM200 [GeForce GTX TITAN X] (rev a1)
02:00.1 Audio device: NVIDIA Corporation GM200 High Definition Audio (rev a1)

# Tensorflow的GPU版本的安装
```
安装nvidia-cuda-toolkit
```
sudo apt-get -f install
sudo apt install nvidia-cuda-toolkit
```
安装完成后可以使用nvcc --version检查Cuda compiler driver的version
```
(xai) zikun@abr-Z170X-UD3:~$ nvcc --version

nvcc: NVIDIA (R) Cuda compiler driver
Copyright (c) 2005-2015 NVIDIA Corporation
Built on Tue_Aug_11_14:27:32_CDT_2015
Cuda compilation tools, release 7.5, V7.5.17
```

cuda的检查
```
(xai) zikun@abr-Z170X-UD3:~$ nvidia-smi
Mon Jan 24 15:40:45 2022
+-----------------------------------------------------------------------------+
| NVIDIA-SMI 455.45.01    Driver Version: 455.45.01    CUDA Version: 11.1     |
|-------------------------------+----------------------+----------------------+
| GPU  Name        Persistence-M| Bus-Id        Disp.A | Volatile Uncorr. ECC |
| Fan  Temp  Perf  Pwr:Usage/Cap|         Memory-Usage | GPU-Util  Compute M. |
|                               |                      |               MIG M. |
|===============================+======================+======================|
|   0  GeForce GTX TIT...  Off  | 00000000:01:00.0 Off |                  N/A |
| 22%   36C    P8    14W / 250W |      3MiB / 12210MiB |      0%      Default |
|                               |                      |                  N/A |
+-------------------------------+----------------------+----------------------+
|   1  GeForce GTX TIT...  Off  | 00000000:02:00.0 Off |                  N/A |
| 22%   35C    P8    13W / 250W |      3MiB / 12212MiB |      0%      Default |
|                               |                      |                  N/A |
+-------------------------------+----------------------+----------------------+


+-----------------------------------------------------------------------------+
| Processes:
 |
|  GPU   GI   CI        PID   Type   Process name                  GPU Memory |
|        ID   ID                                                   Usage      |
|=============================================================================|
|  No running processes found
 |
+-----------------------------------------------------------------------------+
```

# Pytorch查看模型参数并计算模型参数量与可训练参数量


