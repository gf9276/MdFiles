
<!-- TOC -->

- [1. IDEA.md](#1-ideamd)
- [2. windows下idea的设置](#2-windows下idea的设置)
  - [2.1. 安装](#21-安装)
  - [2.2. wsl连接](#22-wsl连接)
  - [2.3. 删除](#23-删除)
- [3. ubuntu内IDEA的安装、破解](#3-ubuntu内idea的安装破解)
  - [3.1. 安装+破解——idea 2022.3.3](#31-安装破解idea-202233)
    - [3.1.1. 这里是下载](#311-这里是下载)
    - [3.1.2. 这里是破解](#312-这里是破解)
  - [3.2. 安装+破解——idea 2020.1.1](#32-安装破解idea-202011)
  - [3.3. 输入法在左下角的bug（官方的2023.3版本将会解决）](#33-输入法在左下角的bug官方的20233版本将会解决)
  - [3.4. 卸载idea](#34-卸载idea)
  - [3.5. 创建桌面快捷方式](#35-创建桌面快捷方式)
    - [3.5.1. 有完整桌面](#351-有完整桌面)
    - [3.5.2. 如果你用的是wslg](#352-如果你用的是wslg)
- [4. 设置](#4-设置)
  - [4.1. 安装插件](#41-安装插件)
  - [4.2. settings](#42-settings)

<!-- /TOC -->


# 1. IDEA.md

idea 安装+破解+一些设置

1. 项目目前用的是jdk8，wsl有三种方案：a. windows下安装idea并调用wsl里的jdk; b. wsl里装idea, 使用wslg进行开发; c. wsl里装idea和桌面, 使用xrdp或者nomachine远程连接桌面进行开发。兜兜转转我还是回到了a。

2. windows里的idea调用wsl里的jdk8，需要wsl里另外装有至少11或以上版本的jdk，建议17。

3. 如果windows里的idea无法调用wsl里的jdk8，检查windows defender是否添加wsl排除项和idea进程排除项。（idea版本为2023.2.1）

4. 如果windows里的idea打开项目卡在什么detecting jdk，或者什么扫描maven库啊，不用怀疑，百分之一百因为你没有把wsl加入病毒扫描排除项。

# 2. windows下idea的设置

## 2.1. 安装

安装直接装就行了，安装完先别打开，加个排除项先，不然会一直detect jdk，直接卡死。

进程就在你安装的idea目录下，我的目录是这个

![](https://cdn.jsdelivr.net/gh/gf9276/image/java/20230314121424.png)

## 2.2. wsl连接

**<font size=7>还要把wsl加入排除项</font>**

1. 方法一：添加白名单是绝对可行的

![](https://cdn.jsdelivr.net/gh/gf9276/image/wsl2/QQ图片20230826233143.png)

2. 方法二：我试了，这个没用，还是添加白名单好

![](https://cdn.jsdelivr.net/gh/gf9276/image/wsl2/20230827142321.png)

![](https://cdn.jsdelivr.net/gh/gf9276/image/wsl2/v2-50442dee852a321fdc44b0ac407a91dc.png)

## 2.3. 删除

[参考连接](https://www.quanxiaoha.com/idea/uninstall-idea.html)

如果是通过jetbrain toolbox装的，注意这些目录

*如果你想删除 IDEA 相关，则只需要删除 JetBrains 目录下包含 IDEA 的文件夹即可*

1. C:\用户\${用户名称}\IdeaProjects\
2. C:\用户\${用户名称}\AppData\Roaming\JetBrains
3. C:\用户\${用户名称}\AppData\Local\JetBrains
4. C:\Users\${用户名称}\AppData\Local\JetBrains\Toolbox\apps
5. C:\用户\公用\.jetbrains
6. C:\Program Files\JetBrains （软件本体）
7. C:\ProgramData\Microsoft\Windows\Start Menu\Programs\JetBrains\ （快捷方式）


# 3. ubuntu内IDEA的安装、破解

安装和破解我搜集了两个版本的（2020版本的会简单很多，但是太老旧了）。

虽然我自己用的是教育优惠，用不上破解就是了。

安装很简单，直接去官网下载压缩包，解压然后到他那个文件夹的bin/目录下执行 idea.sh 脚本就行。

因为他不是.deb包，可以用dpkg -i 安装的那种，所以，如果你缺依赖的话，他不会自己给你补全，会直接报错的。**所以，如果你用的是wsl，你需要先多装几个桌面应用，把依赖补全了先。**

## 3.1. 安装+破解——idea 2022.3.3

[找来找去，只有这一种可用](https://www.quanxiaoha.com/article/idea-jihuoma.html)

注意，由于注册信息写在个人用户下，所以，如果你想要用root用户打开idea的话，你得用root账户再注册一次（一般也不会用root打开idea吧，这很不安全）

### 3.1.1. 这里是下载

* 下载idea
  
  找到安装包，直接下载

  ![](https://cdn.jsdelivr.net/gh/gf9276/image/linux/20230520104020.png)

* 压缩包挪到 /opt 目录下

* 解压缩并删除压缩包

      tar -zxvf ideaIU-2022.3.3.tar.gz && rm ideaIU-2022.3.3.tar.gz

* 打开idea（**建议先破解完再打开**）

    路径这里，换成你自己的版本名字

    ```
    cd /opt/idea-IU-231.9011.34/bin && ./idea.sh
    ```

### 3.1.2. 这里是破解

* 下载破解文件
  
    ![](https://cdn.jsdelivr.net/gh/gf9276/image/linux/20230520104520.png)

    ![](https://cdn.jsdelivr.net/gh/gf9276/image/linux/20230520104606.png)

* 把这个 `je-netilter` 挪到/opt下

* 修改文件 `idea64.vmoption`

    ```
    cd /opt/idea-IU-223.8836.41/bin/
    ```
    ```
    vim idea64.vmoptions
    ```

    在最后一行写上（路径换成你自己的）

    ```
    # 引用补丁，开头必须以 -javaagent: 开头，后面跟着补丁的绝对路径（可根据你实际的位置进行修改）,注意路径一定要填写正确，且不能包含中文，否则会导致 IDEA 无法启动
    -javaagent:/opt/ja-netfilter/ja-netfilter.jar

    # 最新 IDEA 版本需要添加下面两行，以支持 Java 17, 否则会报 Key is invalid
    --add-opens=java.base/jdk.internal.org.objectweb.asm=ALL-UNNAMED
    --add-opens=java.base/jdk.internal.org.objectweb.asm.tree=ALL-UNNAMED
    ```

    ![](https://cdn.jsdelivr.net/gh/gf9276/image/linux/20230520104946.png)

* 回到普通用户（如果你现在是root），打开idea

    ```
    ctrl+d
    ```
    ```
    cd /opt/idea-IU-223.8836.41/bin/
    ```
    ```
    ./idea.sh
    ```

* 粘贴激活码，（直接复制下面的）

    ![](https://cdn.jsdelivr.net/gh/gf9276/image/linux/20230520105214.png)

  ```
  6G5NXCPJZB-eyJsaWNlbnNlSWQiOiI2RzVOWENQSlpCIiwibGljZW5zZWVOYW1lIjoic2lnbnVwIHNjb290ZXIiLCJhc3NpZ25lZU5hbWUiOiIiLCJhc3NpZ25lZUVtYWlsIjoiIiwibGljZW5zZVJlc3RyaWN0aW9uIjoiIiwiY2hlY2tDb25jdXJyZW50VXNlIjpmYWxzZSwicHJvZHVjdHMiOlt7ImNvZGUiOiJQU0kiLCJmYWxsYmFja0RhdGUiOiIyMDI1LTA4LTAxIiwicGFpZFVwVG8iOiIyMDI1LTA4LTAxIiwiZXh0ZW5kZWQiOnRydWV9LHsiY29kZSI6IlBEQiIsImZhbGxiYWNrRGF0ZSI6IjIwMjUtMDgtMDEiLCJwYWlkVXBUbyI6IjIwMjUtMDgtMDEiLCJleHRlbmRlZCI6dHJ1ZX0seyJjb2RlIjoiSUkiLCJmYWxsYmFja0RhdGUiOiIyMDI1LTA4LTAxIiwicGFpZFVwVG8iOiIyMDI1LTA4LTAxIiwiZXh0ZW5kZWQiOmZhbHNlfSx7ImNvZGUiOiJQUEMiLCJmYWxsYmFja0RhdGUiOiIyMDI1LTA4LTAxIiwicGFpZFVwVG8iOiIyMDI1LTA4LTAxIiwiZXh0ZW5kZWQiOnRydWV9LHsiY29kZSI6IlBHTyIsImZhbGxiYWNrRGF0ZSI6IjIwMjUtMDgtMDEiLCJwYWlkVXBUbyI6IjIwMjUtMDgtMDEiLCJleHRlbmRlZCI6dHJ1ZX0seyJjb2RlIjoiUFNXIiwiZmFsbGJhY2tEYXRlIjoiMjAyNS0wOC0wMSIsInBhaWRVcFRvIjoiMjAyNS0wOC0wMSIsImV4dGVuZGVkIjp0cnVlfSx7ImNvZGUiOiJQV1MiLCJmYWxsYmFja0RhdGUiOiIyMDI1LTA4LTAxIiwicGFpZFVwVG8iOiIyMDI1LTA4LTAxIiwiZXh0ZW5kZWQiOnRydWV9LHsiY29kZSI6IlBQUyIsImZhbGxiYWNrRGF0ZSI6IjIwMjUtMDgtMDEiLCJwYWlkVXBUbyI6IjIwMjUtMDgtMDEiLCJleHRlbmRlZCI6dHJ1ZX0seyJjb2RlIjoiUFJCIiwiZmFsbGJhY2tEYXRlIjoiMjAyNS0wOC0wMSIsInBhaWRVcFRvIjoiMjAyNS0wOC0wMSIsImV4dGVuZGVkIjp0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
  ```

  
## 3.2. 安装+破解——idea 2020.1.1

这个是老旧版本的，这个破解起来很方便就是了，如果wslg不能拖动的话，直接去插件那里，打开文件导入就行

[2020版的安装和破解过程，看这个就行](https://www.jianshu.com/p/5f2cd3bfb9a4)

[安装大致过，多一个参考一下](https://blog.csdn.net/weixin_46988707/article/details/127568436)


## 3.3. 输入法在左下角的bug（官方的2023.3版本将会解决）

idea在ubuntu下面会有一个bug，就是输入法在左下角，这个bug10年前就有，他就是不修，下面是通过修改jbr解决这个问题，**我建议不要用，会有其他bug出现的**，反正官网说近期会解决（2023年6月14日留）。

[deepin社区讨论情况](https://bbs.deepin.org/post/252575)

[官网问题反馈情况](https://youtrack.jetbrains.com/issue/JBR-2460)

idea 13年就存在的问题 23年还没解决捏

<font size=4 >解决办法：</font> 下载大佬编译好的jbr，替换idea安装目录下本来的jbr

<font size=4 >注意：</font> 该办法虽然修好了输入法，但是整个idea会变态起来。。。

* 下载jbr压缩文件
  
  [下载链接](https://github.com/RikudouPatrickstar/JetBrainsRuntime-for-Linux-x64/releases)

  ![](https://cdn.jsdelivr.net/gh/gf9276/image/linux/20230519200529.png)

  指令下载（慢的话换换代理，重新开始下载记得把原来的先删除掉）：
  
  ```
  cd /tmp
  ```
  ```
  wget https://github.com/RikudouPatrickstar/JetBrainsRuntime-for-Linux-x64/releases/download/jbr-release-17.0.6b829.5/jbr_jcef-17.0.6-linux-x64-b829.5.tar.gz
  ```
  ```
  tar -zxvf jbr_jcef-17.0.6-linux-x64-b829.5.tar.gz
  ```


* 放置到idea安装目录下

  ```
  cd /opt/idea-IU-223.8836.41
  ```

  ![](https://cdn.jsdelivr.net/gh/gf9276/image/linux/20230519200642.png)

  ```
  rm -rf jbr/
  ```

  ```
  mv /tmp/jbr_jcef-17.0.6-linux-x64-b829.5 jbr
  ```

  ![](https://cdn.jsdelivr.net/gh/gf9276/image/linux/20230519201419.png)


## 3.4. 卸载idea

主要是本体和一些个人配置

除了安装目录，还要删除以下

    ~/.config/JetBrains/<product><version>
    ~/.cache/JetBrains/<product><version>
    ~/.local/share/JetBrains/<product><version>
    ~/.gnome/apps/jetbrains-idea.desktop
    ~/.local/share/applications/jetbrains-idea.desktop

还有目录 `/usr/share/applications` 与idea相关的快捷方式

其实在个人目录下用`find`指令找一下和idea有关的文件就知道就知道

  ![](https://cdn.jsdelivr.net/gh/gf9276/image/linux/20230520102336.png)

## 3.5. 创建桌面快捷方式

分两种情况

### 3.5.1. 有完整桌面

如果你有完整的linux界面，如gnome、xfce4、KDE之类的，这个很方便，打开你的idea，直接选这个create desktop entry就行

![](https://cdn.jsdelivr.net/gh/gf9276/image/IDEA/20230614180643.png)

![](https://cdn.jsdelivr.net/gh/gf9276/image/IDEA/20230614180736.png)

### 3.5.2. 如果你用的是wslg

那就得手动生成

```
cd /usr/share/applications
```
```
vim jetbrains-idea.desktop
```

写入以下内容（这个是官方生成的样式，Icon那里因为win无法识别.svg格式的图标，所以我改成了.png）

wslg官方在`WSLg 版本： 1.0.54`解决了无法识别svg格式图片的问题 --> guof, add in 2023/8/27

```
[Desktop Entry]
Version=1.0
Type=Application
Name=IntelliJ IDEA Ultimate Edition
Icon=/opt/idea-IU-223.8836.41/bin/idea.png
Exec="/opt/idea-IU-223.8836.41/bin/idea.sh" %f
Comment=Capable and Ergonomic IDE for JVM
Categories=Development;IDE;
Terminal=false
StartupWMClass=jetbrains-idea
StartupNotify=true
```

# 4. 设置

有些东西要设置一下

## 4.1. 安装插件

直接插件商店搜索就行

1. CodeGlance Pro
2. atom icon
3. MyBatisX

## 4.2. settings

搬运一下尚硅谷的，我只放了对我有用的

![](https://cdn.jsdelivr.net/gh/gf9276/image/IDEA/20230614181930.png)

![](https://cdn.jsdelivr.net/gh/gf9276/image/IDEA/20230614182031.png)
