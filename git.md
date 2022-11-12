# git

## git 指令

TODO

## github

### github 做图床

小心大小超过1G，谨防

#### 1、创建仓库

这个你会的。。。
![](https://cdn.jsdelivr.net/gh/gf9276/image/git/20221112171150.png)

#### 2、下载个picgo用于上传图片

[官网](https://molunerfinn.com/PicGo/)，挑个稳定版本。比如 2.3.0。

#### 3、获取token

这玩意是用来调用 github API 的。直接嫖别人的图片了

![](https://cdn.jsdelivr.net/gh/gf9276/image/git/20221112171311.png)

![](https://cdn.jsdelivr.net/gh/gf9276/image/git/20221112171337.png)

![](https://cdn.jsdelivr.net/gh/gf9276/image/git/20221112171347.png)

![](https://cdn.jsdelivr.net/gh/gf9276/image/git/20221112171403.png)


token过期了直接复活就行，不用重新申请

#### 4、设置

![](https://cdn.jsdelivr.net/gh/gf9276/image/git/20221112171458.png)

- 仓库名就是 用户/仓库

- 分支名去github看，有的可能是 master

![](https://cdn.jsdelivr.net/gh/gf9276/image/git/20221112171555.png)


- Token前面有

- 指定存储路径 是 以仓库名为根目录的，没有就直接存在仓库名下

- 自定义域名，上传加速用的

```
https://cdn.jsdelivr.net/gh/用户名/仓库名
```