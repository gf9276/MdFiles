
<!-- TOC -->

- [1. ncross\_well\_log](#1-ncross_well_log)
- [2. 环境搭建](#2-环境搭建)
  - [2.1. java](#21-java)
  - [2.2. maven](#22-maven)
  - [2.3. opencv](#23-opencv)
    - [2.3.1. 安装ant](#231-安装ant)
    - [2.3.2. 安装cmake](#232-安装cmake)
    - [2.3.3. 安装其他依赖](#233-安装其他依赖)
    - [2.3.4. 下载opencv并解压](#234-下载opencv并解压)
    - [2.3.5. 下载 IPPICV 并配置 （你要是能上外网，这个不需要）](#235-下载-ippicv-并配置-你要是能上外网这个不需要)
    - [2.3.6. 准备开始编译](#236-准备开始编译)
    - [2.3.7. 配置环境](#237-配置环境)
    - [2.3.8. 检查安装是否成功](#238-检查安装是否成功)
  - [2.4. mysql](#24-mysql)
  - [2.5. minio](#25-minio)
    - [2.5.1. 下载](#251-下载)
    - [2.5.2. 创建静态文件放置位置](#252-创建静态文件放置位置)
    - [2.5.3. 启动minio（不用这个）](#253-启动minio不用这个)
    - [2.5.4. 写入配置（准备供systemd调用）](#254-写入配置准备供systemd调用)
    - [2.5.5. 设置开机自启动](#255-设置开机自启动)
  - [2.6. hdf5](#26-hdf5)
    - [2.6.1. 建议](#261-建议)
    - [2.6.2. 安装和配置](#262-安装和配置)

<!-- /TOC -->


# 1. ncross_well_log

**测井相关的代码，注意事项和环境搭建**

**<font color=#D81D4F > 如果你用的是代理，注意！！！ </font>**

**<font color=#D81D4F > sudo命令不会带上环境变量，这也就意味着，使用sudo apt的时候，你的apt是不会走代理的，因为你的代理是通过全局变量all_proxy之类的指定的 </font>**

**<font color=#D81D4F > 要想使用代理，可以使用sudo su命令成为root用户，重新开启代理后执行安装命令，安装完之后再ctrl+d退出 </font>**

**<font color=#D81D4F > 要不是我突然发现 wget和sudo wget速度怎么差的这么大，我还不知道 </font>**


# 2. 环境搭建

有以下几个部分，**<font color=#D81D4F > 按照顺序来 </font>**

## 2.1. java

看 [java.md](https://github.com/gf9276/MdFiles/blob/master/java.md)

## 2.2. maven

看 [maven.md](https://github.com/gf9276/MdFiles/blob/master/maven.md)

## 2.3. opencv

麻烦死了

注意，安装Opencv不能只有java 17, java 17不带jni的, opencv会编译失败的

提前制定好java的版本，编译的时候会用的，选11或者8

最好是java8，但是我为什么一直默认java11，日了，可能编译要加java路径，md，他自己不会去读 JAVA_HOME 的吗，cccccc

用代理的话, 最好是sudo su（有root密码的话直接su更好）切换到root用户下再执行下面指令, **<font color=#D81D4F > 这时候一定要去掉命令前面的sudo </font>**

[参考链接1](https://zoyi14.smartapps.cn/pages/note/index?slug=90c542ddf54a&origin=share&_swebfr=1&_swebFromHost=heytapbrowser)

[参考链接2](https://blog.csdn.net/qq_15737599/article/details/90200152)

[参考链接3](https://blog.csdn.net/Undefinedefity/article/details/106180033)

### 2.3.1. 安装ant

```
apt update
```

```
apt install ant
```

使用 ```ant -version```检查是否安装成功


### 2.3.2. 安装cmake

```
apt install cmake
```

使用 ```cmake --version```检查是否安装成功


### 2.3.3. 安装其他依赖

小心，sudo会清除环境变量，导致代理无法使用

参数的具体含义要查一下，我忘了

```
apt install build-essential libgtk2.0-dev libavcodec-dev libavformat-dev libjpeg-dev libswscale-dev libtiff5-dev libgtk2.0-dev pkg-config
```

### 2.3.4. 下载opencv并解压

存放到/opt/opencv下（路径其实无所谓，这只是个需要要编译的文件）

```
mkdir /opt/opencv
```

```
cd /opt/opencv
```

```
wget https://github.com/opencv/opencv/archive/4.5.3.zip
```

解压，没有unzip的需要apt install zip unzip
```
unzip 4.5.3.zip && rm -rf 4.5.3.zip
```

### 2.3.5. 下载 IPPICV 并配置 （你要是能上外网，这个不需要）

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

下载完是这样子的

![](https://cdn.jsdelivr.net/gh/gf9276/image/ncross_well_log/20230417120646.png)

* 改一下ippv的地址

```
vim /opt/opencv/opencv-4.5.3/3rdparty/ippicv/ippicv.cmake
```

光标在的那一行，改成 ```file:/opt/opencv/opencv_3rdparty-ippicv-master_20191018/ippicv/```

![](https://cdn.jsdelivr.net/gh/gf9276/image/ncross_well_log/20230417121913.png)

### 2.3.6. 准备开始编译

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

j8是线程数量，cpu线程数量少一点的改成j4，前面用java11编译的，这里会报一点错，不知道有没有影响

```
make -j8
```

* 开始安装

```
make install
```

### 2.3.7. 配置环境

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
sudo ldconfig
```
会报错，有两个文件一样的，把其中一个改成软连接就好了（不用改了--20230417）
![](https://cdn.jsdelivr.net/gh/gf9276/image/java/20230331234339.png)

[听说是无害的，还是别管了](https://superuser.com/questions/1707681/wsl-libcuda-is-not-a-symbolic-link)

[另外一个讨论](https://github.com/microsoft/WSL/issues/5663)

* 修改/etc/bash.bashrc 
```
vim /etc/bash.bashrc 
```

最后加上

```
PKG_CONFIG_PATH=$PKG_CONFIG_PATH:/usr/local/lib/pkgconfig
export PKG_CONFIG_PATH
```

然后
```
source /etc/bash.bashrc
```

### 2.3.8. 检查安装是否成功

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

* 如果idea执行代码显示opencv不在java库中，可以执行（或者在idea里面指定）

```
cp -r /opt/opencv/opencv-4.5.3/build/lib/libopencv_java453.so /usr/lib
```

## 2.4. mysql

看 [mysql.md](https://github.com/gf9276/MdFiles/blob/master/mysql.md)

**注意，按照代码里的配置**

* 需要创建用户 nan ，密码 nan416，并赋予所有权限（我嫌麻烦，一次给全得了）
* 需要设置大小写不敏感
* 端口使用 3306


## 2.5. minio

### 2.5.1. 下载

```
wget https://dl.min.io/server/minio/release/linux-amd64/minio
```

挪到/usr/local/bin/下面，其实我更喜欢/opt，但是别人这么干的，我先顺从他，哈哈
```
sudo cp minio /usr/local/bin/ && sudo chmod +x /usr/local/bin/minio
```

### 2.5.2. 创建静态文件放置位置

```
sudo mkdir /data
```

### 2.5.3. 启动minio（不用这个）

```
sudo minio server /data --console-address ":9099"
```

### 2.5.4. 写入配置（准备供systemd调用）

```
sudo vim /etc/default/minio
```

```
# 指定数据存储目录(注意：这个目录要存在且拥有相对应的权限)
MINIO_VOLUMES="/data"

# 监听端口
MINIO_OPTS="--address :9000 --console-address :9099"

# 老版本使用MINIO_ACCESS_KEY/MINIO_SECRET_KEY，新版本已不建议使用
# Access key (账号)
# MINIO_ACCESS_KEY="minioadmin"
# Secret key (密码)
# MINIO_SECRET_KEY="minioadmin"

# 新版本使用；指定默认的用户名和密码，其中用户名必须大于3个字母，否则不能启动
MINIO_ROOT_USER="minio"
MINIO_ROOT_PASSWORD="miniominio"

# 区域值，标准格式是“国家-区域-编号”，
MINIO_REGION="cn-north-1"

# 域名
# MINIO_DOMAIN=minio.your_domain.com
```

### 2.5.5. 设置开机自启动

```
sudo vim /usr/lib/systemd/system/minio.service
```

```
[Unit]
Description=MinIO
Documentation=https://docs.min.io
Wants=network-online.target
After=network-online.target
AssertFileIsExecutable=/usr/local/bin/minio
[Service]
WorkingDirectory=/usr/local/

ProtectProc=invisible

# 指向3.1节中的配置文件
EnvironmentFile=/etc/default/minio

ExecStartPre=/bin/bash -c "if [ -z \"${MINIO_VOLUMES}\" ]; then echo \"Variable MINIO_VOLUMES not set in /etc/default/minio\"; exit 1; fi"
ExecStart=/usr/local/bin/minio server $MINIO_OPTS $MINIO_VOLUMES

# Let systemd restart this service always
Restart=always

# Specifies the maximum (1M) file descriptor number that can be opened by this process
LimitNOFILE=1048576

# Specifies the maximum number of threads this process can create
TasksMax=infinity

# Disable timeout logic and wait until process is stopped
TimeoutStopSec=infinity
SendSIGKILL=no
SuccessExitStatus=0

[Install]
WantedBy=multi-user.target
Alias=minio.service
```

```
# 重新加载服务配置文件，使服务生效
sudo systemctl daemon-reload

# 将服务设置为开机启动
sudo systemctl enable minio

# 服务立即启动
sudo systemctl start minio

# 查看minio服务当前状态
sudo systemctl status minio

```

具体的配置有待继续

## 2.6. hdf5

这个找了我挺久的，日了

### 2.6.1. 建议

读文件用 jhdf

写文件用 hdf

### 2.6.2. 安装和配置

[下载地址](https://support.hdfgroup.org/products/java/release/download.html#script)

我找了很久，只找到了centos，不过能用。。。

* 安装hdf
```
cd /opt && wget http://support.hdfgroup.org/ftp/HDF5/releases/HDF-JAVA/hdfjni-3.3.2/bin/HDFJava-3.3.2_Stat-centos7_64.tar.gz
```

我懒，直接赋予最高权限了。。。
```
tar -zxvf HDFJava-3.3.2_Stat-centos7_64.tar.gz && rm HDFJava-3.3.2_Stat-centos7_64.tar.gz && chmod -R 777 /opt/hdf
```

```
cd /opt/hdf && ./HDFJava-3.3.2-Linux.sh
```

```
cd /opt/hdf/HDFJava-3.3.2-Linux/HDF_Group/HDFJava/3.3.2/lib
```

* 导入到maven中，你得先装好maven，注意安装的仓库哈，不要安装到root用户下的.m2里了

```
mvn install:install-file -Dfile=./jarhdf5-3.3.2.jar -DgroupId=org.hdfgroup -DartifactId=hdf-java -Dversion=3.3.2 -Dpackaging=jar
```

* pom配置

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