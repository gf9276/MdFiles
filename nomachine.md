<!-- TOC -->

- [1. nomachine](#1-nomachine)
- [2. window端安装nomachine](#2-window端安装nomachine)
  - [2.1. windows作为服务端的server设置](#21-windows作为服务端的server设置)
  - [2.2. windows作为客户端的player设置](#22-windows作为客户端的player设置)
- [3. ubuntu 安装](#3-ubuntu-安装)
  - [3.1. ubuntu作为服务端server设置](#31-ubuntu作为服务端server设置)
- [4. 连接](#4-连接)
  - [4.1. 屏幕设置](#41-屏幕设置)

<!-- /TOC -->

# 1. nomachine

还是ubuntu20.04bug少点

局域网内连接软件，我主要是用于wsl2

这玩意用在wsl2上，丝滑到一种境界了，吊着xrdp打

[nomachine官网](https://www.nomachine.com/)

[说明文档](https://kb.nomachine.com/DT07S00236?s=minimize%20the%20windows)

[Knowledge Base](https://kb.nomachine.com/)

随便折腾一下就会了

# 2. window端安装nomachine

直接下载.exe安装就行

![](https://cdn.jsdelivr.net/gh/gf9276/image/nomachine/0.png)

装好之后两个图标，按哪个都可以进入，其实应该按NoMachine那个

![](https://cdn.jsdelivr.net/gh/gf9276/image/nomachine/2.png)

用法直接看[说明文档](https://kb.nomachine.com/DT07S00236?s=minimize%20the%20windows)，这里记录几个设置

![](https://cdn.jsdelivr.net/gh/gf9276/image/nomachine/3.png)

## 2.1. windows作为服务端的server设置

直接把所有server关了，反正连接wsl，windows不做服务端。其他都不用改

![](https://cdn.jsdelivr.net/gh/gf9276/image/nomachine/20230815211220.png)

## 2.2. windows作为客户端的player设置

改几个默认快捷键就行（为了防止快捷键冲突，所以没用的最好关了，有用的改的复杂一点）

![](https://cdn.jsdelivr.net/gh/gf9276/image/nomachine/20230815211330.png)

![](https://cdn.jsdelivr.net/gh/gf9276/image/nomachine/20230815211611.png)


# 3. ubuntu 安装

ubuntu 肯定是.deb啊

[速达链接](https://downloads.nomachine.com/download/?id=3)

![](https://cdn.jsdelivr.net/gh/gf9276/image/nomachine/20230815213009.png)

点进去后都写着呢

![](https://cdn.jsdelivr.net/gh/gf9276/image/nomachine/20230815213049.png)

装完都是开机默认自动启动的，不用管，自启动才好

## 3.1. ubuntu作为服务端server设置

关掉server，然后把端口改成4001，防止和windows打起来（好像不改也不会咋样），其他默认。

这里需要先用Xrdp连接进去，然后才能改，命令行我没试过。

![](https://cdn.jsdelivr.net/gh/gf9276/image/nomachine/20230815213214.png)


# 4. 连接


![](https://cdn.jsdelivr.net/gh/gf9276/image/nomachine/20230815211913.png)

![](https://cdn.jsdelivr.net/gh/gf9276/image/nomachine/20230815212024.png)

然后一直ok就行

进入界面长这样子，点一下右上角会进入设置模式（或者 ctrl alt 0）

![](https://cdn.jsdelivr.net/gh/gf9276/image/nomachine/20230815212206.png)

## 4.1. 屏幕设置

![](https://cdn.jsdelivr.net/gh/gf9276/image/nomachine/20230815212250.png)

**从左到右，分别是：**

1. viewpoint mode

    ![](https://cdn.jsdelivr.net/gh/gf9276/image/nomachine/s0.png)

2. scale to window

    ![](https://cdn.jsdelivr.net/gh/gf9276/image/nomachine/s1.png)

3. resize remote display

    ![](https://cdn.jsdelivr.net/gh/gf9276/image/nomachine/s2.png)

(･∀･)​ 20:10:45
fullscreen 全屏

(･∀･)​ 20:11:21
fullscreen on all monitors 拼接所有屏幕

(･∀･)​ 20:11:39
iconize 缩小为图标

质量设置为最佳，wslg难道还会缺网速？

![](https://cdn.jsdelivr.net/gh/gf9276/image/nomachine/20230815214357.png)

