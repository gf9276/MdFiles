# wsl2

## 安装
* [参考链接1-bilibili](https://www.bilibili.com/read/cv9561666)
* [参考链接2-博客园](https://www.cnblogs.com/ittranslator/p/14128570.html)
* [参考链接3-CSDN](https://blog.csdn.net/li1325169021/article/details/124285018)
  
### 第一步，启动wsl
不管您想要使用哪个版本的 WSL，都首先需要启用它。为此，请以**管理员身份**打开 **PowerShell** 工具并运行以下命令。小心不要在命令中输入错误或遗漏任何字符：
```
dism.exe /online /enable-feature /featurename:Microsoft-Windows-Subsystem-Linux /all /norestart
```

### 第二步，启动虚拟机平台、重启、下载内核包
WSL 2 需要启用 Windows 10 的 “虚拟机平台” 特性。它独立于 Hyper-V，并提供了一些在 Linux 的 Windows 子系统新版本中可用的更有趣的平台集成。
要在 Windows 10（2004）上启用虚拟机平台，请以 **管理员身份打开 PowerShell** 并运行：
```
dism.exe /online /enable-feature /featurename:VirtualMachinePlatform /all /norestart
```

为了确保所有相关部件都整齐到位，您应该在此时 **重启系统**，否则可能会发现事情没按预期进行。

然后一般来说，需要下载 Linux 内核更新包（适用于 x64 计算机的 WSL2 Linux 内核更新包）

* [官方问题提出路径](https://learn.microsoft.com/zh-cn/windows/wsl/troubleshooting)
* [具体下载路径](https://wslstorestorage.blob.core.windows.net/wslblob/wsl_update_x64.msi)

### 第三步，设置 WSL 2 为默认值

以 **管理员身份打开 PowerShell**，然后运行以下命令以将 WSL 2 设置为 WSL 的默认版本：
```
wsl --set-default-version 2
```

### 第四步，安装一个 Linux 发行版、挪动到其他盘（可选）
直接去微软商店搜就行了，Ubuntu 22.04。
然后直接打开就行，按照他的提示，输入用户名和密码，有点奇怪，有时候是黑色框，有时候出来彩色的，emmm。不过都一样的。彩色的和下面的一样：

![](https://cdn.jsdelivr.net/gh/gf9276/image/wsl2/20221109193951.png)

如果想挪动，执行下面的命令

* 查看已安装的子系统版本
```
wsl -l -v
```
* 关闭wsl
```
wsl --shutdown
```
* 导出分发版为tar文件到D盘
```
wsl --export Ubuntu-22.04 d:\wsl-ubuntu-22.04.tar
```
* 注销当前分发版
```
wsl --unregister Ubuntu-22.04
```
* 重新导入并安装wsl在G盘
```
wsl --import Ubuntu-22.04 d:\wsl-ubuntu-22.04 d:\wsl-ubuntu-22.04.tar --version 2
```
* 设置默认登陆用户为安装时用户名
```
ubuntu2204 config --default-user guof
```
* 删除tar文件
```
del d:\wsl-ubuntu-22.04.tar
```


### 第五步，直接安装 windows terminal

设置大小为
![](https://cdn.jsdelivr.net/gh/gf9276/image/wsl2/20221109193242.png)

剩下的看 oh-my-posh.md