## git常用命令展示图 [	](git_main_20191219101334804)

![image-20191219213439810](git_main.assets/image-20191219213439810.png)

## 环境  [	](git_main_20191219101334806)

### 在Windows上安装Git  [	](git_main_20191219101334808)

下载地址： https://gitforwindows.org/ 

安装完成后输入以下命令用以标识自己所使用的机器：

{{c1::

```shell
git config --global user.name "xieyun"
git config --global user.email "136412052@qq.com"
```

}}

## Git初始化  [	](git_main_20191219101334809)

### 初始化一个版本库  [	](git_main_20191219101334811)

在想要创建版本库的文件夹下输入以下命令：

{{c1::

```shell
git init
```

}}

### 把一个文件放到Git仓库需要两步：  [	](git_main_20191219101334813)

{{c1::

1. 第一步：用`git add`将文件提交到暂存区。

   ```shell
   git add readme.txt
   ```

2. 第二步：用`git commit`将文件提交仓库。

   ```shell 
   git commit -m "wrote a readme file"
   ```

}}

### Git常见操作 [	](git_main_20191220121129130)

```shell
# 查看工作区状态 {{c1:: 
git status }}
# 对比文件差异 {{c1:: 
git diff }}
# 查看版本历史 {{c1:: 
git log }}
# 以行为单位查看版本历史 {{c1:: 
git log --pretty=oneline }}
```

### 关于Git `warning：LF will be replaced by CRLF in readme.txt`问题的原因  [	](git_main_20191220070310161)

首先问题出{{c1:: 在不同操作系统所使用的换行符是不一样的。}}

`Uinx/Linux`采用换行符LF表示下一行{{c1:: （`LF：LineFeed`，中文意思是换行）；}}

Dos和Windows采用回车+换行CRLF表示下一行{{c1:: （`CRLF：CarriageReturn LineFeed`，中文意思是回车换行）；}}

Mac OS采用回车CR表示下一行{{c1:: （`CR：CarriageReturn`，中文意思是回车）。}}

### 在Git中，可以通过以下命令来显示当前你的Git中采取哪种对待换行符的方式 [	](git_main_20191220070310166)

```shell
#{{c1::  
git config core.autocrlf 
#}}
```

此命令会有三个输出，“true”，“false”或者“input”

- 为true时:{{c1:: Git会将你add的所有文件视为文本文件，将结尾的CRLF转换为LF，而checkout时会再将文件的LF格式转为CRLF格式。}}

- 为false时:{{c1:: line endings不做任何改变，文本文件保持其原来的样子。}}

- 为input时:{{c1:: add时Git会把CRLF转换为LF，而check时仍旧为LF，所以Windows操作系统不建议设置此值。}}


当 `core autocrlf`为true时，**还有一个需要慎重的地方**，{{c1::当你上传一个二进制文件，Git可能会将二进制文件误以为是文本文件，从而也会修改你的二进制文件，从而产生隐患。}}

## 版本管理  [	](git_main_20191219101334819)

### `git log`命令  [	](git_main_20191219101334821)

- `git log`:以列表的方式查看当前版本库的历史纪录
- `git log --pretty=oneline `:以行的方式查看当前版本库的历史纪录

### GIT的commit id（版本号）含义  [	](git_main_20191219101334823)

{{c1:: 

Git是分布式的系统，为了避免与其他人的版本冲突，GIT 的commit id（版本号）是由SHA1计算出的一串很大的16进制数。

}}

### Git版本回退  [	](git_main_20191219101334824)

```shell
# 回退到上一个版本{{c1:: 
git reset --hard HEAD^ }}
# 回退到前20版本{{c1::
git reset --hard HEAD~20 }}
# 使用commit id（版本号）回退到指定的版本{{c1:: 
git reset --hard 3628164 }}
# 查询历史命令记录{{c1:: 
git reflog }}
```

### 文件对比 [	](git_main_20191219101334825)

先列出两个版本间发生更改的文件列表{{c1:: 

```shell
git diff commit1 commit2 --stat --name-only
```

}}

查看指定文件在两个版本间发生的变更{{c1:: 

```shell
git diff commit1 commit2 -- somefile.js
```

}}

如果感觉这种显示不够直观，可以使用 `vimdiff `查看{{c1:: 

```shell
git difftool commit1 commit2 -- somefile.js
```

}}

### git 回退单个文件 [	](git_main_20191219101334827)

- 1.查看要回退的文件的历史版本
  
  ```shell
  # {{c1::
$ git log MainActivity.java
  #}}
  ```
  
- 2.回退到指定的版本
  
  ```shell
  # {{c1::
  $ git reset a4e215234aa4927c85693dca7b68e9976948a35e MainActivity.java
  #}}
  ```
- 3.提交到本地参考，注意不需要git add。
  
  ```shell
  # {{c1::
  $ git commit -m "revert old file because yjl commmit have a bug"
  #}}
  ```
- 4.更新到工作目录
  
  ```shell
  # {{c1::
  $ git checkout MainActivity.java
  #}}
  ```

### Git的一些名词解释:  [	](git_main_20191219101334829)

- 工作区：{{c1::当前项目中能看见的目录。}}
- 版本库：{{c1::.git是Git的版本库，不算工作区。}}
- 暂存区：{{c1::保存还没有提交到创库的修改。}}

## 文件修改  [	](git_main_20191219101334832)

### 管理修改  [	](git_main_20191219101334834)

```shell
# 对比工作区和版本库中最新版本的区别{{c1:: 
git diff HEAD -- readme.txt }}
# 让文件回到最近一次git commit或git add时的状态{{c1:: 
git checkout -- readme.txt}}
# 将暂存区的修改撤消掉{{c1:: 
git reset HEAD readme.txt }}
```

### 撤销修改的3种场景  [	](git_main_20191219101334835)

场景1：当你改乱了工作区某个文件的内容，想直接丢弃工作区的修改时，用命令{{c1:: `git checkout -- file`。}}

场景2：当你不但改乱了工作区某个文件的内容，还添加到了暂存区时，想丢弃修改，分两步，第一步用命令{{c1:: `git reset HEAD `，}}就回到了场景1，第二步按场景1操作。

场景3：已经提交了不合适的修改到版本库时，想要撤销本次提交，{{c1::`git reset HEAD^`}}，不过前提是没有推送到远程库。

### 删除操作：  [	](git_main_20191219101334837)

- 普通删除：在资源管理器中删除或者使用了`rm`命令:

- 从版本库中删除：{{c1:: 

   ```shell  
  $ git rm test
  ```

	}}

- 从版本库撤销普通删除：
{{c1:: 
  
  ```shell
  $ git checkout -- test.txt
  ```
}}

## 版本库推送  [	](git_main_20191219101334838)

### 在用户主目录中创建SSH KEY  [	](git_main_20191219101334840)

{{c1:: 

```shell
ssh-keygen -t rsa -C "136412052@qq.com"
```

}}

- `id_rsa`文件：{{c1:: 私钥}}
- `id_rsa.pub`文件：{{c1:: 公钥，目前用于添加到GitHub上用于推送，标识自己的计算机。}}

### 将本地项目推送到GitHub上  [	](git_main_20191219101334842)

- 第一步：{{c1::在GitHub上创建一个仓库。}}

- 第二步：{{c1::使用命令关联到GitHub

  ```shell
  git remote add origin git@github.com:xieyun/codeNote
  ```

  }}

- 
  第三步：{{c1::第一次推送推送(将当前分支推送到远程库上)

  ```shell
  #第一次推送加上-u，Git会把本地的master分支与github上的关联起来
  git push -u origin master
  ```

  }}

- 第四步：之后的推送

  ```shell
  #把本地的master分支的最新修改推送到GitHub
  {{c1::git push origin master}}
  ```

### 从远程克隆库  [	](git_main_20191219101334844)

{{c1::

```shell
git clone git@github.com:alibaba/easyexcel.git
```

- Git支持多种协议，包括https，但是通过SSH支持的原生git协议最快。}}

## 分支  [	](git_main_20191219101334846)

### 分支创建与切换  [	](git_main_20191219101334847)

```shell 
#git checkout命令加上-b参数标识创建并切换{{c1:: 
git checkout -b name }}
#相当于
#创建{{c1:: 
git branch name }}
#切换{{c1:: 
git checkout name }}
#查看当前分支{{c1:: 
git branch }}
```

### 分支的合并与删除  [	](git_main_20191219101334848)

```shell
#将指定分支合并到当前分支上{{c1:: 
git merge name }}
#删除分支{{c1:: 
git branch -d name }}
```

### 查看分支的合并情况  [	](git_main_20191219101334849)

```shell 
#{{c1:: 
git log --graph --pretty=oneline --abbrev-commit }}
```

### 分支合并策略  [	](git_main_20191219101334851)

- Fast Forward
  1. {{c1::当前分支合并到另一分支时，如果没有冲突，就会直接移动文件指针,合并后没有历史记录。 }}
  2. 命令：{{c1:: `git merge name` }}
- No Fast Forward
  1. {{c1:: 当前分支合并到另一分支时会新创建一个版本，合并后的历史有分支 }}
  2. 命令：{{c1:: `git merge -no-ff name` }}

### 工作现场的保留与恢复  [	](git_main_20191219101334853)

```shell
#把当前分支的工作现场储藏起来{{c1::
git stash }}
#查看储藏的工作现场{{c1::
git stash list }}
#恢复之前的工作现场,但是恢复后stash内容并不删除{{c1::
git stash apply }}
#恢复之前的工作现场,同时删除stash的内容{{c1::
git stash pop }}
```

### 删除分支  [	](git_main_20191219101334855)

```shell
#删除已被合并的分支
git branch -d feature-test
#无论是否合并强行删除分支
git branch -D feature-test
```

### 远程库管理  [	](git_main_20191219101334856)

```shell
#查看远程库的名字{{c1::
git remote
}}
#查看远程库的详细信息，权限，地址等{{c1::
git remote -v
}}
#推送分支：将该分支上所有的本地提交推送到远程库{{c1::
git push origin master
#如果推送其他分支
git push origin dev
}}
  
```

### 多人协作  [	](git_main_20191219101334857)

```shell
#从github上clone一个项目{{c1:: 
git clone git@github.com：alibaba/easyExcel }}
#默认是master分支，现在切换到dev分支{{c1:: 
git checkout -b dev easyExcel/dev }}
#完成修改后推送提交到远程{{c1:: 
git push easyExcel/dev }}
#如果推送失败，则重新拉取最新版{{c1::
git pull }}
#如果提示”no tracking information“则指定本地DEV分支与远程easyExcel/dev的链接{{c1:: 
git branch --set-upstream-to=origin/remote_branch  your_branch }}
#再次拉取{{c1:: 
git pull }}
#如果有冲突，解决冲突后再次推送{{c1:: 
git push easyExcel/dev  }}
```

## 标签管理  [	](git_main_20191219101334859)

### 创建，查看，删除标签  [	](git_main_20191219101334861)

```shell
#创建标签{{c1:: 
git tag v1.0 }}
#查看当前分支所有的标签{{c1:: 
git tag }}
#为指定的版本打上标签{{c1:: 
git tag v0.9 commitId }}
#查看指定标签的信息{{c1:: 
git show v0.9 }}
#创建带有说明文字的标签{{c1::
git tag -a v0.1 -m "version 0.1 released"  3628164 }}
#用私钥签名一个标签{{c1:: 
git tag -s tagname -m "blalbla....." 3628164 }}
#删除标签{{c1:: 
git tag -d v0.1 }}
```

### 远程库标签管理  [	](git_main_20191219101334863)

```shell
#推送某个标签到远程{{c1:: 
git push origin v1.0 }}
#一次性推送全部标签到远程{{c1:: 
git push origin --tags }}
#删除已经推送到远程的标签{{c1::
#第一步：先删除本地标签
git tag -d v0.9
#第二步：
git push origin :refs/tags/v0.9
}}
```

### 如何在GitHub上参与开源项目  [	](git_main_20191219101334865)

{{c1::

- 在感兴趣的项目上fork项目到自己的账户，从自己的clone到本地就可以直接推送了。
- 如果直接在开源项目上clone将无法推送修改。

}}

## 全局配置  [	](git_main_20191219101334868)

### 忽略特殊文件  [	](git_main_20191219101334870)

GitHub的忽略特殊文件的模板地址：{{c1: https://github.com/github/gitignore }}

### 配置别名  [	](git_main_20191219101334871)

```shell
#{{c1::
git comfig --global alias.st status
git comfig --global alias.co checkout
git comfig --global alias.ci commit
git comfig --global alias.br branch
}}
#以后的提交
git ci -m "bala bala bala...”
#配置unstage命令：撤销暂存区的修改
git config --global alias.unstage 'reset HEAD'
```

### 配置版本历史显示效果  [	](git_main_20191219101334874)

```shell
git config --global alias.lg "log --color --graph --pretty=format:'%Cred%h%Creset -%C(yellow)%d%Creset %s %Cgreen(%cr) %C(bold blue)<%an>%Creset' --abbrev-commit"
```

![image-20191120235332694](git_main.assets/image-20191120235332694.png)