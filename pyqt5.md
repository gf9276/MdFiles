<!-- TOC -->

- [1. pyqt5.md](#1-pyqt5md)
- [2. windows下安装](#2-windows下安装)
- [3. pyqt5操作](#3-pyqt5操作)
  - [3.1. 参考链接（暂时放在这里）](#31-参考链接暂时放在这里)
  - [3.2. ui转py的方法](#32-ui转py的方法)

<!-- /TOC -->

# 1. pyqt5.md

万一以后会用到呢，留着没错的，咱就是说万一捏

# 2. windows下安装

python 版本不要太高，不然容易出问题

可能需要先执行这个：

```
pip install --upgrade setuptools
```

先安装pyqt5

```
pip install pyqt5
```

再安装qt designer

```
pip install pyqt5-tools
```

安装好后 designer 默认路径如下所示：

```
C:\Users\92762\miniconda3\envs\pyqt5\Lib\site-packages\qt5_applications\Qt\bin\designer.exe
```

命令行打开

```
pyqt5-tools designer
```

# 3. pyqt5操作

## 3.1. 参考链接（暂时放在这里）

[PyQt入门教程](https://www.cnblogs.com/linyfeng/p/11223707.html#:~:text=Qt%20Designer%E5%B7%A5%E5%85%B7%E4%B8%BB%E7%95%8C%E9%9D%A2,%E6%89%93%E5%BC%80%E8%B7%AF%E5%BE%84%EF%BC%9A%24%20%7Bpython%E5%AE%89%E8%A3%85%E7%9B%AE%E5%BD%95%7D%2FLib%2Fsite-packages%2Fpyqt5_tools%2Fdesigner.exe%E3%80%82)

[PyQt中文教程](https://maicss.gitbook.io/pyqt-chinese-tutoral/)

## 3.2. ui转py的方法
```
pyuic5 -o 1.py 1.ui
```