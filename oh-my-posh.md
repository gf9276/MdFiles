<!-- TOC -->

- [1. oh-my-posh.md](#1-oh-my-poshmd)
- [2. 安装](#2-安装)
  - [2.1. 下载字体](#21-下载字体)
  - [2.2. 安装oh my posh](#22-安装oh-my-posh)
    - [2.2.1. windows powershell](#221-windows-powershell)
    - [2.2.2. wsl](#222-wsl)
  - [2.3. 修改终端配置文件](#23-修改终端配置文件)
    - [2.3.1. windows powershell](#231-windows-powershell)
    - [2.3.2. wsl](#232-wsl)
- [3. 设置风格（以及遇到的问题）](#3-设置风格以及遇到的问题)
  - [3.1. night owl](#31-night-owl)
  - [3.2. 1\_shell](#32-1_shell)

<!-- /TOC -->


# 1. oh-my-posh.md

oh my posh 是一个美化终端的东西，主要是用在powershell上的，也支持ubuntu就是了

我主要是用在 windows terminal 上的

博客参考链接如下：[博客参考链接](https://sspai.com/post/69911#!)  

官方文档如下：[oh my posh 官方文档](https://ohmyposh.dev/docs/installation/windows)  

# 2. 安装

## 2.1. 下载字体

部分 Oh my posh 主题有一些特殊的字符，例如表示系统类型的徽标、GitHub 标志，这些字符需要特殊的字体支持。如果读者看上了一款有这些字符的主题，必须提前下载安装合适的字体，并将它们设置为终端显示的字体。使用[Agave Nerd Fonts](https://www.nerdfonts.com/font-downloads)字体，打开并安装。windows terminal下设置成这样比较好看：
```
"font": 
{
    "face": "agave Nerd Font Mono r",
    "size": 11,
    "weight": "semi-light"
}
```
或者默认配置如下图所示：
![](https://cdn.jsdelivr.net/gh/gf9276/image/oh-my-posh/20221109142953.png)

## 2.2. 安装oh my posh

具体指令查看 [oh my posh 官方文档](https://ohmyposh.dev/docs/installation/windows)。或者直接按照下面的执行

### 2.2.1. windows powershell  

执行以下指令，下载oh my posh
```
winget install JanDeDobbeleer.OhMyPosh -s winget
```

### 2.2.2. wsl

直接在wsl里下载oh my posh，打开wsl，回到根目录下，执行指令：

新版本貌似有问题，我现在用的是老版本的，就没有直接下载了

```
wget https://github.com/JanDeDobbeleer/oh-my-posh/releases/latest/download/posh-linux-amd64 -O /usr/local/bin/oh-my-posh
```

```
chmod +x /usr/local/bin/oh-my-posh
```

用户目录下执行以下指令，下载主题：

```
mkdir ~/.poshthemes
```
```
wget https://github.com/JanDeDobbeleer/oh-my-posh/releases/latest/download/themes.zip -O ~/.poshthemes/themes.zip
```
```
unzip ~/.poshthemes/themes.zip -d ~/.poshthemes
```
```
chmod u+rw ~/.poshthemes/*.omp.*
```
```
rm ~/.poshthemes/themes.zip
```

## 2.3. 修改终端配置文件

### 2.3.1. windows powershell  

打开PowerShell，键入$Profile，查询配置文件路径，编辑此文件，若没有，则新建一个。一般情况下长这样：  
```
C:\Users\92762\Documents\WindowsPowerShell\Microsoft.PowerShell_profile.ps1  
```
其中.ps1是他的后缀。记事本方式打开。输入以下指令即可，--config后面是主题的具体路径  
```   
oh-my-posh --init --shell pwsh --config C:\Users\92762\AppData\Local\Programs\oh-my-posh\themes\M365Princess.omp.json | Invoke-Expression  
```
### 2.3.2. wsl

回到用户路径下，打开.bashrc文件

```
cd /home/guof
vi .bashrc
```

在最后写入

```
eval "$(oh-my-posh --init --shell bash --config ~/.poshthemes/night-owl.omp.json)"
```

--config后面是具体的主题路径 

# 3. 设置风格（以及遇到的问题）

不显示base环境，具体我得看看他的指令集。[具体原因](https://github.com/JanDeDobbeleer/oh-my-posh/issues/2792)，我的回答是：Thank you for your reply. The problem has been solved, most of the themes have not been added to the python segment, so the virtual environment of conda cannot be displayed, and I have made a little modification to its code. 查阅[oh my posh 官方文档](https://ohmyposh.dev/docs/installation/windows)，主要内容以下三个：

* Configuration -> segment
* Segments
* Themes  

第一个查阅具体关键词的含义，第二个查阅关键词type的类型，第三个是各种主题和源码。建议参考源码有[atomicBit](https://github.com/JanDeDobbeleer/oh-my-posh/blob/main/themes/atomicBit.omp.json)、[night-owl](https://github.com/JanDeDobbeleer/oh-my-posh/blob/main/themes/night-owl.omp.json)、[M365Princess](https://github.com/JanDeDobbeleer/oh-my-posh/blob/main/themes/M365Princess.omp.json)、[1_shell](https://github.com/JanDeDobbeleer/oh-my-posh/blob/main/themes/1_shell.omp.json)。添加对应的python段，即可显示conda环境。night-owl在vs code下，会出现diamond显示问题（通病），我弄了两种style，一种基于1_shell，一种基于night_owl。

## 3.1. night owl

没有对源码进行更改，windows terminal里使用的背景配色为#051B30。效果如下：  

![](https://cdn.jsdelivr.net/gh/gf9276/image/oh-my-posh/20220918212744.png)  

但是在vs code下会有Bug，如下：  

![](https://cdn.jsdelivr.net/gh/gf9276/image/oh-my-posh/20220918212942.png)  

所以还是用下面的比较好

## 3.2. 1_shell
对源码进行更改，如下所示：
```
{
  "$schema": "https://raw.githubusercontent.com/JanDeDobbeleer/oh-my-posh/main/themes/schema.json",
  "blocks": [
    {
      "alignment": "left",
      "newline": true,
      "segments": [
        {
          "foreground": "#c0ebd7",
	  "leading_diamond": "<#ff70a6> \ue200</>",
          "properties": {
            "linux": "<##c0ebd7>\ue27f</>",
            "macos": "<##c0ebd7>\ue27f</>",
            "windows": "<##c0ebd7>\ue27f</>"
          },
          "style": "diamond",
          "template": " {{ if .WSL }}WSL at {{ end }}{{.Icon}} ",
          "type": "os"
        },
        {
          "foreground": "#4c8dae",
          "style": "diamond",
          "template": "\uf489 {{ .Name }} ",
          "type": "shell"
        },
        {
          "foreground": "#ffbebc",
          "properties": {
            "display_host": true
          },
          "style": "diamond",
          "template": "{{ .UserName }}",
          "type": "session"
        },
        {
          "foreground": "#bc93ff",
          "properties": {
            "time_format": "15:04:05"
          },
          "style": "diamond",
          "template": " \uf64f {{ .CurrentDate | date .Format }} ",
          "type": "time"
        },
        {
          "foreground": "#ee79d1",
          "properties": {
            "branch_icon": "\ue725 ",
            "fetch_stash_count": true,
            "fetch_status": true,
            "fetch_upstream_icon": true,
            "fetch_worktree_count": true
          },
          "style": "diamond",
          "template": " {{ .UpstreamIcon }}{{ .HEAD }}{{if .BranchStatus }} {{ .BranchStatus }}{{ end }}{{ if .Working.Changed }} \uf044 {{ .Working.String }}{{ end }}{{ if and (.Working.Changed) (.Staging.Changed) }} |{{ end }}{{ if .Staging.Changed }} \uf046 {{ .Staging.String }}{{ end }}{{ if gt .StashCount 0 }} \uf692 {{ .StashCount }}{{ end }} ",
          "type": "git"
        }
      ],
      "type": "prompt"
    },
    {
      "alignment": "right",
      "segments": [
        {
          "foreground": "#a9ffb4",
          "style": "plain",
          "type": "text"
        },
        {
          "foreground": "#FFE873",
          "style": "diamond",
          "template": "<#ffffff>(</>\ue235 {{ if .Error }}{{ .Error }}{{ else }}{{ if .Venv }}{{ .Venv }} {{ end }}{{ .Full }}{{ end }}<#ffffff>)</>",
          "type": "python"
        },
        {
          "foreground": "#ec2729",
          "style": "plain",
          "template": "<#ffffff>(</>{{ if .Error }}{{ .Error }}{{ else }}{{ .Full }}{{ end }}<#ffffff>)</>",
          "type": "java"
        },
        {
          "foreground": "#0d6da8",
          "style": "plain",
          "template": "<#ffffff>(</>{{ if .Unsupported }}\uf071{{ else }}{{ .Full }}{{ end }}<#ffffff>)</>",
          "type": "dotnet"
        },
        {
          "foreground": "#06aad5",
          "style": "plain",
          "template": "<#ffffff>(</>{{ if .Error }}{{ .Error }}{{ else }}{{ .Full }}{{ end }}<#ffffff>)</>",
          "type": "go"
        },
        {
          "foreground": "#925837",
          "style": "plain",
          "template": "<#ffffff>(</>{{ if .Error }}{{ .Error }}{{ else }}{{ .Full }}{{ end }}<#ffffff>)</>",
          "type": "rust"
        },
        {
          "foreground": "#055b9c",
          "style": "plain",
          "template": "<#ffffff>(</>{{ if .Error }}{{ .Error }}{{ else }}{{ .Full }}{{ end }}<#ffffff>)</>",
          "type": "dart"
        },
        {
          "foreground": "#ce092f",
          "style": "plain",
          "template": "<#ffffff>(</>{{ if .Error }}{{ .Error }}{{ else }}{{ .Full }}{{ end }}<#ffffff>)</>",
          "type": "angular"
        },
        {
          "foreground": "#ffffff",
          "style": "plain",
          "template": "<#1e293b>(</>{{ if .Error }}{{ .Error }}{{ else }}Nx {{ .Full }}{{ end }}<#1e293b>)</>",
          "type": "nx"
        },
        {
          "foreground": "#359a25",
          "style": "plain",
          "template": "<#ffffff>(</>{{ if .Error }}{{ .Error }}{{ else }}{{ .Full }}{{ end }}<#ffffff>)</>",
          "type": "julia"
        },
        {
          "foreground": "#9c1006",
          "style": "plain",
          "template": "<#ffffff>(</>{{ if .Error }}{{ .Error }}{{ else }}{{ .Full }}{{ end }}<#ffffff>)</>",
          "type": "ruby"
        },
        {
          "foreground": "#5398c2",
          "style": "plain",
          "template": "<#ffffff>(</>{{ if .Error }}{{ .Error }}{{ else }}{{ .Full }}{{ end }}<#ffffff>)</>",
          "type": "azfunc"
        },
        {
          "foreground": "#faa029",
          "style": "plain",
          "template": "<#ffffff>(</>{{.Profile}}{{if .Region}}@{{.Region}}{{end}}<#ffffff>)</>",
          "type": "aws"
        },
        {
          "foreground": "#316ce4",
          "style": "plain",
          "template": "<#ffffff>(</>{{.Context}}{{if .Namespace}} :: {{.Namespace}}{{end}}<#ffffff>)</>",
          "type": "kubectl"
   	},
        {
          "foreground": "#a9ffb4",
          "properties": {
            "style": "dallas",
            "threshold": 0
          },
          "style": "diamond",
          "template": " {{ .FormattedMs }}s <#ffffff>\ue601</>",
          "type": "executiontime"
        },
        {
          "properties": {
            "root_icon": "\uf292 "
          },
          "style": "diamond",
          "template": " \uf0e7 ",
          "type": "root"
        },
        {
          "foreground": "#94ffa2",
          "style": "diamond",
          "template": " <#ffffff>CPU:</> {{ round .PhysicalPercentUsed .Precision }}% ",
          "type": "sysinfo"
        },
        {
          "foreground": "#81ff91",
          "style": "diamond",
          "template": "<#ffffff>\uf6dc</> <#ffffff>RAM:</> {{ (div ((sub .PhysicalTotalMemory .PhysicalFreeMemory)|float64) 1000000000.0) }}/{{ (div .PhysicalTotalMemory 1000000000.0) }}GB ",
          "type": "sysinfo"
        }
      ],
      "type": "prompt"
    },
    {
      "alignment": "left",
      "newline": true,
      "segments": [
        {
          "foreground": "#ffafd2",
          "leading_diamond": "<#00c7fc> \ue285 </><#ffafd2>{</>",
          "properties": {
            "folder_icon": "\uf07b",
            "folder_separator_icon": "\uf9e0",
            "home_icon": "\uf7db",
            "style": "agnoster_full"
          },
          "style": "diamond",
          "template": " \ue5ff {{ .Path }} ",
          "trailing_diamond": "<#ffafd2>}</>",
          "type": "path"
        },
        {
          "foreground": "#A9FFB4",
          "foreground_templates": [
            "{{ if gt .Code 0 }}#ef5350{{ end }}"
          ],
          "properties": {
            "always_enabled": true
          },
          "style": "plain",
          "template": " \ue286 ",
          "type": "exit"
        }
      ],
      "type": "prompt"
    }
  ],
  "console_title_template": "{{ .Folder }}",
  "transient_prompt": {
    "background": "transparent",
    "foreground": "#FEF5ED",
    "template": "\ue285 "
  },
  "version": 2
}
```

windows terminal 的配色方案为

```
        {
            "background": "#292D3E",
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
```
背景色建议改成`15,17,26`（十进制）

当然了，终端字体都需要更改。terminal效果图如下所示：

![](https://cdn.jsdelivr.net/gh/gf9276/image/oh-my-posh/terminal_1_shell.png)

vscode效果图如下所示（别忘了换字体）：

![](https://cdn.jsdelivr.net/gh/gf9276/image/oh-my-posh/vscode_1_shell.png)

在~/路径下按照上面代码创建 my_1_shell.omp.json 文件，.bashrc 里添加如下代码：

```
# oh my posh 的主题路径
eval "$(oh-my-posh --init --shell bash --config ~/my_1_shell.omp.json)"
```
