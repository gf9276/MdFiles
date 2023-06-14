<!-- TOC -->

- [1. vscode.md](#1-vscodemd)
- [2. windows 安装vscode](#2-windows-安装vscode)
- [3. wsl2 关联 vscode](#3-wsl2-关联-vscode)
- [4. ubuntu 或 wsl 里安装 vscode](#4-ubuntu-或-wsl-里安装-vscode)
- [5. 关于github](#5-关于github)

<!-- /TOC -->

# 1. vscode.md

记录 vscode 的一些设置

# 2. windows 安装vscode

下载便携版就行（方便我在不同电脑里挪动），解压到D，
如改图所示，创建一个 data 文件夹

![](https://cdn.jsdelivr.net/gh/gf9276/image/vscode/20221109195229.png)

再创建一个 tmp 文件夹

![](https://cdn.jsdelivr.net/gh/gf9276/image/vscode/20221109195308.png)

换 vscode 版本的时候把 data 复制走就行，很方便的

# 3. wsl2 关联 vscode 

wsl可以直接用windows里的vscode 

.bashrc 文件夹里添加

```
# 便携版vscode存放路径
export PATH="$PATH:/mnt/d/VSCode-win32-x64-portable/bin"
```

即可关联，```/mnt/d/VSCode-win32-x64-portable``` 表示 vscode 便携版的路径

然后在 wsl2 下用 code . 就可以打开 vscode 了

# 4. ubuntu 或 wsl 里安装 vscode 

如果想直接在wsl里面安装vscode，和ubuntu没啥区别

直接去官网下载.deb就好了

![](https://cdn.jsdelivr.net/gh/gf9276/image/vscode/20230614184159.png)

下载下来直接 

```
sudo dpkg -i code_1.79.0-1686149120_amd64.deb
```

# 5. 关于github

很神奇，普通的git上传需要token ssh密钥之类的，为什么vscode 连密码都省了，写不需要token，直接就能用。密码我可能在哪里已经输过了，但是居然连密钥都不需要，很神奇。

<font color=#A020F0 > 好像是因为，vscode第一次会和github连起来，就像[这里的一样](https://blog.csdn.net/weixin_46161565/article/details/121010385)  </font>，那没事了，可是我直接git还是要token或者ssh，真是麻烦捏，好！vscode 真好用