---
title: 在这做一些日常搜索的笔记
date: 2019-01-09 11:47:11
category: 后台
tags: python
---
![](https://images.unsplash.com/photo-1484480974693-6ca0a78fb36b?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=crop&w=1200&q=20)
<!-- more -->

## 当使用makemigrations时报错No changes detected：
(转https://blog.csdn.net/qq_39291784/article/details/78397589)

在修改了models.py后，有些用户会喜欢用python manage.py makemigrations生成对应的py代码。

但有时执行python manage.py makemigrations命令（也可能人比较皮，把migrations文件夹给删了），会提示"No changes detected." 可能有用的解决方式如下：

先 python manage.py makemigrations --empty yourappname 生成一个空的initial.py

再 python manage.py makemigrations 生成原先的model对应的migration file



## 记录下git的一些操作指令，有时候忘了老要查
(转https://www.jianshu.com/p/3be4029ce854)

```shell
git branch -r       #查看远程所有分支

git branch           #查看本地所有分支

git branch -a       #查看本地及远程的所有分支，如下图

git fetch   #将某个远程主机的更新，全部取回本地：

git branch -a  #查看远程分支

git branch  #查看本地分支：

git checkout 分支 #切换分支：

git push origin -d 分支名  #删除远程分支: 

 git branch -d 分支名  #删除本地分支

git remote show origin  #查看远程分支和本地分支的对应关系

git remote prune origin #删除远程已经删除过的分支
```


情景1：同步别人新增到远程的分支
```
1.git branch查看一下本地分支，再git branch -a查看一下远程分支，对比下，远程存在哪些本地没有的新分支.
  2.将某个远程主机的更新，全部取回本地：git fetch
  3.再次查看远程分支：git branch -a 发现远程的分支已经可以看见了
  4.拉取远程分支到本地：git checkout -b 远程分支名
```

情景2：本地删除了分支，远程也想删除
2.1:本地想要删除某个分支，远程仓库的这个分支也要删掉怎么办？
```
a.使用git branch -d 分支名来删除本地分支。
  b.使用git push origin -d 分支名直接来删除远程分支。在次使用git branch -a,发现分支已经不存在了。
or
   a.使用git branch -d 分支名来删除本地分支。
    b.最简单的解决办法就是直接到gitlab/github进行删除.
```

2.2:只把远程的删除掉怎么办？
```
a.使用git push origin -d 分支名直接来删除远程分支。此时删除的只是远程的分支，本地仍然存在
or
a.直接到gitlab/github进行删除.
```

2.3:远程删除了分支，本地也想删除
eg:直接到gitlab/github删除了某个分支，我在本地使用git branch -a查看远程分支，依然存在并且可以切换使用。我本地也想把远程分支记录删除怎么办？
```
1.git branch -a查看远程分支，红色的是本地远程远程分支记录。

2.执行下面命令查看远程仓库分支和本地仓库的远程分支记录的对应关系：

  git remote show origin  

3.会看到：
 
 refs/remotes/origin/远程仓库已经删除的分支名    stale (use 'git remote prune' to remove)

 其中：

 Local refs configured for 'git push':  命令下面的分支是本地仓库的远程分支记录中仍存在的分支，但远程仓库已经不存在。

4.输入git remote prune origin来删除远程仓库已经删除过的分支

5.验证 git branch -a

  此时可以看到本地远程分支记录已经和远程仓库保持一致了。

6.修改当前最新未提交的commit
  git commit --amend

7.修改过往的commit
  git rebase -i commitId
  其中中，-i 的参数是不需要合并的 commit 的 hash 值

8.回滚指定文件
    暂定此文件为a.jsp

    1.首先到a.jsp所在目录：
    通过 git  log a.jsp
    查看a.jsp的更改记录

    2.找到想要回退的版本号：例如 fcd2093
    通过 git reset  fcd2093 a.jsp
    把文件回退

    3.提交本次回退
    git commit -m "注释内容"

    4.切回目录
    git checkout a.jsp

    5.提交到远程仓库
    git push origin master

```

#### 解决 canvas 将图片转为base64报错: Uncaught DOMException: Failed to execute 'toDataURL' 

问题描述
当用户点击分享按钮时，生成一张海报，可以保存图片分享到朋友圈，用户的图片是存储在阿里云的OSS,当海报完成后，执行.canvas.toDataURL("image/png")时，出现index.html:28 Uncaught DOMException: Failed to execute 'toDataURL' on 'HTMLCanvasElement': Tainted canvases may not be exported的错误提示，这句话的翻译是uncaught domexception:未能对“htmlcanvaselement”执行“todataurl”：无法导出受污染的画布。因为图片跨域了，对画布造成了污染。

解决方法
搜索相关问题，大多是为img设置crossOrigin属性，实现图片允许跨域，即：img.setAttribute("crossOrigin",'Anonymous')，我为图片添加这个属性后，图片无法显示了，报了一个错误:  Access to image at 'https://claystar.oss-cn-shenzhen.aliyuncs.com/pic/9dd6d55c5c7334d9448c4628e6ff69f6.jpg' from origin 'null' has been blocked by CORS policy: No 'Access-Control-Allow-Origin' header is present on the requested resource.        

通过和同事沟通讨论得知，只需要为图片添加一个时间戳即可。最后解决方式：                                         

img.src='http://www.xxxx.png' + '?time=' + new Date()