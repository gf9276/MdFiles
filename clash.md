# 代理设置

## windows

### 下载 clash for windows
直接安装 [clash for windows](https://github.com/Fndroid/clash_for_windows_pkg/releases) 

找一个合适版本的便携版就行了，比如：
![](https://cdn.jsdelivr.net/gh/gf9276/image/clash/QD]A0KGPUJ0SGMM2AZITYWM.png)

### 下载中文包

[clahs for windows 中文包](https://github.com/BoyceLig/Clash_Chinese_Patch/releases/)

按照他的提示替换对应文件

至于代理，自己找

## wsl2

先设置 windows for clash，（主要是允许局域网连接）
![](https://cdn.jsdelivr.net/gh/gf9276/image/clash/20221120134915.png)

局域网连接里可以看到wsl的IP地址，此时不打开win10的系统代理按钮，wsl2也可以上外网，只需要把clash挂在后台就行了
![](https://cdn.jsdelivr.net/gh/gf9276/image/clash/20230223152410.png)


wsl2是依托于windows的，具体的config应该就是按照windows走的

在 ~/ 下写一个脚本：

**<font color=#D81D4F >注意：是 ~/ 路径下</font>**

```
cd ~/ && touch my_proxy.sh
```

编辑 my_proxy.sh

```
vi my_proxy.sh
```

写入内容如下

```
#!/bin/sh
hostip=$(cat /etc/resolv.conf | grep nameserver | awk '{ print $2 }')
wslip=$(hostname -I | awk '{print $1}')
port=7890

PROXY_HTTP="http://${hostip}:${port}"

set_proxy(){
  export all_proxy="${PROXY_HTTP}"
  export ALL_PROXY="${PROXY_HTTP}"

  git config --global http.https://github.com.proxy ${PROXY_HTTP}
  git config --global https.https://github.com.proxy ${PROXY_HTTP}

  echo "Proxy has been opened."
}

unset_proxy(){
  unset ALL_PROXY
  unset all_proxy
  git config --global --unset http.https://github.com.proxy
  git config --global --unset https.https://github.com.proxy

  echo "Proxy has been closed."
}

test_setting(){
  echo "Host IP:" ${hostip}
  echo "WSL IP:" ${wslip}
  echo "Try to connect to Google..."
  resp=$(curl -I -s --connect-timeout 5 -m 5 -w "%{http_code}" -o /dev/null www.google.com)
  if [ ${resp} = 200 ]; then
    echo "Proxy setup succeeded!"
  else
    echo "Proxy setup failed!"
  fi
}

if [ "$1" = "set" ]
then
  set_proxy

elif [ "$1" = "unset" ]
then
  unset_proxy

elif [ "$1" = "test" ]
then
  test_setting
else
  echo "Unsupported arguments."
fi

```

打开.bashrc

```
vi .bashrc
```

写入(.可以用bash代替，我暂时没找到.的用法):

```
# 科学上网的指令映射
alias my_proxy="source /home/guof/my_proxy.sh"
# 自动开网
. /home/guof/my_proxy.sh set
```

重启 bash，生效，可用指令如下：

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

## linux 服务器

实验室的服务器上没有docker，我也没办法通过云端调配具体的config，总之先用着吧

不能换节点的问题以后解决看看，这个有点麻烦

* [参考链接1](https://juejin.cn/post/7054941050216906760)
* [参考连接2](https://www.fastlink.pro/user/tutorial?os=linux&client=clash)
* [参考连接3](https://maintao.com/2021/use-clash-as-a-proxy/#:~:text=%E6%9C%8D%E5%8A%A1%E5%99%A8%E7%AB%AF%E7%9A%84%20Clash%20Clash%20%E5%9C%A8%20github%20%E4%B8%8A%E6%98%AF%E4%B8%AA%E6%B4%BB%E8%B7%83%E7%9A%84%E9%A1%B9%E7%9B%AE%EF%BC%8C%E7%BB%B4%E6%8A%A4%E8%80%85%E6%98%AF%E4%B8%AD%E5%9B%BD%E4%BA%BA%EF%BC%8C%E7%9B%AE%E5%89%8D%E5%B7%B2%E7%BB%8F%E6%9C%89%E4%B8%A4%E4%B8%87%E4%B8%AA,star%E3%80%82%20%E8%BF%99%E4%B8%AA%20Clash%20%E6%98%AF%E7%94%A8%20Go%20%E8%AF%AD%E8%A8%80%E5%86%99%E7%9A%84%E4%B8%8D%E5%B8%A6%E5%9B%BE%E5%BD%A2%E7%95%8C%E9%9D%A2%E7%9A%84%E5%91%BD%E4%BB%A4%E8%A1%8C%E7%A8%8B%E5%BA%8F%E3%80%82)


### 下载内核
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

## 设置

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


## 使用

```
export all_proxy=http://127.0.0.1:7890 && setsid ~/clash/clash
```

## 测试

```
curl www.google.com
```

## 关闭 
```
ps -ef | grep clash
kill 进程号
```