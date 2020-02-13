##　参考资料



<https://blog.csdn.net/buknow/article/details/80325986>

<https://blog.csdn.net/qq_41782425/article/details/85183250>

##　注册github

## 下载git bash

ssh默认保存的地址

![1581345537440](C:\Users\HC\AppData\Roaming\Typora\typora-user-images\1581345537440.png)

## 上传

~~~java


~~~

~~~java
//系统兼容性问题
$ git config core.autocrlf false

~~~

git步骤:

克隆整个仓库

~~~java
//创建一个文件夹GitHub_upload
mkdir GitHub_upload
//将GitHub上的仓库克隆到本地
$ git clone https://github.com/hc-java/java.git
//cd 仓库名,进去后会有一个（master）
//将工作区创建的test文件夹添加到暂存区
$ git add test/
//将暂存区的内容提交到仓库区，-m 为备注信息
$ git commit -m "test"
//将仓库去的内容推送到远程仓库GitHub上
$ git push origin master
~~~



不用克隆整个仓库

~~~java
//创建仓库,然后在用上面的上传
git init

~~~



## 下载



~~~java
git clone https://github.com/hc-java/java.git
~~~



## 查看历史版本

~~~java
//查看提交记录
git log
//回到某一个历史版本
git checkout commitID
//看完回到最新版本
git checkout master

~~~



github版本回退：<https://blog.csdn.net/ccorg/article/details/80115408>

~~~java
$ git reset --hard commitId
$ git push -f -u origin master 
$ git pull
~~~



## DownGit

原博客：<https://www.itsvse.com/thread-7086-1-1.html>

DownGit：<https://www.itsvse.com/downgit/#/home>

可以下载整个仓库中的所有项目（单独某个文件也可以下载），无法单独下载项目（下载下来的包是空的）

## 创建新分支并且合并分支

<https://blog.csdn.net/weixin_40703303/article/details/97157501>



## 删除

只删除远程仓库，不删除本地仓库

~~~git
//删除test文件夹
git rm -r --cached test
//将暂存区的内容提交到本地仓库区，-m 为备注信息
git commit -m 'del-test'
//master分支推送origin主机
git push -u origin master

~~~

报错：

~~~git
To https://github.com/hc-java/java.git
 ! [rejected]        master -> master (fetch first)
error: failed to push some refs to 'https://github.com/hc-java/java.git'
hint: Updates were rejected because the remote contains work that you do
hint: not have locally. This is usually caused by another repository pushing
hint: to the same ref. You may want to first integrate the remote changes
hint: (e.g., 'git pull ...') before pushing again.
hint: See the 'Note about fast-forwards' in 'git push --help' for details.

~~~

解决：

~~~git
//取回远程主机某个分支的更新，再与本地的指定分支合并
$ git pull origin master
//
$ git push origin master
~~~







## 可能出现的错误

### Q1

fatal: not a git repository (or any of the parent directories): .git

没有进入(master)

### Q2

warning: LF will be replaced by CRLF”

~~~java
#提交时转换为LF，检出时转换为CRLF
$ git config --global core.autocrlf true
~~~

