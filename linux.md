# 一些指令

## ubuntu

### 上下键补全历史指令

CentOS下，有一个很智能的功能，就是只输入一条历史命令的前几个字母，再按PageUp和PageDown键，就可以在以此字母为前缀的历史命令中上下切换。这个功能非常实用，而且比CTRL+R使用起来更友善、更方便。遗憾的是，ubuntu上并没有这个功能。Google上搜索才直到，这个只是linux在终端对键盘的映射而已，和linux的某个发行版无关。只是CentOS下默认打开了这个功能，而ubuntu默认禁止了而已。

输入指令，修改一下文件
```
sudo vim /etc/inputrc
```

如图，删除两行的注释
![](https://cdn.jsdelivr.net/gh/gf9276/image/linux/20221112163803.png)


5~ 改 A，6~ 改 B，即可

![](https://cdn.jsdelivr.net/gh/gf9276/image/linux/20221112164130.png)

### 复制命令

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

### 移动命令

```
mv IfElseTest1.java chapter03/
```

### 查看文件夹下的文件数

```
ls -l | grep "-" | wc -l
```

### whereis which


### 中文，设置中文

* 先更新，后面还是会用到的
```
sudo apt update
```

* 然后安装locales, wsl 的 ubuntu 有的，这一步跳过

```
sudo apt install locales
```

* 区域设置
```
sudo dpkg-reconfigure locales
```

出现如下所示，这个界面是让你添加可选时区用的，选三个（空格选中），分别是```en_US.UTF-8```、```zh_CN.GBK```、```zh_CH.UTF-8``` （选几个无所谓的，万一用上了呢），然后回车确定。
![](https://cdn.jsdelivr.net/gh/gf9276/image/linux/20230331202709.png)

然后出现，这个才是让你选择默认时区的，中文一般选择```zh_CH.UTF-8```
![](https://cdn.jsdelivr.net/gh/gf9276/image/linux/20230331203242.png)

* 安装语言包和字体，好像只用装一个？

```
sudo apt install language-pack-zh-hans && sudo apt install ttf-wqy-microhei ttf-wqy-zenhei xfonts-intl-chinese
```

### 安装gedit，反正不大，安装玩玩

* 执行指令 ```sudo apt-get update && sudo apt install gedit -y```

打开会有警告，无伤大雅
![](https://cdn.jsdelivr.net/gh/gf9276/image/linux/20230331205033.png)

输入法安装好麻烦，我懒得装了，开玩笑的

### 安装搜狗拼音（没成功）

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