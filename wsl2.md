<!-- TOC -->

- [1. wsl2.md](#1-wsl2md)
- [2. 安装 wsl](#2-安装-wsl)
  - [2.1. 第一步，启动wsl](#21-第一步启动wsl)
  - [2.2. 第二步，启动虚拟机平台、重启、下载内核包](#22-第二步启动虚拟机平台重启下载内核包)
  - [2.3. 第三步，设置 WSL 2 为默认值](#23-第三步设置-wsl-2-为默认值)
  - [2.4. 第四步，去微软商店下载一个wsl](#24-第四步去微软商店下载一个wsl)
  - [2.5. 第五步，安装一个 Linux 发行版](#25-第五步安装一个-linux-发行版)
  - [2.6. 第六步，直接安装 windows terminal](#26-第六步直接安装-windows-terminal)
- [3. wsl指令与配置](#3-wsl指令与配置)
  - [3.1. 挪动到其他盘](#31-挪动到其他盘)
  - [3.2. 退出](#32-退出)
  - [3.3. 设置默认登录用户](#33-设置默认登录用户)
- [4. wslg](#4-wslg)
  - [4.1. 安装](#41-安装)
  - [4.2. wslg效果](#42-wslg效果)
  - [4.3. wslg缺点](#43-wslg缺点)
- [5. xrdp远程桌面](#5-xrdp远程桌面)
  - [5.1. 安装](#51-安装)
    - [5.1.1. 安装apt-fast](#511-安装apt-fast)
    - [5.1.2. 设置snap代理](#512-设置snap代理)
    - [5.1.3. 锁住！](#513-锁住)
    - [5.1.4. 安装gnome](#514-安装gnome)
    - [5.1.5. 安装xrdp](#515-安装xrdp)
    - [5.1.6. 重装dbus](#516-重装dbus)
    - [5.1.7. 修复网络](#517-修复网络)
    - [5.1.8. 重启并远程连接](#518-重启并远程连接)
  - [5.2. 完善处理](#52-完善处理)
    - [5.2.1. 中文化](#521-中文化)
    - [5.2.2. 安装谷歌拼音](#522-安装谷歌拼音)
    - [5.2.3. 显卡问题](#523-显卡问题)
    - [5.2.4. todo](#524-todo)
- [6. 坑](#6-坑)
  - [6.1. wslg代理问题](#61-wslg代理问题)
  - [6.2. wslg无法复制](#62-wslg无法复制)
  - [6.3. wslg下idea报错](#63-wslg下idea报错)
  - [6.4. Error code: Wsl/Service/0x8007273d](#64-error-code-wslservice0x8007273d)
    - [6.4.1. 解决方法1](#641-解决方法1)
    - [6.4.2. 解决方法2](#642-解决方法2)
    - [6.4.3. 最好用的方法](#643-最好用的方法)

<!-- /TOC -->

# 1. wsl2.md

记录 wsl2 的操作，包括软件安装、UI之类的

**其实大部分ubuntu普通发行版也是通用的**

[官方文档](https://learn.microsoft.com/zh-cn/windows/wsl/)

**<font color=#D81D4F > 官方文档很好用的，大部分直接看官方文档就行了 </font>**

# 2. 安装 wsl

**这是旧版本的手动安装，新版本直接```wsl install```就好了，不过要考虑到网的问题（翻墙+微软商店UWP网络回环），所以还是手动好了**

* [参考链接1-bilibili](https://www.bilibili.com/read/cv9561666)
* [参考链接2-博客园](https://www.cnblogs.com/ittranslator/p/14128570.html)
* [参考链接3-CSDN](https://blog.csdn.net/li1325169021/article/details/124285018)
  
## 2.1. 第一步，启动wsl
不管您想要使用哪个版本的 WSL，都首先需要启用它。为此，请以**管理员身份**打开 **PowerShell** 工具并运行以下命令。小心不要在命令中输入错误或遗漏任何字符：
```
dism.exe /online /enable-feature /featurename:Microsoft-Windows-Subsystem-Linux /all /norestart
```

## 2.2. 第二步，启动虚拟机平台、重启、下载内核包
WSL 2 需要启用 Windows 10 的 “虚拟机平台” 特性。它独立于 Hyper-V，并提供了一些在 Linux 的 Windows 子系统新版本中可用的更有趣的平台集成。
要在 Windows 10（2004）上启用虚拟机平台，请以 **管理员身份打开 PowerShell** 并运行：
```
dism.exe /online /enable-feature /featurename:VirtualMachinePlatform /all /norestart
```

为了确保所有相关部件都整齐到位，您应该在此时 **重启系统**，否则可能会发现事情没按预期进行。

然后一般来说，需要下载 Linux 内核更新包（适用于 x64 计算机的 WSL2 Linux 内核更新包）

* [官方问题提出路径](https://learn.microsoft.com/zh-cn/windows/wsl/troubleshooting)
* [具体下载路径](https://wslstorestorage.blob.core.windows.net/wslblob/wsl_update_x64.msi)

## 2.3. 第三步，设置 WSL 2 为默认值

以 **管理员身份打开 PowerShell**，然后运行以下命令以将 WSL 2 设置为 WSL 的默认版本：
```
wsl --set-default-version 2
```

## 2.4. 第四步，去微软商店下载一个wsl

这是我后来加的。

新版本的支持wslg和systemctl，所以下一个还是很有必要的。

但是直接下载不行，你得把前面的几步都给完成了先。

![](https://cdn.jsdelivr.net/gh/gf9276/image/wsl2/20230614200024.png)


## 2.5. 第五步，安装一个 Linux 发行版

直接去微软商店搜就行了，Ubuntu 22.04。
然后直接打开就行，按照他的提示，输入用户名和密码，有点奇怪，有时候是黑色框，有时候出来彩色的，emmm。不过都一样的。彩色的和下面的一样：

![](https://cdn.jsdelivr.net/gh/gf9276/image/wsl2/20221109193951.png)

如果想挪动，底下有写怎么挪动


## 2.6. 第六步，直接安装 windows terminal

他会自动识别到你安装的ubuntu发行版的

美化看 **oh-my-posh.md**

# 3. wsl指令与配置

## 3.1. 挪动到其他盘

发行版和文件夹名字改成自己的

* 查看已安装的子系统版本
    ```
    wsl -l -v
    ```
* 关闭wsl
    ```
    wsl --shutdown
    ```
* 导出分发版为tar文件到D盘
    ```
    wsl --export Ubuntu-22.04 d:\wsl-ubuntu-22.04.tar
    ```
* 注销当前分发版
    ```
    wsl --unregister Ubuntu-22.04
    ```
* 重新导入并安装wsl在D盘
    ```
    wsl --import Ubuntu-22.04 d:\wsl-ubuntu-22.04 d:\wsl-ubuntu-22.04.tar --version 2
    ```
* 设置默认登陆用户为安装时用户名
    ```
    ubuntu2204 config --default-user guof
    ```
* 删除tar文件
    ```
    del d:\wsl-ubuntu-22.04.tar
    ```

## 3.2. 退出

```
exit
```

或者

```
ctrl+d
```

## 3.3. 设置默认登录用户

```
echo -e "[user]\ndefault=guof" >> /etc/wsl.conf
```

参考这两个

[导入任何Linux分发版](https://learn.microsoft.com/zh-cn/windows/wsl/use-custom-distro)

[高级设置配置](https://learn.microsoft.com/zh-cn/windows/wsl/wsl-config#wslconf)（主要是看wsl.conf里的用户设置[user]）

所以ubuntu2204.exe是什么？这玩意可以直接在powershell里设置，我怀疑是微软下载的软件自带的。这个的权限高于```/etc/wsl.conf```


# 4. wslg

想要wsl的可视化界面，有两种方法，一种是使用官方的wslg，另外一种是安装完整的桌面系统，然后使用windows远程桌面或者vcxsrv远程连接。

wslg的话，直接看这里就好了 ———— [链接](https://learn.microsoft.com/zh-cn/windows/wsl/tutorials/gui-apps)

注意：为了防止后面安装其他软件，比如idea的时候出现什么依赖缺失的bug，建议把他这里提到的东西都给装了，能补全大部分的环境依赖。当然最好的还是直接装一个gnome、xfce4之类的了（很大就是了）。

## 4.1. 安装

这东西直接可以用，但是我还是推荐把这些应用都装一下，完善依赖

安装官网推荐的全套应用指令如下

```
apt -y update
```

```
apt -y install gedit gimp nautilus vlc x11-apps 
```

打开会有警告，无伤大雅
![](https://cdn.jsdelivr.net/gh/gf9276/image/linux/20230331205033.png)

再安装Google Chrome

[参考](https://learn.microsoft.com/zh-cn/windows/wsl/tutorials/gui-apps)

1. 将目录更改为 temp 文件夹：`cd /tmp`
2. 使用 wget 下载它：`wget https://dl.google.com/linux/direct/google-chrome-stable_current_amd64.deb`
3. 获取当前稳定版本：`dpkg -i google-chrome-stable_current_amd64.deb`
4. 修复包：`apt install --fix-broken -y`
5. 配置包：`dpkg -i google-chrome-stable_current_amd64.deb`

若要启动，请输入：`google-chrome`

## 4.2. wslg效果

如果需要中文化或者安装谷歌拼音什么的直接看[这里](https://github.com/gf9276/MdFiles/blob/master/linux.md)就行

这玩意的效果是这样的，就是在windows里面嵌入一个ubuntu的应用，挺有意思的。

![](https://cdn.jsdelivr.net/gh/gf9276/image/wsl2/20230614201317.png)

## 4.3. wslg缺点

1. 复制有问题，复制内容过长，会导致无法从win复制东西到wslg里面。我提了issues，也没管，日了。这问题去微软在github上的wslg项目里搜搜就知道了，一大堆，多少年了，微软就这水平，我呸。
   
   [问题链接](https://github.com/microsoft/wslg/issues/15)

   [问题链接2](https://github.com/microsoft/wslg/issues/1066)

2. IDEA有时候会崩溃，就是debug的时候，突然动不了了，这时候你得`wsl --shutdown`重启wsl，很影响我的体验啊

3. 依赖问题，有时候不是用apt或者.deb包安装的应用，不会自动补全依赖，运行界面会报错的。所以最好还是装一个gnome或者xfc4，这种ubuntu桌面能把99%的依赖补全咯。

# 5. xrdp远程桌面

其实远程桌面有两种，一种是vnc协议的，这种要在win里下载一个 vcxsrc 软件，然后连接wsl，这种方法其实挺流畅的，但是我实际用下来还是有点bug，比如黑屏、卡顿、崩溃，最重要的是在里面快捷键会和windows冲突，难绷。第二种是使用Xrdp的，xrdp是基于微软rdp的一种开源实现，ubuntu安装xrdp就可以直接用windows的远程桌面连接ubuntu了，流畅度会稍微差一点，但是用起来很稳定，非常不错。

这一块主要是参考[这里](https://github.com/microsoft/WSL/discussions/9350#discussioncomment-6119188)和[这里](https://github.com/microsoft/WSL/issues/3344#issuecomment-1589103251)，没错就是我，哼哼

## 5.1. 安装

### 5.1.1. 安装apt-fast

apt-fast 下载更快，不然安装gnome要一个小时

```
add-apt-repository ppa:apt-fast/stable && apt-get install apt-fast -y
```

### 5.1.2. 设置snap代理

22.04火狐和软件商店用的都是snap，然鹅这玩意不会读取全局代理，所以得自己设置

换成你自己的代理ip，分开执行（下面那个也是没有s的）
```
snap set system proxy.http="http://172.22.160.1:7890"

snap set system proxy.https="http://172.22.160.1:7890"
```

### 5.1.3. 锁住！

安装gnome前，锁住三个东西，避免变得不幸。如果你安装了，直接apt purge -y 删掉他们

```
apt-mark hold acpid acpi-support modemmanager
```

acpid是管理电源的，systemctl检测到容器（就是wsl2）会因为这玩意卡死的

[参考链接](https://github.com/microsoft/WSL/discussions/9350#discussioncomment-6119188)

modemmanager是管理网络的，也是检测到容器网络设置直接卡死

[参考链接](https://forum.ubuntu.com.cn/viewtopic.php?t=490583)

### 5.1.4. 安装gnome

**安装snap-firefox会卡在那里一会儿，不要惊讶**

先更新

```
apt update -y && apt upgrade -y
```

再下载（apt-fast快很多，不然大半个小时）

```
apt-fast install ubuntu-desktop gnome -y
```

执行完，再执行这个，看看有没有漏的

```
apt install ubuntu-desktop gnome -y
```

### 5.1.5. 安装xrdp

**远程连接.md 里面也有写**

1. 安装

    安装xrdp
    ```
    apt install xrdp
    ```
    换端口，防止和win打架，这是匹配更换指令，把3389字符串改成3390
    ```
    sed -i 's/3389/3390/g' /etc/xrdp/xrdp.ini
    ```
    重启服务
    ```
    systemctl restart xrdp
    ```

2. 设置

    写入信息（切回普通用户）
    ```
    vim ~/.xsessionrc
    ```
    内容如下
    ```
    export GNOME_SHELL_SESSION_MODE=ubuntu
    export XDG_CURRENT_DESKTOP=ubuntu:GNOME
    export XDG_DATA_DIRS=/usr/share/ubuntu:/usr/local/share:/usr/share:/var/lib/snapd/desktop
    export WAYLAND_DISPLAY=
    export XDG_CONFIG_DIRS=/etc/xdg/xdg-ubuntu:/etc/xdg
    ```

### 5.1.6. 重装dbus

必须重装，不然会不幸（su -l变回root）

不重装，进不了 **软件与更新** 这个应用界面

```
apt install --reinstall dbus
```

下面的**不要执行**，我只是记录一下（这玩意权限是`-rwsr-xr--`就是对的）
```
sudo chmod 4754 /usr/lib/dbus-1.0/dbus-daemon-launch-helper
sudo chown root:messagebus /usr/lib/dbus-1.0/dbus-daemon-launch-helper
```

### 5.1.7. 修复网络

这文件就是空的，不过这么干能把网络图标给搞回来。。。

而且不搞这个 **软件与更新** 还是进不去，说识别不到网络。。。

```
touch /etc/NetworkManager/conf.d/10-globally-managed-devices.conf
```

### 5.1.8. 重启并远程连接

想要优化的话，看远程连接.md

测试了一下打开设置

打开软件与更新

https://www.testufo.com/ 显示帧率59（实际估计只有40）

一切正常

www.google.com

www.youtube.com

www.github.com

也都连接正常

最后效果是这样子，完美

![](https://cdn.jsdelivr.net/gh/gf9276/image/wsl2/20230614203353.png)

![](https://cdn.jsdelivr.net/gh/gf9276/image/wsl2/20230614203415.png)


## 5.2. 完善处理

### 5.2.1. 中文化

[这里有写](https://github.com/gf9276/MdFiles/blob/master/linux.md)

中文化之后，如果再进入远程连接，他让你选是否保留文件名字，选择保留英文的。如果文件夹名字换成中文的以后会很麻烦的。

### 5.2.2. 安装谷歌拼音

[这里有写](https://github.com/gf9276/MdFiles/blob/master/linux.md)

在wslg下有以下缺点

1. 功能不全
2. gedit下拼音提示界面跑来跑去的

完整界面就没这个问题了


### 5.2.3. 显卡问题

直接命令行 `ln -s /usr/lib/wsl/lib/nvidia-smi /usr/bin/nvidia-smi` 显卡，这样就可以正常识别`nvidia-smi`命令了，因为正常显卡驱动就是在后面那个路径下面的。

### 5.2.4. todo

然后就可以随便玩了，安装vscode，安装idea之类的和普通的ubuntu没有一点区别。

# 6. 坑

## 6.1. wslg代理问题

* 要把代理写入profile里，不然浏览器用不了

## 6.2. wslg无法复制

解决不了，md

[问题链接](https://github.com/microsoft/wslg/issues/15)

[问题链接2](https://github.com/microsoft/wslg/issues/1066)

## 6.3. wslg下idea报错

多半是依赖不全，多装一点桌面应用没准就补全了。

## 6.4. Error code: Wsl/Service/0x8007273d

这种就没法用了

![](https://cdn.jsdelivr.net/gh/gf9276/image/wsl2/L%]TR~I6_K`0NR98OO]YWBQ.png)

### 6.4.1. 解决方法1

管理员权限打开 powershell

执行命令
```
netsh winsock reset
```

然后重新打开wsl2

### 6.4.2. 解决方法2

这种方法我用不了

下载 

[http://file2.happyjava.cn/NoLsp.exe](http://file2.happyjava.cn/NoLsp.exe)


放到 ```C:\windows\system32``` 目录下，执行命令

```
.\NoLsp.exe C:\windows\system32\wsl.exe
```

其中 ```C:\windows\system32\wsl.exe```对应```wsl.exe```在的目录

但是这个方法我用不了

### 6.4.3. 最好用的方法

不如重新下载，然后再导入分发包（记得提前导出）

[github问题地址](https://github.com/microsoft/WSL/issues/9331)

* in Add or remove programs: uninstall first "Windows Subsystem for Linux Update" and then "Windows Subsystem for Linux"
* try to start your distribution again and it will point you to: https://aka.ms/wsl2kernel
* from there download the wsl_update_x64.msi file and run it , install the WSL kernel
* start your distribution again
