#  GIT

## 一、什么是git

Git是开源的分布式的管理系统，是由Linux之父——Linus开发的。前期主要用于Linux的版本管理。

## 二、Git与其他版本管理工具的区别

在Linus开发Git之前，有一段历史。当时对于Linux的版本管理是采用另一套闭源的版本管理系统，尤其在管理Linux的过程，Linus的团队成员有人尝试破解此版本管理系统，被厂商监控发现，停止提供免费服务。Linus决定自己开发一套版本管理系统，就是现在的最牛逼的版本管理系统Git。

在统一时期，SVN也是开源的，但Linus看不上SVN。由于它是中心的版本管理软件，中心化导致需要网络才能访问中心提交文件。同时多人访问会导致网络拥堵，所以这一系列的缺点，导致Linus看不上SVN。

那经过上述简单的历史，Git到底有什么牛逼之处呢？

1. 分布式，去中心化；分布式版本控制系统，每个开发者都可以在本地拥有一个完整的代码仓库，不需要依赖中央服务器，可以在没有网络连接的情况下进行版本控制和代码管理。-------设计理念
2. 快速；Git设计使用对象存储优化了传输过程，使用了快速的算法，使得Git在处理大型项目和大量数据时表现得非常高效。----对象存储以及数据结构
3. 强大的分支功能；可以轻松创建、合并和删除分支，使得团队协作更加灵活和高效。每个分支都可以独立进行开发，不影响其他分支的代码。----------branch
4. 强大的版本控制功能；Git可以对代码的每一次修改进行版本控制，记录了修改的时间、内容、作者等信息，并可以方便地查看和比较不同版本之间的差异。----------
5. 完整性；Git使用哈希值来标识版本，每一次提交的代码都会计算一个唯一的哈希值，保证了版本的完整性和可追溯性并追踪版本的改变；-------一致性哈希算法
6. 缓存机制：Git引入了缓存机制，将文件的变化在内存中暂存，只有在需要提交时才会写入磁盘，大大提高了文件的读写效率。-------stage

## 三、 git的基本概念与操作

在Git中，根据工作区域可分为：工作目录、暂存区、git仓库、远程仓库；

1. 工作目录：顾名思义就是本地工作的目录，可对本地文件操作跟记录的位置；
2. 暂存区：对工作区已修改的文件，存放提交快照的区域；
3. 仓库：存放文件元数据的位置，其中元数据包含文件以及版本信息等；

![image-20240321180229680](https://raw.githubusercontent.com/tiansy9981/pictuer/master/image-20240321180229680.png)

**在git中文件有三种状态**：

1. 修改modified：已经更改了文件，但尚未将其提交到数据库。
2. 阶段中staged：已经在当前版本中标记了已修改的文件，以便将其放入下一次提交快照中。
3. 提交committed：表示数据安全地存储在本地数据库中。

**工作目录的文件生命周期有：untracked、unmodified、modified、staged**

![image-20240321215213847](https://raw.githubusercontent.com/tiansy9981/pictuer/master/image-20240321215213847.png)

如何获取文件的状态呢？通过git status命令即可获取git中文件的状态。

```bash
 $ git status
  On branch master
  Your branch is up-to-date with 'origin/master'.
  nothing to commit, working tree clean
```

以上表示本地工作树master分支中的文件跟仓库master分支的文件版本一致，未做修改；

如果增加一个新的文件，如下：

```bash
$ echo 'My Project' > README
  $ git status
  On branch master
  Your branch is up-to-date with 'origin/master'.
  Untracked files:
    (use "git add <file>..." to include in what will be committed)
      README
  nothing added to commit but untracked files present (use "git add" to track)

```

增加了一个新的文件README，通过git status命令会提示文件在当前分支中未被追踪（当前分支的仓库以及暂存区没有此文件）。可使用git add 追踪文件，即提交至暂存区。

此时再通过输入git status，提示new file。Changes to be committed表示文件已经被追踪以及被记录到暂存区等待被提交。

```bash
 $ git status
  On branch master
  Your branch is up-to-date with 'origin/master'.
  Changes to be committed:
  (use "git restore --staged <file>..." to unstage)
      new file:   README
```

当修改一个已被追踪的文件时，modified文件已在工作目录中修改，Changes not staged for commit表示尚未暂存并提交。可通过git add将修改的文件提交暂存区

```bash
$ git status
  On branch master
  Your branch is up-to-date with 'origin/master'.
  Changes to be committed:
    (use "git reset HEAD <file>..." to unstage)
      new file:   README
  Changes not staged for commit:
    (use "git add <file>..." to update what will be committed)
    (use "git checkout -- <file>..." to discard changes in working directory)
      modified:   CONTRIBUTING.md
```

通过以上可知，git add命令能跟踪新文件，暂存文件，后面将分支中讲解它能做其他事情，如将合并冲突的文件标记为已解决。





