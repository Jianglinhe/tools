## Git 操作手册

### 1. git简介

Git是**分布式**版本控制系统，有强大的**分支管理**

**集中式版本控制**

​		版本库是集中存放在中央服务器的，而干活的时候，用的都是自己的电脑，所以先要从中央服务器取得最新的版本，然后再干活，干完活后，再把自己的活推送给中央服务器。

![](../images/集中式.jpg)

​		集中式版本控制系统的最大缺点：必须联网才能工作，如果在局域网内还好，带宽够大，速度够快，可如果在互联网上，遇到网速慢，可能提交一个10M的文件就需要5分钟。

**分布式版本控制系统**

​		分布式版本控制系统没有"中央服务器"，每个人的电脑上都是一个完整的版本库。既然每个人电脑上都有一个完整的版本库，那多个人如何协作呢？比方说你在自己电脑上改了文件A，你的同事也在他的电脑上改了文件A，这时，你们俩之间只需把各自的修改推送给对方，就可以互相看到对方的修改了。

​		在实际使用分布式版本控制系统的时候，其实很少在两个人之见的电脑上推送版本库的修改。分布式版本控制系统中也有一台充当"中央服务器"的电脑，但这个服务器的作用仅仅是用来方便"交换"大家的修改，没有它，大家也一样干活，只是交换修改不方便而已。

![](../images/分布式.jpg)

### 2. git安装

git最早是在linux上开发的，很长一段时间内，现在git可以在linux、unix、mac、windows上运行。

- 在**windows**上安装

  直接从Git官网上下载安装程序，然后默认选项安装即可。

  安装完成后，还需要设置，在git bash命令窗口中输入:

  ```bash
  # 因为git是分布式版本控制系统，因此，没台机器都必须自报家门:你的名字和Email地址
  git config --global user.name "Jianglinhe"
  git config --global user.email "Jianglinhe888@163.com"
  ```

  <font color='red'>```git config```</font>命令的<font color='red'>```--global```</font>参数，表示这台机器上所有的git仓库都会使用这个配置，当然也可以对某个仓库指定不同的用户名和Email地址

  ```bash
  # 获取config信息
  git config --list
  ```

- 在**Linux**上安装

  ```bash
  sudo apt-get install git
  ```

### 3. git本地操作

 **版本库**，又名**repository**，可以理解成一个目录，这个目录里所有文件都可以被git管理起来，每个文件的修改、删除，Git都能跟踪，以便任何时刻都可以追溯历史，或者在将来某个时刻可以还原。

#### 3.1 创建版本库

1. 新建一个空目录

   ```bash
   mkdir c_learn
   ```

2. 初始化仓库，将这个目录编程git可以管理的仓库

   ```bash
   # cd c_learn
   git init
   ```

   当前目录下会多一个``.git``的目录(默认隐藏的)，这个目录是git用来跟踪管理版本库的，不要手动修改这个目录文件。

#### 3.2 将文件添加到版本库

所有的版本控制系统，其实只能跟踪文本文件的改动，比如:txt文件，网页，所有的程序代码等等。

编写一个<font color='red'>```readme.txt```</font>文件，不要使用记事本(容易出现编码问题，建议notepad++)，内容为说明文档

该文件放到```c_learn```目录下，然后

```bash
# 1.使用 git add 将文件添加到仓库
git add readme.txt 
# 2.使用 git commit 告诉git，把文件提交到仓库
git commit -m "wrote a readme file"
```

#### 3.3 查看工作区状态

```bash
git status
```

```git status```告诉你哪些文件被修改过

- 状态1: 修改了，没有添加到缓冲区(红色)
- 状态2: 修改了，添加到了缓冲区(绿色)
- 状态3: nothing to commit, working tree clean

```bash
git diff
```

```git diff```就是查看difference，可以查看文件修改内容

#### 3.4 版本回退

在工作中，可能会每次修改然后提交，可以通过```git log```来查看历史记录

```bash
# 显示从最近到最远的日志
git log
# 如果嫌输出的信息泰国，可以加上 --pretty=oneline参数
git log --pretty=oneline
```

![](../images/git_log.png)

> commit后面一大串是```commit id```(版本号)

**退回版本**

在git中，用<font color='green'>```HEAD```</font>表示当前版本，也就是最近一次提交的那个版本，上一个版本就是<font color='green'>```HEAD^```</font>，上上一个版本就是<font color='green'>```HEAD^^```</font>，往上100个版本就是<font color='green'>```HEAD~100```</font>

```bash
# 退回到上一个版本
git reset --hard HEAD^ 
```

此时，已经退回到了上一个版本，使用```git log```已经看不到未来的那个版本了，如果穿梭回去，只要上面的命令窗口没有关闭，就可顺着往上找

```bash
git reset --hard 未来版本号(版本号没必要写全，前几位就可以了)
```

如果关掉了窗口，git提供了```git reflog```来记录每一条命令，可以用过该命令查看记录

**总结**

- ```HEAD```指向的版本就是当前版本，因此，Git允许我们在版本的历史之间穿梭，使用命令```git reset --hard commit_id```
- 穿梭前，使用```git log```可以查看提交历史，以便确定要回退到哪个版本
- 要重返未来，用```git reflog```查看历史命令，以便确定要回到哪个未来的版本

#### 3.5 工作区和暂存区

**工作区**

​		就是电脑中能看到的目录，比如:c_learn(不包括隐藏目录)

**版本库**

![](../images/work_stage.jpg)

​		工作区一次隐藏目录```.git```，就是git的版本库。版本库有很多东西，最重要的就是称为stage(或叫index)的暂存区，还要git自动创建的第一个分支```master```，以及指向```master```的一个指针叫```HEAD```

**往git库添加文件的本质**

- ```git add```，实际上就是把文件修改添加到缓冲区
- ```git commit```，实际上就是把暂存区的所有内容提交到当前分支

#### 3.6 管理修改

理解其概念，git跟踪管理的是修改，而非文件

#### 3.7 撤销修改

```bash
git checkout -- file
```

- 一种是```file```自修改后货还没有被放到暂存区，现在，撤销修改就回到和版本库一模一样的状态
- 一种是```file```已添加到暂存区，又做了修改，现在，撤销修改就回到添加暂存区后的状态。
- 总之，就是让文件回到最近一次```git commit```或```git add```时的状态

#### 3.8 删除文件

```bash
# 1.先在文件管理器里直接将文件删除
rm file
# 2.查看此时的状态，工作区和版本库不一致，会发现哪些文件被删除了
git status
# 3. 从版本库中删除文件
git rm file
git commit -m "删除备注信息"
```

【tip】先手动删除文件，然后使用```git rm file```和```git add file```效果一样

如果是厝山文件，而版本库里还有，可以轻松的将误删文件恢复到最新版本

```bash
git checkout -- filename
# 其实就是用版本库里的版本替换工作区的版本，无论工作区是修改还是删除，都可以一键还原
```

### 4. 远程仓库

**使用github作为git服务器**

- 创建SSH Key

  ```bash
  ssh-keygen -t rsa -C "youremail@example.com"
  ```

  在git bash使用上述命令，会生成两个目录，```id_rsa```和```id_rsa.pub```，这是一对秘钥，前者是私钥，后者是公钥。

- 登录github，打开"Account settings"，"SSH Keys"页面，点"Add SSH Key"，填上任意title，在Key文本框里粘贴```id_rsa.pub```文件的内容保存即可

> Github允许添加多个key

#### 4.1 添加远程仓

如果你**已经在本地创建了一个Git仓库后，又想在Github创建一个Git仓库，并且让这两个仓库进行远程同步**。

<font color='red'>**先有本地仓，后有远程仓**</font>

- 登录github，创建新的仓库，"Create a new repo"

- 在本地仓库下运行命令

  ```bash
  git remote add origin git@github.com:Jianglinhe/仓库名.git
  ```

- 将本地仓的内容推送到远程

  ```bash
  git push -u origin master
  ```

  远程仓库的名字就是```origin```，这是git默认叫法。```git push```命令，实际上是把当前分支```master```推送到远程，由于远程仓库是空的，第一次推送```master```分支是，加上```-u```参数，会将本地```master```和远程```master```分支关联起来，后续的推送或拉取可以简化命令

  ```bash
  # 后续简化推送命令
  git push origin master
  ```
  
  【注意】github默认分支名master已更改为main分支，导致推送命令更改为
  
  ```bash
  # 将master分支替换为main分支
  git push origin main
  ```

#### 4.2 从远程库克隆(方便)

<font color='red'>**先建远程库，再从远程库克隆**</font>

- 在github上新建一个仓库，注意勾选```Initialize this repository with a README```

- 使用```git clone```命令进行克隆

  ```bash
  git clone git@github.com:Jianglinhe/CPlusPlus.git
  ```

  github给出的地址不止一个，因为git支持多种协议，默认使用ssh协议(最快)，还可以使用https协议

### 5.分支管理

[分支管理教程地址](https://www.liaoxuefeng.com/wiki/896043488029600)

