---
layout: post
title: Powershell + Git，让windows上的编程更舒适
category: blog
description: Powershell为windows下的编程提供了如同LInux终端般的命令行输入窗口，而git则让代码管理更加清晰便捷。
coments: true
tags: [PowerShell,Git]
---

一个星期在自己的windows系统上搭建了比较顺手的编程环境，并且借用了别人的样式创建了一个基于GitHub Pages的个人博客。一切都还属于起步阶段，这篇文章先记录一下基本的环境搭建。

##复制Linux下的操作体验——Windows Powershell

直到下载了Github的windows下图形界面才知道windows已经自带了Powershell这样一个强大存在。关于它的基本介绍可以查看[PowerShell-wiki][1]，简单来说，在这样一个命令行界面里面，你可以愉快地在Windows下使用Linux命令，而且界面要比cmd好看多倍，就像下面这样：

![PowerShell-wiki](/images/Windows_PowerShell_1.0_PD.png "PowerShell-wiki")

![PowerShell-v4.0](/images/powershell2.jpg "PowerShell-v4.0")

另外，到后面安装了Git之后，顺带也会把ssh给安装了，是不是有了一种都不需要用PUTTY的感觉了呢？

##让你的PowerShell用上Git

使用GitHub无疑是程序员的标志，它不仅仅是一个高效管理代码的工具，更是强大的编程学习和交流的平台。相对Linux和Mac OS而言，在Windows下使用Git要略麻烦一些，但是如果配合使用PowerShell，用起来还是很顺畅的。下面具体介绍安装配置方法：

###第一步，安装Git

在[这里][2]下载最新版Git for windows，如果无法下载到，可以通过[这里][3]下载（版本1.9.4），之后点击安装，安装过程中有一步需要注意，如下图：

![install git](http://cdn.imtraum.com/blog/images/install-git2.png "install git")

**一定不要选择第一个，否则安装好后依旧不能在powershell或者cmd中使用**，我选择的是第二个。

有一篇[博文][4]上写，在安装完成后需要做这样一个步骤：在PowerShell中输入如下命令：`Set-ExecutionPolicy RemoteSigned`，否则在使用远程仓库时可能会遇到问题。

###第二步，安装Posh-git

github代码仓库对于修改提交代码分了不同的分支和状态，如果命令行中有相应的提示将事半功倍，为此可以安装[posh-git][5]工具。安装过程比较简单：

1. 必须保证git能够在PowerShell上顺利运行；
2. 到电脑的环境变量中，在path变量中添加`%ProgramFiles(x86)%\Git\cmd`或者`%ProgramFiles%\Git\cmd`；
3. 将posh-git仓库克隆到本地：`git clone https://github.com/dahlbyk/posh-git`；
4. 命令行进入到本地的posh-git文件夹，运行`.\install.psl`。

之后，当你在命令行中进入一个代码仓库时，命令行就会显示如下样式：

![posh-git](/images/posh-git.jpg "posh-git")

这里表示我们现在处于master分支，第一行中的数字从左到右表示没有新文件，有一个修改过的文件，没有被删除的文件，红色表示这些文件没有提交（not staged for commit）。第二行表示我们提交了这些改进，所以变成了绿色。

###第三步，缓存SSH密钥

上述步骤完成后重新打开PowerShell我们可能会发现下面这种情况：

![install step](http://cdn.imtraum.com/blog/images/could-not-find-ssh-agent-warning.png)

它表示posh-git试图启动ssh-agent.exe，但是shell环境不知道从哪里启动。这时我们用文本编辑器（如Sublime Text）打开`C:\Users\cathy\Documents\WindowsPowerShell\Microsoft.PowerShell_profile.ps1`（根据自己情况修改相应路径），在文件开头添加：

`$env:path += ";" + (Get-Item "Env:ProgramFiles(x86)").Value + "\Git\bin"`

添加后的文件如下所示：

![profile.psl](http://cdn.imtraum.com/blog/images/add-ssh-agent-to-powershell-profile.png)

接下来生成一个SSH密钥。github上有相应[教程][6]，如果不习惯命令行操作或者遇到问题，可以打开Git GUI，这个在安装Git之后就会有，如图：

![git gui](/images/gitgui.jpg "git gui")

点击帮助->Show SSH key->Generate Key，复制生成的密钥，到github个人页面的设置，选择SSH keys:

![github ssh key](/images/gitkeys.jpg)

点击Add SSH key添加进去即可。

![ssh key](/images/sshkeys2.jpg)

重新打开PowerShell后就添加成功了。

###Bingo！

至此工具就配置完成了，要用ssh远程连接linux主机也只需要用PowerShell就可以了，接下来就是享（ku）受（bi）敲代码的过程了，来一句老外的话：Enjoy！

参考资料：

1. [Streamline Git With Powershell](http://www.imtraum.com/blog/streamline-git-with-powershell)
2. [Better Git with PowerShell](http://haacked.com/archive/2011/12/13/better-git-with-powershell.aspx)

[1]: http://zh.wikipedia.org/wiki/Windows_PowerShell
[2]: http://git-scm.com/download/win
[3]: http://pan.baidu.com/s/1sjPnOuT
[4]: http://haacked.com/archive/2011/12/13/better-git-with-powershell.aspx
[5]: https://github.com/dahlbyk/posh-git
[6]: https://help.github.com/articles/generating-ssh-keys

{{page.date | date_to_string}}
