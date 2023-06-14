<!-- TOC -->

- [1. pip.md](#1-pipmd)
- [2. windows下 pip 走代理（蓝色小猫也能用）](#2-windows下-pip-走代理蓝色小猫也能用)
  - [2.1. 命令生成](#21-命令生成)
- [3. linux下 pip 走代理](#3-linux下-pip-走代理)

<!-- /TOC -->

# 1. pip.md

pip相关的指令之类的

# 2. windows下 pip 走代理（蓝色小猫也能用）

## 2.1. 命令生成

powershell 执行

```
pip config set global.proxy http://127.0.0.1:7890
```

默认写入的配置文件路径为```C:\Users\92762\AppData\Roaming\pip\pip.ini```

成功配置如下图所示：

![](https://cdn.jsdelivr.net/gh/gf9276/image/conda/20230223200109.png)

如果没有进行上面的配置，但是你又开了代理（clash变成黄色小猫），则会出现以下报错：

![](https://cdn.jsdelivr.net/gh/gf9276/image/conda/20230223200214.png)

# 3. linux下 pip 走代理

不需要，linux下会自动调用全局代理信息