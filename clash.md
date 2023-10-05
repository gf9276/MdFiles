<!-- TOC -->

- [1. clash.md](#1-clashmd)
- [2. 代理设置](#2-代理设置)
  - [2.1. 注意事项](#21-注意事项)
  - [屏蔽设置（随时更新）](#屏蔽设置随时更新)
  - [2.2. windows](#22-windows)
    - [2.2.1. 下载 clash for windows](#221-下载-clash-for-windows)
    - [2.2.2. 下载中文包](#222-下载中文包)
  - [2.3. wsl2](#23-wsl2)
    - [2.3.1. 在 ~/ 下写一个脚本：](#231-在--下写一个脚本)
    - [2.3.2. 配置环境变量](#232-配置环境变量)
    - [2.3.3. 可用指令](#233-可用指令)
  - [2.4. linux 服务器](#24-linux-服务器)
    - [2.4.1. 下载内核](#241-下载内核)
    - [2.4.2. 设置](#242-设置)
    - [2.4.3. 使用](#243-使用)
    - [2.4.4. 测试](#244-测试)
    - [2.4.5. 关闭](#245-关闭)

<!-- /TOC -->

# 1. clash.md

记录一下设置代理

# 2. 代理设置

## 2.1. 注意事项

非常重要！！！非常重要！！！非常重要！！！非常重要！！！非常重要！！！非常重要！！！

**如果没有在clash里设置的话，clash是默认 mix_port=7890，就是http和socks都是7890端口的**

**所以写代码的时候不要自作聪明，http的端口用7890，socks的端口用7891，简直蠢爆了**

![](https://cdn.jsdelivr.net/gh/gf9276/image/clash/20230905142206.png)


## 屏蔽设置（随时更新）

bypass:
  - "*.cn*" # 国内网站
  - "*gitee.*" # gitee
  - "*.baidu.*" # 百度
  - "*.steamcontent.*" # steam下载
  - "*.bilibili.*" # bilibili
  - localhost # 本地
  - 127.* # 本地
  - 10.* # 服务器
  - 172.16.*
  - 172.17.*
  - 172.18.*
  - 172.19.*
  - 172.20.*
  - 172.21.*
  - 172.22.*
  - 172.23.*
  - 172.24.*
  - 172.25.*
  - 172.26.*
  - 172.27.*
  - 172.28.*
  - 172.29.*
  - 172.30.*
  - 172.31.*
  - 192.168.*
  - <local>
  


## 2.2. windows

### 2.2.1. 下载 clash for windows
直接安装 [clash for windows](https://github.com/Fndroid/clash_for_windows_pkg/releases) 

找一个合适版本的便携版就行了，比如：

![](https://cdn.jsdelivr.net/gh/gf9276/image/clash/QD]A0KGPUJ0SGMM2AZITYWM.png)

### 2.2.2. 下载中文包

[clahs for windows 中文包](https://github.com/BoyceLig/Clash_Chinese_Patch/releases/)

按照他的提示替换对应文件

至于代理，自己找

## 2.3. wsl2

先设置 windows for clash，（主要是允许局域网连接）
![](https://cdn.jsdelivr.net/gh/gf9276/image/clash/20221120134915.png)

局域网连接里可以看到wsl的IP地址，此时不打开win10的系统代理按钮，wsl2也可以上外网，只需要把clash挂在后台就行了
![](https://cdn.jsdelivr.net/gh/gf9276/image/clash/20230223152410.png)


wsl2是依托于windows的，具体的config应该就是按照windows走的

### 2.3.1. 在 ~/ 下写一个脚本：

**<font color=#D81D4F >注意：是 ~/ 路径下</font>**

```
cd ~/ && touch my_proxy.sh
```

编辑 my_proxy.sh

```
vi my_proxy.sh
```

<details>
<summary>写入内容该代码块所示</summary>

[最新内容参考这里](https://github.com/gf9276/ShFiles/blob/main/my_proxy.sh)

```
#!/bin/sh

hostip=$(cat /etc/resolv.conf | grep nameserver | awk '{ print $2 }')
wslip=$(hostname -I | awk '{print $1}')
port=7890
socks_port=7890 # 看clash的配置，混合模式默认用的确实是7890

PROXY_HTTP="http://${hostip}:${port}"
PROXY_SOCKS="socks5h://${hostip}:${socks_port}"

set_proxy() {
  export all_proxy="${PROXY_SOCKS}"
  export ALL_PROXY="${PROXY_SOCKS}"
  export https_proxy="${PROXY_HTTP}"
  export HTTPS_PROXY="${PROXY_HTTP}"
  export http_proxy="${PROXY_HTTP}"
  export HTTP_PROXY="${PROXY_HTTP}"
  export no_proxy="localhost, 127.0.0.0/8, ::1, ${hostip}, ${wslip}"
  export NO_PROXY="localhost, 127.0.0.0/8, ::1, ${hostip}, ${wslip}"

  git config --global http.https://github.com.proxy ${PROXY_SOCKS}
  git config --global https.https://github.com.proxy ${PROXY_SOCKS}

  echo "Proxy has been opened."
}

unset_proxy() {
  unset ALL_PROXY
  unset all_proxy
  unset HTTPS_PROXY
  unset https_proxy
  unset HTTP_PROXY
  unset http_proxy
  unset NO_PROXY
  unset no_proxy

  git config --global --unset http.https://github.com.proxy
  git config --global --unset https.https://github.com.proxy

  echo "Proxy has been closed."
}

test_setting() {
  echo "Host IP:" ${hostip}
  echo "WSL IP:" ${wslip}
  echo "Try to connect to Google..."
  resp=$(curl -I -s --connect-timeout 5 -m 5 -w "%{http_code}" -o /dev/null www.google.com)
  if [ ${resp} = 200 ]; then
    echo "Proxy setup succeeded!"
  else
    echo "Proxy setup failed!"
    echo "反正都没用, 帮你关上"
    unset_proxy
  fi
}

v1=${1:-"set"} # 设置默认为set，即开启
if [ "$v1" = "set" ]; then
  set_proxy

elif [ "$v1" = "unset" ]; then
  unset_proxy

elif [ "$v1" = "test" ]; then
  test_setting
else
  echo "Unsupported arguments."
fi
```
</details>


### 2.3.2. 配置环境变量

打开.bashrc

```
vim .bashrc
```

写入( 命令`.` 即`source`这里不可以用bash代替):

```
# 科学上网的指令映射
alias my_proxy="source /home/guof/my_proxy.sh"
# 自动开网
. /home/guof/my_proxy.sh set
```

重启 bash，生效

### 2.3.3. 可用指令

可用指令如下：

* 开启代理
```
my_proxy set
```

* 关闭代理
```
my_proxy unset
```

* 测试代理
```
my_proxy test
```

## 2.4. linux 服务器

实验室的服务器上没有docker，我也没办法通过云端调配具体的config，总之先用着吧

不能换节点的问题以后解决看看，这个有点麻烦

* [参考链接1](https://juejin.cn/post/7054941050216906760)
* [参考连接2](https://www.fastlink.pro/user/tutorial?os=linux&client=clash)
* [参考连接3](https://maintao.com/2021/use-clash-as-a-proxy/#:~:text=%E6%9C%8D%E5%8A%A1%E5%99%A8%E7%AB%AF%E7%9A%84%20Clash%20Clash%20%E5%9C%A8%20github%20%E4%B8%8A%E6%98%AF%E4%B8%AA%E6%B4%BB%E8%B7%83%E7%9A%84%E9%A1%B9%E7%9B%AE%EF%BC%8C%E7%BB%B4%E6%8A%A4%E8%80%85%E6%98%AF%E4%B8%AD%E5%9B%BD%E4%BA%BA%EF%BC%8C%E7%9B%AE%E5%89%8D%E5%B7%B2%E7%BB%8F%E6%9C%89%E4%B8%A4%E4%B8%87%E4%B8%AA,star%E3%80%82%20%E8%BF%99%E4%B8%AA%20Clash%20%E6%98%AF%E7%94%A8%20Go%20%E8%AF%AD%E8%A8%80%E5%86%99%E7%9A%84%E4%B8%8D%E5%B8%A6%E5%9B%BE%E5%BD%A2%E7%95%8C%E9%9D%A2%E7%9A%84%E5%91%BD%E4%BB%A4%E8%A1%8C%E7%A8%8B%E5%BA%8F%E3%80%82)


### 2.4.1. 下载内核
[github 地址](https://github.com/Dreamacro/clash/releases)

以1.8版本为例，选这个
![](https://cdn.jsdelivr.net/gh/gf9276/image/clash/20221109160338.png)

执行命令
```
cd && mkdir clash
```

下载下来，解压并重命名为 clash 放置到 ~/clash 目录下

在 ~/clash 目录下执行命令，赋予权限
```
chmod +x clash
```

### 2.4.2. 设置

创建配置目录

```
cd ~/ && mkdir ~/.config/clash
```

下载订阅链接和 Country.mmdb

```
wget -O config.yaml 您的订阅链接
wget -O Country.mmdb https://www.sub-speeder.com/client-download/Country.mmdb
mv ~/Clash/config.yaml ~/.config/clash
mv ~/Clash/Country.mmdb ~/.config/clash
```

config文件可以从windows上嫖过去，Country.mmdb也可以直接从链接下载


### 2.4.3. 使用

```
export all_proxy=http://127.0.0.1:7890 && setsid ~/clash/clash
```

### 2.4.4. 测试

```
curl www.google.com
```

### 2.4.5. 关闭 
```
ps -ef | grep clash
kill 进程号
```