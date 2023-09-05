<!-- TOC -->

- [1. linux.md](#1-linuxmd)
- [2. 知识](#2-知识)
  - [2.1. 文件](#21-文件)
    - [2.1.1. 举个例子](#211-举个例子)
- [3. 指令、操作和坑（ubuntu）](#3-指令操作和坑ubuntu)
  - [3.1. 代理大坑](#31-代理大坑)
  - [3.2. 修改密码](#32-修改密码)
  - [3.3. apt指令介绍](#33-apt指令介绍)
  - [3.4. 安装apt-fast](#34-安装apt-fast)
  - [3.5. 设置snap代理](#35-设置snap代理)
  - [3.6. 上下键补全历史指令](#36-上下键补全历史指令)
  - [3.7. nload](#37-nload)
  - [3.8. 复制命令](#38-复制命令)
  - [3.9. 移动命令](#39-移动命令)
  - [3.10. 查看文件夹下的文件数](#310-查看文件夹下的文件数)
  - [3.11. find 指令](#311-find-指令)
  - [3.12. whereis which](#312-whereis-which)
  - [3.13. nohup](#313-nohup)
  - [3.14. 关于wsl2可视化界面](#314-关于wsl2可视化界面)
  - [3.15. 中文化配置](#315-中文化配置)
  - [3.16. 安装google-pinyin](#316-安装google-pinyin)
- [4. shell脚本（ubuntu）](#4-shell脚本ubuntu)
  - [4.1. 判断格式](#41-判断格式)
  - [4.2. 常见的判断](#42-常见的判断)
  - [4.3. 函数 与 ifelse](#43-函数-与-ifelse)
  - [4.4. sh脚本输入设置默认参数](#44-sh脚本输入设置默认参数)
  - [4.5. echo 变色](#45-echo-变色)
  - [4.6. 交互 read](#46-交互-read)
  - [4.7. for 循环遍历arr](#47-for-循环遍历arr)

<!-- /TOC -->

# 1. linux.md

学习 和 记录，也包括shell脚本的编写

基本就是Ubuntu，其他发行版我也没用过。。。

wsl和正常ubuntu都有


# 2. 知识

## 2.1. 文件

/opt 存放的是源码之类的（独立的那种，删掉直接就能把整个软件删掉的那种）

/usr/local/bin 存放的是用户自己编译的可执行文件

/usr/local 用户级的软件目录，用来存放用户安装编译的软件，用户自己编译安装的软件也默认存放在这里

/usr/local/src 这个目录是存放用户编译软件所用的源码的

### 2.1.1. 举个例子

1. idea或者maven应该放在/opt下面

2. 自己编译的opencv源码应该放在 /usr/local/src下面，执行make install 指令后，opencv安装路径如下

    ```
    make install 指令的默认安装路径为：
    /usr/local/bin - executable files
    /usr/local/lib - libraries (.so)
    /usr/local/cmake/opencv4 - cmake package
    /usr/local/include/opencv4 - headers
    /usr/local/share/opencv4 - other files (e.g. trained cascades in XML format)
    ```


# 3. 指令、操作和坑（ubuntu）

## 3.1. 代理大坑

配置环境你可能会用到代理。如果你用的是代理，注意 **<font color=#D81D4F > sudo命令不会带上环境变量，这也就意味着，使用sudo apt的时候，你的apt是不会走代理的，因为你的代理是通过全局变量all_proxy之类的指定的 </font>**。

是的，这是安全的，但是不是很方便。

用代理的话, 要么使用```sudo su -l```切换到root用户下开启代理后再执行安装指令, **root用户下命令前面就不要带sudo了**；要么修改sudo的配置文件，保留all_proxy之类的与代理相关的环境变量。

我也是在使用 sudo wget 和 wget，速度不一样才发现的。

## 3.2. 修改密码

后面是具体的用户，sudo 能修改 root 的密码，也是挺有意思的事情

```
sudo passwd root
```

## 3.3. apt指令介绍

[参考链接](https://blog.csdn.net/LEON1741/article/details/85114318)

<font color=#D81D4F size=6 >警告：</font>执行了`apt-get autoremove`就等着完蛋吧。

| 命令 | 特点 |
| :----: |:----: |
| apt-get autoremove | 删除为了满足依赖而安装的，但现在不再需要的软件包（包括已安装包），保留配置文件；<font color=#D81D4F size=4 >警告：</font>慎用本命令！！！它会在你不知情的情况下，一股脑删除很多“它认为”你不再使用的软件； |
| apt-get remove xxxx | 删除已安装的软件包xxxx（保留配置文件），不会删除依赖软件包，保留配置文件； |
| apt-get purge xxxx | 删除已安装的软件包（不保留配置文件)，删除软件包，同时删除相应依赖软件包； |
| apt-get --purge remove xxxx | 同apt-get purge |
| apt-get autoclean | 删除为了满足某些依赖安装的，但现在不再需要的软件包；apt的底层包是dpkg, 而dpkg安装软件包时, 会将*.deb文件放在/var/cache/apt/archives/中；因此本命令会删除该目录下已经过期的deb； |
| apt-get clean | 删除已经安装过的的软件安装包；即自动将/var/cache/apt/archives/下的所有deb删掉，相当于清理下载的软件安装包； |

## 3.4. 安装apt-fast

apt太慢了，这玩意是单线程的，太折磨了，apt-fast是多线程的，下载起来很快，[我写了脚本](https://github.com/gf9276/ShFiles.git)，直接bash执行就行。或者像下面一样手动安装。

```
add-apt-repository ppa:apt-fast/stable && apt-get install apt-fast -y
```

接下来选择，第一个是包管理器，现在一般都用apt的吧，我反正很少用apt-get；第二个是线程数，看自己CPU线程数选就是了；第三个是安装包的时候是否跳过询问（是这样吗），我反正直接选No（默认的）。

apt --> 12 --> No


## 3.5. 设置snap代理

ubuntu 现在强推snap，22.04版本里，火狐和软件商店用的都是snap，然鹅这玩意不会读取全局代理，我们要是不设置代理的话，下载东西会很慢的。

看看代理ip是啥（前提是你设置了代理ip）

```
echo $http_proxy
```

换成你自己的代理ip，分开执行（下面那个也是没有s的）
```
snap set system proxy.http="http://172.22.160.1:7890"

snap set system proxy.https="http://172.22.160.1:7890"
```

## 3.6. 上下键补全历史指令

CentOS下，有一个很智能的功能，就是只输入一条历史命令的前几个字母，再按PageUp和PageDown键，就可以在以此字母为前缀的历史命令中上下切换。这个功能非常实用，而且比CTRL+R使用起来更友善、更方便。遗憾的是，ubuntu上并没有这个功能。Google上搜索才直到，这个只是linux在终端对键盘的映射而已，和linux的某个发行版无关。只是CentOS下默认打开了这个功能，而ubuntu默认禁止了而已。

输入指令，修改一下文件

```
vim /etc/inputrc
```

如图，删除两行的注释
![](https://cdn.jsdelivr.net/gh/gf9276/image/linux/20221112163803.png)


5~ 改 A，6~ 改 B，即可

![](https://cdn.jsdelivr.net/gh/gf9276/image/linux/20221112164130.png)

## 3.7. nload

安装nload

```
sudo apt-get install nload
```

使用nload

```
nload -m
```

这东西能检测网卡流量，挺好用的

## 3.8. 复制命令

下面四个命令结果相同，都是递归拷贝 packageA 文件及其任意层的结构到 packageB 中:

```
cp -r /home/packageA /home/packageB
cp -r /home/packageA /home/packageB/
cp -r /home/packageA/ /home/packageB
cp -r /home/packageA/ /home/packageB/
```

下面两个命令结果相同，都是不拷贝 packageA 文件，只递归拷贝其任意层的子结构到 packageB 中:
```
cp -r packageA/* packageB
cp -r packageA/* packageB/
```

## 3.9. 移动命令

```
mv IfElseTest1.java chapter03/
```

## 3.10. 查看文件夹下的文件数

```
ls -l | grep "-" | wc -l
```

## 3.11. find 指令


在当前目录下寻找文件或目录 filename

```
find . filename
```

可以把 . 换成指定路径

## 3.12. whereis which
TODO

## 3.13. nohup

nohup 后台执行并保存pid与log

```
nohup  java -jar ~/JavaFiles/tmp/nLogging/nWeb/admin/target/admin.jar > command.log 2>&1 & echo $! > command.pid
```

## 3.14. 关于wsl2可视化界面

**现在我装上了，用起来还行 2023/6/14，详情看[这里](https://github.com/gf9276/MdFiles/blob/master/wsl2.md)**

[Linux桌面系统介绍](https://www.eet-china.com/mp/a10976.html)

别装可视化界面了，很坑很坑很坑，托马斯无敌回旋坑。装上了也没卵用，没有显卡支持，卡卡的

gnome和meta（是叫这个名字吧）因为wsl（准确来说是容器）不支持acpi无法安装，装了等着自带的systemctl瘫痪吧，我修了好久才找到办法修回来。[问题链接](https://github.com/microsoft/WSL/issues/10059)

xfce4我装上了，但是不支持显卡，nvidia-smi命令无效，win10给wsl2的显卡驱动不能用于界面显示，简直鸡肋。Xrdp还间歇性闪退黑屏，累的。

最好的还是用wslg了，有vGPU的支持，还算流畅，ieda和输入法和谷歌浏览器我都试过，都正常安装

[wsl 官方 issue](https://github.com/microsoft/wslg/issues/4)

[wsl 官方 issue](https://github.com/microsoft/wslg/issues?q=Desktop+entry+creation+failed)


## 3.15. 中文化配置

这个我写了脚本

* 更新
  ```
  apt -y update
  ```

* 安装locales（语言设置的一个软件）
  ```
  apt -y install locales
  ```

* 安装语言包（管你简体繁体，是中文我就装，省的后面麻烦）
  ```
  apt -y install language-pack-zh-han*
  ```

* 安装字体
  ```
  apt -y install ttf-wqy-microhei ttf-wqy-zenhei xfonts-intl-chinese
  ```

* 区域设置
  ```
  dpkg-reconfigure locales
  ```
  
  依次出现两个界面

  * 界面一用于添加可选时区

    方向键控制光标移动，空格键选中```en_US.UTF-8```、```zh_CN.GBK```、```zh_CH.UTF-8（这个前面装的语言包里有，不用选）``` ，然后Tab键到OK选项，回车确定。

    ![](https://cdn.jsdelivr.net/gh/gf9276/image/linux/20230331202709.png)

  * 界面二用于选择默认时区

    选择```zh_CH.UTF-8```

    ![](https://cdn.jsdelivr.net/gh/gf9276/image/linux/20230331203242.png)


## 3.16. 安装google-pinyin

**这里的安装考虑到了wslg**

[参考链接1](https://patrickwu.space/2019/10/28/wsl-fcitx-setup-cn/)

[参考链接2](https://www.cnblogs.com/37yan/p/16169564.html)

<font size=4>注意：安装之前请按照前面的过程配置好中文环境</font>

<font size=4>警告：不要安装搜狗拼音，装了我一天没装成 😀</font>

1. 安装cjk字型和fcitx核心（是fcitx4，5的话我没试过）和googlepinyin
    ```
    apt -y install fcitx fonts-noto-cjk fonts-noto-color-emoji dbus-x11 fcitx-googlepinyin
    ```

2. dbus自启动
  
    不用担心，现在的wsl有systemctl，会自动启动dbus，所以不需要设置自启动

    至于正常的linux或者虚拟机就更不需要担心了。。。


3. 使用root账号生成dbus机器码（**这一步很重要**）

    我看很多csdn上都没这一步，坑死了，唉，主要是这玩意只能在国内找，国外的又不用中文输入法。

    我不知道机器码是什么，这好像是个随机数，机器标识符？这玩意如果有桌面环境的话，他就会有，如果没装过桌面环境的话，他就没有。

    反正是给dbus用的，没这玩意fcitx启动不了

    查看一下机器码
    ```
    cat /var/lib/dbus/machine-id
    ```
    没有东西的话，生成一个
    ```
    dbus-uuidgen > /var/lib/dbus/machine-id
    ```

4. 创建全局环境变量

    ```
    vim /etc/profile.d/my_fcitx.sh
    ```

    写入
    
    ```
    #!/bin/bash

    # 设置默认输入方式
    export QT_IM_MODULE=fcitx
    export GTK_IM_MODULE=fcitx
    export XMODIFIERS=@im=fcitx
    export DefaultIMModule=fcitx

    # fcitx自启动
    fcitx-autostart &>/dev/null
    ```

    如果使用远程连接，这时候右上角会有两个键盘，因为桌面自己启动了一个，你在profile里又启动了一个，不影响使用。

5. wsl --shutdown 后重启 wsl（可能有点慢）
  
6. 设置fcitx

    ```
    fcitx-configtool
    ```

    只留这两个

    ![](https://cdn.jsdelivr.net/gh/gf9276/image/linux/20230519152606.png)

    为了防止和win里快捷键打架，设置``` Alt+` ```为切换中英文，其他的都关掉，防止打架

    ![](https://cdn.jsdelivr.net/gh/gf9276/image/linux/20230519200156.png)



7. 安装可视化的 gnome-language-selector
    
    如果用wslg，它会提示没有装完语言，不要点击安装，因为装不了。
    
    如果用完整的桌面，打开 gnome-language-selector，它会提示没有装完语言，安装没装完的，然后下面选项里语言换成中文（记得输入法换成Fcitx 4，省得后面调）。这样整个系统的中文环境就完善了。

    ```
    apt -y install language-selector-gnome
    ```

    ```
    gnome-language-selector
    ```

    出现下面的界面，键盘输入法系统是fcitx4然后选的是中文就行了，这玩意好像有点怪。

    ![](https://cdn.jsdelivr.net/gh/gf9276/image/linux/20230519152046.png)

    ![](https://cdn.jsdelivr.net/gh/gf9276/image/linux/20230519152106.png)

8. im-config（可选）

    im-config是Input Method Configuration的缩写。

    这玩意也是用来选择输入方式，我放在这里记录一下，不用执行。

9.  测试（如果安装了gedit的话）
      
        gedit hehe.txt

    ![](https://cdn.jsdelivr.net/gh/gf9276/image/linux/20230519152820.png)



# 4. shell脚本（ubuntu）

## 4.1. 判断格式

括号内为真，输出0；为假输出1

```
[ -f /home/guof/my_check_up_ln.sh ]; echo $?
```

## 4.2. 常见的判断

[参考链接](https://www.cnblogs.com/ph829/p/5057914.html)

- 文件判断
  ```
  -e filename 如果 filename存在，则为真
  -d filename 如果 filename为目录，则为真 
  -f filename 如果 filename为常规文件，则为真
  -L filename 如果 filename为符号链接，则为真
  -r filename 如果 filename可读，则为真 
  -w filename 如果 filename可写，则为真 
  -x filename 如果 filename可执行，则为真
  -s filename 如果文件长度不为0，则为真
  -h filename 如果文件是软链接，则为真，这个和-L是什么区别
  filename1 -nt filename2 如果 filename1比 filename2新，则为真。
  filename1 -ot filename2 如果 filename1比 filename2旧，则为真。
  ```

- 整数变量表达式
  ```
  -eq 等于
  -ne 不等于
  -gt 大于
  -ge 大于等于
  -lt 小于
  -le 小于等于
  ```

- 字符串变量表达式
  ```
  If  [ $a = $b ]                 如果string1等于string2，则为真
                                  字符串允许使用赋值号做等号
  if  [ $string1 !=  $string2 ]   如果string1不等于string2，则为真       
  if  [ -n $string  ]             如果string 非空(非0），返回0(true)  
  if  [ -z $string  ]             如果string 为空，则为真
  if  [ $sting ]                  如果string 非空，返回0 (和-n类似) 
  ```

- 逻辑
  ```
      逻辑非 !                   条件表达式的相反
  if [ ! 表达式 ]
  if [ ! -d $num ]               如果不存在目录$num


      逻辑与 –a                   条件表达式的并列
  if [ 表达式1  –a  表达式2 ]


      逻辑或 -o                   条件表达式的或
  if [ 表达式1  –o 表达式2 ]
  ```

- 实例
  ```
  #!/bin/bash
  #if [ -d x.txt ]

  if [ -d ]
  then
      cd toolchain; \
      ls *.patch | sort \
      #FILES=$$(ls *.patch | sort); \
      echo "ok"
  else
      echo "nok"
  fi
  ```

## 4.3. 函数 与 ifelse

这个看脚本就行

[函数的参考链接](https://www.runoob.com/linux/linux-shell-func.html)

[将命令输出导入到变量中](http://c.biancheng.net/view/1164.html)，使用方法如下

```
variable=`commands`
variable=$(commands)
```

## 4.4. sh脚本输入设置默认参数

```
v1=${1:-"set"} # 第一个参数默认为字符串 "set"
```

## 4.5. echo 变色

[参考链接](https://blog.csdn.net/Dreamhai/article/details/103432525)

例子
```
echo -e "\e[36m不是yes, 退出\e[0m"
```

## 4.6. 交互 read
```
read -ra arr  # 将输入的东西以空格为间隔，输入到数组arr, -r表示进制转义符
```

## 4.7. for 循环遍历arr
```
for element in ${arr[*]}
do
    ###
done
```
