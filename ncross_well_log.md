
<!-- TOC -->

- [1. ncross\_well\_log](#1-ncross_well_log)
- [2. 环境搭建](#2-环境搭建)
  - [2.1. 代理问题](#21-代理问题)
  - [2.2. java](#22-java)
    - [2.2.1. 版本和注意事项](#221-版本和注意事项)
    - [2.2.2. 安装](#222-安装)
  - [2.3. maven](#23-maven)
    - [2.3.1. 版本和注意事项](#231-版本和注意事项)
    - [2.3.2. 安装](#232-安装)
  - [2.4. opencv](#24-opencv)
    - [2.4.1. 版本和注意事项](#241-版本和注意事项)
    - [2.4.2. 安装ant](#242-安装ant)
      - [2.4.2.1. ant的坑](#2421-ant的坑)
    - [2.4.3. 安装cmake](#243-安装cmake)
    - [2.4.4. 安装其他依赖](#244-安装其他依赖)
    - [2.4.5. 下载opencv并解压](#245-下载opencv并解压)
    - [2.4.6. 下载 IPPICV 并配置 （你要是能用代理，能翻墙，这个不需要）](#246-下载-ippicv-并配置-你要是能用代理能翻墙这个不需要)
    - [2.4.7. 准备开始编译](#247-准备开始编译)
    - [2.4.8. 配置环境](#248-配置环境)
    - [2.4.9. 检查安装是否成功](#249-检查安装是否成功)
  - [2.5. mysql](#25-mysql)
    - [2.5.1. 版本和注意事项](#251-版本和注意事项)
    - [2.5.2. 安装](#252-安装)
  - [2.6. minio](#26-minio)
    - [2.6.1. 版本和注意事项](#261-版本和注意事项)
    - [2.6.2. 安装](#262-安装)
  - [2.7. hdf5](#27-hdf5)
    - [2.7.1. 版本和注意事项](#271-版本和注意事项)
    - [2.7.2. 建议](#272-建议)
    - [2.7.3. 安装HDFJava](#273-安装hdfjava)
    - [2.7.4. 动态库关联（也可以在idea里设置）](#274-动态库关联也可以在idea里设置)
    - [2.7.5. 将 .jar 导入到 maven 中](#275-将-jar-导入到-maven-中)
    - [2.7.6. 项目里pom的配置](#276-项目里pom的配置)
- [3. idea 里的项目配置](#3-idea-里的项目配置)
  - [3.1. maven选择](#31-maven选择)
  - [3.2. java11要加个东西](#32-java11要加个东西)

<!-- /TOC -->


# 1. ncross_well_log

**测井相关的代码，注意事项和环境搭建**


# 2. 环境搭建

有以下几个部分，**<font color=#D81D4F > 按照顺序来 </font>**

## 2.1. 代理问题

配置环境你可能会用到代理。如果你用的是代理，注意 **<font color=#D81D4F > sudo命令不会带上环境变量，这也就意味着，使用sudo apt的时候，你的apt是不会走代理的，因为你的代理是通过全局变量all_proxy之类的指定的 </font>**。

用代理的话, 要么使用```sudo su -l```切换到root用户下开启代理后再执行安装指令, **root用户下命令前面就不要带sudo了**；要么修改sudo的配置文件，保留all_proxy之类的与代理相关的环境变量。

## 2.2. java

### 2.2.1. 版本和注意事项

- 师兄和我说的java8，但是我调试的时候有点问题，所以我目前用的java11（已经解决了wsl jdk8的问题-2023.5.20）

- 建议把java8和java11都装了，反正可以选择

- 这里先安装java8，等后面装完opencv再安装java11。因为编译的时候cmake优先会选择jkd11不选择jdk8，而java11貌似又太新了（不过实测11也没有问题）

### 2.2.2. 安装 

看 [java.md](https://github.com/gf9276/MdFiles/blob/master/java.md)

## 2.3. maven

### 2.3.1. 版本和注意事项

- maven 3.9.1 （这个版本问题不大）

### 2.3.2. 安装

看 [maven.md](https://github.com/gf9276/MdFiles/blob/master/maven.md)

## 2.4. opencv

这个有点麻烦

[参考链接1](https://zoyi14.smartapps.cn/pages/note/index?slug=90c542ddf54a&origin=share&_swebfr=1&_swebFromHost=heytapbrowser)

[参考链接2](https://blog.csdn.net/qq_15737599/article/details/90200152)

[参考链接3](https://blog.csdn.net/Undefinedefity/article/details/106180033)


### 2.4.1. 版本和注意事项

- opencv 4.5.3 

- 其实代码里 opencv 用的很少，几乎没有

- 安装opencv需要java8或者java11，不能只有java 17, java 17不带jni的, opencv会编译失败的

- 如果同时存在java8和java11，会优先选择java11的jni，我暂时没法指定

**<font color=#D81D4F > 下面的一定按照顺序来，不然会错的 </font>**

### 2.4.2. 安装ant

```
apt update
```

```
apt install ant
```

使用 ```ant -version```检查是否安装成功

#### 2.4.2.1. ant的坑

可能有坑，我在22.04没发现问题，但是在20.04上发现了

如果后面cmake编译文件显示找不到ant的话，大概率是软链接问题，执行该命令解决

```
ln -snf /usr/share/ant/bin/ant /bin/ant
```

[参考链接1，提出问题](https://blog.csdn.net/quantum7/article/details/104625253)

[参考链接2，解决问题](https://blog.csdn.net/quantum7/article/details/104625736)

### 2.4.3. 安装cmake

```
apt install cmake
```

使用 ```cmake --version```检查是否安装成功


### 2.4.4. 安装其他依赖

小心，sudo会清除环境变量，导致代理无法使用

参数的具体含义要查一下，我忘了

```
apt install build-essential libgtk2.0-dev libavcodec-dev libavformat-dev libjpeg-dev libswscale-dev libtiff5-dev libgtk2.0-dev pkg-config
```

### 2.4.5. 下载opencv并解压

存放到/opt/opencv下（路径其实无所谓，这只是个需要编译的文件）

```
mkdir /opt/opencv
```

```
cd /opt/opencv
```

```
wget https://github.com/opencv/opencv/archive/4.5.3.zip
```

解压，没有unzip的需要先执行```apt install zip unzip```
```
unzip 4.5.3.zip && rm 4.5.3.zip
```

### 2.4.6. 下载 IPPICV 并配置 （你要是能用代理，能翻墙，这个不需要）

IPPICV 下载到这个路径下

```
cd /opt/opencv
```

* 下载压缩包

对应的github地址：https://github.com/opencv/opencv_3rdparty.git

对应命令

```
wget https://github.com/opencv/opencv_3rdparty/archive/refs/heads/ippicv/master_20191018.zip
```

解压缩
```
unzip master_20191018.zip && rm master_20191018.zip
```

下载压缩完是这样子的

![](https://cdn.jsdelivr.net/gh/gf9276/image/ncross_well_log/20230417120646.png)

* 改一下ippv的地址

```
vim /opt/opencv/opencv-4.5.3/3rdparty/ippicv/ippicv.cmake
```

光标在的那一行，改成 ```file:/opt/opencv/opencv_3rdparty-ippicv-master_20191018/ippicv/```

![](https://cdn.jsdelivr.net/gh/gf9276/image/ncross_well_log/20230417121913.png)

### 2.4.7. 准备开始编译

* 创建build文件夹
```
cd /opt/opencv/opencv-4.5.3 && mkdir build && cd build
```

* 开始cmake

这里应该要用java8，这里有点奇怪，不管我怎么设置java版本，这玩意用的都是java11，不过java17是没有JNI的，但是JAVA8怎么回事，打不过11，难绷

```
cmake -D CMAKE_BUILD_TYPE=Release -D CMAKE_INSTALL_PREFIX=/usr/local -D  OPENCV_GENERATE_PKGCONFIG=ON -D OPENCV_ENABLE_NONFREE=True ..
```

这样子成功

![](https://cdn.jsdelivr.net/gh/gf9276/image/ncross_well_log/20230417122224.png)


* 开始编译

j8是线程数量，cpu线程数量少一点的改成j4，前面用java11编译的，**这里会报一点错，不知道有没有影响，emmmmm....**

```
make -j8
```

* 开始安装

```
make install
```

### 2.4.8. 配置环境

* 修改 /etc/ld.so.conf

```
vim /etc/ld.so.conf
```

写入

```
include /usr/local/lib
```

运行
```
ldconfig
```
wsl下会报错，有两个文件一样的，把其中一个改成软链接就好了（[听说是无害的，还是别管了](https://superuser.com/questions/1707681/wsl-libcuda-is-not-a-symbolic-link) ——20230417）

![](https://cdn.jsdelivr.net/gh/gf9276/image/java/20230331234339.png)

[另外一个讨论](https://github.com/microsoft/WSL/issues/5663)


* 修改/etc/bash.bashrc 
```
vim /etc/bash.bashrc 
```

在最后加上

```
PKG_CONFIG_PATH=$PKG_CONFIG_PATH:/usr/local/lib/pkgconfig
export PKG_CONFIG_PATH
```

然后
```
source /etc/bash.bashrc
```

### 2.4.9. 检查安装是否成功

指令

```
pkg-config --modversion opencv4
```

或者 

```
pkg-config --modversion opencv
```

* 看看有没有.jar文件
  
```
ls /opt/opencv/opencv-4.5.3/build/bin/
```

* 这里需要把.jar文件复制到项目下面，在项目下创建一个opencv文件夹，然后直接复制过去

```
mkdir ~/JavaFiles/ncross_well_log/n3rdParty/opencv
```
```
cd ~/JavaFiles/ncross_well_log/n3rdParty/opencv
```
```
cp /opt/opencv/opencv-4.5.3/build/bin/opencv-453.jar ./
```

* 如果idea执行代码显示opencv不在java库中，可以执行（或者在idea里面指定）

```
cp -r /opt/opencv/opencv-4.5.3/build/lib/libopencv_java453.so /usr/lib
```

## 2.5. mysql

### 2.5.1. 版本和注意事项

* mysql 8.0.32 （这个版本问题不大）
* 不要安全安装！安全安装对密码长度有限制，这样子还得改代码里的配置，麻烦。
* 需要创建用户 nan ，密码 nan416，并赋予所有权限（方便开发）
* 需要设置大小写不敏感（low_case_table_names = 1）
* 端口使用 3306

### 2.5.2. 安装

看 [mysql.md](https://github.com/gf9276/MdFiles/blob/master/mysql.md)


## 2.6. minio

### 2.6.1. 版本和注意事项

- minio RELEASE.2023-03-24T21-41-23Z （这个版本问题不大）

### 2.6.2. 安装

看 [minio.md](https://github.com/gf9276/MdFiles/blob/master/minio.md)

## 2.7. hdf5

这个找了我挺久的

### 2.7.1. 版本和注意事项

- HDFJava 3.3.2 

- jhdf 0.6.9

- 只用安装 HDFJava，jhdf之类的用maven坐标就可以直接导入了

### 2.7.2. 建议

读文件用 jhdf，直接用maven坐标就可以了。有点编码问题，不过API很好用

写文件用 hdf，需要手动下载配置。用起来很麻烦，但jhdf只能读不能写，所以必须用这个

[官方示例](https://portal.hdfgroup.org/display/HDF5/Examples+from+Learning+the+Basics)

[hdfview：windows下看hdf5文件的软件](https://portal.hdfgroup.org/display/support/HDFView+3.1.4#files)

[hdf5文档](https://portal.hdfgroup.org/display/HDF5/HDF5%20javadocs)

[这个也是手册，但是感觉乱七八糟的](http://web.mit.edu/fwtools_v3.1.0/www/H5.intro.html#Intro-APIs)

[参考链接1，对安装有整体的概念](https://blog.csdn.net/luoying_1993/article/details/72979022)

[参考链接2，用于补充官方文档](https://blog.csdn.net/shyjhyp11/article/details/109011262)

[参考连接3，纯代码，一般般](https://www.cnblogs.com/Husir-boky/p/16855534.html)

[参考链接4，纯代码，一般般](https://blog.csdn.net/qq_40298231/article/details/99549532)

[讨论：老版本和新版本使用上的区别](https://stackoverflow.com/questions/47753363/hdf5-in-maven-project)

### 2.7.3. 安装HDFJava

好像需要三个.jar和一个.so

1. jarhdf5-3.2.1.jar
2. slf4j-api-1.7.5.jar
3. slf4j-nop-1.7.5.jar
4. libjhdf5.so

其中两个.jar（2和3）可以通过maven坐标直接下载，剩下的一个.jar（1）和.so（4）需要自己装了

[下载地址](https://support.hdfgroup.org/products/java/release/download.html#script)

我找了很久，只找到了centos，不过能用。。。

* 下载hdf
```
cd /opt && wget http://support.hdfgroup.org/ftp/HDF5/releases/HDF-JAVA/hdfjni-3.3.2/bin/HDFJava-3.3.2_Stat-centos7_64.tar.gz
```

* 解压缩并且运行脚本

这里赋予权限应该不需要 -R，我只要能进hdf文件夹就行，其他的我只要能读就行

```
tar -zxvf HDFJava-3.3.2_Stat-centos7_64.tar.gz && rm HDFJava-3.3.2_Stat-centos7_64.tar.gz && chmod 755 /opt/hdf
```

```
cd /opt/hdf && ./HDFJava-3.3.2-Linux.sh
```

.jar和.so在这个目录下面

```
cd /opt/hdf/HDFJava-3.3.2-Linux/HDF_Group/HDFJava/3.3.2/lib && ls -ahl
```

### 2.7.4. 动态库关联（也可以在idea里设置）

就是设置那个 .so 文件

```
ln -s /opt/hdf/HDFJava-3.3.2-Linux/HDF_Group/HDFJava/3.3.2/lib/libjhdf5.so.3.3.2 /usr/lib/libjhdf5.so
```

### 2.7.5. 将 .jar 导入到 maven 中

就是设置那个 .jar 文件

导入到maven中，你得先装好maven，注意安装的仓库哈

**<font color=#D81D4F > 在root用户下执行这条命令默认可是安装到/root/.m2下的 </font>**

所以要在普通用户下执行这条命令，而且不能加 sudo 

```
mvn install:install-file -Dfile=/opt/hdf/HDFJava-3.3.2-Linux/HDF_Group/HDFJava/3.3.2/lib/jarhdf5-3.3.2.jar -DgroupId=org.hdfgroup -DartifactId=hdf-java -Dversion=3.3.2 -Dpackaging=jar
```

### 2.7.6. 项目里pom的配置

```
        <dependency>
            <groupId>org.slf4j</groupId>
            <artifactId>slf4j-simple</artifactId>
            <version>2.0.5</version>
            <scope>compile</scope>
        </dependency>

        <dependency>
            <groupId>org.hdfgroup</groupId>
            <artifactId>hdf-java</artifactId>
            <version>3.3.2</version>
            <type>jar</type>

        <dependency>
            <groupId>io.jhdf</groupId>
            <artifactId>jhdf</artifactId>
            <version>0.6.9</version>
        </dependency>
```


# 3. idea 里的项目配置

## 3.1. maven选择

在设置里选择之前装的那个，比如这样子（使用 .mvn/maven.config 中的配置不要勾选）

![](https://cdn.jsdelivr.net/gh/gf9276/image/ncross_well_log/20230419220153.png)

## 3.2. java11要加个东西

```
--illegal-access=deny --add-opens java.base/java.lang=ALL-UNNAMED
```

![](https://cdn.jsdelivr.net/gh/gf9276/image/ncross_well_log/20230419220243.png)
