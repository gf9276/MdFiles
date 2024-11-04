<!-- TOC -->

- [1. conda.md](#1-condamd)
- [2. 安装 conda (linux)](#2-安装-conda-linux)
- [3. 配置 conda](#3-配置-conda)
    - [3.1. 换源](#31-换源)
    - [3.2. Windows 系统下 Anaconda Powershell Prompt (miniconda3) 部署到Windows Terminal](#32-windows-系统下-anaconda-powershell-prompt-miniconda3-部署到windows-terminal)
    - [3.3. windows下 conda 走代理（蓝色小猫也能用）](#33-windows下-conda-走代理蓝色小猫也能用)
        - [3.3.1. 命令生成](#331-命令生成)
    - [3.4. linux下 conda 走代理](#34-linux下-conda-走代理)
    - [3.5. windows 系统下 powershell 加载 conda 的环境](#35-windows-系统下-powershell-加载-conda-的环境)
- [4. 操作 conda](#4-操作-conda)
    - [4.1. 一点很简单的指令](#41-一点很简单的指令)
    - [4.2. 打包 conda 环境](#42-打包-conda-环境)
        - [4.2.1. 安装打包的工具](#421-安装打包的工具)
        - [4.2.2. 打包将要迁移的环境](#422-打包将要迁移的环境)
        - [4.2.3. 复制压缩文件到新的电脑环境](#423-复制压缩文件到新的电脑环境)

<!-- /TOC -->

# 1. conda.md

记录 conda 的一些操作

# 2. 安装 conda (linux)

**<font color=#8470FF > 安装在当前用户目录下，命令前面不要加sudo </font>**

* 下载

```
cd ~/ && wget https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh
```

* 执行

```
bash Miniconda3-latest-Linux-x86_64.sh
```

* 重新激活环境变量，并检查conda是否安装成功

```
source ~/.bashrc
```

```
conda -h
```

# 3. 配置 conda

## 3.1. 换源

我一般用代理，不用换，留在这里以防万一

[参考链接](https://www.cnblogs.com/sethnie/p/15847897.html)

```
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud/msys2/
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud/conda-forge
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/free/
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud/pytorch/
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/main/
conda config --set show_channel_urls yes
```

他会自动创建一个 ~/.condarc 文件

执行以下命令，换回默认源（就是把channels这个键值给删了）

```
conda config --remove-key channels
```

## 3.2. Windows 系统下 Anaconda Powershell Prompt (miniconda3) 部署到Windows Terminal

直接```添加新配置文件```然后复制cmd的格式，直接这样子设置就行了

![](https://cdn.jsdelivr.net/gh/gf9276/image/conda/20230223172351.png)

命令行参数来源于此：

![](https://cdn.jsdelivr.net/gh/gf9276/image/conda/20230223172539.png)

## 3.3. windows下 conda 走代理（蓝色小猫也能用）

### 3.3.1. 命令生成

[可以参考这个连接](https://www.cnblogs.com/treasury-manager/p/13952394.html#1%E4%B8%8D%E4%BD%BF%E7%94%A8%E4%BB%A3%E7%90%86%E7%94%A8%E6%88%B7%E5%90%8D%E5%AF%86%E7%A0%81%E7%9A%84)

```
conda config --set proxy_servers.http http://user:password@xxxx:8080
conda config --set proxy_servers.https https://user:password@xxxx:8080
```

没有账号密码的
```
conda config --set proxy_servers.http http://127.0.0.1:7890
conda config --set proxy_servers.https http://127.0.0.1:7890
```

默认文件路径为```C:\Users\92762\.condarc```

## 3.4. linux下 conda 走代理

不需要，它会自动调用全局代理的信息，起码我是这样的，如果不能调用，就和windows一样，执行添加代理的命令就行

## 3.5. windows 系统下 powershell 加载 conda 的环境

先在 conda 界面下执行
```
conda init powershell
```

随后以管理员模式打开 PowerShell, 执行

```
Set-ExecutionPolicy -ExecutionPolicy RemoteSigned
```

[参考这里](http://www.splaybow.com/post/powershellexecps1.html)

# 4. 操作 conda

## 4.1. 一点很简单的指令

```
conda list             // 显示conda安装的python包
conda search xxx       // 搜索python包
conda install xxx=1.2  // 安装指定安装包版本
conda install D:xxx    // 安装本地python包（绝对路径）
conda env list         // 查看当前存在哪些虚拟环境
conda create --name pytorch python=3.7 // 创建虚拟环境pytorch，python版本为3.7
conda activate pytorch         // 启用虚拟环境
conda deactivate               // 退出当前虚拟环境
conda remove -n pytorch --all  // 删除虚拟环境
conda info                     // 显示相关信息
conda update conda             // 更新conda

```

## 4.2. 打包 conda 环境

**注意：迁移conda环境不要直接复制envs底下的文件夹，适配性很差的。**

**不会有人直接复制envs底下的文件夹吧，不会吧不会吧 😎**

迁移conda环境时注意，miniconda和conda是互通的，但是windows上的conda和linux上的conda应该是不能混用的（我没试过）。

### 4.2.1. 安装打包的工具

```
conda install -c conda-forge conda-pack
```

### 4.2.2. 打包将要迁移的环境

```
conda pack -n 虚拟环境名称 -o output.tar.gz
```

### 4.2.3. 复制压缩文件到新的电脑环境

- 进到conda的安装目录

```
cd ~/miniconda3/envs/
```

- 将```output.tar.gz```压缩包放置到该目录下


- 在该目录下创建文件夹，名字与虚拟环境名称一致

```
mkdir env_name
```

- 解压压缩包到该目录下

```
tar -xzvf output.tar.gz -C env_name/
```

- 查看虚拟环境

```
conda env list
```

- 随后可删除多余的压缩包

```
rm output.tar.gz
```

