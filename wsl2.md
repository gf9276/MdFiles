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



建议设置.json文件为
```
{
    "$help": "https://aka.ms/terminal-documentation",
    "$schema": "https://aka.ms/terminal-profiles-schema",
    "actions": 
    [
        {
            "command": 
            {
                "action": "copy",
                "singleLine": false
            },
            "keys": "ctrl+c"
        },
        {
            "command": "paste",
            "keys": "ctrl+v"
        },
        {
            "command": "find",
            "keys": "ctrl+shift+f"
        },
        {
            "command": 
            {
                "action": "splitPane",
                "split": "auto",
                "splitMode": "duplicate"
            },
            "keys": "alt+shift+d"
        }
    ],
    "copyFormatting": "none",
    "copyOnSelect": false,
    "defaultProfile": "{f9ceaf27-504c-58d7-927c-d1d6a7ac7d3c}",
    "initialCols": 150,
    "initialRows": 38,
    "profiles": 
    {
        "defaults": 
        {
            "font": 
            {
                "face": "agave Nerd Font Mono r",
                "size": 11,
                "weight": "semi-light"
            }
        },
        "list": 
        [
            {
                "commandline": "%SystemRoot%\\System32\\WindowsPowerShell\\v1.0\\powershell.exe",
                "guid": "{61c54bbd-c2c6-5271-96e7-009a87ff44bf}",
                "hidden": false,
                "name": "Windows PowerShell"
            },
            {
                "commandline": "%SystemRoot%\\System32\\cmd.exe",
                "guid": "{0caa0dad-35be-5f56-a8ff-afceeeaa6101}",
                "hidden": false,
                "name": "\u547d\u4ee4\u63d0\u793a\u7b26"
            },
            {
                "guid": "{17bf3de4-5353-5709-bcf9-835bd952a95e}",
                "hidden": true,
                "name": "Ubuntu-22.04",
                "source": "Windows.Terminal.Wsl"
            },
            {
                "guid": "{b453ae62-4e3d-5e58-b989-0a998ec441b8}",
                "hidden": false,
                "name": "Azure Cloud Shell",
                "source": "Windows.Terminal.Azure"
            },
            {
                "colorScheme": "my_1_shell",
                "cursorShape": "bar",
                "guid": "{f9ceaf27-504c-58d7-927c-d1d6a7ac7d3c}",
                "hidden": false,
                "name": "Ubuntu 22.04.1 LTS",
                "source": "CanonicalGroupLimited.Ubuntu22.04LTS_79rhkp1fndgsc"
            },
            {
                "colorScheme": "Ubuntu-22.04-ColorScheme",
                "cursorShape": "bar",
                "font": 
                {
                    "face": "agave Nerd Font Mono r"
                },
                "guid": "{FE8EE342-1361-7980-41BE-A44462E516FC}",
                "hidden": false,
                "name": "实验室1080ti",
                "commandline": "ssh guof@10.68.77.68",
                "useAcrylic": true,
                "icon" : "ms-appx:///ProfileIcons/{9acb9455-ca41-5af7-950f-6bca1bc9722f}.png"
            }
        ]
    },
    "schemes": 
    [
        {
            "background": "#0C0C0C",
            "black": "#0C0C0C",
            "blue": "#0037DA",
            "brightBlack": "#767676",
            "brightBlue": "#3B78FF",
            "brightCyan": "#61D6D6",
            "brightGreen": "#16C60C",
            "brightPurple": "#B4009E",
            "brightRed": "#E74856",
            "brightWhite": "#F2F2F2",
            "brightYellow": "#F9F1A5",
            "cursorColor": "#FFFFFF",
            "cyan": "#3A96DD",
            "foreground": "#CCCCCC",
            "green": "#13A10E",
            "name": "Campbell",
            "purple": "#881798",
            "red": "#C50F1F",
            "selectionBackground": "#FFFFFF",
            "white": "#CCCCCC",
            "yellow": "#C19C00"
        },
        {
            "background": "#012456",
            "black": "#0C0C0C",
            "blue": "#0037DA",
            "brightBlack": "#767676",
            "brightBlue": "#3B78FF",
            "brightCyan": "#61D6D6",
            "brightGreen": "#16C60C",
            "brightPurple": "#B4009E",
            "brightRed": "#E74856",
            "brightWhite": "#F2F2F2",
            "brightYellow": "#F9F1A5",
            "cursorColor": "#FFFFFF",
            "cyan": "#3A96DD",
            "foreground": "#CCCCCC",
            "green": "#13A10E",
            "name": "Campbell Powershell",
            "purple": "#881798",
            "red": "#C50F1F",
            "selectionBackground": "#FFFFFF",
            "white": "#CCCCCC",
            "yellow": "#C19C00"
        },
        {
            "background": "#282C34",
            "black": "#282C34",
            "blue": "#61AFEF",
            "brightBlack": "#5A6374",
            "brightBlue": "#61AFEF",
            "brightCyan": "#56B6C2",
            "brightGreen": "#98C379",
            "brightPurple": "#C678DD",
            "brightRed": "#E06C75",
            "brightWhite": "#DCDFE4",
            "brightYellow": "#E5C07B",
            "cursorColor": "#FFFFFF",
            "cyan": "#56B6C2",
            "foreground": "#DCDFE4",
            "green": "#98C379",
            "name": "One Half Dark",
            "purple": "#C678DD",
            "red": "#E06C75",
            "selectionBackground": "#FFFFFF",
            "white": "#DCDFE4",
            "yellow": "#E5C07B"
        },
        {
            "background": "#FAFAFA",
            "black": "#383A42",
            "blue": "#0184BC",
            "brightBlack": "#4F525D",
            "brightBlue": "#61AFEF",
            "brightCyan": "#56B5C1",
            "brightGreen": "#98C379",
            "brightPurple": "#C577DD",
            "brightRed": "#DF6C75",
            "brightWhite": "#FFFFFF",
            "brightYellow": "#E4C07A",
            "cursorColor": "#4F525D",
            "cyan": "#0997B3",
            "foreground": "#383A42",
            "green": "#50A14F",
            "name": "One Half Light",
            "purple": "#A626A4",
            "red": "#E45649",
            "selectionBackground": "#FFFFFF",
            "white": "#FAFAFA",
            "yellow": "#C18301"
        },
        {
            "background": "#002B36",
            "black": "#002B36",
            "blue": "#268BD2",
            "brightBlack": "#073642",
            "brightBlue": "#839496",
            "brightCyan": "#93A1A1",
            "brightGreen": "#586E75",
            "brightPurple": "#6C71C4",
            "brightRed": "#CB4B16",
            "brightWhite": "#FDF6E3",
            "brightYellow": "#657B83",
            "cursorColor": "#FFFFFF",
            "cyan": "#2AA198",
            "foreground": "#839496",
            "green": "#859900",
            "name": "Solarized Dark",
            "purple": "#D33682",
            "red": "#DC322F",
            "selectionBackground": "#FFFFFF",
            "white": "#EEE8D5",
            "yellow": "#B58900"
        },
        {
            "background": "#FDF6E3",
            "black": "#002B36",
            "blue": "#268BD2",
            "brightBlack": "#073642",
            "brightBlue": "#839496",
            "brightCyan": "#93A1A1",
            "brightGreen": "#586E75",
            "brightPurple": "#6C71C4",
            "brightRed": "#CB4B16",
            "brightWhite": "#FDF6E3",
            "brightYellow": "#657B83",
            "cursorColor": "#002B36",
            "cyan": "#2AA198",
            "foreground": "#657B83",
            "green": "#859900",
            "name": "Solarized Light",
            "purple": "#D33682",
            "red": "#DC322F",
            "selectionBackground": "#FFFFFF",
            "white": "#EEE8D5",
            "yellow": "#B58900"
        },
        {
            "background": "#000000",
            "black": "#000000",
            "blue": "#3465A4",
            "brightBlack": "#555753",
            "brightBlue": "#729FCF",
            "brightCyan": "#34E2E2",
            "brightGreen": "#8AE234",
            "brightPurple": "#AD7FA8",
            "brightRed": "#EF2929",
            "brightWhite": "#EEEEEC",
            "brightYellow": "#FCE94F",
            "cursorColor": "#FFFFFF",
            "cyan": "#06989A",
            "foreground": "#D3D7CF",
            "green": "#4E9A06",
            "name": "Tango Dark",
            "purple": "#75507B",
            "red": "#CC0000",
            "selectionBackground": "#FFFFFF",
            "white": "#D3D7CF",
            "yellow": "#C4A000"
        },
        {
            "background": "#FFFFFF",
            "black": "#000000",
            "blue": "#3465A4",
            "brightBlack": "#555753",
            "brightBlue": "#729FCF",
            "brightCyan": "#34E2E2",
            "brightGreen": "#8AE234",
            "brightPurple": "#AD7FA8",
            "brightRed": "#EF2929",
            "brightWhite": "#EEEEEC",
            "brightYellow": "#FCE94F",
            "cursorColor": "#000000",
            "cyan": "#06989A",
            "foreground": "#555753",
            "green": "#4E9A06",
            "name": "Tango Light",
            "purple": "#75507B",
            "red": "#CC0000",
            "selectionBackground": "#FFFFFF",
            "white": "#D3D7CF",
            "yellow": "#C4A000"
        },
        {
            "background": "#300A24",
            "black": "#171421",
            "blue": "#0037DA",
            "brightBlack": "#767676",
            "brightBlue": "#08458F",
            "brightCyan": "#2C9FB3",
            "brightGreen": "#26A269",
            "brightPurple": "#A347BA",
            "brightRed": "#C01C28",
            "brightWhite": "#F2F2F2",
            "brightYellow": "#A2734C",
            "cursorColor": "#FFFFFF",
            "cyan": "#3A96DD",
            "foreground": "#FFFFFF",
            "green": "#26A269",
            "name": "Ubuntu-22.04-ColorScheme",
            "purple": "#881798",
            "red": "#C21A23",
            "selectionBackground": "#FFFFFF",
            "white": "#CCCCCC",
            "yellow": "#A2734C"
        },
        {
            "background": "#000000",
            "black": "#000000",
            "blue": "#000080",
            "brightBlack": "#808080",
            "brightBlue": "#0000FF",
            "brightCyan": "#00FFFF",
            "brightGreen": "#00FF00",
            "brightPurple": "#FF00FF",
            "brightRed": "#FF0000",
            "brightWhite": "#FFFFFF",
            "brightYellow": "#FFFF00",
            "cursorColor": "#FFFFFF",
            "cyan": "#008080",
            "foreground": "#C0C0C0",
            "green": "#008000",
            "name": "Vintage",
            "purple": "#800080",
            "red": "#800000",
            "selectionBackground": "#FFFFFF",
            "white": "#C0C0C0",
            "yellow": "#808000"
        },
        {
            "background": "#0F111A",
            "black": "#282C34",
            "blue": "#61AFEF",
            "brightBlack": "#5A6374",
            "brightBlue": "#61AFEF",
            "brightCyan": "#56B6C2",
            "brightGreen": "#98C379",
            "brightPurple": "#C678DD",
            "brightRed": "#E06C75",
            "brightWhite": "#DCDFE4",
            "brightYellow": "#E5C07B",
            "cursorColor": "#FFFFFF",
            "cyan": "#56B6C2",
            "foreground": "#DCDFE4",
            "green": "#98C379",
            "name": "my_1_shell",
            "purple": "#C678DD",
            "red": "#E06C75",
            "selectionBackground": "#FFFFFF",
            "white": "#DCDFE4",
            "yellow": "#E5C07B"
        }
    ]
}
```