# 帮助手册页

<!-- https://ryanstutorials.net/linuxtutorial/manual.php -->

Linux 命令行提供了丰富的功能和机会。如果你的记忆与我的记忆相似，那么你会发现很难记住大量的细节。

幸运的是，有一个易于使用的资源可以告诉我们在命令行上我们可以做的所有伟大的事情。这就是我们将在本节中学习的内容。我知道你很热衷于急于做事，我们将在下一节开始讨论，我保证，首先我们需要学习如何使用手册页。

如果你的英语与我一样烂，还请跳过本节，记住一个网址：Linux 命令大全：<http://man.linuxde.net/> ，搜索命令寻求中文帮助。

## 那他们到底是什么？

手册页是一组页面，用于解释系统上可用的每个命令，包括它们的作用，运行方式的具体信息以及它们接受的命令行参数。

其中一些有点难以理解，但它们的结构相当一致，所以一旦掌握了它，它就不会太糟糕。

可以使用以下命令调用手册页：

```bash
man 命令
```

例如

```bash
[root@localhost ~]# man ls
LS(1)                                                     User Commands                                                     LS(1)

NAME
       ls - list directory contents

SYNOPSIS
       ls [OPTION]... [FILE]...

DESCRIPTION
       List information about the FILEs (the current directory by default).  Sort entries alphabetically if none of -cftuvSUX nor
       --sort is specified.

       Mandatory arguments to long options are mandatory for short options too.

       -a, --all
              do not ignore entries starting with .

       -A, --almost-all
              do not list implied . and ..

       --author
              with -l, print the author of each file

       -b, --escape
              print C-style escapes for nongraphic characters

       --block-size=SIZE
              scale sizes by SIZE before printing them; e.g., '--block-size=M' prints sizes in units of 1,048,576 bytes; see SIZE
              format below

       -B, --ignore-backups
              do not list implied entries ending with ~

       -c     with  -lt: sort by, and show, ctime (time of last modification of file status information); with -l: show ctime and
              sort by name; otherwise: sort by ctime, newest first

       -C     list entries by columns
 Manual page ls(1) line 1 (press h for help or q to quit)

```

让我们分解一下：

* 第 5 行告诉我们实际命令后跟一个简单的一行描述它的功能。
* 第 8 行是所谓的概要。
  这实际上只是对命令应该如何运行的快速概述。方括号 `[]` 表示某些内容是可选的。（此行上的选项指的是描述下面列出的命令行选项）
* 第 11 行向我们提供了该命令的更详细描述。
* 第 14 行以下描述下面将始终是该命令可用的所有命令行选项的列表。

>要退出手册页，请按 `q` 退出。

## 搜索

可以在手册页面上进行关键字搜索。如果你不太确定你可能想要使用什么命令但是你知道你想要实现什么，这可能会有所帮助。为了使这种方法有效，你可能需要一些努力。在许多手册页中发现特定单词的情况并不少见。

```bash
man -k <搜索词>
```

如果你想在手册页中进行搜索，也可以。当你在特定的手册页中时，输入正斜杠 `/`，然后是你要搜索的术语，按 `enter` 如果该术语出现多次，你可以循环浏览它们按 `n` 进行下一个。

## 有关命令运行的更多信息

很多精通 Linux 的人都知道我们应该使用哪些命令行选项来修改命令行为以满足我们的需求。其中很多命令选项都有长和短版本。

例如。在上面你会注意到要列出所有目录条目（包括隐藏文件），我们可以使用选项 `-a` 或 `--all`。长选项实际上只是一种人类可读的形式。你也可以使用它们，它们都做同样的事情。使用长选项的一个优点是，你可以更容易地记住你的命令正在做什么。使用短选项的一个优点是，你可以更轻松地将多个链接在一起。

```bash
[root@localhost ~]# pwd
/root
[root@localhost ~]# ls -a
.  ..  anaconda-ks.cfg  .bash_history  .bash_logout  .bash_profile  .bashrc  .cshrc  .tcshrc
[root@localhost ~]# ls --all
.  ..  anaconda-ks.cfg  .bash_history  .bash_logout  .bash_profile  .bashrc  .cshrc  .tcshrc
[root@localhost ~]# ls -al
总用量 28
dr-xr-x---.  2 root root  135 5月   8 13:13 .
dr-xr-xr-x. 17 root root  224 5月   8 13:09 ..
-rw-------.  1 root root 1241 5月   8 13:09 anaconda-ks.cfg
-rw-------.  1 root root    9 5月   8 13:13 .bash_history
-rw-r--r--.  1 root root   18 12月 29 2013 .bash_logout
-rw-r--r--.  1 root root  176 12月 29 2013 .bash_profile
-rw-r--r--.  1 root root  176 12月 29 2013 .bashrc
-rw-r--r--.  1 root root  100 12月 29 2013 .cshrc
-rw-r--r--.  1 root root  129 12月 29 2013 .tcshrc
[root@localhost ~]#

```

如你所见，长命令行选项以两个 `-` 开头，而短选项以单个 `-` 开头。当我们使用单个短划线时，我们可以通过在短划线后将表示这些选项的所有字母放在一起来调用多个选项。（有一些实例，特定选项需要一个参数来配合它，这些选项通常必须与它们相应的参数分开放置。现在不要过于担心这些特殊情况。当我们遇到它们时，再聊。）

## 摘要

### 学到什么

* `man <command>`
  查找特定命令的手册页。

* `man -k <搜索词>`
  对包含给定搜索词的所有手册页进行关键字搜索。

* `/ <术语>`
  在手册页中，搜索“术语”

* `n`
  在手动页面中执行搜索后，选择下一个找到的项目。

### 重要概念

让手册页做你的朋友。

而不是试图记住所有内容，而是记住你可以轻松地在手册页中查找内容。

## 活动

是的，现在让我们把这些东西付诸实践。请参阅以下内容：

* 浏览 `ls` 的手册页。
  玩一些你在那里找到的命令行选项。确保你玩一些组合。同时确保你使用绝对路径和相对路径的 `ls`。
* 现在尝试通过手册页进行一些搜索。
