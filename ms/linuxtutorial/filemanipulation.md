# 文件操作

<!-- https://ryanstutorials.net/linuxtutorial/filemanipulation.php -->

我们已经有了一些基础知识。现在我们可以开始玩了。首先，我们将学习制作一些文件和目录并移动它们。未来部分将讨论将内容放入其中以及更有趣的操作。

## 创建目录

Linux 以分层方式组织它的文件系统。
随着时间的推移，你将建立相当数量的数据文件（存储容量总是在增加）。
我们必须创建一个目录结构，以帮助我们以可管理的方式组织数据。
我已经看到太多人只是将所有文件直接转储到他们的主目录的基础上，这将浪费了大量的时间来尝试在 100（甚至 1000）个文件中找到想要的文件。
养成将你的文件组织成一个优雅的文件结构的习惯，你将在未来几年感谢自己。

创建目录非常简单。命令是 `mkdir`，它是 `Make Directory` 的缩写。

```bash
mkdir [options] <目录>
```

在它最基本的形式中，我们可以运行 `mkdir` 只提供一个目录，它将为我们创建它。

```bash
[root@localhost ~]# pwd
/root
[root@localhost ~]# ls
anaconda-ks.cfg
[root@localhost ~]# mkdir liushilive
[root@localhost ~]# ls
anaconda-ks.cfg  liushilive
[root@localhost ~]#

```

让我们分解一下：

* 第 1 行确定我们当前所在路径。（在上面的示例中，我在我的主目录中）
* 第 3 行快速列出内容，以便我们知道目录中已有的内容。
* 第 5 行运行命令 `mkdir` 并创建一个目录 `liushilive`
* 第 7 行快速列出内容，以便我们知道目录中已经创建了新的内容。

请记住，当我们在上面的命令中提供目录时，我们实际上提供了一个路径。我们指定的路径是相对的还是绝对的？

以下是我们提供要如何创建目录的更多示例

```bash
mkdir /root/liushi_1
mkdir ./liushi_2
mkdir ../root/liushi_3
mkdir ~/liushi_4

```

如果不认识其中符号，请查看 [基本导航](navigation.md)

`mkdir` 有一些有用的选项。你能记得我们去哪里找出特定命令支持的命令行选项吗？

第一个是 `-p`，它告诉 `mkdir` 根据需要创建父目录（演示下面实际意味着什么）。

```bash
[root@localhost ~]# mkdir -p liushilive/foo/bar
[root@localhost ~]# cd liushilive/foo/bar/
[root@localhost bar]# pwd
/root/liushilive/foo/bar
[root@localhost bar]#

```

第二个是 `-v`，这使得 `mkdir` 告诉我们它在做什么。

```bash
[root@localhost ~]# cd ~
[root@localhost ~]# mkdir -pv liushilive_1/foo/bar
mkdir: 已创建目录 "liushilive_1"
mkdir: 已创建目录 "liushilive_1/foo"
mkdir: 已创建目录 "liushilive_1/foo/bar"
[root@localhost ~]#
```

## 删除目录

创建目录非常简单。删除或删除目录也很容易。然而，需要注意的一点是，Linux 上的命令行没有撤消（Linux GUI 桌面环境通常会提供撤消功能，但命令行却没有）。小心你做的事情。删除目录的命令是 `rmdir`，`remove directory` 的缩写。

```bash
rmdir [options] <目录>
```

有两点需要注意。首先，`rmdir` 支持类似于 `mkdir` 的 `-v` 和 `-p` 选项。其次，一个目录必须是空的才能被删除（稍后我们会看到一种方法来解决这个问题）。

```bash
[root@localhost ~]# rmdir liushilive/foo/bar
[root@localhost ~]# ls liushilive/foo/
[root@localhost ~]#
```

## 创建空白文件

许多涉及在文件中操作数据的命令都有一个很好的特性，即如果我们引用文件而该文件不存在，它们将自动创建该文件。事实上，我们可以利用这个特性来创建空白文件。

```bash
touch [options] <文件名>
```

```bash
[root@localhost ~]# cd liushilive
[root@localhost liushilive]# pwd
/root/liushilive
[root@localhost liushilive]# ls
foo
[root@localhost liushilive]# touch example1
[root@localhost liushilive]# ls
example1  foo
[root@localhost liushilive]#
```

我知道有些学生尝试过使用这个命令，使他们的作业文件看起来在截止日期之后没有被修改（有一些方法可以检测到这一点，所以它永远不会起作用，但要有创造性）。我们在这里利用的是，如果我们触摸了一个文件，但它不存在，这个命令会帮我们一个忙，并自动为我们创建它。

`touch` 实际上是一个我们可以用来修改文件访问和修改时间的命令（通常不需要，但有时当你测试一个依赖于文件访问或修改时间的系统时，它可能是有用的）。我们在这里所利用的优势是，如果我们 `touch` 一个文件，它不存在，命令将帮助我们，并自动为我们创建它。

>在 Linux 中，许多事情不是直接完成的，而是通过了解某些命令的行为和系统的某些方面，并以创造性的方式使用它们来达到预期的结果。
>还记得我们在介绍中提到的命令行为你提供了一系列构建块。你可以自由地以任何你喜欢的方式使用这些构建块，但是只有你了解它们是如何以及为什么这样做，你才能真正有效地做到这一点。

目前文件是空白的，这有点无聊，但在接下来的章节中，我们将介绍如何将数据放入文件并从中提取数据。

## 复制文件或目录

我们需要复制文件或目录的原因有很多。通常在更改某些内容之前，我们可能希望创建一个副本，以便在出现错误时可以轻松地恢复到原来的状态。我们使用的命令是 `cp`，代表 `copy`。

```bash
cp [options] <源> <目标>
```

有相当多的选项可用于 `cp`。 下面我将进一步介绍其中的一个，但是有必要查看 `cp` 的 `man` 页面，看看还有什么可用的。

```bash
[root@localhost liushilive]# ls
example1  foo
[root@localhost liushilive]# cp example1 example2
[root@localhost liushilive]# ls
example1  example2  foo
[root@localhost liushilive]#
```

请注意，源和目标都是路径。这意味着我们可以使用绝对路径和相对路径来引用它们。

以下是一些例子：

```bash
cp /root/liushilive/example2 example3
cp example2 ../../tmp
cp example2 ../../tmp/example4
cp /root/liushilive/example2 /root/liushilive/example5
```

使用 `cp` 时，目的地可以是文件或目录的路径。
如果是到一个文件（例如上面的示例 1、3 和 4)，那么它将创建源的一个副本，但是将副本命名为目标文件中指定的文件名。如果我们提供一个目录作为目标，那么它将把文件复制到该目录中，并且复制文件将具有与源文件相同的名称。

在它的默认行为 `cp` 将只复制一个文件（有一种一次复制多个文件的方法，但是我们将在后面讨论这个问题：[通配符](wildcards.md)）。使用 `-r` 选项（表示递归），我们可以复制目录。递归意味着我们要查看一个目录以及其中的所有文件和目录，对于子目录，进入它们并执行相同的操作，然后继续这样做。

```bash
[root@localhost liushilive]# pwd
/root/liushilive
[root@localhost liushilive]# ls
example1  example2  example3  example5  foo
[root@localhost liushilive]# cp foo foo2
cp: 略过目录"foo"
[root@localhost liushilive]# ls
example1  example2  example3  example5  foo
[root@localhost liushilive]# cp -r foo foo2
[root@localhost liushilive]# ls
example1  example2  example3  example5  foo  foo2
[root@localhost liushilive]#
```

## 移动文件或目录

要移动一个文件，我们使用命令 `mv`，它是 `move` 的缩写。它的工作方式与 `cp` 相似。一个小小的优点是，我们可以移动目录而不必提供 `-r` 选项。

```bash
mv [options] <源> <目标>
```

```bash
[root@localhost liushilive]# pwd
/root/liushilive
[root@localhost liushilive]# ls
example1  example2  example3  example5  foo  foo2
[root@localhost liushilive]# mkdir backups
[root@localhost liushilive]# mv foo2 backups/foo3
[root@localhost liushilive]# ls
backups  example1  example2  example3  example5  foo
[root@localhost liushilive]#
```

让我们来分析一下：

* 第 5 行 我们创建了一个名为 `backups` 的新目录
* 第 6 行 我们将目录 `foo2` 移动到目录 `backups` 中，并将其重命名为 `foo3`

请再次注意，源和目标都是路径，可以为绝对路径或相对路径。

## 重命名文件和目录

现在就像上面的命令 `touch` 一样，我们可以创造性地使用命令 `mv` 的基本行为来实现一个稍微不同的结果。
通常，`mv` 将用于将文件或目录移动到新目录中。
正如我们在上面的第 6 行中看到的，我们可以为文件或目录提供一个新的名称，作为移动的一部分，它还将重命名它。
现在，如果我们将目的地指定为与源目录相同的目录，但使用不同的名称，那么我们就有效地使用了 `mv` 来重命名文件或目录。

```bash
[root@localhost liushilive]# ls
backups  example1  example2  example3  example5  foo
[root@localhost liushilive]# mv foo foo3
[root@localhost liushilive]# ls
backups  example1  example2  example3  example5  foo3
[root@localhost liushilive]#
```

让我们来分析一下：

* 第 3 行 我们将 `foo` 重命名为 `foo3`

## 删除文件（和非空目录）

与 `rmdir` 一样，删除文件是一个不能撤消的操作，所以要小心。删除或删除文件的命令是 `rm`，它代表 `remove`。

```bash
rm [options] <file>
```

```bash
[root@localhost liushilive]# clear
[root@localhost liushilive]# ls
backups  example1  example2  example3  example5  foo3
[root@localhost liushilive]# rm example1
rm：是否删除普通空文件 "example1"？y
[root@localhost liushilive]# ls
backups  example2  example3  example5  foo3
[root@localhost liushilive]#
```

## 删除非空目录

与本节介绍的其他几个命令一样，`rm` 有几个选项可以改变它的行为。
我将让你自己来查看 `man` 页面，看看它们是什么，但是我将介绍一个特别有用的选项，它是 `-r`，类似于 `cp`，它代表递归。
当 `rm` 使用 `-r` 选项运行时，它允许我们删除目录以及其中包含的所有文件和目录。

```bash
[root@localhost liushilive]# ls
backups  example2  example3  example5  foo3
[root@localhost liushilive]# rmdir backups/
rmdir: 删除 "backups/" 失败: 目录非空
[root@localhost liushilive]# rm backups/
rm: 无法删除"backups/": 是一个目录
[root@localhost liushilive]# rm -r backups/
rm：是否进入目录"backups/"? y
rm：是否删除目录 "backups/foo3"？y
rm：是否删除目录 "backups/"？y
[root@localhost liushilive]# ls
example2  example3  example5  foo3
[root@localhost liushilive]#

```

在删除每个文件和目录之前提示你，并为你提供取消命令的选项。如果不想交互式提示，可加入 `-f`，将不会有任何提示，直接删除！！！

## 最后一点

我知道我已经强调了这一点，但是现在希望你能够理解，每当我们在命令行中引用一个文件或目录时，它实际上是一个路径。

因此，它可以指定为绝对路径或相对路径。

这是几乎总是这样的情况，所以记住这一点很重要。

在以后的章节中，我不会一直提醒你这一点，给出的例子通常也不会说明这一点。

记住在命令中实验绝对路径和相对路径，因为它们有时会在输出中提供细微但有用的差异。

（命令的输出可能略有不同，但是命令的操作总是相同的。)

## 摘要

### 学到什么

* `mkdir`
  创建一个目录

* `rmdir`
  删除一个目录

* `touch`
  创建一个空白文件

* `cp`
  复制文件或目录

* `mv`
  移动文件或目录（也可用于重命名）

* `rm`
   删除文件

### 重要概念

* 没有挽回的余地
  Linux 命令行没有撤销功能！！！
* 命令行选项
  大多数命令都有许多有用的命令行选项。

  请确保浏览[手册页](manual.md)以获取新命令，以便熟悉它们的功能和可用内容

## 活动

是的，现在让我们把这些东西付诸实践。请参阅以下内容：

* 首先在你的主目录中创建一个目录，在其中进行试验
* 在该目录中，创建一系列文件和目录（以及这些目录中的文件和目录）
* 现在重命名一些文件和目录
* 删除包含其他文件和目录的目录之一
* 回到你的主目录，从那里将一个文件从一个子目录复制到你创建的初始目录中
* 现在将该文件移回另一个目录
* 重命名一些文件
* 接下来，移动一个文件并在此过程中重命名它
* 最后，查看一下你的主目录中的现有目录
