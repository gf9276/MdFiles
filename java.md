<!-- TOC -->

- [1. java.md](#1-javamd)
- [2. 安装](#2-安装)
  - [2.1. 更新apt并安装java](#21-更新apt并安装java)
  - [2.2. 添加环境变量（不同java版本记得改数字）](#22-添加环境变量不同java版本记得改数字)
  - [2.3. 多个版本java版本管理](#23-多个版本java版本管理)
  - [2.4. 刷新环境变量，并使用指令查看安装是否成功](#24-刷新环境变量并使用指令查看安装是否成功)

<!-- /TOC -->

# 1. java.md

安装和学习的资料

学习看尚硅谷2023年的java视频

JDK (Java Development Kit):是Java程序开发工具包，包含JRE和开发人员使用的工具。

JRE (Java Runtime Environment):是Java程序的运行时环境，包含JVM和运时所需要的核心类库。

JAVA11 LTS版本之后，JRE集成在JDK中了，不用独立安装

# 2. 安装


用代理的话, 要么使用```sudo su -l```切换到root用户下开启代理后再执行安装指令, **root用户下命令前面就不要带sudo了**；要么修改sudo的配置文件，保留all_proxy之类的与代理相关的环境变量


[参考连接1（不需要看）](https://cloud.tencent.com/developer/article/1626610)

[参考链接2（不需要看）](https://cloud.tencent.com/developer/article/1897987)


已经写了脚本，地址 https://github.com/gf9276/ShFiles.git ，直接运行

## 2.1. 更新apt并安装java

使用java版本 8 或者 11, 目前我用的是11, 8怪怪的好吧(2023-04-16)

一般项目不用17，因为他太新了。。。不过我写leetcode用的17

```
apt update
```

```
apt install openjdk-11-jdk
```

## 2.2. 添加环境变量（不同java版本记得改数字）

有两种方法，按照《Linux命令行与shell脚本编程大全（第4版）》的说法，推荐第二种。因为，当ubuntu更新的时候，/etc/profile 文件会被重置。
   
* **第一种，将变量写在 /etc/profile 文件中**

打开 /etc/profile 文件
```
vim /etc/profile
```
在文件的最后写入
```
export JAVA_HOME=/usr/lib/jvm/java-11-openjdk-amd64
export JRE_HOME=$JAVA_HOME/jre
export PATH=$JAVA_HOME/bin:$JRE_HOME/bin:$PATH
```

* **第二种，在 /etc/profile.d 路径下创建新的.sh文件，并在新文件中写入环境变量（profile中会用循环读取/etc/profile.d路径下的.sh脚本，并用source指令执行）**

创建文件 my_java_cfg.sh （文件名字不重要，只要在路径 /etc/profile.d 下就行）

```
touch /etc/profile.d/my_java_cfg.sh
```

打开文件 my_java_cfg.sh
```
vim /etc/profile.d/my_java_cfg.sh
```

写入环境变量
```
export JAVA_HOME=/usr/lib/jvm/java-11-openjdk-amd64
export JRE_HOME=$JAVA_HOME/jre
export PATH=$JAVA_HOME/bin:$JRE_HOME/bin:$PATH
```

## 2.3. 多个版本java版本管理

执行下面指令，并选择与环境变量中版本一致的java（虽然直接设置环境变量也可以，但是最好这么做）
```
update-alternatives --config java
```

该指令也可以用来查看java的安装路径

## 2.4. 刷新环境变量，并使用指令查看安装是否成功

```
. /etc/profile
```

```
. .bashrc # 这条是为了恢复用户环境
```

```
javac -version
```

```
java -version
```

成功结果如下图

![](https://cdn.jsdelivr.net/gh/gf9276/image/java/20230416231943.png)

