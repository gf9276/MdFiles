# vscode

## 安装

下载便携版就行，解压到D，
如改图所示，创建一个 data 文件夹

![](https://cdn.jsdelivr.net/gh/gf9276/image/vscode/20221109195229.png)

再创建一个 tmp 文件夹

![](https://cdn.jsdelivr.net/gh/gf9276/image/vscode/20221109195308.png)

换 vscode 版本的时候把 data 复制走就行，很方便的

## wsl2 关联 vscode 

.bashrc 文件夹里添加

```
# 便携版vscode存放路径
export PATH="$PATH:/mnt/d/VSCode-win32-x64-portable/bin"
```

即可关联，```/mnt/d/VSCode-win32-x64-portable``` 表示 vscode 便携版的路径

然后在 wsl2 下用 code . 就可以打开 vscode 了