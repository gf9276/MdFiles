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

```
sudo update-alternatives --config java
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