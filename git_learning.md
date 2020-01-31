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
#把当前版本回退到上一个版本，使用`git reset`命令：
git reset --hard HEAD^

```



