<!-- TOC -->

- [1. docker.md](#1-dockermd)
- [2. 安装 docker](#2-安装-docker)
  - [2.1. wsl（wsl的ubuntu） 下安装](#21-wslwsl的ubuntu-下安装)
  - [2.2. linux（普通ubuntu） 下安装](#22-linux普通ubuntu-下安装)
    - [2.2.1. 解决gpu无法调用问题（小心，会导致显卡驱动崩溃）](#221-解决gpu无法调用问题小心会导致显卡驱动崩溃)
  - [2.3. 命令](#23-命令)
  - [2.4. 挪动docker的目录](#24-挪动docker的目录)

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

### 2.2.1. 解决gpu无法调用问题（小心，会导致显卡驱动崩溃）

[参考链接](https://docs.nvidia.com/datacenter/cloud-native/container-toolkit/latest/install-guide.html)

## 2.3. 命令

* docker ps

* docker images

* docker run -d --name ???? -p ??:?? ???

## 2.4. 挪动docker的目录

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