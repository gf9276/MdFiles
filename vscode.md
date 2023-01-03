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

## github

很神奇，普通的git上传需要token ssh密钥之类的，为什么vscode 连密码都省了，写不需要token，直接就能用。密码我可能在哪里已经输过了，但是居然连密钥都不需要，很神奇。

<font color=#A020F0 > 好像是因为，vscode第一次会和github连起来，就像[这里的一样](https://blog.csdn.net/weixin_46161565/article/details/121010385)  </font>，那没事了，可是我直接git还是要token或者ssh，真是麻烦捏，好！vscode 真好用