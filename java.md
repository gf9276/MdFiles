# java

安装和学习的资料

JDK (Java Development Kit):是Java程序开发工具包，包含JRE和开发人员使用的工具。
JRE (Java Runtime Environment):是Java程序的运行时环境，包含JVM和运时所需要的核心类库。

JAVA11 LTS版本之后，JRE集成在JDK中了，不用独立安装

## 安装

[参考连接1（这玩意不一定是对的）](https://cloud.tencent.com/developer/article/1626610)

[参考链接2（这玩意不一定是对的）](https://cloud.tencent.com/developer/article/1897987)

### 直接安装

```
sudo apt install openjdk-11-jdk
```

### 获取路径

我只是用这个来获取路径，没用这个进行版本管理，我直接用环境变量了。因为mvn需要用到JAVA_HOME环境变量。（最好是两个一起用，保持一致）

```
sudo update-alternatives --config java
```

### 添加环境变量到 /etc/profile

请注意，java8的位置不一样的，不过问题好像不大，有软链接

```
export JAVA_HOME=/usr/lib/jvm/java-11-openjdk-amd64
export JRE_HOME=$JAVA_HOME/jre
export PATH=$JAVA_HOME/bin:$JRE_HOME/bin:$PATH
```

### 全部删除

注意是全部删除
```
sudo apt-get remove openjdk*
```

## 解决IDEA连接WSL卡顿问题

[参考链接](https://youtrack.jetbrains.com/issue/IDEA-293604/IntelliJ-is-slow-hanging-when-working-with-WSL-filesystem#focus=Comments-27-6180537.0-0)

按照里面的说法，在windows defender里面添加病毒排除项就好了，我也不知道是什么原理，如下：

![](https://cdn.jsdelivr.net/gh/gf9276/image/java/20230314121424.png)

问题确实解决了

# Maven

## 安装

[参考链接1](https://cloud.tencent.com/developer/article/1649751#:~:text=%E5%9C%A8%20Ubuntu%20%E4%BD%BF%E7%94%A8%20apt%20%E5%AE%89%E8%A3%85%20Maven%20%E9%9D%9E%E5%B8%B8%E7%AE%80%E5%8D%95%E7%9B%B4%E6%8E%A5%E3%80%82%20%E5%8D%87%E7%BA%A7%E8%BD%AF%E4%BB%B6%E5%8C%85%E7%B4%A2%E5%BC%95%EF%BC%8C%E5%B9%B6%E4%B8%94%E8%BE%93%E5%85%A5%E4%B8%8B%E9%9D%A2%E7%9A%84%E5%91%BD%E4%BB%A4%EF%BC%8C%E5%AE%89%E8%A3%85,maven%20%E6%83%B3%E8%A6%81%E9%AA%8C%E8%AF%81%E5%AE%89%E8%A3%85%E6%98%AF%E5%90%A6%E6%88%90%E5%8A%9F%EF%BC%8C%E8%BF%90%E8%A1%8C%20mvn%20-version%20%EF%BC%9A%20mvn%20-version%20%E8%BE%93%E5%87%BA%E7%9C%8B%E8%B5%B7%E6%9D%A5%E5%83%8F%E4%B8%8B%E9%9D%A2%E8%BF%99%E6%A0%B7%EF%BC%9A)

[参考链接2](https://blog.csdn.net/weixin_45428910/article/details/127956322)

[参考链接3](https://www.cnblogs.com/chinda/p/14297338.html)

* 下载指定版本的Maven到/opt目录下，解压，然后就可以删掉压缩包了

```
sudo tar -zxvf apache-maven-3.9.1-bin.tar.gz
```

![](https://cdn.jsdelivr.net/gh/gf9276/image/java/20230331222803.png)


* 添加环境变量到/etc/profile

```
sudo vim /etc/profile
```

加入

```
export M2_HOME=/usr/local/src/maven/apache-maven-3.9.1
export MAVEN_HOME=M2_HOME
export PATH=${M2_HOME}/bin:${PATH}
```

```
. /etc/profile
```

* 添加阿里源

```
sudo vim /opt/maven/apache-maven-3.9.1/conf/settings.xml
```

加入

```
    <mirror>
      <id>aliyun</id>
      <mirrorOf>central</mirrorOf>
      <name>aliyun</name>
      <url>https://maven.aliyun.com/repository/public</url>
    </mirror>
```

![](https://cdn.jsdelivr.net/gh/gf9276/image/java/20230401195604.png)

# opencv

## 安装

提前制定好java的版本，编译的时候会用的

[参考链接1](https://zoyi14.smartapps.cn/pages/note/index?slug=90c542ddf54a&origin=share&_swebfr=1&_swebFromHost=heytapbrowser)


[参考链接2](https://blog.csdn.net/qq_15737599/article/details/90200152)

[参考链接3](https://blog.csdn.net/Undefinedefity/article/details/106180033)

* 安装ant

```
sudo apt-get update && sudo apt-get install ant
```
```ant -version```检查是否安装成功

* 安装cmake

```
sudo apt-get install cmake
```
```cmake --version```检查是否安装成功

* 安装其他依赖
```
sudo apt-get install build-essential libgtk2.0-dev libavcodec-dev libavformat-dev libjpeg-dev libswscale-dev libtiff5-dev libgtk2.0-dev pkg-config
```

* 下载Opencv并且随便找个地方放了，并且解压，我干脆放到 ~/opencv 下了

* 顺便把IPPICV也下载了，这玩意不知道为什么开了代理也下载不下来，真tm神奇

```
https://github.com/opencv/opencv_3rdparty/tree/ippicv/master_20191018/ippicv
```

最终如下图
![](https://cdn.jsdelivr.net/gh/gf9276/image/java/20230402205415.png)

* 改一下ippv的地址

编辑这个叫做 ippicv.cmake 的文件，所在的目录路径为 ```~/opencv/opencv-4.5.3/3rdparty/ippicv```

![](https://cdn.jsdelivr.net/gh/gf9276/image/java/20230402205534.png)

把ippicv路径改一下，我的是 ```file:/home/guof/opencv/opencv_3rdparty-ippicv-master_20191018/ippicv/```

**<font color=#D81D4F >注意：图里的路劲最后少个/，我忘加了</font>**

![](https://cdn.jsdelivr.net/gh/gf9276/image/java/20230402210758.png)

* 创建build文件夹
```
cd ~/opencv/opencv-4.5.3 && mkdir build && cd build
```

* 开始cmake，这里太奇怪了，不管我怎么设置java版本，这玩意用的都是java11，太诡异了
```
sudo cmake -D CMAKE_BUILD_TYPE=Release -D CMAKE_INSTALL_PREFIX=/usr/local -D  OPENCV_GENERATE_PKGCONFIG=ON -D OPENCV_ENABLE_NONFREE=True ..
```

* 开始编译
```
sudo make -j8
```

* 开始安装

```
sudo make install
```

* 配置环境

```
sudo vim /etc/ld.so.conf
```

写入

```
include /usr/local/lib
```

运行
```
sudo ldconfig
```
会报错，有两个文件一样的，把其中一个改成软连接就好了
![](https://cdn.jsdelivr.net/gh/gf9276/image/java/20230331234339.png)

[听说是无害的，还是别管了](https://superuser.com/questions/1707681/wsl-libcuda-is-not-a-symbolic-link)

[另外一个讨论](https://github.com/microsoft/WSL/issues/5663)

* 修改/etc/bash.bashrc 
```
sudo vim /etc/bash.bashrc 
```

加上

```
PKG_CONFIG_PATH=$PKG_CONFIG_PATH:/usr/local/lib/pkgconfig
export PKG_CONFIG_PATH
```

然后
```
source /etc/bash.bashrc
```

* 检查安装

```
pkg-config --modversion opencv4 或者 pkg-config --modversion opencv
```


* 看看有没有.jar文件
  
![](https://cdn.jsdelivr.net/gh/gf9276/image/java/20230402212308.png)

* 如果显示opencv不在java库中，可以执行

```
sudo cp -r ~/opencv/opencv-4.5.3/build/lib/libopencv_java453.so /usr/lib
```

# mysql

## 安装

多个wsl只能有一个开启mysql（一个发行版运行mysql会导致另外一个安装失败），我猜测原因可能是共用端口，导致打起来了

* wsl 要开启systemd

```
sudo vim /etc/wsl.conf
```

写入
```
[boot]
systemd=true
```

```wsl --shutdown```后重启，在发行版里输入指令```systemctl list-unit-files --type=service```进行验证

* 安装mysql

```
sudo apt update && sudo apt install mysql-server
```

* 删除mysql（安装失败了可以直接用）

```
sudo apt purge mysql-* && sudo rm -rf /etc/mysql/ /var/lib/mysql && sudo apt autoremove && sudo apt autoclean
```

* 查看状态
```
sudo systemctl status mysql
```

* 启动

```
sudo /etc/init.d/mysql start
```

## 启动安全脚本提示符（可选？）
[参考1](https://stackoverflow.com/questions/72334158/im-getting-an-error-when-i-ran-mysql-secure-installation)

[参考2（我选的都是y）](https://blog.csdn.net/weixin_43279138/article/details/126872698)

```
sudo mysql_secure_installation
```

好像会报错，先进入mysql的root用户
```
sudo mysql
```

执行命令改密码
```
ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY 'mypasswordhere';
```

然后再执行启动安全脚本提示符就行了
```
sudo mysql_secure_installation
```

之后登录需要密码
```
sudo mysql -u root -p
```

## 一些操作

### 注册用户+给权限

[参考链接](https://www.yiibai.com/mysql/grant.html)

### 显示数据库

```
SHOW DATABASES
```

### 命令行执行.sql文件

```
source /path/to/.sql
```

# minio

## 安装