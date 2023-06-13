<!-- TOC -->

- [1. mysql.md](#1-mysqlmd)
- [2. wsl 下 mysql 安装](#2-wsl-下-mysql-安装)
  - [2.1. 开启systemd](#21-开启systemd)
  - [2.2. apt安装mysql](#22-apt安装mysql)
  - [2.3. 启动安全脚本提示符（别搞这个了，反正自己用的，这个规范太多了）](#23-启动安全脚本提示符别搞这个了反正自己用的这个规范太多了)
- [3. windows 下 mysql 安装](#3-windows-下-mysql-安装)
- [4. 一些常用操作和报错信息](#4-一些常用操作和报错信息)
  - [4.1. 命令行执行.sql文件](#41-命令行执行sql文件)
  - [4.2. 删除mysql（ubuntu-mysql8）](#42-删除mysqlubuntu-mysql8)
  - [4.3. 修改端口（ubuntu-mysql8）](#43-修改端口ubuntu-mysql8)
  - [4.4. 注册用户并且赋予权限](#44-注册用户并且赋予权限)
    - [4.4.1. 进入mysql 的 root用户](#441-进入mysql-的-root用户)
    - [4.4.2. 创建账户](#442-创建账户)
    - [4.4.3. 赋予全部权限](#443-赋予全部权限)
    - [4.4.4. 查看所有用户](#444-查看所有用户)
    - [4.4.5. 查看用户权限](#445-查看用户权限)
  - [4.5. 设置大小写不敏感（ubuntu-mysql8，这个麻烦）](#45-设置大小写不敏感ubuntu-mysql8这个麻烦)
    - [4.5.1. 备份数据库](#451-备份数据库)
    - [4.5.2. 停止Mysql服务](#452-停止mysql服务)
    - [4.5.3. 删除错误日记（主要是方便查看密码）](#453-删除错误日记主要是方便查看密码)
    - [4.5.4. 删除系统数据库与用户数据库](#454-删除系统数据库与用户数据库)
    - [4.5.5. 新建创建数据库目录 并且 授权](#455-新建创建数据库目录-并且-授权)
    - [4.5.6. 配置添加](#456-配置添加)
    - [4.5.7. 初始化mysql](#457-初始化mysql)
    - [4.5.8. 启动并修改密码](#458-启动并修改密码)
    - [4.5.9. 查看大小写不敏感设置是否成功](#459-查看大小写不敏感设置是否成功)
  - [4.6. Navicat for MySQL 1055报错](#46-navicat-for-mysql-1055报错)
    - [4.6.1. 问题](#461-问题)
    - [4.6.2. 解决方法](#462-解决方法)
  - [4.7. Navicat for MySQL 版本太低导致连接失败](#47-navicat-for-mysql-版本太低导致连接失败)
    - [4.7.1. 问题](#471-问题)
    - [4.7.2. 解决方法](#472-解决方法)

<!-- /TOC -->

# 1. mysql.md

mysql 学习

推荐书籍 《从0到1 MySQL即学即用》

至于视频，黑马和尚硅谷的应该差差不多吧，其实看书学的更快 =￣ω￣=

# 2. wsl 下 mysql 安装

建议先在windows上安装一个 ```Navicat for MySQL破解版``` 很好找的

[安装链接，不过版本太老了](https://www.cnblogs.com/zpy1993-09/p/14929860.html)

这是数据库可视化软件，缺点就是要收费

我是感觉这玩意比mysql workbench好用

它长这样：

![](https://cdn.jsdelivr.net/gh/gf9276/image/mysql/20230417102752.png)

用代理的话, 要么使用```sudo su -l```切换到root用户下开启代理后再执行安装指令, **root用户下命令前面就不要带sudo了**；要么修改sudo的配置文件，保留all_proxy之类的与代理相关的环境变量

**<font color=#D81D4F > 注意注意，所有wsl和win共用一个localhost噢，端口也是公用的，这也就意味着，如果你不改默认的mysql端口，那你一台电脑只能开一个mysql服务端！ </font>**

## 2.1. 开启systemd

wsl 要手动开启systemd

```
vim /etc/wsl.conf
```

在最后两行写入
```
[boot]
systemd=true
```

![](https://cdn.jsdelivr.net/gh/gf9276/image/mysql/20230417103345.png)

powershell里执行```wsl --shutdown```或者```wsl --terminate <Distribution Name>```后重新打开发行版，在发行版里输入指令```systemctl list-unit-files --type=service```进行验证

![](https://cdn.jsdelivr.net/gh/gf9276/image/mysql/20230417103616.png)


## 2.2. apt安装mysql

更新apt
```
apt update
```

服务端安装
```
apt install mysql-server
```

客户端安装(可选)
```
apt install mysql-client
```

安装的如果出现了这样子的报错，应该是端口冲突导致启动失败，**往下面翻，有修改端口**

![](https://cdn.jsdelivr.net/gh/gf9276/image/mysql/20230417104154.png)

安装成功了都是自启动的，使用
```
systemctl status mysql
```
可以查看状态

![](https://cdn.jsdelivr.net/gh/gf9276/image/mysql/20230613203813.png)


## 2.3. 启动安全脚本提示符（别搞这个了，反正自己用的，这个规范太多了）

[参考1](https://stackoverflow.com/questions/72334158/im-getting-an-error-when-i-ran-mysql-secure-installation)

[参考2（我选的都是y）](https://blog.csdn.net/weixin_43279138/article/details/126872698)

```
mysql_secure_installation
```

好像会报错，先进入mysql的root用户
```
mysql -u root
```

执行命令改密码
```
ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY 'mypasswordhere';
```

然后再执行启动安全脚本提示符就行了
```
mysql_secure_installation
```

之后登录需要密码
```
mysql -u root -p
```

# 3. windows 下 mysql 安装

看书 / 或者视频

如果要用wsl里的mysql，安装的时候一定要设置开机  **不**  自启动

如果已经开启了，需要在管理员模式下使用命令```net stop mysql80```停止mysql运行


# 4. 一些常用操作和报错信息

## 4.1. 命令行执行.sql文件

```
source /path/to/.sql
```

## 4.2. 删除mysql（ubuntu-mysql8）

```
apt purge mysql-* && rm -rf /etc/mysql/ /var/lib/mysql && apt autoremove && apt autoclean
```

## 4.3. 修改端口（ubuntu-mysql8）

* 修改文件 /etc/mysql/mysql.conf.d/mysqld.cnf

```
vim /etc/mysql/mysql.conf.d/mysqld.cnf
```
把里面的port改掉就行，**默认是3306，一般别改了**

![](https://cdn.jsdelivr.net/gh/gf9276/image/mysql/20230417104507.png)


* 重启mysql 

```
systemctl restart mysql
```

* 查看状态

```
systemctl status mysql
```

![](https://cdn.jsdelivr.net/gh/gf9276/image/mysql/20230417104744.png)

## 4.4. 注册用户并且赋予权限

[参考链接](https://www.yiibai.com/mysql/grant.html)

以这个为例子
```
用户名：nan
密码：nan416
```

### 4.4.1. 进入mysql 的 root用户

```
mysql -u root -p
```

### 4.4.2. 创建账户
```
CREATE USER 'nan'@'localhost' IDENTIFIED BY 'nan416';
```

### 4.4.3. 赋予全部权限

```
GRANT ALL ON *.* TO 'nan'@'localhost' WITH GRANT OPTION;
```

### 4.4.4. 查看所有用户

```
select user from mysql.user;
```

![](https://cdn.jsdelivr.net/gh/gf9276/image/mysql/20230417111759.png)

### 4.4.5. 查看用户权限

因为赋予了所有权限，所以下面指令显示出来有很多

```
SHOW GRANTS FOR nan@localhost;
```


## 4.5. 设置大小写不敏感（ubuntu-mysql8，这个麻烦）

ubuntu默认大小写敏感，和windows一样，导入win下的数据库可能会出现一点问题，所以这里改成大小写不敏感的，老版本Mysql直接写入配置文件就能解决，新版本还得删库再初始化

[参考链接1](https://stackoverflow.com/questions/53103588/lower-case-table-names-1-on-ubuntu-18-04-doesnt-let-mysql-to-start/63141850#63141850)

[参开链接2](https://bbs.huaweicloud.com/blogs/380008#:~:text=1%20%E5%A6%82%E6%9E%9C%E9%9C%80%E8%A6%81%E5%8C%BA%E5%88%86%E5%A4%A7%E5%B0%8F%E5%86%99%EF%BC%8C%E9%9C%80%E8%A6%81%E5%9C%A8mysqld.cnf%20%E4%B8%AD%E6%B7%BB%E5%8A%A0%E9%85%8D%E7%BD%AE%E6%9D%A5%E5%BF%BD%E7%95%A5%E5%A4%A7%E5%B0%8F%E5%86%99%EF%BC%8C%E5%91%BD%E4%BB%A4%E5%A6%82%E4%B8%8B%EF%BC%9A%20root%40ubuntu%3A%2Fetc%2Fmysql%23,vim%20.%2Fmysql.conf.d%2Fmysqld.cnf%202%20%E5%91%BD%E4%BB%A4%E8%BE%93%E5%85%A5%E5%90%8E%EF%BC%8C%E6%B7%BB%E5%8A%A0%E5%A6%82%E5%9B%BE%E9%85%8D%E7%BD%AE%EF%BC%8C%E4%BF%9D%E5%AD%98%E9%80%80%E5%87%BA)


### 4.5.1. 备份数据库

很重要，因为接下来会删库。。。所以最好还是建议在刚安装好的时候设置大小写不敏感

### 4.5.2. 停止Mysql服务

```
systemctl stop mysql
```

### 4.5.3. 删除错误日记（主要是方便查看密码）

```
rm /var/log/mysqld.log
```

### 4.5.4. 删除系统数据库与用户数据库

```
rm -rf /var/lib/mysql
```

### 4.5.5. 新建创建数据库目录 并且 授权

```
mkdir /var/lib/mysql
```

```
chown -R mysql:mysql /var/lib/mysql
```

重建前后，mysql文件夹权限对比

![](https://cdn.jsdelivr.net/gh/gf9276/image/mysql/20230417110046.png)

### 4.5.6. 配置添加

```
vim /etc/mysql/mysql.conf.d/mysqld.cnf
```

最后一行加上 `lower_case_table_names=1`，如下图


![](https://cdn.jsdelivr.net/gh/gf9276/image/mysql/20230417110235.png)

保存退出

### 4.5.7. 初始化mysql

我感觉这里不需要加basedir，因为我查到的basedir是下面这个

![](https://cdn.jsdelivr.net/gh/gf9276/image/mysql/20230417112822.png)

直接使用```--initialize-insecure```无密码初始化，等下再设置，如果用的是```--initialize```，去/var/log/mysql/error.log里找临时密码


~~```mysqld --defaults-file=/etc/mysql/my.cnf --initialize-insecure --user=mysql --basedir=/var/lib/mysql --datadir=/var/lib/mysql```~~

```
mysqld --defaults-file=/etc/mysql/my.cnf --initialize-insecure --user=mysql --datadir=/var/lib/mysql --console
```

### 4.5.8. 启动并修改密码

```
systemctl start mysql
```

```
mysql -u root
```

```
ALTER USER 'root'@'localhost' IDENTIFIED with mysql_native_password BY 'gf927621609';
```

```
flush privileges;
```

成功如下图所示

![](https://cdn.jsdelivr.net/gh/gf9276/image/mysql/20230417110857.png)

### 4.5.9. 查看大小写不敏感设置是否成功

```
show global variables like '%lower_case%';
```


0 表示敏感；1 表示不敏感；2 是mac下的，具体忘了

![](https://cdn.jsdelivr.net/gh/gf9276/image/mysql/20230417111008.png)

## 4.6. Navicat for MySQL 1055报错

[详细解决方法和原因参考这个](https://blog.csdn.net/zhuzicc/article/details/105990490)

### 4.6.1. 问题

在查询时使用group by语句，出现错误代码：1055；

### 4.6.2. 解决方法

```
vim /etc/mysql/mysql.conf.d/mysqld.cnf
```

最后一行添加

```
# 设置sql_mode,关闭ONLY_FULL_GROUP_BY,避免使用group by函数导致1055错误
sql_mode=STRICT_TRANS_TABLES,NO_ZERO_IN_DATE,NO_ZERO_DATE,ERROR_FOR_DIVISION_BY_ZERO,NO_ENGINE_SUBSTITUTION
```

## 4.7. Navicat for MySQL 版本太低导致连接失败

[参考链接](https://blog.csdn.net/yaoyaoyao_zhi/article/details/115255287)

[参考链接](https://blog.csdn.net/mcband/article/details/126091908#:~:text=navicat%E8%BF%9E%E6%8E%A5MySQL%20%E6%95%B0%E6%8D%AE%E6%97%B6%E9%81%87%E5%88%B01045%20%E9%94%99%E8%AF%AF%20%EF%BC%8C%E4%B8%80%E8%88%AC%E6%98%AF%E5%9B%A0%E4%B8%BA%E8%BE%93%E5%85%A5%E7%9A%84%E7%94%A8%E6%88%B7%E5%90%8D%E6%88%96%E8%80%85,%E5%AF%86%E7%A0%81%20%E8%A2%AB%E6%8B%92%E7%BB%9D%E8%AE%BF%E9%97%AE%EF%BC%8C%E6%AD%A4%E6%97%B6%E5%8F%AF%E4%BB%A5%E9%87%8D%E7%BD%AE%20MySQL%E6%95%B0%E6%8D%AE%E5%BA%93%20%E5%AF%86%E7%A0%81%20%E8%A7%A3%E5%86%B3%E3%80%82)

### 4.7.1. 问题

旧版本的navicat for mysql不支持新的加密方式

### 4.7.2. 解决方法

更改密码的加密方式

```
ALTER USER 'jack'@'localhost' IDENTIFIED WITH mysql_native_password BY 'jack4321';
```