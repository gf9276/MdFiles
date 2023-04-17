<!-- TOC -->

- [1. maven.md](#1-mavenmd)
- [2. 安装](#2-安装)
  - [2.1. 下载指定版本的Maven到 /opt/maven 目录下](#21-下载指定版本的maven到-optmaven-目录下)
  - [2.2. 解压并删除多余的压缩包](#22-解压并删除多余的压缩包)
  - [2.3. 添加环境变量](#23-添加环境变量)
  - [2.4. 刷新环境变量并查看版本](#24-刷新环境变量并查看版本)
  - [2.5. 添加阿里源](#25-添加阿里源)
- [3. 一些操作](#3-一些操作)
  - [3.1. 手动导入maven包的方法](#31-手动导入maven包的方法)

<!-- /TOC -->

# 1. maven.md

安装和学习的资料

B站看尚硅谷或者黑马

# 2. 安装

用代理的话, 要么使用```sudo su -l```切换到root用户下开启代理后再执行安装指令, **root用户下命令前面就不要带sudo了**；要么修改sudo的配置文件，保留all_proxy之类的与代理相关的环境变量

已经写了脚本，地址 https://github.com/gf9276/ShFiles.git ，直接运行

[参考链接1](https://cloud.tencent.com/developer/article/1649751#:~:text=%E5%9C%A8%20Ubuntu%20%E4%BD%BF%E7%94%A8%20apt%20%E5%AE%89%E8%A3%85%20Maven%20%E9%9D%9E%E5%B8%B8%E7%AE%80%E5%8D%95%E7%9B%B4%E6%8E%A5%E3%80%82%20%E5%8D%87%E7%BA%A7%E8%BD%AF%E4%BB%B6%E5%8C%85%E7%B4%A2%E5%BC%95%EF%BC%8C%E5%B9%B6%E4%B8%94%E8%BE%93%E5%85%A5%E4%B8%8B%E9%9D%A2%E7%9A%84%E5%91%BD%E4%BB%A4%EF%BC%8C%E5%AE%89%E8%A3%85,maven%20%E6%83%B3%E8%A6%81%E9%AA%8C%E8%AF%81%E5%AE%89%E8%A3%85%E6%98%AF%E5%90%A6%E6%88%90%E5%8A%9F%EF%BC%8C%E8%BF%90%E8%A1%8C%20mvn%20-version%20%EF%BC%9A%20mvn%20-version%20%E8%BE%93%E5%87%BA%E7%9C%8B%E8%B5%B7%E6%9D%A5%E5%83%8F%E4%B8%8B%E9%9D%A2%E8%BF%99%E6%A0%B7%EF%BC%9A)

[参考链接2](https://blog.csdn.net/weixin_45428910/article/details/127956322)

[参考链接3](https://www.cnblogs.com/chinda/p/14297338.html)


## 2.1. 下载指定版本的Maven到 /opt/maven 目录下

```
mkdir /opt/maven && cd /opt/maven && wget https://dlcdn.apache.org/maven/maven-3/3.9.1/binaries/apache-maven-3.9.1-bin.tar.gz
```

## 2.2. 解压并删除多余的压缩包

```
cd /opt/maven && tar -zxvf apache-maven-3.9.1-bin.tar.gz && rm apache-maven-3.9.1-bin.tar.gz
```

## 2.3. 添加环境变量

方法一、
```
vim /etc/profile
```

在最后面加入

```
export M2_HOME=/opt/maven/apache-maven-3.9.1
export MAVEN_HOME=$M2_HOME
export PATH=${M2_HOME}/bin:${PATH}
```

方法二、
```
touch /etc/profile.d/my_maven_cfg.sh
```

打开文件 my_maven_cfg.sh
```
vim /etc/profile.d/my_maven_cfg.sh
```

写入环境变量
```
export M2_HOME=/opt/maven/apache-maven-3.9.1
export MAVEN_HOME=$M2_HOME
export PATH=${M2_HOME}/bin:${PATH}
```

## 2.4. 刷新环境变量并查看版本

```
. /etc/profile
```

```
. .bashrc # 这条是为了恢复用户环境
```

```
mvn -v
```

mvn -v 里会显示java版本，这个版本是跟着环境变量 JAVA_HOME 的

如下图：

![](https://cdn.jsdelivr.net/gh/gf9276/image/java/20230416232240.png)

## 2.5. 添加阿里源

写在前面，如果想用代理而不是阿里源的话，需要在配置文件里配置具体代理（TODO）


修改配置文件
```
vim /opt/maven/apache-maven-3.9.1/conf/settings.xml
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


# 3. 一些操作

## 3.1. 手动导入maven包的方法

阿里源有时候找不到，这时候我们就需要去 https://mvnrepository.com/artifact/org.geotools/gt-geojson 手动找了，找到了再手动导入maven库里，唉，要是没有墙就好了。

* **[手动导入maven包的方法](https://blog.csdn.net/nickDaDa/article/details/105674344)**

```
mvn install:install-file -Dfile=.\jhdf5-14.12.6.jar -DgroupId=cisd -DartifactId=jhdf5 -Dversion=14.12.6 -Dpackaging=jar
```

```
命令中的-Dfile指向文件路径，-DgroupId指向dependency.groupId，-DartifactId指dependency.artifactId，-Dversion指向dependency.version，-Dpackaging指的是包类型。

说的简单点, -DgroupId, -DartifactId, -Dversion就是maven包的三个坐标啦
```