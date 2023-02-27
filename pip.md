# pip 

pip相关的指令之类的

## windows下 pip 走代理（蓝色小猫也能用）

### 命令生成

```
pip config set global.proxy http://127.0.0.1:7890
```

默认文件路径为```C:\Users\92762\AppData\Roaming\pip\pip.ini```

成功配置如下图所示：

![](https://cdn.jsdelivr.net/gh/gf9276/image/conda/20230223200109.png)

如果没有进行上面的配置，但是你又开了代理（clash变成黄色小猫），则会出现以下报错：

![](https://cdn.jsdelivr.net/gh/gf9276/image/conda/20230223200214.png)