# conda 

## 安装

没什么好说的，几条指令的事情

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
conda -h
```

## 换源

一般不用换，留在这里以防万一

[参考链接](https://www.cnblogs.com/sethnie/p/15847897.html)

```
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud/msys2/
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud/conda-forge
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/free/
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud/pytorch/
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/main/
conda config --set show_channel_urls yes
```

他会自动创建一个 .condarc 文件

执行以下命令，换回默认源

```
conda config --remove-key channels
```

## 一点很简单的指令

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