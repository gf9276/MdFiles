# git

## git 指令

[看这个网站(廖雪峰的)](https://www.liaoxuefeng.com/wiki/896043488029600)


### 查看修改git账户信息

```
git config --global --list   // 查看当前git的配置信息
git config user.name
git config user.email   
```

### 设置全局（修改和添加都是这样子的）

```
git config --global user.password ******
git config --global user.name ******
git config --global user.email ******
```

### 创建版本库

直接在目录底下，执行```git init```就可以了

### 添加和提交

```git add file1.txt file2.txt``` 直接将文件添加到仓库，可以是新的文件，也可以是旧的改过的文件

```git commit -m xxxx``` 将文件提交到仓库 ， ```xxxx```是版本说明，想写什么写什么


### 版本管理

#### 查看版本变化

```git log``` --> 显示各个版本的信息

#### 回退到上一个版本

```git reset --hard HEAD^```


#### 选择指定id的版本（可以用于回退或者恢复）

```git reset --hard 1094a```，```1094a```就是对应般的 commit  id

#### 如果忘记 commit id 怎么办

执行指令```git reflog```，就可以看到了

### 工作区和暂存区

```git add```指令就是将文件添加到暂存区

```git commit```指令将暂存区文件提交到工作区

工作区就是我们在用的，文件所在地

```git status```会告诉你，什么文件修改了，什么文件待添加

### 管理修改

修改只有在add了之后，commit才有效。

### 撤销修改

```git checkout -- readme.txt``` 将工作区内容回退到最近一次add或者commit后的状态

```git reset HEAD readme.txt``` 抛弃暂存区的修改

### 删除文件

```git rm test.txt``` git 删除文件，然后再提交即可

想要恢复删除，使用前面的```git commit -- test.txt```就可以了


## github

### 创建一个新的仓库

![](https://cdn.jsdelivr.net/gh/gf9276/image/git/20230103155214.png)

直接创建就行了，名字写git_test

ok 现在创建完成了
![](https://cdn.jsdelivr.net/gh/gf9276/image/git/20230103155352.png)

### 关联本地仓库

在本地使用```git remote add origin https://github.com/gf9276/git_test.git```，其中```https://github.com/gf9276/git_test.git```是仓库的url，```origin```是远程仓库的别名


随后，使用 ```git pull --rebase origin main```拉取并且合并信息，这里的```main```是本地分支，也是远程分支的名字，如果是同一个的话，可以这样子写，省略掉，实际上，完整的应该是
```
git push <远程主机名> <本地分支名>:<远程分支名>
git push origin master:main
```

### 分支管理

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

```git branch```可用于查看当前分支

```git merge```用于合并分支到当前分支上（当前分支应该版本需要比较靠前），感觉怪怪的，感觉这不叫合并，叫复制覆盖之类的差不多

```git branch -d <name>```删除分支

### 解决冲突

![](https://cdn.jsdelivr.net/gh/gf9276/image/git/20230103184307.png)

这样子merge会有冲突，居然要我手动改，真是奇怪，要我说这种就不好合并，直接选一个保留就行。


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