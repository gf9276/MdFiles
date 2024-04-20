<!-- TOC -->

- [1. git.md](#1-gitmd)
- [2. git 指令](#2-git-指令)
  - [2.1. ubuntu 下静止自动转成 crlf ！！！](#21-ubuntu-下静止自动转成-crlf-)
  - [2.2. 设置全局代理](#22-设置全局代理)
  - [2.3. 查看修改 git 账户信息](#23-查看修改-git-账户信息)
  - [2.4. 设置全局（修改和添加都是这样子的）](#24-设置全局修改和添加都是这样子的)
  - [2.5. 创建版本库](#25-创建版本库)
  - [2.6. 添加和提交](#26-添加和提交)
  - [2.7. 版本管理](#27-版本管理)
    - [2.7.1. 查看版本变化](#271-查看版本变化)
    - [2.7.2. 回退到上一个版本](#272-回退到上一个版本)
    - [2.7.3. 选择指定 id 的版本（可以用于回退或者恢复）](#273-选择指定-id-的版本可以用于回退或者恢复)
    - [2.7.4. 如果忘记 commit id 怎么办](#274-如果忘记-commit-id-怎么办)
  - [2.8. 工作区和暂存区](#28-工作区和暂存区)
  - [2.9. 管理修改](#29-管理修改)
  - [2.10. 撤销修改](#210-撤销修改)
  - [2.11. 删除文件](#211-删除文件)
- [3. github](#3-github)
  - [3.1. 创建一个新的仓库](#31-创建一个新的仓库)
  - [3.2. 关联本地仓库](#32-关联本地仓库)
  - [3.3. 分支管理](#33-分支管理)
  - [3.4. 解决冲突](#34-解决冲突)
  - [3.5. github 做图床](#35-github-做图床)
    - [3.5.1. 创建仓库](#351-创建仓库)
    - [3.5.2. 下载个 picgo 用于上传图片](#352-下载个-picgo-用于上传图片)
    - [3.5.3. 获取 token](#353-获取-token)
    - [3.5.4. 设置](#354-设置)

<!-- /TOC -->

# 1. git.md

记录 git 的用法 和 学习 git

这个写的有点乱

# 2. git 指令

[看这个网站(廖雪峰的)](https://www.liaoxuefeng.com/wiki/896043488029600)

## 2.1. ubuntu 下静止自动转成 crlf ！！！

```
git config --global core.autocrlf false
```

## 2.2. 设置全局代理

```
git config --global http.https://github.com.proxy socks5h://127.0.0.1:7890
git config --global https.https://github.com.proxy socks5h://127.0.0.1:7890
```

啥系统都一样的，后面`socks5h://127.0.0.1:7890`是代理模式、ip 和端口，自己改

## 2.3. 查看修改 git 账户信息

```
git config --global --list   // 查看当前git的配置信息
git config user.name
git config user.email
```

## 2.4. 设置全局（修改和添加都是这样子的）

```
git config --global user.password ******
git config --global user.name ******
git config --global user.email ******
```

## 2.5. 创建版本库

直接在目录底下，执行`git init`就可以了

## 2.6. 添加和提交

`git add file1.txt file2.txt` 直接将文件添加到仓库，可以是新的文件，也可以是旧的改过的文件

`git commit -m xxxx` 将文件提交到仓库 ， `xxxx`是版本说明，想写什么写什么

## 2.7. 版本管理

### 2.7.1. 查看版本变化

使用 `git log` --> 显示各个版本的信息

### 2.7.2. 回退到上一个版本

使用 `git reset --hard HEAD^`

### 2.7.3. 选择指定 id 的版本（可以用于回退或者恢复）

使用 `git reset --hard 1094a`，`1094a`就是对应般的 commit id

### 2.7.4. 如果忘记 commit id 怎么办

执行指令`git reflog`，就可以看到了

## 2.8. 工作区和暂存区

使用 `git add` 指令就是将文件添加到暂存区

使用 `git commit` 指令将暂存区文件提交到工作区

工作区就是我们在用的，文件所在地

使用 `git status` 会告诉你，什么文件修改了，什么文件待添加

## 2.9. 管理修改

修改只有在 add 了之后，commit 才有效。

## 2.10. 撤销修改

使用 `git checkout -- readme.txt` 将工作区内容回退到最近一次 add 或者 commit 后的状态

使用 `git reset HEAD readme.txt` 抛弃暂存区的修改

## 2.11. 删除文件

使用 `git rm test.txt` git 删除文件，然后再提交即可

想要恢复删除，使用前面的`git commit -- test.txt`就可以了

# 3. github

## 3.1. 创建一个新的仓库

![](https://cdn.jsdelivr.net/gh/gf9276/image/git/20230103155214.png)

直接创建就行了，名字写 git_test

ok 现在创建完成了
![](https://cdn.jsdelivr.net/gh/gf9276/image/git/20230103155352.png)

## 3.2. 关联本地仓库

在本地使用`git remote add origin https://github.com/gf9276/git_test.git`，其中`https://github.com/gf9276/git_test.git`是仓库的 url，`origin`是远程仓库的别名

随后，使用 `git pull --rebase origin main`拉取并且合并信息，这里的`main`是本地分支，也是远程分支的名字，如果是同一个的话，可以这样子写，省略掉，实际上，完整的应该是

```
git push <远程主机名> <本地分支名>:<远程分支名>
git push origin master:main
```

## 3.3. 分支管理

看看这个[分支管理](https://www.liaoxuefeng.com/wiki/896043488029600/900003767775424)

创建新的分支 dev

```
$ git checkout -b dev  或者  git switch -c dev
```

等效于（创建并切换）

```
$ git branch dev
$ git checkout dev  或者 git switch dev
```

使用 `git branch` 可用于查看当前分支

使用 `git merge` 用于合并分支到当前分支上（当前分支应该版本需要比较靠前），感觉怪怪的，感觉这不叫合并，叫复制覆盖之类的差不多

使用 `git branch -d <name>` 删除分支

## 3.4. 解决冲突

![](https://cdn.jsdelivr.net/gh/gf9276/image/git/20230103184307.png)

这样子 merge 会有冲突，居然要我手动改，真是奇怪，要我说这种就不好合并，直接选一个保留就行。

## 3.5. github 做图床

小心大小超过 1G，谨防

### 3.5.1. 创建仓库

这个你会的。。。
![](https://cdn.jsdelivr.net/gh/gf9276/image/git/20221112171150.png)

### 3.5.2. 下载个 picgo 用于上传图片

[官网](https://molunerfinn.com/PicGo/)，挑个稳定版本。比如 2.3.0。

### 3.5.3. 获取 token

这玩意是用来调用 github API 的。直接嫖别人的图片了

![](https://cdn.jsdelivr.net/gh/gf9276/image/git/20221112171311.png)

![](https://cdn.jsdelivr.net/gh/gf9276/image/git/20221112171337.png)

![](https://cdn.jsdelivr.net/gh/gf9276/image/git/20221112171347.png)

![](https://cdn.jsdelivr.net/gh/gf9276/image/git/20221112171403.png)

token 过期了直接复活就行，不用重新申请

### 3.5.4. 设置

![](https://cdn.jsdelivr.net/gh/gf9276/image/git/20221112171458.png)

- 仓库名就是 用户/仓库

- 分支名去 github 看，有的可能是 master

![](https://cdn.jsdelivr.net/gh/gf9276/image/git/20221112171555.png)

- Token 前面有

- 指定存储路径 是 以仓库名为根目录的，没有就直接存在仓库名下

- 自定义域名，上传加速用的

```
https://cdn.jsdelivr.net/gh/用户名/仓库名
```
