<!-- TOC -->

- [1. minio.md](#1-miniomd)
- [2. 安装 minio](#2-安装-minio)
  - [2.1. 下载可执行文件](#21-下载可执行文件)
  - [2.2. 创建静态文件存放目录](#22-创建静态文件存放目录)
  - [2.3. 普通启动minio](#23-普通启动minio)
  - [2.4. systemctl启动minio](#24-systemctl启动minio)
    - [2.4.1. 设置minio启动参数](#241-设置minio启动参数)
    - [2.4.2. 设置开机自启动](#242-设置开机自启动)

<!-- /TOC -->

# 1. minio.md

Min IO 的相关内容

[官方文档](http://www.minio.org.cn/docs/minio/linux/operations/install-deploy-manage/deploy-minio-single-node-single-drive.html)

# 2. 安装 minio

**<font color=#8470FF > 该加sudo加sudo，我是建议直接 sudo su -l 登录到root用户再安装 </font>**

## 2.1. 下载可执行文件

放到```/usr/local/bin/```目录下，该目录下放一些个人的可执行文件，```/opt```放的一般是源码
```
cd /usr/local/bin/
```

下载
```
wget https://dl.min.io/server/minio/release/linux-amd64/minio
```

赋予可执行权限
```
chmod +x /usr/local/bin/minio
```

## 2.2. 创建静态文件存放目录

```
mkdir /data/minio_data && chmod 777 /data/minio_data
```

## 2.3. 普通启动minio

可用这个，也可以用后面的systemctl

```
minio server /data/minio_data --console-address ":9099"
```

## 2.4. systemctl启动minio

### 2.4.1. 设置minio启动参数

该参数文件是准备供systemd调用的

- 创建默认的配置文件（话说居然没有后缀名。。。）
```
vim /etc/default/minio
```

- 写入如下内容
```
# 指定数据存储目录(注意：这个目录要存在且拥有相对应的权限)
# 该目录与前面创建的目录一致
MINIO_VOLUMES="/data/minio_data"

# 监听端口
MINIO_OPTS="--address :9000 --console-address :9099"

# 老版本使用MINIO_ACCESS_KEY/MINIO_SECRET_KEY，新版本已不建议使用
# Access key (账号)
# MINIO_ACCESS_KEY="minioadmin"
# Secret key (密码)
# MINIO_SECRET_KEY="minioadmin"

# 新版本使用；指定默认的用户名和密码，其中用户名必须大于3个字母，否则不能启动
MINIO_ROOT_USER="minio"
MINIO_ROOT_PASSWORD="miniominio"

# 区域值，标准格式是“国家-区域-编号”，
MINIO_REGION="cn-north-1"

# 域名
# MINIO_DOMAIN=minio.your_domain.com
```

### 2.4.2. 设置开机自启动

- 创建并编辑.service文件
```
vim /usr/lib/systemd/system/minio.service
```

- 写入如下内容
```
[Unit]
Description=MinIO
Documentation=https://docs.min.io
Wants=network-online.target
After=network-online.target
AssertFileIsExecutable=/usr/local/bin/minio
[Service]
# 我认为应该是 /usr/local/bin，但官方是下面这个
WorkingDirectory=/usr/local

# 貌似systemctl版本老旧的要删掉这个，不然会警告，不过貌似无碍
ProtectProc=invisible

# 指向3.1节中的配置文件
EnvironmentFile=/etc/default/minio

ExecStartPre=/bin/bash -c "if [ -z \"${MINIO_VOLUMES}\" ]; then echo \"Variable MINIO_VOLUMES not set in /etc/default/minio\"; exit 1; fi"
ExecStart=/usr/local/bin/minio server $MINIO_OPTS $MINIO_VOLUMES

# Let systemd restart this service always
Restart=always

# Specifies the maximum (1M) file descriptor number that can be opened by this process
LimitNOFILE=1048576

# Specifies the maximum number of threads this process can create
TasksMax=infinity

# Disable timeout logic and wait until process is stopped
TimeoutStopSec=infinity
SendSIGKILL=no
SuccessExitStatus=0

[Install]
WantedBy=multi-user.target
Alias=minio.service
```

- 重新加载服务配置文件，使服务生效
```
systemctl daemon-reload
```

- 将服务设置为开机启动
```
systemctl enable minio
```

- 服务立即启动
```
systemctl start minio
```

- 查看minio服务当前状态
```
systemctl status minio
```
