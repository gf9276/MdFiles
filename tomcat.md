# tomcat.md

安装tomcat

# 安装

## 获得安装包

[tomcat10官网链接](https://tomcat.apache.org/download-10.cgi)

选择如下安装包：

![](https://cdn.jsdelivr.net/gh/gf9276/image/tomcat/QQ图片20240318230631.png)

当然，也可以直接在命令行中执行如下命令（注意，链接可能会变动，所以最好还是去官网下载）：

```
wget https://dlcdn.apache.org/tomcat/tomcat-10/v10.1.19/bin/apache-tomcat-10.1.19.tar.gz
```

## 移动安装包并解压缩

移动安装包到个人用户目录下，也可以移动到/usr/local/bin之类的目录下，不过权限有点麻烦，所以还是个人用户目录吧

```
mkdir ~/tomcat && mv apache-tomcat-10.1.19.tar.gz ~/tomcat/ && cd ~/tomcat
```

解压缩并删除压缩包

```
tar -zxvf apache-tomcat-10.1.19.tar.gz
```

可以删除压缩包

```
rm -rf apache-tomcat-10.1.19.tar.gz
```


# 启动tomcat

```
cd ~/tomcat/apache-tomcat-10.1.19/bin && ./startup.sh
```

