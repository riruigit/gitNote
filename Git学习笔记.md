# Git学习笔记

## 简单描述

免费开源的==分布式版本控制系统==，SVN 是集中管理，Git是分布式。

## 官方文档

中文版：https://git-scm.com/book/zh/v2

## 下载安装

官网链接： https://git-scm.com/download  不想安装的话，可以使用免安装的版本，配置一下环境变量就可以了。

## 首次使用

Git安装完了之后，需要配置一下用户签名，否则提交不了代码。**==注意==**：这里设置的用户签名以及邮箱最好是和Github上面的保持一致。

## 初始化本地库

```xml
git init 
```

进到一个目录，然后使用init初始化一个仓库，**初始化成功的标志**：在目录下生成一个  .git  的隐藏文件夹。

## 查看库状态

```xml
git status
```

没有添加到暂存区的（也就是还没有执行 git add ）的文件会显示红色。如果加入 -s   就是简单输出。

-   ？？ 指的是新添加的文件，还没执行add的文件
-   A 指的是，已经执行了add的指令，新追加的文件
-   AM 指的是，新添加的文件发生了改变，没有执行add，所以M是红色的
-   M 指的是，原本就存在的文件，发生了修改，然后又add一次。（M是绿色的）
-   MM指的是，原本就存在的文件，发生了修改，然后没有执行add。（后面的M是红色的）

## 添加暂存区

git在本地分为三个区，一个是工作区，一个暂存区，一个本地库

```xml
git add 文件名
git add .
```

一个是添加单个文件，当然可以把整个文件夹也添加上去。添加之后，文件会放到暂存区，在使用 git status  ， 文件就会从红色变成绿色。

## 复原暂存区

```xml
git restore --staged 文件名
git restore 文件名
```

两者的区别就是：-- staged 将文件从暂存区撤出，但不会撤销工作区文件的更改。

不带参数：将不在暂存区的文件撤销更改（即git status 提示修改的，但没有加入暂存区的内容，会被撤销）

## 删除暂存区文件

```xml
git rm --cached 文件名
```

## 提交本地库

```xml
git commit -m "日志信息" 文件名
```

如果上面的指令再加上 -a 那么就会把工作区的修改过的都提交上去，相当于add 和 commit 操作

```xml
git restore --staged
```

可以重新编辑commit时候写的日志信息

## 查看版本

```xml
git reflog
git log
```

reflog 是查看版本信息（比较简单）log 是查看版本详细的信息

## 版本穿梭

```xml
git reset --hard 版本号
```

版本穿梭的基本步骤： 先查看版本号，然后在 reset 过去。（底层就是在移动 HEAD 指针的指向）

## 创建分支

```xml
git branch 分支名
```

## 查看分支

```xml
git branch -v 查看分支
```

## 重命名分支

```xml
git branch -m 分支名
```

## 切换分支

```xml
git checkout 分支名
```

## 删除分支

```xml
git branch -d 分支名
```

## 合并分支

```xml
git merge 分支名
```

## 合并冲突

冲突产生的表现：状态为 MERGING 

冲突产生的原因：两个分支在同一个文件的同一个位置有两套完全不同的修改，Git 无法决定，需要人为决定。

## 解决冲突

先查看状态，然后编辑有冲突的文件，删除特殊符号，人工解决冲突。

冲突标记：<<<HEAD  当前代码 =\== 合并过来的代码 >>>> 合并分支名

## 远程仓库克隆

```xml
git clone 链接
```

通过clone把整个项目克隆下来。（此操作不用初始化）

## 查看远程仓库

```xml
git remote -v
```

查看远程仓库的别名以及对应的链接

## 添加远程仓库

```xml
git remote add <别名> 链接
```

设置了别名，相当于用别名代替链接。

这里推荐使用ssh的那个链接，一个是免密码登录，一个是比较稳妥（https可能有网络问题，以及密码不对啥的）

## 移除远程仓库

```xml
git remote remove 别名
```

注意这里也不会删除github上面的远程仓库，删除的是本地的一个关联（别名的删除应该）

## 从远程库抓取与拉取

```xml
git fetch <远程库别名>
```

这个命令会访问远程仓库，从中拉取所有你还没有的数据，执行完之后，执行完之后，你将拥有那个远程仓库中所有分支的引用。

## 推送远程仓库

```xml
git push 别名 分支名
```

## 远程仓库的重命名

```xml
git remote rename 旧的名字 新的名字
```

注意这里是不会修改github上面的命名。

## 删除远程仓库分支

```xml
git push 别名 --delete 分支名
```

这个是会删除github上面的分支的。

## 团队协作

远程库（一般是github 或者 git） 克隆整个仓库下来使用 clone , 如果本地有了这个项目，但是github 或者 git 上面修改了本地想同步一下，就使用 pull , 如果是推送到GitHub 就使用 push 。

邀请别人一起开发（同一个团队）：仓库Setting -> Manage access -> Invite a collaborator -> 发送邀请。

## 跨团队协作

先把别人的远程库给 ==fork==过来，然后自己修改好了，然后 Pull request , 等待审核，然后别人 merge(合并) 。

## 配置免密码登录

推荐使用的方式，ssh验证会比https那个稳定好多。如果本机有了ssh的密钥，可以先删除在重新操作。

```xml
ssh-keygen -t rsa -C 邮箱
```

这时候C盘用户文件夹下的本机文件夹会有一个 .ssh 的文件夹，复制 id_rsa.pub文件，登陆 github -> 用户头像 -> Settings -> SSH and GPG keys  -> new SSH key -> 粘贴就可以了。

# idea集成idea

## idea初始化仓库

VCS -> import into Version Control -> Create Git Repository 然后选择整个项目文件夹就可以。

## idea添加暂存区

右键点击项目，选择 Git -> Add 即可将文件添加到暂存区。

## idea提交本地库

右键点击项目，选择 Git -> Commit Directory..  即可将文件提交到本地库。

## idea切换版本

在idea的左下角，点击 Version Control ,然后点击 Log 查看版本，选择切换的版本右键，选择 Checkout Revision。

## idea创建分支

idea中项目右键选择 Git -> Repository -> Branches -> New Beanch

## idea切换分支

idea窗口右下角git的入口那里，选择分支，然后 Checkout。

## idea合并分支

iea窗口右下角，合并分支是 Merge into Current

## idea分享到 GitHub

VCS -> Import into Version Control -> Share Project on Github

## idea push到 Github

idea中项目右键选择 Git -> Repository -> Branches -> push

注意：这个可以自己 Define Remote ,要在这里设置 ssh 链接，上传github都用 ssh 协议！！！

还有一点，本地库的版本号要比远程库的高才可以成功。在修改代码之前，可以先 pull 一下，保持同步。不然很多麻烦。

