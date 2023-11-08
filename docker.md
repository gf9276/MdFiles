<!-- TOC -->

- [1. docker.md](#1-dockermd)
- [2. 安装 docker](#2-安装-docker)
  - [2.1. wsl（wsl的ubuntu） 下安装](#21-wslwsl的ubuntu-下安装)
  - [2.2. linux（普通ubuntu） 下安装](#22-linux普通ubuntu-下安装)
    - [解决gpu无法调用问题（小心，会导致显卡驱动崩溃）](#解决gpu无法调用问题小心会导致显卡驱动崩溃)
  - [命令](#命令)

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

### 解决gpu无法调用问题（小心，会导致显卡驱动崩溃）

[参考链接](https://docs.nvidia.com/datacenter/cloud-native/container-toolkit/latest/install-guide.html)

## 命令

* docker ps

* docker images

* docker run -d --name ???? -p ??:?? ???