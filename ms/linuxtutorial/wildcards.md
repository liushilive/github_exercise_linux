# 通配符

<!-- https://ryanstutorials.net/linuxtutorial/wildcards.php -->

在文件处理部分，我们学习了一些做有趣的事情的命令。问题是他们每次都在同一个文件上操作，效率不高。现在我要介绍一种同时处理一组文件的方法。

## 那么它们是什么呢？

通配符是一组构建块，允许你创建定义一组文件或目录的模式。正如你所记得的，每当我们在命令行中引用一个文件或目录时，我们实际上是在引用一个路径。每当我们引用一个路径时，我们也可以在该路径中使用通配符将其转换为一组文件或目录。

下面是通配符的基本集合：

* `*` - 表示零个或多个字符
* `?` - 表示单个字符
* `[]` - 表示一系列字符

作为基本的第一个例子，我们将介绍 `*` 。 在下面的例子中，我们将列出以 `e` 开头的所有条目。

```bash
[root@localhost liushilive]# pwd
/root/liushilive
[root@localhost liushilive]# ls
example2  example3  example5  firstfile  foo3
[root@localhost liushilive]# ls e*
example2  example3  example5
[root@localhost liushilive]#
```

## 高级选项

这里的机制其实挺有趣的。乍一看，你可能会假设上面  `ls` 的命令接收到参数 `e*`，然后继续将其转换为所需的匹配。实际上是 bash （提供命令行接口的程序）为我们进行转换。当我们提供这个命令时，它看到我们已经使用了通配符，因此，在运行命令 `ls` 之前，它用匹配该模式的每个文件或目录（即路径）替换该模式。我们发出以下命令：

```bash
ls e*
```

然后系统将其转化为：

```bash
ls example2 example3 example5
```

然后执行程序。程序永远不会看到通配符，也不知道我们使用它们。这很时髦，因为这意味着我们可以随时在命令行中使用它们。我们不仅限于某些程序或情况。

## 再举几个例子

再举一些例子来说明他们的行为。对于下面的所有示例，假设我们在 `liushilive` 目录中，并且将创建一系列文件。还要注意，我在这些示例中使用 `ls` 只是因为它是说明其用法的一种方便的方式。任何命令都可以使用通配符。

```bash
[root@localhost liushilive]# pwd
/root/liushilive
[root@localhost liushilive]# ls
example2  example3  example5  firstfile  foo3
[root@localhost liushilive]# touch 100.txt 101.txt 110.txt a.txt b.txt
[root@localhost liushilive]# ls
100.txt  101.txt  110.txt  a.txt  b.txt  example2  example3  example5  firstfile  foo3
[root@localhost liushilive]#

```

分解一下：

* 第 5 行 连续 创建 `100.txt 101.txt 110.txt a.txt b.txt` 文件

列出所有 `txt` 扩展名。在这个例子中，我们使用了绝对路径。如果路径是绝对或相对的，则通配符的工作方式是一样的。

```bash
[root@localhost liushilive]# ls *.txt
100.txt  101.txt  110.txt  a.txt  b.txt
[root@localhost liushilive]#
```

现在我们来介绍一下 `?`。在这个例子中，我们正在寻找第二个字符是 `i` 的每个文件。

```bash
[root@localhost liushilive]# ls ?i*
firstfile
[root@localhost liushilive]#
```

或者有三个字符的 txt 文件

```bash
[root@localhost liushilive]# ls ???.txt
100.txt  101.txt  110.txt
[root@localhost liushilive]#

```

最后是操作符 `[]`。与前两个指定任何字符的通配符不同，范围运算符允许你限制到字符的子集。在这个例子中，我们正在寻找每一个名字以 `a` 或 `f` 开头的文件。

```bash
[root@localhost liushilive]# ls [af]*
a.txt  firstfile

foo3:
[root@localhost liushilive]#
```

对于范围，我们也可以使用连续字符包括一个集合。例如，如果我们想找到每个文件的名字中包含一个数字，我们可以这样做：

```bash
[root@localhost liushilive]# ls *[1-9]*
100.txt  101.txt  110.txt  example2  example3  example5

foo3:
[root@localhost liushilive]#
```

我们还可以使用插入符号 `^` 反转一个范围，这意味着查找不属于以下任何一个字符的任何字符。

```bash
[root@localhost liushilive]# ls [^a-f]*
100.txt  101.txt  110.txt
[root@localhost liushilive]#

```

## 一些现实世界的例子

上面的例子说明了通配符是如何工作的，但是你可能想知道它们实际上有什么用处。人们在任何地方都使用它们，随着你的进步，我相信你会找到许多方法来使用它们，使你的生活更加轻松。这里有一些例子可以让你尝到什么是可能的。请记住，这些只是可能性的一小部分，只要你在命令行上指定路径，就可以使用它们。只要有一点点创造性思维，你就会发现它们可以在各种情况下使用。

查找目录中每个文件的文件类型。

```bash
[root@localhost liushilive]# file *
100.txt:   empty
101.txt:   empty
110.txt:   empty
a.txt:     empty
b.txt:     empty
example2:  empty
example3:  empty
example5:  empty
firstfile: UTF-8 Unicode text
foo3:      directory
[root@localhost liushilive]#
```

将 txt 文件移动到另一个目录中。

```bash
[root@localhost liushilive]# mv *.txt foo3/
[root@localhost liushilive]# ls
example2  example3  example5  firstfile  foo3
[root@localhost liushilive]# cd foo3
[root@localhost foo3]# ls
100.txt  101.txt  110.txt  a.txt  b.txt
[root@localhost foo3]#
```

## 摘要

### 学到什么

本节中没有介绍新命令。

### 重要概念

* 任何地方，任何路径
  可以在路径的任何部分使用通配符
* 无论在哪里使用
  因为通配符替换是由系统完成的，而不是命令，所以它们可以在任何使用路径的地方使用

## 活动

是的，现在让我们把这些东西付诸实践。请参阅以下内容：

* 你能列出名字有 4 个字符长的文件吗？
* 列出名称包含大写字母的文件？
