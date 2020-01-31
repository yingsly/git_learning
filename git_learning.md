## 学习使用git

创建版本库

 千万不要使用Windows自带的**记事本**编辑任何文本文件。原因是Microsoft开发记事本的团队使用了一个非常弱智的行为来保存UTF-8编码的文件，他们自作聪明地在每个文件开头添加了0xefbbbf（十六进制）的字符，你会遇到很多不可思议的问题，比如，网页第一行可能会显示一个“?”，明明正确的程序一编译就报语法错误，等等，都是由记事本的弱智行为带来的。建议你下载[Notepad++](http://notepad-plus-plus.org/)代替记事本，不但功能强大，而且免费！记得把Notepad++的默认编码设置为UTF-8 without BOM即可 

```python
mkdir learngit
cd learngit
# 显示当前目录，使用Windows系统，为了避免遇到各种莫名其妙的问题，请确保#目录名（包括父目录）不包含中文。
pwd 
# 第2步，git init 初始化,把这个目录变成Git可以管理的仓库
#当前目录下多了一个.git的目录，这个目录是Git来跟踪管理版本库的
git init
##所有的版本控制系统，其实只能跟踪文本文件的改动
## 如果没有历史遗留问题，强烈建议使用标准的UTF-8编码，所有语言使用同##一种编码，既没有冲突，又被所有平台所支持。

```

将文件放到**git管理目录**下（子目录也行），因为这是一个Git仓库，放到其他地方Git再厉害也找不到这个文件。 

```python
# step1 使用 git add 告诉Git,把文件添加到仓库
git add git_learning.md
# step2 使用 git commit 告诉Git,把文件提交到仓库
git commit -m "wrote a readme file"
```

 简单解释一下**git commit**`命令，`-m后面输入的是本次提交的说明，可以输入任意内容，当然最好是有意义的，这样你就能从历史记录里方便地找到改动记录。 
 `git commit`命令执行成功后会告诉你，`1 file changed`：1个文件被改动（我们新添加的readme.txt文件）；`2 insertions`：插入了两行内容（readme.txt有两行内容 

GIT添加文件需要 **add** 、 **commit** 两个命令，commit 可以一次提交多个文件，可以多次 **add** 多个文件。

```python
#当前目录下多了一个.git的目录，这个目录是Git来跟踪管理版本库的
git init
## git add filename 添加文件
git add fname.md
## git commit -m "message" 提交并"修改信息"
git commit -m 

## git log 显示log信息
git log -oneline
## git status -s 显示当前状态
git status -s 
## git diff 显示不同点
git diff
```



### 时光穿梭机

 **git status**命令可以让我们时刻掌握仓库当前的状态， 

```python
git status
git add git_learning.md
git commit -m "修改后提交"
```

#### 版本回退

 不断对文件进行修改、提交修改到版本库里，就好比玩RPG游戏时，每通过一关就会自动把游戏状态存盘，如果某一关没过去，你还可以选择读取前一关的状态。Git也是一样，每当你觉得文件修改到一定程度的时候，就可以**“保存一个快照”，这个快照在Git中被称为commit**`。一旦你把文件改乱了，或者误删了文件，还可以从最近的一个commit恢复，然后继续工作，而不是把几个月的工作成果全部丢失。 

 在实际工作中，我们脑子里怎么可能记得一个几千行的文件每次都改了什么内容，不然要版本控制系统干什么。版本控制系统肯定有某个命令可以告诉我们历史记录，在Git中，我们用`git log`命令查看 

**git log后似乎进入了VIM模式，用 :q 退出，进入命令模式**

 ```python
#git log命令显示从最近到最远的提交日志，日志内容即为每次提交的注释
git log
#如果嫌输出信息太多，可以试试加上--pretty=oneline参数
git log --pretty=oneline

 ```

现在我们启动时光穿梭机，准备把`readme.txt`回退到上一个版本，也就是`add distributed`的那个版本，怎么做呢？

首先，Git必须知道当前版本是哪个版本，在Git中，用`HEAD`表示当前版本，也就是最新的提交`1094adb...`（注意我的提交ID和你的肯定不一样），上一个版本就是`HEAD^`，上上一个版本就是`HEAD^^`，当然往上100个版本写100个`^`比较容易数不过来，所以写成`HEAD~100`。

```python
# git log --oneline 查看版本信息
git log --oneline

#把当前版本回退到上一个版本，使用`git reset`命令：
git reset --hard HEAD^

```

用`git log`再看看现在版本库的状态：最新的那个版本`append GPL`已经看不到了！好比你从21世纪坐时光穿梭机来到了19世纪，想再回去已经回不去了，肿么办？只要上面的命令行窗口还没有被关掉，你就可以顺着往上找啊找啊，找到那个版本的`commit id`是`1094adb...`，于是就可以指定回到未来的某个版本：

```python
$ git reset --hard 1094a
HEAD is now at 83b0afe append GPL
```

回退到了某个版本，关掉了电脑，第二天早上就后悔了，想恢复到新版本怎么办？找不到新版本的`commit id`怎么办？Git提供了一个命令`git reflog`用来记录你的每一次命令：

```python
HEAD指向的版本就是当前版本，因此，Git使用命令在版本的历史之间穿梭
git reset --hard commit_id。
穿梭前，用git log可以查看提交历史，以便确定要回退到哪个版本:
git log
要重返未来，用git reflog查看命令历史，以便确定要回到未来的哪个版本:
git reflog
```

#### 工作区和暂存区

Git和其他版本控制系统如SVN的一个不同之处就是有暂存区的概念。

**工作区（Working Directory）**就是你在电脑里能看到的目录，比如我的`D:\ProgramData\GitHubProject\GitTUT`文件夹就是一个工作**版本库（Repository)**,工作区有一个隐藏目录<font color=red>**.git**</font>，这个不算工作区，而是Git的版本库。版本库里存了很多东西，其中最重要的就是成为**<font color=red>stage(或者叫index)的暂存区</font>**，还有Git为我们自动创建的第一个分支**master**,以及指向master的一个指针叫**HEAD**。

![](https://www.liaoxuefeng.com/files/attachments/919020037470528/0)区。



前面讲了我们把文件往Git版本库里添加的时候，是分两步执行的：

第一步用`git add`把文件添加进去，把文件修改添加到暂存区；
第二步用`git commit`提交更改，把暂存区的所有内容提交到当前分支。

因为我们创建Git版本库时，Git自动为我们创建了唯一一个`master`分支，所以，现在，`git commit`就是往`master`分支上提交更改。

你可以简单理解为，需要提交的文件修改通通放到暂存区，然后，一次性提交暂存区的所有修改。

**`git add`命令实际上就是把要提交的所有修改放到暂存区（Stage），然后，执行`git commit`就可以一次性把暂存区的所有修改提交到分支。**

一旦提交后，如果你又没有对工作区做任何修改，那么工作区就是“干净”的.

![image-20200131224827574](D:\ProgramData\GitHubProject\GitTUT\干净的工作区.jpg)

#### 管理修改

现在，假定你已经完全掌握了暂存区的概念。下面，我们要讨论的就是，为什么Git比其他版本控制系统设计得优秀，因为Git跟踪并管理的是修改，而非文件。

你会问，什么是修改？比如你新增了一行，这就是一个修改，删除了一行，也是一个修改，更改了某些字符，也是一个修改，删了一些又加了一些，也是一个修改，甚至创建一个新文件，也算一个修改。

```python
###为什么说Git管理的是修改，而不是文件呢？我们还是做实验。第一步，对readme.txt做一个修改，比如加一行内容：
$ cat readme.txt
Git is a distributed version control system.
Git is free software distributed under the GPL.
Git has a mutable index called stage.
Git tracks changes.
###然后，添加：
$ git add readme.txt
$ git status
# On branch master
# Changes to be committed:
#   (use "git reset HEAD <file>..." to unstage)
#
#       modified:   readme.txt
#
### 然后，再修改readme.txt：
$ cat readme.txt 
Git is a distributed version control system.
Git is free software distributed under the GPL.
Git has a mutable index called stage.
Git tracks changes of files
###提交
$ git commit -m "git tracks changes"
[master 519219b] git tracks changes
 1 file changed, 1 insertion(+)
###提交后，再看看状态：  
$ git status
On branch master
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

	modified:   readme.txt

no changes added to commit (use "git add" and/or "git commit -a")
```

怎么第二次的修改没有被提交？我们回顾一下操作过程：

第一次修改 -> `git add` -> 第二次修改 -> `git commit`

你看，我们前面讲了，Git管理的是修改，当你用`git add`命令后，在工作区的第一次修改被放入暂存区，准备提交，但是，在工作区的第二次修改并没有放入暂存区，所以，`git commit`只负责把暂存区的修改提交了，也就是第一次的修改被提交了，第二次的修改不会被提交。

那怎么提交第二次修改呢？你可以继续`git add`再`git commit`，也可以别着急提交第一次修改，先`git add`第二次修改，再`git commit`，就相当于把两次修改合并后一块提交了：
第一次修改 -> `git add` -> 第二次修改 -> `git add` -> `git commit`

**理解Git是如何跟踪修改的，每次修改，如果不用`git add`到暂存区，那就不会加入到`commit`中。**



