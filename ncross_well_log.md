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
    - [2.4.2. 安装 ant](#242-安装-ant)
      - [2.4.2.1. ant 的坑](#2421-ant-的坑)
    - [2.4.3. 安装 cmake](#243-安装-cmake)
    - [2.4.4. 安装其他依赖](#244-安装其他依赖)
    - [2.4.5. 下载 opencv 并解压](#245-下载-opencv-并解压)
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
    - [2.7.3. 安装 HDFJava](#273-安装-hdfjava)
    - [2.7.4. 动态库关联（也可以在 idea 里设置）](#274-动态库关联也可以在-idea-里设置)
    - [2.7.5. 将 .jar 导入到 maven 中](#275-将-jar-导入到-maven-中)
    - [2.7.6. 项目里 pom 的配置](#276-项目里-pom-的配置)
- [3. idea 里的项目配置](#3-idea-里的项目配置)
  - [3.1. maven 选择](#31-maven-选择)
  - [3.2. java11 要加个东西](#32-java11-要加个东西)

<!-- /TOC -->

# 1. ncross_well_log

**测井相关的代码，注意事项和环境搭建**

# 2. 环境搭建

有以下几个部分，**<font color=#D81D4F > 按照顺序来 </font>**

## 2.1. 代理问题

配置环境你可能会用到代理。如果你用的是代理，注意 **<font color=#D81D4F > sudo 命令不会带上环境变量，这也就意味着，使用 sudo apt 的时候，你的 apt 是不会走代理的，因为你的代理是通过全局变量 all_proxy 之类的指定的 </font>**。

用代理的话, 要么使用`sudo su -l`切换到 root 用户下开启代理后再执行安装指令, **root 用户下命令前面就不要带 sudo 了**；要么修改 sudo 的配置文件，保留 all_proxy 之类的与代理相关的环境变量。

## 2.2. java

### 2.2.1. 版本和注意事项

- 师兄和我说的 java8，但是我调试的时候有点问题，所以我目前用的 java11（已经解决了 wsl jdk8 的问题-2023.5.20）

- 建议把 java8 和 java11 都装了，反正可以选择

- 这里先安装 java8，等后面装完 opencv 再安装 java11。因为编译的时候 cmake 优先会选择 jkd11 不选择 jdk8，而 java11 貌似又太新了（不过实测 11 也没有问题）

### 2.2.2. 安装

看 [java.md](https://github.com/gf9276/MdFiles/blob/master/java.md)

## 2.3. maven

### 2.3.1. 版本和注意事项

- maven 3.9.1 （这个版本问题不大）

### 2.3.2. 安装

看 [maven.md](https://github.com/gf9276/MdFiles/blob/master/maven.md)

## 2.4. opencv

20230817 update:

    目前没用上，不用装了，我看这东西不爽很久了

---

老内容:

这个有点麻烦

[参考链接 1](https://zoyi14.smartapps.cn/pages/note/index?slug=90c542ddf54a&origin=share&_swebfr=1&_swebFromHost=heytapbrowser)

[参考链接 2](https://blog.csdn.net/qq_15737599/article/details/90200152)

[参考链接 3](https://blog.csdn.net/Undefinedefity/article/details/106180033)

### 2.4.1. 版本和注意事项

- opencv 4.5.3

- 其实代码里 opencv 用的很少，几乎没有

- 安装 opencv 需要 java8 或者 java11，不能只有 java 17, java 17 不带 jni 的, opencv 会编译失败的

- 如果同时存在 java8 和 java11，会优先选择 java11 的 jni，我暂时没法指定

**<font color=#D81D4F > 下面的一定按照顺序来，不然会错的 </font>**

### 2.4.2. 安装 ant

```
apt update
```

```
apt install ant
```

使用 `ant -version`检查是否安装成功

#### 2.4.2.1. ant 的坑

可能有坑，我在 22.04 没发现问题，但是在 20.04 上发现了

如果后面 cmake 编译文件显示找不到 ant 的话，大概率是软链接问题，执行该命令解决

```
ln -snf /usr/share/ant/bin/ant /bin/ant
```

[参考链接 1，提出问题](https://blog.csdn.net/quantum7/article/details/104625253)

[参考链接 2，解决问题](https://blog.csdn.net/quantum7/article/details/104625736)

### 2.4.3. 安装 cmake

```
apt install cmake
```

使用 `cmake --version`检查是否安装成功

### 2.4.4. 安装其他依赖

小心，sudo 会清除环境变量，导致代理无法使用

参数的具体含义要查一下，我忘了

```
apt install build-essential libgtk2.0-dev libavcodec-dev libavformat-dev libjpeg-dev libswscale-dev libtiff5-dev libgtk2.0-dev pkg-config
```

### 2.4.5. 下载 opencv 并解压

/usr/local/src ：这个目录是存放用户编译软件所用的源码的

存放到/usr/local/src/opencv 下（路径其实无所谓，这只是个需要编译的文件）

```
mkdir /usr/local/src/opencv
```

```
cd /usr/local/src/opencv
```

```
wget https://github.com/opencv/opencv/archive/4.5.3.zip
```

解压，没有 unzip 的需要先执行`apt install zip unzip`

```
unzip 4.5.3.zip && rm 4.5.3.zip
```

### 2.4.6. 下载 IPPICV 并配置 （你要是能用代理，能翻墙，这个不需要）

IPPICV 下载到这个路径下

```
cd /usr/local/src/opencv
```

- 下载压缩包

对应的 github 地址：https://github.com/opencv/opencv_3rdparty.git

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

- 改一下 ippv 的地址

```
vim /usr/local/src/opencv/opencv-4.5.3/3rdparty/ippicv/ippicv.cmake
```

光标在的那一行，改成 `file:/usr/local/src/opencv/opencv_3rdparty-ippicv-master_20191018/ippicv/`

![](https://cdn.jsdelivr.net/gh/gf9276/image/ncross_well_log/20230417121913.png)

### 2.4.7. 准备开始编译

- 创建 build 文件夹

```
cd /usr/local/src/opencv/opencv-4.5.3 && mkdir build && cd build
```

- 开始 cmake

这里应该要用 java8，这里有点奇怪，不管我怎么设置 java 版本，这玩意用的都是 java11，不过 java17 是没有 JNI 的，但是 JAVA8 怎么回事，打不过 11，难绷

```
cmake -D CMAKE_BUILD_TYPE=Release -D CMAKE_INSTALL_PREFIX=/usr/local -D  OPENCV_GENERATE_PKGCONFIG=ON -D OPENCV_ENABLE_NONFREE=True ..
```

这样子成功

![](https://cdn.jsdelivr.net/gh/gf9276/image/ncross_well_log/20230417122224.png)

- 开始编译

j8 是线程数量，cpu 线程数量少一点的改成 j4，前面用 java11 编译的，**这里会报一点错，不知道有没有影响，emmmmm....**

```
make -j8
```

- 开始安装

```
make install
```

```
make install 指令的默认安装路径为：
/usr/local/bin - executable files
/usr/local/lib - libraries (.so)
/usr/local/cmake/opencv4 - cmake package
/usr/local/include/opencv4 - headers
/usr/local/share/opencv4 - other files (e.g. trained cascades in XML format)
```

### 2.4.8. 配置环境

- 修改 /etc/ld.so.conf

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

wsl 下会报错，有两个文件一样的，把其中一个改成软链接就好了（[听说是无害的，还是别管了](https://superuser.com/questions/1707681/wsl-libcuda-is-not-a-symbolic-link) ——20230417）

![](https://cdn.jsdelivr.net/gh/gf9276/image/java/20230331234339.png)

[另外一个讨论](https://github.com/microsoft/WSL/issues/5663)

- 修改/etc/bash.bashrc

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

- 看看有没有.jar 文件

```
ls /usr/local/src/opencv/opencv-4.5.3/build/bin/
```

- 这里需要把.jar 文件复制到项目下面，在项目下创建一个 opencv 文件夹，然后直接复制过去

```
mkdir ~/JavaFiles/ncross_well_log/n3rdParty/opencv
```

```
cd ~/JavaFiles/ncross_well_log/n3rdParty/opencv
```

```
cp /usr/local/src/opencv/opencv-4.5.3/build/bin/opencv-453.jar ./
```

- 如果 idea 执行代码显示 opencv 不在 java 库中，可以执行（或者在 idea 里面指定）

```
cp -r /usr/local/src/opencv/opencv-4.5.3/build/lib/libopencv_java453.so /usr/lib
```

## 2.5. mysql

### 2.5.1. 版本和注意事项

- mysql 8.0.32 （这个版本问题不大）
- 不要安全安装！安全安装对密码长度有限制，这样子还得改代码里的配置，麻烦。
- 需要创建用户 nan ，密码 nan416，并赋予所有权限（方便开发）
- 需要设置大小写不敏感（low_case_table_names = 1）
- 端口使用 3306

### 2.5.2. 安装

看 [mysql.md](https://github.com/gf9276/MdFiles/blob/master/mysql.md)

## 2.6. minio

### 2.6.1. 版本和注意事项

- minio RELEASE.2023-03-24T21-41-23Z （这个版本问题不大）

### 2.6.2. 安装

看 [minio.md](https://github.com/gf9276/MdFiles/blob/master/minio.md)

## 2.7. hdf5

这个找了我挺久的。

傻子才用 java 做数据处理呢。哥们直接用 Python 上面的 h5py 处理数据，然后 java 里面直接调用 py 脚本了。不用这玩意了，写起来真 tm 费劲 -- 2023/6/14。

### 2.7.1. 版本和注意事项

- HDFJava 3.3.2

- jhdf 0.6.9

- 只用安装 HDFJava，jhdf 之类的用 maven 坐标就可以直接导入了

### 2.7.2. 建议

读文件用 jhdf，直接用 maven 坐标就可以了。有点编码问题，不过 API 很好用

写文件用 hdf，需要手动下载配置。用起来很麻烦，但 jhdf 只能读不能写，所以必须用这个

[官方示例](https://portal.hdfgroup.org/display/HDF5/Examples+from+Learning+the+Basics)

[hdfview：windows 下看 hdf5 文件的软件](https://portal.hdfgroup.org/display/support/HDFView+3.1.4#files)

[hdf5 文档](https://portal.hdfgroup.org/display/HDF5/HDF5%20javadocs)

[这个也是手册，但是感觉乱七八糟的](http://web.mit.edu/fwtools_v3.1.0/www/H5.intro.html#Intro-APIs)

[参考链接 1，对安装有整体的概念](https://blog.csdn.net/luoying_1993/article/details/72979022)

[参考链接 2，用于补充官方文档](https://blog.csdn.net/shyjhyp11/article/details/109011262)

[参考连接 3，纯代码，一般般](https://www.cnblogs.com/Husir-boky/p/16855534.html)

[参考链接 4，纯代码，一般般](https://blog.csdn.net/qq_40298231/article/details/99549532)

[讨论：老版本和新版本使用上的区别](https://stackoverflow.com/questions/47753363/hdf5-in-maven-project)

### 2.7.3. 安装 HDFJava

好像需要三个.jar 和一个.so

1. jarhdf5-3.2.1.jar
2. slf4j-api-1.7.5.jar
3. slf4j-nop-1.7.5.jar
4. libjhdf5.so

其中两个.jar（2 和 3）可以通过 maven 坐标直接下载，剩下的一个.jar（1）和.so（4）需要自己装了

[下载地址](https://support.hdfgroup.org/products/java/release/download.html#script)

我找了很久，只找到了 centos，不过能用。。。

- 下载 hdf

```
cd /opt && wget http://support.hdfgroup.org/ftp/HDF5/releases/HDF-JAVA/hdfjni-3.3.2/bin/HDFJava-3.3.2_Stat-centos7_64.tar.gz
```

- 解压缩并且运行脚本

这里赋予权限应该不需要 -R，我只要能进 hdf 文件夹就行，其他的我只要能读就行

```
tar -zxvf HDFJava-3.3.2_Stat-centos7_64.tar.gz && rm HDFJava-3.3.2_Stat-centos7_64.tar.gz && chmod 755 /opt/hdf
```

```
cd /opt/hdf && ./HDFJava-3.3.2-Linux.sh
```

.jar 和.so 在这个目录下面

```
cd /opt/hdf/HDFJava-3.3.2-Linux/HDF_Group/HDFJava/3.3.2/lib && ls -ahl
```

### 2.7.4. 动态库关联（也可以在 idea 里设置）

就是设置那个 .so 文件

```
ln -s /opt/hdf/HDFJava-3.3.2-Linux/HDF_Group/HDFJava/3.3.2/lib/libjhdf5.so.3.3.2 /usr/lib/libjhdf5.so
```

### 2.7.5. 将 .jar 导入到 maven 中

就是设置那个 .jar 文件

导入到 maven 中，你得先装好 maven，注意安装的仓库哈

**<font color=#D81D4F > 在 root 用户下执行这条命令默认可是安装到/root/.m2 下的 </font>**

所以要在普通用户下执行这条命令，而且不能加 sudo

```
mvn install:install-file -Dfile=/opt/hdf/HDFJava-3.3.2-Linux/HDF_Group/HDFJava/3.3.2/lib/jarhdf5-3.3.2.jar -DgroupId=org.hdfgroup -DartifactId=hdf-java -Dversion=3.3.2 -Dpackaging=jar
```

### 2.7.6. 项目里 pom 的配置

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

## 3.1. maven 选择

在设置里选择之前装的那个，比如这样子（使用 .mvn/maven.config 中的配置不要勾选）

![](https://cdn.jsdelivr.net/gh/gf9276/image/ncross_well_log/20230419220153.png)

## 3.2. java11 要加个东西

```
--illegal-access=deny --add-opens java.base/java.lang=ALL-UNNAMED
```

![](https://cdn.jsdelivr.net/gh/gf9276/image/ncross_well_log/20230419220243.png)
