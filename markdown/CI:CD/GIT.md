#  GIT

## 一、什么是git

Git是开源的分布式的管理系统，是由Linux之父——Linus开发的。前期主要用于Linux的版本管理。Git基本上是一个内容可寻址的文件系统。

## 二、Git与其他版本管理工具的区别

在Linus开发Git之前，有一段历史。当时对于Linux的版本管理是采用另一套闭源的版本管理系统，尤其在管理Linux的过程，Linus的团队成员有人尝试破解此版本管理系统，被厂商监控发现，停止提供免费服务。Linus决定自己开发一套版本管理系统，就是现在的最牛逼的版本管理系统Git。

在统一时期，SVN也是开源的，但Linus看不上SVN。由于它是中心的版本管理软件，中心化导致需要网络才能访问中心提交文件。同时多人访问会导致网络拥堵，所以这一系列的缺点，导致Linus看不上SVN。

那经过上述简单的历史，Git到底有什么牛逼之处呢？

1. 分布式，去中心化；分布式版本控制系统，每个开发者都可以在本地拥有一个完整的代码仓库，不需要依赖中央服务器，可以在没有网络连接的情况下进行版本控制和代码管理。-------设计理念
2. 快速；Git设计使用对象存储优化了传输过程，使用了快速的算法，使得Git在处理大型项目和大量数据时表现得非常高效。----对象存储以及数据结构
3. 强大的分支功能；可以轻松创建、合并和删除分支，使得团队协作更加灵活和高效。每个分支都可以独立进行开发，不影响其他分支的代码。----------branch
4. 强大的版本控制功能；Git可以对代码的每一次修改进行版本控制，记录了修改的时间、内容、作者等信息，并可以方便地查看和比较不同版本之间的差异。----------commit对象存储版本信息
5. 完整性；Git使用哈希值来标识版本，每一次提交的代码都会计算一个唯一的哈希值，保证了版本的完整性和可追溯性并追踪版本的改变；-------一致性哈希算法sha-1
6. 缓存机制：Git引入了缓存机制，将文件的变化在内存中暂存，只有在需要提交时才会写入磁盘，大大提高了文件的读写效率。-------stage

## 三、Git功能实现原理

### 1、.git目录结构

.git目录，git存储和操作的几乎所有内容都位于此目录。目录架构如下所示：

```bash
-rw-r--r--     COMMIT_EDITMSG
-rw-r--r--@    FETCH_HEAD
-rw-r--r--     HEAD
-rw-r--r--     ORIG_HEAD
-rw-r--r--     config
-rw-r--r--     description
drwxr-xr-x     hooks
-rw-r--r--     index
drwxr-xr-x     info
drwxr-xr-x     logs
drwxr-xr-x     objects
drwxr-xr-x     refs
```

**config**：包含本项目相关的配置选项；

**info**：目录保留了一个全局排除文件，用于不想在.gitignore文件中跟踪的被忽略的模式。

**hooks**：目录包含客户端或服务器端钩子脚本

**objects**：存放所有的 git 对象，哈希值一共40位，前 2 位作为文件夹名称，后 38 位作为对象文件名。

**index**：是Git存储暂存区域信息的地方

**HEAD**：这就是我们常说的HEAD指针，它指向了当前分支

**FETCH_HEAD**：是一个版本链接，记录在本地的一个文件中，指向着目前已经从远程仓库取下来的分支的末端版本。

**ORIG_HEAD**：是由以一种激烈的方式移动HEAD的命令(git am, git merge, git rebase, git reset)创建的，以记录HEAD操作之前的位置，以便您可以轻松地将分支的尖端更改回运行它们之前的状态

**description**：仓库的描述信息，仅供GitWeb程序使用。

**logs**：保存所有更新的引用记录。logs有两个文件，refs文件夹和HEAD文件。

HEAD文件保存的是所有的操作记录，使用git reflog查询的结果就是从这个文件来的。

在refs文件夹中有heads和remote文件夹，

heads文件夹里面存储的是本地分支的对象，每个对象的文件名就是本地的一个分支名。我们使用git branch查看本地所有分支时，查询出的分支就是heads文件夹下所有文件的名称，这些分支文件中存储的是对应分支下的操作记录。

remotes文件夹里存储的是远程的所有分支对象，每个对象的文件名称就是远程的一个分支名称。这些分支文件中保存了远程仓库对应分支所有操作

**refs**：引用；里面包含了三个文件及：heads、remotes、tags；

heads：里面包含所有的本地分支，每个分支都是文件,文件中存储了分支当前指向的commit

remotes：远程仓库信息,包含远程仓库的当前分支指向及分支的commit指向；

tags：叫做里程碑,或者版本发布用等记录重要版本。

**COMMIT_EDITMSG**：保存着最近一次的提交信息,Git系统不会用到这个文件，只是给用户一个参考





Git的核心是一个简单的键值数据存储。这意味着您可以将任何类型的内容插入到Git存储库中，Git将为您提供一个唯一的密钥---hash值，您可以稍后使用该密钥检索该内容。

在object目录中子目录用SHA-1的前2个字符命名，文件名是剩下的38个字符。

blob对象类型用于存储文件内容，通过git cat-file  -p hash_id即可查看到blob的文件内容。

tree对象类型它解决了存储文件名的问题，还允许您将一组文件存储在一起。Git以类似于UNIX文件系统的方式存储内容，但是稍微简化了一些。所有内容都存储为树和blob对象，树对应于UNIX目录条目，blob或多或少对应于索引节点或文件内容。单个树对象包含一个或多个条目，每个条目都是blob或子树的SHA-1哈希值及其相关的模式、类型和文件名

commit对象类型



![image-20240322144828502](https://raw.githubusercontent.com/tiansy9981/pictuer/master/image-20240322144828502.png)



![image-20240322160757958](https://raw.githubusercontent.com/tiansy9981/pictuer/master/image-20240322160757958.png)

其中id：100644是blob类型，表示是一个普通文件；

160000是commit类型，

040000是tree类型

refs目录存储指向该数据中提交对象的指针(分支、标签、远程等等)

HEAD文件指向当前签出的分支

index文件是Git存储暂存区域信息的地方

## 四、 Git的基本概念

### Git的结构

在Git中，根据工作区域可分为：工作目录、暂存区、git仓库、远程仓库；

1. 工作目录：顾名思义就是本地工作的目录，可对本地文件操作跟记录的位置；
2. 暂存区：对工作区已修改的文件，存放提交快照的区域；
3. 仓库：本地存放文件元数据的位置，其中元数据包含文件以及版本信息等，仓库在本地.git目录中；
4. 远程仓库：指GitHub或gitlab等git仓库，用于保存推送本地仓库至远程仓库或者从远程仓库拉取至本地。

![image-20240321180229680](https://raw.githubusercontent.com/tiansy9981/pictuer/master/image-20240321180229680.png)

### 文件的状态

**在git中文件有四种状态**：

1. 修改modified：已经更改了文件，但尚未将其提交到数据库。
2. 阶段中staged：已经在当前版本中标记了已修改的文件，以便将其放入下一次提交快照中。
3. 提交committed：表示数据安全地存储在本地数据库中。
4. unmodified：文件与仓库、暂存区的内容一致。

**工作目录的文件生命周期有：untracked、unmodified、modified、staged**

![image-20240321215213847](https://raw.githubusercontent.com/tiansy9981/pictuer/master/image-20240321215213847.png)

#### 分支与标签

## Git的基本操作

### 1.本地操作

**在git中获取文件状态**

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

**Git add与commit文件的过程：**

当运行add时，暂存文件计算每个文件的校验和(SHA-1哈希)，将该版本的文件存储在Git存储库中(Git将其称为blob)，并将校验和添加到暂存区域；

当运行git commit创建提交时，git会对每个子目录进行校验和，并将它们作为树对象，存储在git存储库中。然后Git创建一个提交对象，该对象包含元数据和一个指向根项目树的指针，以便在需要时重新创建快照。

![image-20240322105522777](https://raw.githubusercontent.com/tiansy9981/pictuer/master/image-20240322105522777.png)

![image-20240322105542847](https://raw.githubusercontent.com/tiansy9981/pictuer/master/image-20240322105542847.png)

**提交文件至暂存区**

通过git add 可以将文件提交至暂存区。

**文件提交**

**比较文件**

**文件移动**

**文件删除**

**文件恢复**

### 远程仓库操作

仓库拉取

仓库推送

标签

## 四、Git分支

Git存储一系列变更集或差异是以==快照==的方式存储。当您进行提交时，Git存储一个提交对象，该对象包含指向所提交内容的快照的指针。这个对象也包含作者姓名和电子邮件地址等其他信息；

Git中的分支只是指向其中一个提交的轻量级可移动指针。

![image-20240322113638111](https://raw.githubusercontent.com/tiansy9981/pictuer/master/image-20240322113638111.png)

创建分支：



![image-20240322113849649](https://raw.githubusercontent.com/tiansy9981/pictuer/master/image-20240322113849649.png)

Git如何知道你当前在哪个分支上?它保留了一个叫做HEAD的特殊指针。

![image-20240322114023891](https://raw.githubusercontent.com/tiansy9981/pictuer/master/image-20240322114023891.png)





