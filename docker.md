<!-- TOC -->

- [1. docker.md](#1-dockermd)
- [2. 安装 docker](#2-安装-docker)
  - [2.1. wsl（wsl的ubuntu） 下安装](#21-wslwsl的ubuntu-下安装)
  - [2.2. linux（普通ubuntu） 下安装](#22-linux普通ubuntu-下安装)
  - [2.3. 解决gpu无法调用问题（小心，会导致显卡驱动崩溃）](#23-解决gpu无法调用问题小心会导致显卡驱动崩溃)
  - [2.4. 命令](#24-命令)
  - [2.5. 挪动docker的目录](#25-挪动docker的目录)
  - [2.6. docker pull 使用代理](#26-docker-pull-使用代理)
- [3. 问题](#3-问题)
  - [3.1. docker下使用 `dpkg-reconfigure locales` 无效](#31-docker下使用-dpkg-reconfigure-locales-无效)
  - [3.2. docker启动时，不会执行/etc/profile](#32-docker启动时不会执行etcprofile)

<!-- /TOC -->

# 1. docker.md

学习docker


# 2. 安装 docker

## 2.1. wsl（wsl的ubuntu） 下安装

直接 Docker Desktop for windows ，直接下载windows版本的docker

## 2.2. linux（普通ubuntu） 下安装

1. Set up Docker's repository `.apt`
```
# Add Docker's official GPG key:
sudo apt-get update
sudo apt-get install ca-certificates curl gnupg
sudo install -m 0755 -d /etc/apt/keyrings
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
sudo chmod a+r /etc/apt/keyrings/docker.gpg

# Add the repository to Apt sources:
echo \
  "deb [arch="$(dpkg --print-architecture)" signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu \
  "$(. /etc/os-release && echo "$VERSION_CODENAME")" stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
sudo apt-get update
```

2. 安装
```
sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
```

安装完之后 自动 systemctl 启动

## 2.3. 解决gpu无法调用问题（小心，会导致显卡驱动崩溃）

[参考链接](https://docs.nvidia.com/datacenter/cloud-native/container-toolkit/latest/install-guide.html)

## 2.4. 命令

* docker ps

* docker images

* docker run -d --name ???? -p ??:?? ???

## 2.5. 挪动docker的目录

本来我只是想挪动一下镜像目录的，但是我发现不好挪动，干脆把整个都挪过去了

[参考链接（里面软连接命令好像是错的，小心）](https://developer.aliyun.com/article/785312)

1. 停掉docker
  
    ```
    systemctl stop docker 
    ```

2. 查看一下docker原始目录在哪

    ![](https://cdn.jsdelivr.net/gh/gf9276/image/docker/E6{FZE[PK8_@98@ECNWG@OB.png)

3. 创建新的docker目录（并指定权限）

    ```
    cd /data1 && sudo mkdir docker && sudo chmod 710 docker/
    ```

4. 迁移数据

    ```
    sudo cp -r /var/lib/docker/. /data1/docker/
    ```
5. 备份数据

    ```
    sudo mv /var/lib/docker /var/lib/docker.bak
    ```

6. 建立软链接

    ```
    sudo ln -s /data1/docker /var/lib/docker
    ```

7. 启动

    ```
    systemctl start docker 
    ```

## 2.6. docker pull 使用代理

[官方文档](https://docs.docker.com/config/daemon/systemd/#httphttps-proxy)

[参考链接2](https://www.cnblogs.com/cocoajin/p/15513305.html)

实际上docker pull这种命令归docker daemon管，即docker的守护进程。而官方文档的操作正是把docker daemon进行代理。

操作前记得不要跑重要容器，因为后面要重启docker的


1. 创建docker.service的目录

    ```
    sudo mkdir -p /etc/systemd/system/docker.service.d
    ```

2. 写入配置

    ```
    sudo vim /etc/systemd/system/docker.service.d/http_proxy.conf 
    ```

    内容如下：

    ```
    [Service]
    Environment="HTTP_PROXY=http://127.0.0.1:7890" 
    Environment="HTTPS_PROXY=http://127.0.0.1:7890"
    Environment="NO_PROXY=localhost,127.0.0.1"
    ```

3. 加载配置并重启

    ```
    sudo systemctl daemon-reload
    sudo systemctl restart docker
    ```

4. 查看环境变量

    ```
    sudo systemctl show --property=Environment docker
    ```

# 3. 问题

## 3.1. docker下使用 `dpkg-reconfigure locales` 无效

需要自己在启动容器的时候-e指定的语言，怪

## 3.2. docker启动时，不会执行/etc/profile

需要手动将环境变量永久化，ENV如果写到一行里，是没办法$调用的，不同行的ENV就可以