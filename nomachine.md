<!-- TOC -->

- [1. nomachine](#1-nomachine)
- [2. 前置条件：wsl 安装桌面](#2-前置条件wsl-安装桌面)
  - [2.1. 安装](#21-安装)
    - [2.1.1. 安装 apt-fast](#211-安装-apt-fast)
    - [2.1.2. 设置 snap 代理](#212-设置-snap-代理)
    - [2.1.3. 锁住！](#213-锁住)
    - [2.1.4. 安装桌面环境](#214-安装桌面环境)
    - [2.1.5. 重装 dbus](#215-重装-dbus)
    - [2.1.6. 修复网络](#216-修复网络)
  - [2.2. 完善处理](#22-完善处理)
    - [2.2.1. 中文化](#221-中文化)
    - [2.2.2. 安装谷歌拼音](#222-安装谷歌拼音)
    - [2.2.3. 显卡问题](#223-显卡问题)
- [3. window 端安装 nomachine](#3-window-端安装-nomachine)
  - [3.1. windows 作为服务端的 server 设置](#31-windows-作为服务端的-server-设置)
  - [3.2. windows 作为客户端的 player 设置](#32-windows-作为客户端的-player-设置)
- [4. ubuntu 安装](#4-ubuntu-安装)
  - [4.1. ubuntu 作为服务端 server 设置](#41-ubuntu-作为服务端-server-设置)
- [5. 连接](#5-连接)
  - [5.1. 屏幕设置](#51-屏幕设置)
    - [5.1.1. 按钮介绍](#511-按钮介绍)
    - [5.1.2. display settings](#512-display-settings)

<!-- /TOC -->

# 1. nomachine

感觉还是 ubuntu20.04bug 少点

局域网内连接软件，我主要是用于 wsl2

这玩意用在 wsl2 上，丝滑到一种境界了，吊着 xrdp 打

nomachine 甚至可以传输声音。。。

[nomachine 官网](https://www.nomachine.com/)

[说明文档](https://kb.nomachine.com/DT07S00236?s=minimize%20the%20windows)

[Knowledge Base](https://kb.nomachine.com/)

**wsl 至少这个版本**

```
WSL 版本： 1.3.15.0
内核版本： 5.15.90.4-1
WSLg 版本： 1.0.55
MSRDC 版本： 1.2.4419
Direct3D 版本： 1.608.2-61064218
DXCore 版本： 10.0.25880.1000-230602-1350.main
Windows 版本： 10.0.19045.3324
```

截止到 2023 年 8 月 27 日，上面这个版本微软还没有正式发布，所以需要去 github 上面手动下载，下载下来直接双击运行就行。

![](https://cdn.jsdelivr.net/gh/gf9276/image/nomachine/20230827164949.png)

[wsl 发行版链接](https://github.com/microsoft/WSL/releases)

# 2. 前置条件：wsl 安装桌面

安装前把科学上网搞好或者把下载源给换了，不然装不好的。

## 2.1. 安装

### 2.1.1. 安装 apt-fast

apt-fast 下载更快，不然安装 gnome 要一个小时

```
add-apt-repository ppa:apt-fast/stable && apt-get install apt-fast -y
```

### 2.1.2. 设置 snap 代理

22.04 火狐和软件商店用的都是 snap，然鹅这玩意不会读取全局代理，所以得自己设置

换成你自己的代理 ip，分开执行（下面那个的 http 也是没有 s 的）

```
snap set system proxy.http="http://172.22.160.1:7890"

snap set system proxy.https="http://172.22.160.1:7890"
```

### 2.1.3. 锁住！

安装 gnome 前，锁住三个东西，避免变得不幸。如果你安装了，直接 apt purge -y 删掉他们

```
apt-mark hold acpid acpi-support modemmanager
```

acpid 是管理电源的，systemctl 检测到容器（就是 wsl2）会因为这玩意卡死的

[参考链接](https://github.com/microsoft/WSL/discussions/9350#discussioncomment-6119188)

modemmanager 是管理网络的，也是检测到容器网络设置直接卡死

[参考链接](https://forum.ubuntu.com.cn/viewtopic.php?t=490583)

### 2.1.4. 安装桌面环境

**安装 snap-firefox 会卡在那里一会儿，不要惊讶**

**如果你前面设置了 snap 代理，你会发现 clash 是在下载东西的**

**但是你要是没有设置，大概率会直接卡死在那里**

1. 更新

   ```
   apt update -y && apt upgrade -y
   ```

2. 使用`apt-fast`下载（apt-fast 快很多，不然大半个小时）

   **挑一个，不要装多个，会出问题的**

   推荐 gnome，kde 卡卡的，不知道为什么

   <details>
     <summary>选择1（推荐）：安装gnome</summary>
     
     推荐这个，起码50hz，接近60hz

   ```
   apt-fast install ubuntu-desktop gnome -y && apt install ubuntu-desktop gnome -y
   ```

   </details>

   <details>
     <summary>选择2：安装kde</summary>

   不要用 kde-desktop 包安装，kde-desktop 会扫描全部磁盘（包括/mnt 下的 window 磁盘），导致卡死在"Initializing plocate database; this may take some time..."

   ```
   apt-fast install kde-full -y
   ```

   </details>

   <details>
     <summary>选择3：安装xfce4</summary>

   ```
   apt-fast install xfce4 -y
   ```

   </details>

   不需要往 ~/.xinitrc 或 ~/.xsession 或 ~/.xsessionrc 写任何东西，如果出了问题，可以把这三个文件删了。
   [~/.xinitrc 与 ~/.xsession 与 ~/.xsessionrc 的作用与区别](https://qastack.cn/unix/281858/difference-between-xinitrc-xsession-and-xsessionrc)。

### 2.1.5. 重装 dbus

必须重装，不然会不幸（su -l 进入 root）

不重装，进不了 **软件与更新** 这个应用界面

```
apt install --reinstall dbus
```

下面的命令**不要执行**，我只是记录一下（这玩意权限是`-rwsr-xr--`就是对的）

<details>
  <summary>不要执行我</summary>
  <pre><code>sudo chmod 4754 /usr/lib/dbus-1.0/dbus-daemon-launch-helper
sudo chown root:messagebus /usr/lib/dbus-1.0/dbus-daemon-launch-helper</code></pre>
</details>

### 2.1.6. 修复网络

这文件就是空的，不过这么干能把网络图标给搞回来。。。

而且不搞这个 **软件与更新** 还是进不去，说识别不到网络。。。

```
touch /etc/NetworkManager/conf.d/10-globally-managed-devices.conf
```

## 2.2. 完善处理

### 2.2.1. 中文化

[这里有写](https://github.com/gf9276/MdFiles/blob/master/linux.md)

中文化之后，如果再进入远程连接，如果他让你选是否保留文件名字，选择保留英文的。如果文件夹名字换成中文的以后会很麻烦的。

### 2.2.2. 安装谷歌拼音

[这里有写](https://github.com/gf9276/MdFiles/blob/master/linux.md)

在 wslg 下有以下缺点

1. 功能不全
2. gedit 下拼音提示界面跑来跑去的

完整界面就没这个问题了

### 2.2.3. 显卡问题

直接命令行 `ln -s /usr/lib/wsl/lib/nvidia-smi /usr/bin/nvidia-smi` 显卡，这样就可以正常识别`nvidia-smi`命令了，因为正常显卡驱动就是在后面那个路径下面的。

# 3. window 端安装 nomachine

直接下载.exe 安装就行

![](https://cdn.jsdelivr.net/gh/gf9276/image/nomachine/0.png)

装好之后两个图标，按哪个都可以进入，其实应该按 NoMachine 那个

![](https://cdn.jsdelivr.net/gh/gf9276/image/nomachine/2.png)

用法直接看[说明文档](https://kb.nomachine.com/DT07S00236?s=minimize%20the%20windows)，这里记录几个设置

![](https://cdn.jsdelivr.net/gh/gf9276/image/nomachine/3.png)

## 3.1. windows 作为服务端的 server 设置

直接把所有 server 关了，反正连接 wsl，windows 不做服务端。其他都不用改

![](https://cdn.jsdelivr.net/gh/gf9276/image/nomachine/20230815211220.png)

## 3.2. windows 作为客户端的 player 设置

改几个默认快捷键就行（为了防止快捷键冲突，所以没用的最好关了，有用的改的复杂一点）

![](https://cdn.jsdelivr.net/gh/gf9276/image/nomachine/20230815211330.png)

![](https://cdn.jsdelivr.net/gh/gf9276/image/nomachine/20230815211611.png)

# 4. ubuntu 安装

ubuntu 肯定是.deb 啊

[速达链接](https://downloads.nomachine.com/download/?id=3)

![](https://cdn.jsdelivr.net/gh/gf9276/image/nomachine/20230815213009.png)

点进去后都写着呢: `sudo dpkg -i nomachine_8.8.1_1_amd64.deb`

![](https://cdn.jsdelivr.net/gh/gf9276/image/nomachine/20230815213049.png)

装完都是开机默认自动启动的，不用管，自启动才好

## 4.1. ubuntu 作为服务端 server 设置

关掉 server，然后把端口改成 4001，防止和 windows 打起来（好像不改也不会咋样），其他默认。

可以用 wslg 打开（装好 nomachine 后，window 的开始菜单里会出现的，直接点一下就可以打开了）

![](https://cdn.jsdelivr.net/gh/gf9276/image/nomachine/20230816134751.png)

![](https://cdn.jsdelivr.net/gh/gf9276/image/nomachine/20230815213214.png)

# 5. 连接

![](https://cdn.jsdelivr.net/gh/gf9276/image/nomachine/20230815211913.png)

![](https://cdn.jsdelivr.net/gh/gf9276/image/nomachine/20230815212024.png)

然后一直 ok 就行

进入界面长这样子，点一下右上角会进入设置模式（或者 ctrl alt 0）

![](https://cdn.jsdelivr.net/gh/gf9276/image/nomachine/20230815212206.png)

## 5.1. 屏幕设置

### 5.1.1. 按钮介绍

![](https://cdn.jsdelivr.net/gh/gf9276/image/nomachine/20230815212250.png)

**从左到右，分别是：**

1. viewpoint mode

   ![](https://cdn.jsdelivr.net/gh/gf9276/image/nomachine/s0.png)

2. scale to window

   ![](https://cdn.jsdelivr.net/gh/gf9276/image/nomachine/s1.png)

3. resize remote display

   ![](https://cdn.jsdelivr.net/gh/gf9276/image/nomachine/s2.png)

4. fullscreen 全屏

5. fullscreen on all monitors 拼接所有屏幕

6. iconize 缩小为图标

7. display settings 屏幕设置

### 5.1.2. display settings

质量设置为最佳，wslg 难道还会缺网速？？？

![](https://cdn.jsdelivr.net/gh/gf9276/image/nomachine/20230815214357.png)
