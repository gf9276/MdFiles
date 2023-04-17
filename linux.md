<!-- TOC -->

- [1. linux.md](#1-linuxmd)
- [2. 指令、操作和坑（ubuntu）](#2-指令操作和坑ubuntu)
  - [2.1. 代理大坑](#21-代理大坑)
  - [2.2. 上下键补全历史指令](#22-上下键补全历史指令)
  - [2.3. 复制命令](#23-复制命令)
  - [2.4. 移动命令](#24-移动命令)
  - [2.5. 查看文件夹下的文件数](#25-查看文件夹下的文件数)
  - [2.6. find 指令](#26-find-指令)
  - [2.7. whereis which](#27-whereis-which)
  - [2.8. 中文，设置中文](#28-中文设置中文)
  - [2.9. 安装gedit](#29-安装gedit)
  - [2.10. 安装搜狗拼音（没成功）](#210-安装搜狗拼音没成功)
- [3. shell脚本（ubuntu）](#3-shell脚本ubuntu)
  - [3.1. 判断格式](#31-判断格式)
  - [3.2. 常见的判断](#32-常见的判断)
  - [3.3. 函数 与 ifelse](#33-函数-与-ifelse)
  - [3.4. sh脚本输入设置默认参数](#34-sh脚本输入设置默认参数)
  - [3.5. echo 变色](#35-echo-变色)
  - [3.6. 交互 read](#36-交互-read)
  - [3.7. for 循环遍历arr](#37-for-循环遍历arr)

<!-- /TOC -->

# 1. linux.md

学习 和 记录，也包括shell脚本的编写

基本就是Ubuntu，其他发行版我也没用过。。。

# 2. 指令、操作和坑（ubuntu）

## 2.1. 代理大坑

配置环境你可能会用到代理。如果你用的是代理，注意 **<font color=#D81D4F > sudo命令不会带上环境变量，这也就意味着，使用sudo apt的时候，你的apt是不会走代理的，因为你的代理是通过全局变量all_proxy之类的指定的 </font>**。

是的，这是安全的，但是不是很方便。

用代理的话, 要么使用```sudo su -l```切换到root用户下开启代理后再执行安装指令, **root用户下命令前面就不要带sudo了**；要么修改sudo的配置文件，保留all_proxy之类的与代理相关的环境变量。

我也是在使用 sudo wget 和 wget，速度不一样才发现的。

## 2.2. 上下键补全历史指令

CentOS下，有一个很智能的功能，就是只输入一条历史命令的前几个字母，再按PageUp和PageDown键，就可以在以此字母为前缀的历史命令中上下切换。这个功能非常实用，而且比CTRL+R使用起来更友善、更方便。遗憾的是，ubuntu上并没有这个功能。Google上搜索才直到，这个只是linux在终端对键盘的映射而已，和linux的某个发行版无关。只是CentOS下默认打开了这个功能，而ubuntu默认禁止了而已。

输入指令，修改一下文件

```
vim /etc/inputrc
```

如图，删除两行的注释
![](https://cdn.jsdelivr.net/gh/gf9276/image/linux/20221112163803.png)


5~ 改 A，6~ 改 B，即可

![](https://cdn.jsdelivr.net/gh/gf9276/image/linux/20221112164130.png)

## 2.3. 复制命令

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

## 2.4. 移动命令

```
mv IfElseTest1.java chapter03/
```

## 2.5. 查看文件夹下的文件数

```
ls -l | grep "-" | wc -l
```

## 2.6. find 指令


在当前目录下寻找文件或目录 filename

```
find . filename
```

可以把 . 换成指定路径

## 2.7. whereis which


## 2.8. 中文，设置中文

这个我写了脚本

* 先更新，后面还是会用到的
```
apt update
```

* 然后安装locales, wsl 的 ubuntu 有的，这一步跳过

```
apt install locales
```

* 区域设置
```
dpkg-reconfigure locales
```

出现如下所示，这个界面是让你添加可选时区用的，选三个（空格选中），分别是```en_US.UTF-8```、```zh_CN.GBK```、```zh_CH.UTF-8``` （选几个无所谓的，万一用上了呢），然后回车确定。

![](https://cdn.jsdelivr.net/gh/gf9276/image/linux/20230331202709.png)

然后出现，这个才是让你选择默认时区的，中文一般选择```zh_CH.UTF-8```

![](https://cdn.jsdelivr.net/gh/gf9276/image/linux/20230331203242.png)

* 安装语言包和字体

注意, 这里不需要安装语言包, 不需要执行```apt install language-pack-zh-hans```, 因为这一步在前面的第一个界面已经做过了

```
apt -y install ttf-wqy-microhei ttf-wqy-zenhei xfonts-intl-chinese
```

## 2.9. 安装gedit

```
apt update
```

```
apt -y install gedit
```

打开会有警告，无伤大雅
![](https://cdn.jsdelivr.net/gh/gf9276/image/linux/20230331205033.png)


## 2.10. 安装搜狗拼音（没成功）

* 执行指令，安装```fcitx```和搜狗拼音
```
sudo apt install fcitx fcitx-googlepinyin -y
```

* 添加环境变量
```
sudo vim /etc/profile
```

写入
```
export GTK_IM_MODULE=fcitx
export QT_IM_MODULE=fcitx
export XMODIFIERS=\@im=fcitx 
```



# 3. shell脚本（ubuntu）

## 3.1. 判断格式

括号内为真，输出0；为假输出1

```
[ -f /home/guof/my_check_up_ln.sh ]; echo $?
```

## 3.2. 常见的判断

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
- 
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

## 3.3. 函数 与 ifelse

这个看脚本就行

[函数的参考链接](https://www.runoob.com/linux/linux-shell-func.html)

[将命令输出导入到变量中](http://c.biancheng.net/view/1164.html)，使用方法如下

```
variable=`commands`
variable=$(commands)
```

## 3.4. sh脚本输入设置默认参数

```
v1=${1:-"set"} # 第一个参数默认为字符串 "set"
```

## 3.5. echo 变色

[参考链接](https://blog.csdn.net/Dreamhai/article/details/103432525)

例子
```
echo -e "\e[36m不是yes, 退出\e[0m"
```

## 3.6. 交互 read
```
read -ra arr  # 将输入的东西以空格为间隔，输入到数组arr, -r表示进制转义符
```

## 3.7. for 循环遍历arr
```
for element in ${arr[*]}
do
    ###
done
```
