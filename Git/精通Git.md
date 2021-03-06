> 精通 GIt（第二版） 
> Pro GIt  
> 2017 年 9 月第一次出版  

<!-- TOC -->

- [第一章 起步](#第一章-起步)
  - [1.1 关于版本控制](#11-关于版本控制)
  - [1.2 Git 简史](#12-git-简史)
  - [1.3 Git 基础](#13-git-基础)
  - [1.4 命令行](#14-命令行)
  - [1.5 安装 Git](#15-安装-git)
  - [1.6 初次运行 GIt 前的配置。](#16-初次运行-git-前的配置)
  - [1.7 获取帮助](#17-获取帮助)
- [第二章 Git 基础](#第二章-git-基础)
  - [2.1 获取 Git 仓库](#21-获取-git-仓库)
  - [2.2 记录每次更新到仓库](#22-记录每次更新到仓库)
  - [2.3 查看提交历史](#23-查看提交历史)
  - [2.4 撤销操作](#24-撤销操作)
  - [2.5 远程仓库的使用](#25-远程仓库的使用)
  - [2.6 打标签](#26-打标签)
  - [2.7 Git别名](#27-git别名)
- [第三章 Git分支](#第三章-git分支)
  - [3.1 分支简介](#31-分支简介)
  - [3.2 分支的新建与合并](#32-分支的新建与合并)
  - [3.3 分支管理](#33-分支管理)
  - [3.4 分支开发工作流](#34-分支开发工作流)
  - [3.5 远程分支](#35-远程分支)
  - [3.6 变基](#36-变基)
- [第四章 服务器上的Git](#第四章-服务器上的Git)
  - [4.1 协议](#41-协议)
  - [4.2 在服务器上搭建 Git](#42-在服务器上搭建-Git)
  - [4.3 生成 SSH 公钥](#43-生成-SSH-公钥)
  - [4.4 配置服务器](#44-配置服务器)
  - [4.5 Git守护进程](#45-Git守护进程)
- [第六章 GitHub](#第六章-GitHub)
  - [6.1 账号的创建和配置](#61-账号的创建和配置)
  - [6.2 对项目做出贡献](#62-对项目做出贡献)

<!-- /TOC -->

# 第一章 起步

## 1.1 关于版本控制

版本控制是一种记录一个或若干文件内容变化，以便将来查阅特定版本修订情况的系统。 在本书所展示的例子中，我们对保存着软件源代码的文件作版本控制，但实际上，你可以对任何类型的文件进行版本控制。

集中化的版本控制系统让在不同系统上的开发者协同工作，这类系统，诸如 CVS、Subversion 以及 Perforce 等，都有一个单一的集中管理的服务器，保存所有文件的修订版本，而协同工作的人们都通过客户端连到这台服务器，取出最新的文件或者提交更新。 多年以来，这已成为版本控制系统的标准做法。这么做最显而易见的缺点是中央服务器的单点故障。

在分布式版本控制系统中，像 Git、Mercurial、Bazaar 以及 Darcs 等，客户端并不只提取最新版本的文件快照，而是把代码仓库完整地镜像下来。 这么一来，任何一处协同工作用的服务器发生故障，事后都可以用任何一个镜像出来的本地仓库恢复。 因为每一次的克隆操作，实际上都是一次对代码仓库的完整备份。

## 1.2 Git 简史

Linux 开源社区（特别是 Linux 的缔造者 Linus Torvalds）基于使用 BitKeeper 时的经验教训，开发出自己的版本系统。 他们对新的系统制订了若干目标：

- 速度
- 简单的设计
- 对非线性开发模式的强力支持（允许成千上万个并行开发的分支）
- 完全分布式
- 有能力高效管理类似 Linux 内核一样的超大规模项目（速度和数据量）

## 1.3 Git 基础

Git 和其它版本控制系统（包括 Subversion 和近似工具）的主要差别在于 Git 对待数据的方法。 概念上来区分，其它大部分系统以文件变更列表的方式存储信息。 这类系统（CVS、Subversion、Perforce、Bazaar 等等）将它们保存的信息看作是一组基本文件和每个文件随时间逐步累积的差异。

Git 不按照以上方式对待或保存数据。 反之，Git 更像是把数据看作是对小型文件系统的一组快照。 每次你提交更新，或在 Git 中保存项目状态时，它主要对当时的全部文件制作一个快照并保存这个快照的索引。 为了高效，如果文件没有修改，Git 不再重新存储该文件，而是只保留一个链接指向之前存储的文件。 Git 对待数据更像是一个 **快照流**。

Git 更像是一个小型的文件系统，提供了许多以此为基础构建的超强工具，而不只是一个简单的 VCS。

在 Git 中的绝大多数操作都只需要访问本地文件和资源，一般不需要来自网络上其它计算机的信息。这也意味着你离线或者没有 VPN 时，几乎可以进行任何操作。

Git 中所有数据在存储前都计算校验和，然后以校验和来引用。 这意味着不可能在 Git 不知情时更改任何文件内容或目录内容。 这个功能建构在 Git 底层，是构成 Git 哲学不可或缺的部分。 若你在传送过程中丢失信息或损坏文件，Git 就能发现。实际上，Git 数据库中保存的信息都是以文件内容的哈希值来索引，而不是文件名。

你执行的 Git 操作，几乎只往 Git 数据库中增加数据。 很难让 Git 执行任何不可逆操作，或者让它以任何方式清除数据。 同别的 VCS 一样，未提交更新时有可能丢失或弄乱修改的内容；但是一旦你提交快照到 Git 中，就难以再丢失数据，特别是如果你定期的推送数据库到其它仓库的话。

Git 有三种状态，你的文件可能处于其中之一：已提交（committed）、已修改（modified）和已暂存（staged）。 已提交表示数据已经安全的保存在本地数据库中。 已修改表示修改了文件，但还没保存到数据库中。 已暂存表示对一个已修改文件的当前版本做了标记，使之包含在下次提交的快照中。

由此引入 Git 项目的三个工作区域的概念：Git 仓库、工作目录以及暂存区域。Git 仓库目录是 Git 用来保存项目的元数据和对象数据库的地方。 这是 Git 中最重要的部分，从其它计算机克隆仓库时，拷贝的就是这里的数据。工作目录是对项目的某个版本独立提取出来的内容。 这些从 Git 仓库的压缩数据库中提取出来的文件，放在磁盘上供你使用或修改。暂存区域是一个文件，保存了下次将提交的文件列表信息，一般在 Git 仓库目录中。 有时候也被称作`‘索引’'，不过一般说法还是叫暂存区域。

![工作目录、暂存区域和Git仓库](https://git-scm.com/book/en/v2/images/areas.png)

基本的 Git 工作流程如下：

1. 在工作目录中修改文件。
2. 暂存文件，将文件的快照放入暂存区域。
3. 提交更新，找到暂存区域的文件，将快照永久性存储到 Git 仓库目录。

## 1.4 命令行

Git 有多种使用方式。可以使用原生的命令行模式，也可以使用 GUI 模式，这些 GUI 软件也能提供多种功能。在本书中，我们将使用命令模式。这是因为首先，只有在命令模式下才能执行 Git 的所有命令，而大多数的 GUI 软件只实现了 Git 所有功能的一个子集以降低操作难度。

## 1.5 安装 Git

在 Linux 上安装

如果你想在 Linux 上用二进制安装程序来安装 Git，可以使用发行版包含的基础软件包管理工具来安装。

```shell
# 如果以 Fedora 上为例，你可以使用 yum：
$ sudo yum install git

# 如果你在基于 Debian 的发行版上，请尝试用 apt-get：
$ sudo apt-get install git
```

在 Mac 上安装

在 Mac 上安装 Git 有多种方式。 最简单的方法是安装 Xcode Command Line Tools。 Mavericks （10.9） 或更高版本的系统中，在 Terminal 里尝试首次运行 _git_ 命令即可。 如果没有安装过命令行开发者工具，将会提示你安装。如果你想安装更新的版本，可以使用二进制安装程序。 官方维护的 OSX Git 安装程序可以在 Git 官方网站下载。

在 Windows 上安装

在 Windows 上安装 Git 也有几种安装方法。 官方版本可以在 Git 官方网站下载。 要注意这是一个名为 Git for Windows 的项目（也叫做 msysGit），和 Git 是分别独立的项目。另一个简单的方法是安装 GitHub for Windows。 该安装程序包含图形化和命令行版本的 Git。 它也能支持 Powershell，提供了稳定的凭证缓存和健全的 CRLF 设置。 稍后我们会对这方面有更多了解，现在只要一句话就够了，这些都是你所需要的。

有人觉得从源码安装 Git 更实用，因为你能得到最新的版本。 二进制安装程序倾向于有一些滞后，当然近几年 Git 已经成熟，这个差异不再显著。如果你想从源码安装 Git，需要安装 Git 依赖的库：curl、zlib、openssl、expat，还有 libiconv。 如果你的系统上有 yum （如 Fedora）或者 apt-get（如基于 Debian 的系统），可以使用命令来安装最小化的依赖包来编译和安装 Git 的二进制版。

## 1.6 初次运行 GIt 前的配置。

Git 自带一个 `git config` 的工具来帮助设置控制 Git 外观和行为的配置变量。 这些变量存储在三个不同的位置：

1. `/etc/gitconfig` 文件: 包含系统上每一个用户及他们仓库的通用配置。 如果使用带有 `--system` 选项的 `git config` 时，它会从此文件读写配置变量。
2. `~/.gitconfig` 或 `~/.config/git/config` 文件：只针对当前用户。 可以传递 `--global` 选项让 Git 读写此文件。
3. 当前使用仓库的 Git 目录中的 `config` 文件（就是 `.git/config`）：针对该仓库。

每一个级别覆盖上一级别的配置，所以 `.git/config` 的配置变量会覆盖 `/etc/gitconfig` 中的配置变量。

在 Windows 系统中，Git 会查找 `$HOME` 目录下（一般情况下是 `C:\Users\$USER`）的 `.gitconfig` 文件。 Git 同样也会寻找 `/etc/gitconfig` 文件，但只限于 MSys 的根目录下，即安装 Git 时所选的目标位置。

当安装完 Git 应该做的第一件事就是设置你的用户名称与邮件地址。 这样做很重要，因为每一个 Git 的提交都会使用这些信息，并且它会写入到每一次提交中，不可更改：

```shell
$ git config --global user.name "John Doe"
$ git config --global user.email johndoe@example.com
```

再次强调，如果使用了 `--global` 选项，那么该命令只需要运行一次，因为之后无论你在该系统上做任何事情， Git 都会使用那些信息。 当你想针对特定项目使用不同的用户名称与邮件地址时，可以在那个项目目录下运行没有 `--global` 选项的命令来配置。

既然用户信息已经设置完毕，可以配置默认文本编辑器了，当 Git 需要输入信息时会调用它。 如果未配置，Git 会使用操作系统默认的文本编辑器，通常是 Vim。

如果想要检查配置，可以使用 `git config --list` 命令来列出所有 Git 当时能找到的配置。也可以通过输入 `git config <key>` 来检查 Git 的某一项配置。

## 1.7 获取帮助

若使用 Git 时需要获取帮助，有三种方法可以找到 Git 命令的使用手册：

```shell
$ git help <verb>
$ git <verb> --help
$ man git-<verb>
```

例如，要想获得 config 命令的手册，执行

```shell
$ git help config
```

这些命令很棒，因为你随时随地可以使用而无需联网。 如果你觉得手册或者本书的内容还不够用，你可以尝试在 Freenode IRC 服务器（ irc.freenode.net ）的 `#git` 或 `#github` 频道寻求帮助。

# 第二章 Git 基础

## 2.1 获取 Git 仓库

有两种取得 Git 项目仓库的方法，第一种是在现有项目或目录下导入所有文件到 Git 中，第二种是从一个服务器克隆一个现有的 Git 仓库。

如果打算使用 Git 来对现有的项目进行管理，只需要进入该项目目录并输入以下命令。该命令将创建一个名为 `.git` 的子目录，这个子目录含有你初始化的 Git 仓库中所有的必须文件，这些文件是 Git 仓库的骨干。 此时，项目里的文件还没有被跟踪。可通过 `git add` 命令来实现对指定文件的跟踪，然后执行 `git commit` 提交。

```shell
$ git init

$ git add *.c
$ git add LICENSE
$ git commit -m 'initial project version'
```

克隆现有的仓库。Git 克隆的是该 Git 仓库服务器上的几乎所有数据，而不是仅仅复制完成你的工作所需要文件。 当执行 `git clone` 命令的时候，默认配置下远程 Git 仓库中的每一个文件的每一个版本都将被拉取下来。如下命令，会在当前目录下创建一个名为 “libgit2” 的目录，并在这个目录下初始化一个 `.git` 文件夹，从远程仓库拉取下所有数据放入 `.git` 文件夹，然后从中读取最新版本的文件的拷贝。同时可以在克隆远程仓库的时候，自定义本地仓库的名字。

```shell
$ git clone https://github.com/libgit2/libgit2

$ git clone https://github.com/libgit2/libgit2 mylibgit
```

## 2.2 记录每次更新到仓库

工作目录下的每一个文件有两种状态：已跟踪或未跟踪。 已跟踪的文件是指那些被纳入了版本控制的文件，在上一次快照中有它们的记录，在工作一段时间后，它们的状态可能处于未修改，已修改或已放入暂存区。 工作目录中除已跟踪文件以外的所有其它文件都属于未跟踪文件，它们既不存在于上次快照的记录中，也没有放入暂存区。 初次克隆某个仓库的时候，工作目录中的所有文件都属于已跟踪文件，并处于未修改状态。

编辑过某些文件之后，由于自上次提交后你对它们做了修改，Git 将它们标记为已修改文件。 我们逐步将这些修改过的文件放入暂存区，然后提交所有暂存了的修改，如此反复。所以使用 Git 时文件的生命周期如下：

![记录版本](https://git-scm.com/book/en/v2/images/lifecycle.png)

要查看哪些文件处于什么状态，可以用 `git status` 命令。

使用命令 `git add` 开始跟踪一个文件。

只要在 `Changes to be committed` 这行下面的，就说明是已暂存状态。 如果此时提交，那么该文件此时此刻的版本将被留存在历史记录中。 如果出现`Changes not staged for commit` ，说明已跟踪文件的内容发生了变化，但还没有放到暂存区。

`git add` 命令使用文件或目录的路径作为参数；如果参数是目录的路径，该命令将递归地跟踪该目录下的所有文件。

`git status` 命令的输出十分详细，但其用语有些繁琐。 如果你使用 `git status -s` 命令或 `git status --short` 命令，你将得到一种更为紧凑的格式输出。

一般我们总会有些文件无需纳入 Git 的管理，也不希望它们总出现在未跟踪文件列表。 通常都是些自动生成的文件，比如日志文件，或者编译过程中创建的临时文件等。 在这种情况下，我们可以创建一个名为 `.gitignore` 的文件，列出要忽略的文件模式。

文件 `.gitignore` 的格式规范如下：

- 所有空行或者以 `＃` 开头的行都会被 Git 忽略。
- 可以使用标准的 glob 模式匹配。
- 匹配模式可以以（`/`）开头防止递归。
- 匹配模式可以以（`/`）结尾指定目录。
- 要忽略指定模式以外的文件或目录，可以在模式前加上惊叹号（`!`）取反。

所谓的 glob 模式是指 shell 所使用的简化了的正则表达式。 星号（`*`）匹配零个或多个任意字符；`[abc]`匹配任何一个列在方括号中的字符（这个例子要么匹配一个 a，要么匹配一个 b，要么匹配一个 c）；问号（`?`）只匹配一个任意字符；如果在方括号中使用短划线分隔两个字符，表示所有在这两个字符范围内的都可以匹配（比如 `[0-9]` 表示匹配所有 0 到 9 的数字）。 使用两个星号（`*`) 表示匹配任意中间目录，比如`a/**/z` 可以匹配 `a/z`, `a/b/z` 或 `a/b/c/z`等。

尽管 `git status` 已经通过在相应栏下列出文件名的方式回答了这个问题，`git diff` 将通过文件补丁的格式显示具体哪些行发生了改变。此命令比较的是工作目录中当前文件和暂存区域快照之间的差异， 也就是修改之后还没有暂存起来的变化内容。若要查看已暂存的将要添加到下次提交里的内容，可以用 `git diff --cached` 命令。（Git 1.6.1 及更高版本还允许使用 `git diff --staged`，效果是相同的，但更好记些。）

每次准备提交前，先用 `git status` 看下，是不是都已暂存起来了， 然后再运行提交命令 `git commit`进行妥当第提交。默认的提交消息包含最后一次运行 `git status` 的输出，放在注释行里，另外开头还有一空行，供你输入提交说明。 退出编辑器时，Git 会丢掉注释行，用你输入提交附带信息生成一次提交。也可以在 `commit` 命令后添加 `-m` 选项，将提交信息与命令放在同一行。

Git 提供了一个跳过使用暂存区域的方式， 只要在提交的时候，给 `git commit` 加上 `-a` 选项，Git 就会自动把所有已经跟踪过的文件暂存起来一并提交，从而跳过 `git add` 步骤。

要从 Git 中移除某个文件，就必须要从已跟踪文件清单中移除（确切地说，是从暂存区域移除），然后提交。 可以用 `git rm` 命令完成此项工作，并连带从工作目录中删除指定的文件，这样以后就不会出现在未跟踪文件清单中了。如果只是简单地从工作目录中手工删除文件，运行 `git status` 时就会在 “Changes not staged for commit” 部分（也就是 _未暂存清单_），然后再运行 `git rm` 记录此次移除文件的操作。下一次提交时，该文件就不再纳入版本管理了。

如果删除之前修改过并且已经放到暂存区域的话，则必须要用强制删除选项 `-f`（译注：即 force 的首字母）。 这是一种安全特性，用于防止误删还没有添加到快照的数据，这样的数据不能被 Git 恢复。

另外一种情况是，如果想把文件从 Git 仓库中删除，也是从暂缓区中移除，也忘记将它们添加到`.gitignore` 文件，但仍然希望保留在当前工作目录中，可以使用 `--cached` 选项。

`git rm` 命令后面可以列出文件或者目录的名字，也可以使用 `glob` 模式。

不像其它的 VCS 系统，Git 并不显式跟踪文件移动操作。 如果在 Git 中重命名了某个文件，仓库中存储的元数据并不会体现出这是一次改名操作。不过 Git 非常聪明，它会推断出究竟发生了什么。要在 Git 中对文件改名，可以直接运行 `git mv` 就行了。其实，运行 `git mv` 就相当于运行了下面三条命令

```shell
$ git mv file_from file_to

$ mv README.md README
$ git rm README.md
$ git add README
```

## 2.3 查看提交历史

在提交了若干更新，又或者克隆了某个项目之后，你也许想回顾下提交历史。 完成这个任务最简单而又有效的工具是 `git log` 命令。默认不用任何参数的话，`git log` 会按提交时间列出所有的更新，最近的更新排在最上面。

一个常用的选项是 `-p`，用来显示每次提交的内容差异。 你也可以加上 `-2` 来仅显示最近两次提交。

可以为 `git log` 附带一系列的总结性选项。 比如说，如果想看到每次提交的简略的统计信息，你可以使用 `--stat` 选项。`--stat` 选项在每次提交的下面列出所有被修改过的文件、有多少文件被修改了以及被修改过的文件的哪些行被移除或是添加了。 在每次提交的最后还有一个总结。

另外一个常用的选项是 `--pretty`。 这个选项可以指定使用不同于默认格式的方式展示提交历史。 这个选项有一些内建的子选项供你使用。 比如用 `oneline` 将每个提交放在一行显示，查看的提交数很大时非常有用。 另外还有 `short`，`full` 和 `fuller` 可以用，展示的信息或多或少有些不同。

但最有意思的是 format，可以定制要显示的记录格式。 这样的输出对后期提取分析格外有用  —  因为你知道输出的格式不会随着 Git 的更新而发生改变

| 选项  | 说明                                        |
| ----- | ------------------------------------------- |
| `%H`  | 提交对象（commit）的完整哈希字串            |
| `%h`  | 提交对象的简短哈希字串                      |
| `%T`  | 树对象（tree）的完整哈希字串                |
| `%t`  | 树对象的简短哈希字串                        |
| `%P`  | 父对象（parent）的完整哈希字串              |
| `%p`  | 父对象的简短哈希字串                        |
| `%an` | 作者（author）的名字                        |
| `%ae` | 作者的电子邮件地址                          |
| `%ad` | 作者修订日期（可以用 --date= 选项定制格式） |
| `%ar` | 作者修订日期，按多久以前的方式显示          |
| `%cn` | 提交者（committer）的名字                   |
| `%ce` | 提交者的电子邮件地址                        |
| `%cd` | 提交日期                                    |
| `%cr` | 提交日期，按多久以前的方式显示              |
| `%s`  | 提交说明                                    |

除了定制输出格式的选项之外，`git log` 还有许多非常实用的限制输出长度的选项，也就是只输出部分提交信息。之前你已经看到过 `-2` 了，它只显示最近的两条提交， 实际上，这是 `-<n>` 选项的写法，其中的 `n` 可以是任何整数，表示仅显示最近的若干条提交。另外还有按照时间作限制的选项，比如 `--since` 和 `--until` 也很有用。 例如，下面的命令列出所有最近两周内的提交`git log --since=2.weeks`

还可以给出若干搜索条件，列出符合的提交。 用 `--author` 选项显示指定作者的提交，用 `--grep` 选项搜索提交说明中的关键字。 （请注意，如果要得到同时满足这两个选项搜索条件的提交，就必须用 `--all-match` 选项。否则，满足任意一个条件的提交都会被匹配出来）

另一个非常有用的筛选选项是 `-S`，可以列出那些添加或移除了某些字符串的提交。 比如说，你想找出添加或移除了某一个特定函数的引用的提交，你可以这样使用：

```shell
$ git log -Sfunction_name
```

最后一个很实用的 `git log` 选项是路径（path）， 如果只关心某些文件或者目录的历史提交，可以在 git log 选项的最后指定它们的路径。 因为是放在最后位置上的选项，所以用两个短划线（--）隔开之前的选项和后面限定的路径名。

## 2.4 撤销操作

有时候我们提交完了才发现漏掉了几个文件没有添加，或者提交信息写错了。 此时，可以运行带有 `--amend` 选项的提交命令尝试重新提交。这个命令会将暂存区中的文件提交。 如果自上次提交以来你还未做任何修改（例如，在上次提交后马上执行了此命令），那么快照会保持不变，而你所修改的只是提交信息。文本编辑器启动后，可以看到之前的提交信息。 编辑后保存会覆盖原来的提交信息。

取消暂存的文件。如果不小心将工作区的文件提交到暂存区，可以使用 `git reset HEAD <file>...` 来取消暂存。虽然在调用时加上 `--hard` 选项可以令 `git reset` 成为一个危险的命令（译注：可能导致工作目录中所有当前进度丢失！），但本例中工作目录内的文件并不会被修改。 不加选项地调用 `git reset` 并不危险 — 它只会修改暂存区域。

撤销对文件的修改。如果不想保留对某个文件的修改，可以使用`git checkout -- [file]`方便地撤销修改，将它还原成上次提交的样子。记住，在 Git 中任何 已提交的东西几乎总是可以恢复的。 甚至那些被删除的分支中的提交或使用 `--amend`选项覆盖的提交也可以恢复。

## 2.5 远程仓库的使用

查看远程仓库。如果想查看你已经配置的远程仓库服务器，可以运用`git remote` 命令，它会列出你指定的每一个远程服务器的简写。如果已经克隆了自己的仓库，那么至少应该能看到 origin，这是 Git 给克隆的仓库服务器的默认名字。可以指定选项`-v`，会显示需要读写远程仓库使用的 Git 保存的简写与其对应的 URL。如果远程仓库不止一个，该命令会将它们全部列出。

```shell
$ git remote
origin

$ git remote -v
origin	https://github.com/schacon/ticgit (fetch)
origin	https://github.com/schacon/ticgit (push)
```

添加远程仓库。运行 `git remote add <shortname> <url>` 添加一个新的远程 Git 仓库，同时指定一个可以轻松引用的简写。

```shell
$ git remote add pb https://github.com/paulboone/ticgit
$ git remote -v
origin	https://github.com/schacon/ticgit (fetch)
origin	https://github.com/schacon/ticgit (push)
pb	https://github.com/paulboone/ticgit (fetch)
pb	https://github.com/paulboone/ticgit (push)
```

从远程仓库中抓取与拉取。从远程仓库中获得数据，可以执行`git fetch [remote-name]`，这个命令会访问远程仓库，从中拉取所有本地还没有的数据。执行完成后，将会拥有那个远程仓库中所有分支的引用，可以随时合并或查看。

如果使用`close`命令克隆了一个仓库，命令会自动将其添加为远程仓库并默认以“origin”为简写。所以，`git fetch origin` 会抓取克隆（或上一次抓取）后新推送的所有工作。

必须注意 `git fetch` 命令会将数据拉取到你的本地仓库 ，它并不会自动合并或修改你当前的工作。 当准备好时你必须手动将其合并入你的工作。

如果有一个分支设置为跟踪一个远程分支，可以使用 `git pull`命令来自动的抓取然后合并远程分支到当前分支。 这可能是一个更简单或更舒服的工作流程；默认情况下，`git clone` 命令会自动设置本地 master 分支跟踪克隆的远程仓库的 master 分支， 运行 `git pull` 通常会从最初克隆的服务器上抓取数据并自动尝试合并到当前所在的分支。

```shell
$ git fetch pb
remote: Counting objects: 43, done.
remote: Compressing objects: 100% (36/36), done.
remote: Total 43 (delta 10), reused 31 (delta 5)
Unpacking objects: 100% (43/43), done.
From https://github.com/paulboone/ticgit
 * [new branch]      master     -> pb/master
 * [new branch]      ticgit     -> pb/ticgit
```

推送到远程仓库。使用`git push [remote-name] [branch-name]`可以将 master 分支推送到 `origin` 服务器时），那么运行这个命令就可以将所做的备份到服务器。

只有当有克隆服务器的写入权限，并且之前没有人推送过时，这条命令才能生效。 当和其他人在同一时间克隆，他们先推送到上游然后你再推送到上游，你的推送就会毫无疑问地被拒绝。 你必须先将他们的工作拉取下来并将其合并进你的工作后才能推送。

```shell
$ git push origin master
```

查看远程仓库。如果想查看某一个远程仓库的更多信息，可以使用 `git remote show [remote-name]` 命令。还可以告诉你运行推送和拉取命令时，所有引用抓取和分支合并的情况。

```shell
$ git remote show origin
* remote origin
  Fetch URL: https://github.com/schacon/ticgit
  Push  URL: https://github.com/schacon/ticgit
  HEAD branch: master
  Remote branches:
    master                               tracked
    dev-branch                           tracked
  Local branch configured for 'git pull':
    master merges with remote master
  Local ref configured for 'git push':
    master pushes to master (up to date)
```

远程仓库的移除与重命名。如果想要重命名引用的名字可以运行 `git remote rename` 去修改一个远程仓库的简写名。例如，想要将 `pb` 重命名为 `paul`， 就可以使用`git remote rename pb paul`命令。值得注意的是这同样也会修改远程分支的名字。如果因为一些原因想要移除一个远程仓库，又或者某一个贡献者不再贡献了，可以使用 `git remote rm` 。

```shell
$ git remote rename pb paul
$ git remote
origin
paul

$ git remote rm paul
$ git remote
origin
```

## 2.6 打标签

像其他版本控制系统（VCS）一样，Git 可以给历史中的某一个提交打上标签，以示重要。

列出标签。在 Git 中列出已有的标签是非常简单直观的。 只需要输入 `git tag`。这个命令以字母顺序列出标签；但是它们出现的顺序并不重要。也可以使用特定的模式查找标签。

创建标签。Git 使用两种主要类型的标签：轻量标签（lightweight）与附注标签（annotated）。一个轻量标签很像一个不会改变的分支，它只是一个特定提交的引用。然而，附注标签是存储在 Git 数据库中的一个完整对象。 它们是可以被校验的；其中包含打标签者的名字、电子邮件地址、日期时间；还有一个标签信息；并且可以使用 GNU Privacy Guard （GPG）签名与验证。

通常建议创建附注标签，这样你可以拥有以上所有信息；但是如果你只是想用一个临时的标签，或者因为某些原因不想要保存那些信息，轻量标签也是可用的。

在 Git 中创建一个附注标签是很简单的。 最简单的方式是当你在运行 `tag` 命令时指定 `-a` 选项。`-m` 选项指定了一条将会存储在标签中的信息。 如果没有为附注标签指定一条信息，Git 会运行编辑器要求你输入信息。

通过使用 `git show` 命令可以看到标签信息与对应的提交信息。输出显示了打标签者的信息、打标签的日期时间、附注信息，然后显示具体的提交信息。

另一种给提交打标签的方式是使用轻量标签。 轻量标签本质上是将提交校验和存储到一个文件中 - 没有保存任何其他信息。 创建轻量标签，不需要使用 `-a`、`-s` 或 `-m` 选项，只需要提供标签名字。这时，如果在标签上运行 `git show`，你不会看到额外的标签信息。 命令只会显示出提交信息。

也可以对过去的提交打标签。要在那个提交上打标签，你需要在命令的末尾指定提交的校验和（或部分校验和）。

共享标签。默认情况下，`git push` 命令并不会传送标签到远程仓库服务器上。 在创建完标签后你必须显式地推送标签到共享服务器上。 这个过程就像共享远程分支一样 。只要运行 `git push origin [tagname]`就行。如果想要一次性推送很多标签，也可以使用带有 `--tags` 选项的 `git push` 命令。 这将会把所有不在远程仓库服务器上的标签全部传送到那里。

在 Git 中并不能真的检出一个标签，因为它们并不能像分支一样来回移动。 如果你想要工作目录与仓库中特定的标签版本完全一样，可以使用 `git checkout -b [branchname] [tagname]` 在特定的标签上创建一个新分支。

## 2.7 Git别名

Git 并不会在输入部分命令时自动推断出想要的命令。 如果不想每次都输入完整的 Git 命令，可以通过 `git config` 文件来轻松地为每一个命令设置一个别名。如果想要执行外部命令，而不是一个 Git 子命令。 如果是那样的话，可以在命令前面加入 `!` 符号。

```shell
$ git config --global alias.co checkout
$ git config --global alias.br branch
$ git config --global alias.ci commit
$ git config --global alias.st status
```

# 第三章 Git分支

几乎所有的版本控制系统都以某种形式支持分支。使用分支意味着可以把工作从开发主线上分离开来，以免影响开发主线。在很多版本控制系统中，这是一个略微低效的过程——通常需要完全创建一个源代码目录的副本。对于大项目来说，这样的过程会耗费很多时间。

Git 处理分支的方式可谓是难以置信的轻量，创建新分支这一操作几乎能在瞬间完成，并且在不同分支之间的切换操作也是一样便捷。与许多其它版本控制系统不同，Git 鼓励在工作流程中频繁地使用分支与合并，哪怕一天之内进行许多次。

## 3.1 分支简介

Git 保存的不是文件的变化或者差异，而是一系列不同时刻的文件快照。在进行提交操作时，Git 会保存一个提交对象（commit object）。知道了 Git 保存数据的方式，可以很自然的想到——该提交对象会包含一个指向暂存内容快照的指针，同时，该提交对象还包含了作者的姓名和邮箱、提交时输入的信息以及指向它的父对象的指针。首次提交产生的提交对象没有父对象，普通提交操作产生的提交对象有一个父对象，而由多个分支合并产生的提交对象有多个父对象。

Git 的分支，其实本质上仅仅是指向提交对象的可变指针。Git 的默认分支名字是 master。在多次提交操作之后，其实已经有一个指向最后那个提交对象的 master 分支。它会在每次的提交操作中自动向前移动。Git 的“master”分支并不是一个特殊分支，与其它分支完全没有区别。之所以几乎每一个仓库都有 master 分支，是因为`git init`命令默认创建它，并且大多数人都懒得去改动它。

分支创建。创建新分支，就是创建了一个可以移动的新的指针。使用`git branch`命令,这就会在当前所在的提交对象上创建一个指针。

```shell
$ git branch testing
```

![两个指向相同提交历史的分支](https://git-scm.com/book/en/v2/images/two-branches.png)

Git 有一个名为 HEAD 的特殊指针，与许多其它版本控制系统的概念不一样，它是一个指针，指向当前所在的本地分支。通过`git branch`命令仅仅创建了一个新分支，并不会自动切换到新分支中去，此时 HEAD 指向 master。

可以简单地使用`git log`命令查看各个分支当前所指的对象。 提供这一功能的参数是`--decorate`。

分支切换。要切换到一个已存在的分支，需要使用`git checkout`命令。在分支切换时，一定要注意工作目录里的文件会被改变。如果切换到一个比较旧的分支，工作目录恢复到该分支最后一次提交时的样子。如果 Git 不能干净利落地完成这个任务，它将禁止切换分支。

可以在不同分之间不断地来回切换和工作，并在时机成熟时将它们合并起来。而所有这些工作，需要的命令只有 branch、checkout 和 commit。

可以简单地使用`git log`命令查看分叉历史。运行`git log --oneline --decorate --graph --all`，它会输出提交历史、各个分支的指向以及项目的分支分叉情况。

```shell
$ git log --oneline --decorate --graph --all
* c2b9e (HEAD, master) made other changes
| * 87ab2 (testing) made a change
|/
* f30ab add feature #32 - ability to add new formats to the
* 34ac2 fixed bug #1328 - stack overflow under certain conditions
* 98ca9 initial commit of my project
```

与过去大多数版本控制系统形成鲜明对比，在 Git 中任何规模的项目都能在瞬间创建新分支。同时，由于每次提交都会记录父对象，所以寻找恰当的合并基础也是同样的简单和高效。

## 3.2 分支的新建与合并

新建分支。想要新建一个分支并同时切换到那个分支上，可以运行一个带有 -b 参数的`git checkout`命令，它是两条命令的简写。

```shell
$ git checkout -b iss53
Switched to a new branch "iss53"

$ git branch iss53
$ git checkout iss53
```

有了 Git 的帮助，不必把这个紧急问题和其它问题的修改混在一起，也不需要花大力气还原原来的修改，然后再添加这个紧急问题的修改，最后将这个修改提交到线上分支。所要做的仅仅是切换回 master 分支。

但是，这样做之前要留意工作目录和暂存区里那些还没有被提交的修改，它可能会和即将检出的分支产生冲突从而阻止 Git 切换到该分支。最好的方法是，在切换分支之前，保持好一个干净的状态。有一些方法可以绕过这个问题，即保存进度（stashing）和修补提交（commit amending）。

请牢记，当切换分支的时候，Git 会重置工作目录，使其看起来像回到了在那个分支上最后一次提交的样子。Git 会自动添加、删除、修改文件以确保此时工作目录和这个分支最后一次提交时一模一样。

修改新分支的内容后，将其合并回 master 分支来部署到线上。可以使用`git merge`命令来达到上述目的。在合并的时候，由于当前 master 分支指向的提交是当前提交的直接上游，所以 Git 只是简单的将指针向前移动。，这种情况下的合并操作没有需要解决的分歧——这就叫做“快进（fast-forward）”。

```shell
$ git checkout master
$ git merge hotfix
Updating f42c576..3a0874c
Fast-forward
 index.html | 2 ++
 1 file changed, 2 insertions(+)
 ```

在紧急问题的解决方案发布之后，使用带 -d 选项的`git branch`命令来删除分支。然后可以切回之前工作的分支继续工作。但是在之前分支上的工作并没有并入工作分支，所以可以通过使用`git merge master`命令将 master 分支合并入工作分支，或者等工作分支完成使命后，再将其合并会 master 分支。

分支的合并。如果开发历史从一个更早的地方开始分叉开来（diverged），即 master 分支所在提交并不是工作分支所在提交的直接祖先，Git 不得不做一些额外的工作。出现这种情况的时候，Git 会使用两个分支的末端所指的快照以及这两个分支的工作祖先，做一个简单的三方合并。

和之前将分支指针向前推进所不同的是，Git 将此次三方合并的结果做了一个新的快照并且自动创建一个新的提交指向它。这个被称作一次合并提交，它的特别之处在于它有不止一个父提交。

需要指出的是，Git 会自动决定选取哪一个提交作为最优的共同祖先，并以此作为合并的基础；这和更加古老的 CVS 系统或者 Subversion（1.5版本之前）不同，在这些古老的版本管理系统中，用户需要自己选择最佳的合并基础。Git 的这个优势使其在合并操作上比其他系统要简单很多。

遇到冲突时的分支合并。有时候合并操作并不会如此顺利。如果在两个不同的分支中，对同一个文件的同一个部分进行了不同的修改，Git 就没法干净的合并它们。

```shell
$ git merge iss53
Auto-merging index.html
CONFLICT (content): Merge conflict in index.html
Automatic merge failed; fix conflicts and then commit the result.
```

此时 Git 做了合并，但是没有自动地创建一个新的合并提交。Git 会暂停下来，等待解决合并产生的冲突。可以在合并冲突后的任意时刻使用`git status`命令来查看那些因包含合并冲突而处于未合并（unmerged）状态的文件。

```shell
$ git status
On branch master
You have unmerged paths.
  (fix conflicts and run "git commit")

Unmerged paths:
  (use "git add <file>..." to mark resolution)

    both modified:      index.html

no changes added to commit (use "git add" and/or "git commit -a")
```

任何因包含合并冲突而有待解决的文件，都会以未合并状态标识出来。Git 会在有冲突的文件中加入标准的冲突解决标记，这样就可以打开这些包含冲突的文件然后手动解决冲突。出现冲突的文件会包含一些特殊区段，看起来像下面这个样子。

```html
<div id="footer">
 please contact us at support@github.com
</div>
```

这表示 HEAD 所指示的版本在这个区段的上半部分（======= 的上半部分），而 iss53 分支所指示的版本在 ======= 的下半部分。为了解决冲突，必须选择使用由 ====== 分割的两部分中的一个，或者也可以自行合并这些内容。并且将 <<<<<<< , ======= , 和 >>>>>>> 这些行完全删除了。在你解决了所有文件里的冲突之后，对每个文件使用`git add`命令来将其标记为冲突已解决。 一旦暂存这些原本有冲突的文件，Git 就会将它们标记为冲突已解决。

如果你想使用图形化工具来解决冲突，你可以运行`git mergetool`，该命令会为你启动一个合适的可视化合并工具，并带领你一步一步解决这些冲突。

如果你对结果感到满意，并且确定之前有冲突的的文件都已经暂存了，这时你可以输入`git commit`来完成合并提交。

## 3.3 分支管理

`git branch`命令不只是可以创建与删除分支。如果不加任何参数运行它，会得到当前所有分支的一个列表。如果分支前出现`*`字符，代表 HEAD 指针所指向的分支。这意味着如果在这时候提交，该分支将会随着新的工作向前移动。如果需要查看每一个分支的最后一次提交，可以运行`git branch -v`命令。

```shell
$ git branch -v
  iss53   93b412c fix javascript issue
* master  7a98805 Merge branch 'iss53'
  testing 782fd34 add scott to the author list in the readmes
```

`--merged`与`--no-merger`这两个有用的选项可以过滤这个列表中已经合并或尚未合并到当前分支的分支。如果要查看哪些分支已经合并到当前当前分支，可以运行`git branch --merged`，在结果列表中分支名字前没有`*`号的分支通常可以使用`git branch -d`删除掉，因为已经将它们的工作整合到了另一个分支，所以并不会失去任何东西。但如果没有合并，删除命令会失效。如果真的想要删除该分支并丢掉哪些工作，必须使用`-D`选项强制删除它。

## 3.4 分支开发工作流

长期分支。在整个项目开发周期的不同阶段，可以同时拥有多个开放的分支；可以定期地把某些特性分支合并入其他分支中。只在 master 分支上保留完全稳定的代码——有可能仅仅是已经发布或即将发布的代码，通常还有些名为 develop 或者 next 的平行分支，被用来做后续开发或者测试稳定性——这些分支不必保持绝对稳定，但是一旦达到稳定状态，它们就可以被合并入 master 分支了。这样，在确保这些已完成的特性分支能够通过所有测试，并且不会引入更多 bug 之后，就可以合并入主分支中，等待下一次的发布。

同时可以采用以下方法维护不同层次的稳定性。一些大型项目还有一个 propsed（建议）或者 pu:propsed updates（建议更新）分支，它可能因包含一些不成熟的内容而不能进入 next 或者 master 分支。这么做的目的是使分支具有不同级别的稳定性。

特性分支。特性分支对任何规模的项目都适用。特性分支是一种短期分支，它被用来实现单一特性或其相关工作。在其他版本控制系统中创建和合并分支通常很费劲，但是在 Git 中一天之内多次创建、使用、合并、删除分支都很常见。

在特性分支中提交了更新，在合并入主干分支后就可以删除它们。这项技术可以快速并且完整地进行上下文切换，因为工作被分散到不同的流水线中，在不同的流水线中每个分支都仅与其目标特性相关。在做代码审查之类的工作就能更加容易地看出做了哪些改动，可以把改动在特性分支中保留几分钟、几天或者几个月，等它们成熟之后再合并，而不用在乎它们建立的顺序或工作进度。

## 3.5 远程分支

远程引用是对远程仓库的引用（指针），包括分支、标签等等，可以通过`git ls-remote(remote)`来显式地获得远程引用的完整列表，或者通过`git remote show(remote)`或者远程分支的更多信息。然而，一个更常见的做法是利用远程跟踪分支。

远程跟踪分支是远程分支状态的引用。它们是不能移动的本地引用，当做任何网络通信操作时，它们会自动移动。远程跟踪分支像连接远程仓库时，那些分支所处状态的书签。它们以 (remote)/(branch) 形式命名。 例如，如果想要看你最后一次与远程仓库 origin 通信时 master 分支的状态，可以查看 origin/master 分支。 与同事合作解决一个问题并且他们推送了一个 iss53 分支，可能有自己的本地 iss53 分支；但是在服务器上的分支会指向 origin/iss53 的提交。

运行`git remote add`命令添加一个新的远程仓库引用到当前的项目。如果要同步工作，运行`git fetch origin`命令。这个命令查找"origin"是哪一个服务器，从中抓取本地没有的数据，并且更新本地数据库，移动 origin/master 指针指向新的、更新后的位置。

推送。当想要公开分享一个分支时，需要将其推送到有写入权限的远程仓库上。本地的分支并不会自动与远程仓库同步，必须显示地推动想要分享的分支。这样，就可以把不愿意分享的内容放到私人分支上，而将需要和别人协作的内容推送到公开分支。运行`git push (remote) (branch)`，意味着推送本地的分支来更新远程仓库的分支（也可以通过这种格式来推送本地分支到一个命名不相同的远程分支。运行`git push origin serverfix:awesomebranch`来将本地的 serverfix 分支推送到远程仓库上的 awesomebranch 分支）。下一次其他协作者从服务器上抓取数据时，会在本地生成一个远程分支 origin/serverfix，指向服务器的 serverfix 分支的引用。

要特别注意的一点是当抓取新的远程跟踪分支时，本地不会自动生成一份可编辑的副本。换一句话说，这种情况狂下，不会有一个新的 serverfix 分支，只有一个不可以修改的 origin/serverfix 指针。

可以运行`git merge origin/serverfix`将这些工作合并到当前所在的分支。 如果想要在自己的 serverfix 分支上工作，可以将其建立在远程跟踪分支之上。这会给你一个用于工作的本地分支，并且起点位于 origin/serverfix。

```shell
$ git checkout -b serverfix origin/serverfix
Branch serverfix set up to track remote branch serverfix from origin.
Switched to a new branch 'serverfix'
```

跟踪分支。从一个远程跟踪分支检出一个本地分支会自动创建一个叫做“跟踪分支”（有时候也叫做“上游分支”）。跟踪分支是与远程分支有直接关系的本地分支。如果在一个跟踪分支上输入`git pull`，Git 能自动地识别去哪个服务器上抓取、合并到哪个分支。

当克隆一个仓库时，它通常会自动地创建一个跟踪 origin/master 的 master 分支。然而，如果想要设置其他的跟踪分支、其他远程仓库上的跟踪分支，或者不跟踪 master 分支。最简单的就是运行`git checkout -b [branch] [remotename]/[branch]`，这是一个十分常用的操作所以 Git 提供了`--track`快捷方式。

```shell
$ git checkout --track origin/serverfix
Branch serverfix set up to track remote branch serverfix from origin.
Switched to a new branch 'serverfix'
```

如果想要将本地分支与远程分支设置为不同名字，你可以轻松地增加一个不同名字的本地分支的上一个命令。运行之后，本地分支 sf 自动从 origin/severfix 拉取。

```shell
$ git checkout -b sf origin/serverfix
Branch sf set up to track remote branch serverfix from origin.
Switched to a new branch 'sf'
```

设置已有的本地分支跟踪一个刚刚拉取下来的远程分支，或者想要修改正在跟踪的上游分支，你可以在任意时间使用`-u`或`--set-upstream-to`选项运行`git branch`来显式地设置。

```shell
$ git branch -u origin/serverfix
Branch serverfix set up to track remote branch serverfix from origin.
```

上游快捷方式。当设置好跟踪分支后，可以通过`@{upstream}`或`@{u}`快捷方式来引用它。 所以在 master 分支时并且它正在跟踪 origin/master 时，如果愿意的话可以使用`git merge @{u}`来取代`git merge origin/master`。

如果想要查看设置的所有跟踪分支，可以使用`git branch`的`-vv`选项。 这会将所有的本地分支列出来并且包含更多的信息，如每一个分支正在跟踪哪个远程分支与本地分支是否是领先、落后或是都有。需要重点注意的是这些数字的值来自于从每个服务器上最后一次抓取的数据。这个命令并没有连接服务器，它只会告诉关于本地缓存的服务器数据。如果想要统计最新的领先与落后数字，需要在运行此命令前抓取所有的远程仓库。可以想这样做：`$ git fetch --all; git branch -vv`。

```shell
$ git branch -vv
  iss53     7e424c3 [origin/iss53: ahead 2] forgot the brackets
  master    1ae2a45 [origin/master] deploying index fix
* serverfix f8674d9 [teamone/server-fix-good: ahead 3, behind 1] this should do it
  testing   5ea463a trying something new
```

拉取。当`git fetch`命令从服务器上抓取本地没有的数据时，它并不会修改工作目录中的内容。它只会获取数据然后让你自己合并。然而，有一个命令叫做`git pull`在大多数情况下它的含义是一个`git fetch`紧接着一个`git merge`命令。不管是显式地设置还是通过 clone 或 checkout 命令创建的跟踪分支，`git pull`都会查找当前分支所跟踪的服务器与分支，从服务器上抓取数据然后尝试合并入那个远程分支。

删除远程分支。可以运行带有`--delete`选项的`git push`命令来删除一个远程分支。基本上这个命令做的只是从服务器上移除这个指针。Git 服务器通常会保留数据一段时间直到垃圾回收运行，所以如果不小心删除掉了，通常是很容易恢复的。

```shell
$ git push origin --delete serverfix
To https://github.com/schacon/simplegit
 - [deleted]         serverfix
```

## 3.6 变基

在 Git 中整合来自不同分支的修改主要有两种方法：merge 以及 rebase。

变基的基本操作。整个分支最容易的方法时 merge 命令。它会把两个分支的最新快照以及两者最近的共同祖先进行三方合并，合并的结果是生成一个新的快照（并提交）。但是还有一种方法，可以提取一个分支中引入的补丁和修改，然后在原来的基础上应用一次。在 Git 中，这种操作就叫做变基。可以使用 rebase 命令将提交到某一分支上的所有修改都移至另一分支上，就好像“重新播放”一样。

它的原理是首先找到这两个分支的最近共同祖先，然后对比当前分支相对于该祖先的历次提交，提取相应的修改并保存为临时文件，然后将当前分支指向目标基底，最后以此将之前另存为临时文件的修改依序应用。然后可以回到 master 分支，进行以此快速合并。

一般我们这样做的目的是为了确保在向远程分支推送时能保持提交历史的整洁——例如向某个其他人维护的项目贡献代码时。在这种情况下，首先在自己的分支进行开发，当开发完成时需要先将代码变基到 origin/master 上，然后再向主项目提交修改。这样的话，该项目的维护者就不需要进行整合工作，只需快速合并即可。

请注意，无论是通过变基，还是通过三方合并，整合的最终结果所指向的快照始终是一样的，只不过提交历史不同罢了。变基是将一系列提交按照原有次序依次应用到另一分支上，而合并是把最终结果合在一起。

更有趣的变基例子。在对两个分支进行变基时，所生成的“重放”并不一定在目标分支上应用，也可以指定另一个分支进行应用。使用`git rebase`命令的`--onto`选项，选中在 client 分支里但不在 server 分支里的修改，将它们在 master 分支上重放。然后进行快速合并。

```shell
$ git rebase --onto master server client
$ git checkout master
$ git merge client
```

接下来可以将 server 分支中的修改也整合进来。 使用`git rebase [basebranch] [topicbranch]`命令可以直接将特性分支（即本例中的 server）变基到目标分支（即 master）上。这样做能省去先切换到 server 分支，再对其执行变基命令的多个步骤。然后就可以快进合并主分支 master 了。

至此，client 和 server 分支中的修改都已经整合到主分支里了，可以删除这两个分支。

变基的风险。不要对仓库外有副本的分支执行变基。变基操作的实质是丢弃一些现有的提交，然后相应地新建一些内容一样但实际上不同的提交。如果已经将提交推送至某个仓库，而其他人也已经从该仓库拉取提交并进行了后续工作。此时，如果用`git rebase`命令重新整理了提交并再次推送，同伴因此将不得不再次将他们手头的工作与自己的提交进行整合，如果接下来还要拉取并整合他们修改过的提交，事情就会变得一团糟。

只要你把变基命令当作是在推送前清理提交使之整洁的工具，并且只在从未推送至共用仓库的提交上执行变基命令，就不会有事。 假如在那些已经被推送至共用仓库的提交上执行变基命令，并因此丢弃了一些别人的开发所基于的提交，那你就有大麻烦了。

如果在某些情形下决意要这么做，请一定要通知每个人执行`git pull --rebase`命令，这样尽管不能避免伤痛，但能有所缓解。

# 第四章 服务器上的Git

尽管在技术上你可以从个人仓库进行推送（push）和拉取（pull）来修改内容，但不鼓励使用这种方法，因为一不留心就很容易弄混其他人的进度。 此外，你希望你的合作者们即使在你的电脑未联机时亦能存取仓库 — 拥有一个更可靠的公用仓库十分有用。 因此，与他人合作的最佳方法即是建立一个你与合作者们都有权利访问，且可从那里推送和拉取资料的共用仓库。

一个远程仓库通常只是一个裸仓库（_bare repository_）— 即一个没有当前工作目录的仓库。 因为该仓库仅仅作为合作媒介，不需要从磁碟检查快照；存放的只有 Git 的资料。 简单的说，裸仓库就是你工程目录内的 `.git` 子目录内容，不包含其他资料。

## 4.1 协议

Git 可以使用四种主要的协议来传输资料：本地协议（Local），HTTP 协议，SSH（Secure Shell）协议及 Git 协议。

本地协议。

最基本的就是 _本地协议（Local protocol）_ ，其中的远程版本库就是硬盘内的另一个目录。 这常见于团队每一个成员都对一个共享的文件系统（例如一个挂载的 NFS）拥有访问权，或者比较少见的多人共用同一台电脑的情况。 后者并不理想，因为你的所有代码版本库如果长存于同一台电脑，更可能发生灾难性的损失。

如果你使用共享文件系统，就可以从本地版本库克隆（clone）、推送（push）以及拉取（pull）。 像这样去克隆一个版本库或者增加一个远程到现有的项目中，使用版本库路径作为 URL。

如果在 URL 开头明确的指定 `file://`，那么 Git 的行为会略有不同。 如果仅是指定路径，Git 会尝试使用硬链接（hard link）或直接复制所需要的文件。 如果指定 `file://`，Git 会触发平时用于网路传输资料的进程，那通常是传输效率较低的方法。

要增加一个本地版本库到现有的 Git 项目，可以执行 `git remote add`命令，这样就可以像在网络上一样从远端版本库推送和拉取更新了。

```shell
$ git clone /opt/git/project.git

$ git clone file:///opt/git/project.git

$ git remote add local_proj /opt/git/project.git
```

基于文件系统的版本库的优点是简单，并且直接使用了现有的文件权限和网络访问权限。这种方法的缺点是，通常共享文件系统比较难配置，并且比起基本的网络连接访问，这不方便从多个位置访问。

HTTP 协议。

Git 通过 HTTP 通信有两种模式。 在 Git 1.6.6 版本之前只有一个方式可用，十分简单并且通常是只读模式的。 Git 1.6.6 版本引入了一种新的、更智能的协议，让 Git 可以像通过 SSH 那样智能的协商和传输数据。 之后几年，这个新的 HTTP 协议因为其简单、智能变的十分流行。 新版本的 HTTP 协议一般被称为“智能” HTTP 协议，旧版本的一般被称为“哑” HTTP 协议。

“智能” HTTP 协议的运行方式和 SSH 及 Git 协议类似，只是运行在标准的 HTTP/S 端口上并且可以使用各种 HTTP 验证机制，这意味着使用起来会比 SSH 协议简单的多，比如可以使用 HTTP 协议的用户名／密码的基础授权，免去设置 SSH 公钥。

智能 HTTP 协议或许已经是最流行的使用 Git 的方式了，它即支持像 `git://` 协议一样设置匿名服务，也可以像 SSH 协议一样提供传输时的授权和加密。 而且只用一个 URL 就可以都做到，省去了为不同的需求设置不同的 URL。 如果你要推送到一个需要授权的服务器上（一般来讲都需要），服务器会提示你输入用户名和密码。 从服务器获取数据时也一样。

如果服务器没有提供智能 HTTP 协议的服务，Git 客户端会尝试使用更简单的“哑” HTTP 协议。 哑 HTTP 协议里 web 服务器仅把裸版本库当作普通文件来对待，提供文件服务。 哑 HTTP 协议的优美之处在于设置起来简单。 基本上，只需要把一个裸版本库放在 HTTP 根目录，设置一个叫做 `post-update` 的挂钩就可以了。 此时，只要能访问 web 服务器上你的版本库，就可以克隆你的版本库。

不同的访问方式只需要一个 URL 以及服务器只在需要授权时提示输入授权信息，这两个简便性让终端用户使用 Git 变得非常简单。 相比 SSH 协议，可以使用用户名／密码授权是一个很大的优势，这样用户就不必须在使用 Git 之前先在本地生成 SSH 密钥对再把公钥上传到服务器。你也可以在 HTTPS 协议上提供只读版本库的服务，如此你在传输数据的时候就可以加密数据；或者，你甚至可以让客户端使用指定的 SSL 证书。另一个好处是 HTTP/S 协议被广泛使用，一般的企业防火墙都会允许这些端口的数据通过。

在一些服务器上，架设 HTTP/S 协议的服务端会比 SSH 协议的棘手一些。如果在 HTTP 上使用需授权的推送，管理凭证会比使用 SSH 密钥认证麻烦一些。然而，可以选择使用凭证存储工具，比如 OSX 的 Keychain 或者 Windows 的凭证管理器。

SSH 协议。

架设 Git 服务器时常用 SSH 协议作为传输协议。 因为大多数环境下已经支持通过 SSH 访问 —— 即时没有也比较很容易架设。 SSH 协议也是一个验证授权的网络协议；并且，因为其普遍性，架设和使用都很容易。

通过 SSH 协议克隆版本库，你可以指定一个 `ssh://` 的 URL ，或者使用一个简短的 scp 式的写法。你也可以不指定用户，Git 会使用当前登录的用户名。

```shell
$ git clone ssh://user@server/project.git

$ git clone user@server:project.git
```

用 SSH 协议的优势有很多。 首先，SSH 架设相对简单 —— SSH 守护进程很常见，多数管理员都有使用经验，并且多数操作系统都包含了它及相关的管理工具。 其次，通过 SSH 访问是安全的 —— 所有传输数据都要经过授权和加密。 最后，与 HTTP/S 协议、Git 协议及本地协议一样，SSH 协议很高效，在传输前也会尽量压缩数据。

SSH 协议的缺点在于你不能通过他实现匿名访问。 即便只要读取数据，使用者也要有通过 SSH 访问你的主机的权限，这使得 SSH 协议不利于开源的项目。

Git 协议。

接下来是 Git 协议。 这是包含在 Git 里的一个特殊的守护进程；它监听在一个特定的端口（9418），类似于 SSH 服务，但是访问无需任何授权。 要让版本库支持 Git 协议，需要先创建一个 `git-daemon-export-ok` 文件 —— 它是 Git 协议守护进程为这个版本库提供服务的必要条件 —— 但是除此之外没有任何安全措施。 要么谁都可以克隆这个版本库，要么谁也不能。 这意味着，通常不能通过 Git 协议推送。 由于没有授权机制，一旦你开放推送操作，意味着网络上知道这个项目 URL 的人都可以向项目推送数据。 不用说，极少会有人这么做。

目前，Git 协议是 Git 使用的网络传输协议里最快的。 如果你的项目有很大的访问量，或者你的项目很庞大并且不需要为写进行用户授权，架设 Git 守护进程来提供服务是不错的选择。 它使用与 SSH 相同的数据传输机制，但是省去了加密和授权的开销。

Git 协议缺点是缺乏授权机制。 把 Git 协议作为访问项目版本库的唯一手段是不可取的。 一般的做法里，会同时提供 SSH 或者 HTTPS 协议的访问服务，只让少数几个开发者有推送（写）权限，其他人通过 `git://` 访问只有读权限。 Git 协议也许也是最难架设的。 它要求有自己的守护进程，这就要配置 `xinetd`或者其他的程序，这些工作并不简单。 它还要求防火墙开放 9418 端口，但是企业防火墙一般不会开放这个非标准端口。 而大型的企业防火墙通常会封锁这个端口。

## 4.2 在服务器上搭建 Git

在开始架设 GIt 服务器前，需要把现有仓库导出为裸仓库——即一个不包含当前工作目录的仓库。为了通过克隆的仓库来创建一个新的裸仓库，在克隆命令后加上`--bare`选项，按照惯例，裸仓库目录以`.git`结尾。

```shell
$ git clone --bare my_project my_project.git
Cloning into bare repository 'my_project.git'...
done.


$ cp -Rf my_project/.git my_project.git
```

把裸仓库放到服务器上。有了裸仓库的副本，剩下要做的是把裸仓库放到服务器上并设置协议。假设一个域名为`git.example.com` 的服务器已经架设好，并可以通过 SSH 链接，希望将所有的 Git 仓库放在 `/opt/git` 目录下。 假设服务器上存在 `/opt/git/` 目录，可以通过以下命令复制裸仓库来创建一个新仓库。

```shell
$ scp -r my_project.git user@git.example.com:/opt/git
```

此时，其他通过 SSH 连接这台服务器并对`/opt/git` 目录拥有可读权限的使用者，通过运行以下命令就可以克隆仓库。

```shell
$ git clone user@git.example.com:/opt/git/my_project.git
```

如果一个用户，通过使用 SSH 连接到一个服务器，并且其对 `/opt/git/my_project.git` 目录拥有可写权限，那么他将自动拥有推送权限。

如果到该项目目录中运行 `git init` 命令，并加上 `--shared` 选项，那么 Git 会自动修改该仓库目录的组权限为可写。

```shell
$ ssh user@git.example.com
$ cd /opt/git/my_project.git
$ git init --bare --shared
```

小型安装。如果设备较少或者你只想在小型开发团队里尝试 Git ，那么一切都很简单。架设 Git 服务最复杂的地方在于用户管理。 如果需要仓库对特定的用户可读，而给另一部分用户读写权限，那么访问和许可安排就会比较困难。

SSH 连接。如果需要团队里的每个人都对仓库有写权限，又不能给每个人在服务器上建立账户，那么提供 SSH 连接就是唯一的选择了。 我们假设用来共享仓库的服务器已经安装了 SSH 服务，而且你通过它访问服务器。

有几个方法可以使你给团队每个成员提供访问权。

第一个就是给团队里的每个人创建账号，这种方法很直接但也很麻烦。 或许你不会想要为每个人运行一次 `adduser` 并且设置临时密码。

第二个办法是在主机上建立一个 _git_ 账户，让每个需要写权限的人发送一个 SSH 公钥，然后将其加入 git 账户的 `~/.ssh/authorized_keys` 文件。 这样一来，所有人都将通过 _git_ 账户访问主机。 这一点也不会影响提交的数据——访问主机用的身份不会影响提交对象的提交者信息。

另一个办法是让 SSH 服务器通过某个 LDAP 服务，或者其他已经设定好的集中授权机制，来进行授权。 只要每个用户可以获得主机的 shell 访问权限，任何 SSH 授权机制你都可视为是有效的。

## 4.3 生成 SSH 公钥

许多 Git 服务器都使用 SSH 公钥进行认证。为了向 Git 服务器提供 SSH 公钥，如果某系统用户尚未拥有密钥，必须事先为其生成一份。这个过程在所有操作系统上都是相似的。首先，需要确认自己是否已经拥有密钥。默认情况下，用户的 SSH 密钥存储在其`~/.ssh`目录下。进入该目录并列出其中内容，便可以快速确认自己是否拥有密钥。

```shell
$ cd ~/.ssh
$ ls
authorized_keys2  id_dsa       known_hosts
config            id_dsa.pub
```

需要寻找一对以 id_dsa 或 id_rsa 命名的文件，其中一个带有 .pub 扩展名，代表公钥，另一个则是私钥。如果找不到这样的文件（或者根本没有 .ssh 目录），可以通过运行 ssh-keygen 程序来创建它们。在 Linux/Mac 系统中，ssh-keygen 随 SSH 软件包提供；在 Windows 上，该程序包含于 MSysGit 软件包中。

首先 ssh-keygen 会确认密钥的存储位置（默认是 .ssh/id_rsa），然后要求输入两次密钥口令。如果不想在使用密钥时输入口令，将其留空即可。

现在，进行了上述操作的用户需要将各自的密钥发送给任意一个 Git 服务器管理员，如果服务器正在使用基于公钥的 SSH 验证设置。

```shell
$ cat ~/.ssh/id_rsa.pub
ssh-rsa AAAAB3NzaC1yc2EAAAABIwAAAQEAklOUpkDHrfHY17SbrmTIpNLTGK9Tjom/BWDSU
GPl+nafzlHDTYW7hdI4yZ5ew18JH4JW9jbhUFrviQzM7xlELEVf4h9lFX5QVkbPppSwg0cda3
Pbv7kOdJ/MTyBlWXFCR+HAo3FXRitBqxiX1nKhXpHAZsMciLq8V6RjsNAQwdsdMFvSlVK/7XA
t3FaoJoAsncM1Q9x5+3V0Ww68/eIFmb1zuUFljQJKprrX88XypNDvjYNby6vw/Pb0rwert/En
mZ+AW4OZPnTPI89ZPmVMLuayrD2cE86Z/il8b+gw3r3+1nKatmIkjn2so1d01QraTlMqVSsbx
NrRFi9wrf+M7Q== schacon@mylaptop.local
```

## 4.4 配置服务器

配置服务器端的 SSH 访问。操作系统是标准的 Linux 发行版，比如 Ubuntu，使用`authorized_keys`方法对用户进行认证。首先，创建一个操作系统用户 git，并为其建立一个 .ssh 目录。

接着，我们需要为系统用户 git 的`authorized_keys`文件添加一些开发者 SSH 公钥。假设我们已经获得了若干受信任的公钥，并将它们保存在临时文件中。将这些公钥加入系统用户 git 的 .ssh 目录下`authorized_keys`文件的末尾。

```shell
$ cat /tmp/id_rsa.john.pub >> ~/.ssh/authorized_keys
$ cat /tmp/id_rsa.josie.pub >> ~/.ssh/authorized_keys
$ cat /tmp/id_rsa.jessica.pub >> ~/.ssh/authorized_keys
```

现在我们来为开发者新建一个空仓库。可以借助带`--bare`选项的`git init`命令来做到这一点，该命令在初始化仓库时不会创建工作目录。

接着，开发者任意一人可以将他们项目的最初版本推送到这个仓库中，他只需将此仓库设置为项目的远程仓库并向其推送分支。请注意，每添加一个新项目，都需要有人登陆服务器取得 shell，并创建一个裸仓库。我们假定这个设置了 git 用户和 Git 仓库的服务器使用 gitserver 作为主机名。同时，假设该服务器运行在内网，并且你已在 DNS 配置中将 gitserver 指向此服务器。那么可以运行如下命令将已有项目 myproject 推送上去。

```shell
# on John's computer
$ cd myproject
$ git init
$ git add .
$ git commit -m 'initial commit'
$ git remote add origin git@gitserver:/opt/git/project.git
$ git push origin master
```

此时，其他开发者可以克隆此仓库，并推回各自的改动。

```shell
$ git clone git@gitserver:/opt/git/project.git
$ cd project
$ vim README
$ git commit -am 'fix for the README file'
$ git push origin master
```

通过这种方法，可以快速搭建一个具有读写权限、面向多个开发者的 Git 服务器。需要注意的是，目前所有（获得授权的）开发者用户都能以系统用户 git 的身份登录服务器从而获得一个普通的 shell。如果想对此加以限制，则需要修改 passwd 文件中（git 用户所对应）的 shell 值。

借助一个名为 git-shell 的受限 shell 工具，可以方便地将用户 git 的活动限制在与 Git 相关的范围内。该工具随 Git 软件包一同提供。如果将 git-shell 设置为用户 git 的登录 shell，那么用户便不能获得此服务器的普通 shell 访问权限。若要使用 git-shell，需要用它替换掉 bash 或 csh，使其成为系统用户的登录 shell。为了进行上述操作，首先必须确保 git-shell 已存在于 /etc/shells 文件中。那么可以使用`chsh<username>`命令修改任一系统用户的 shell。

```shell
$ sudo chsh git
# and enter the path to git-shell, usually: /usr/bin/git-shell
```

这样，用户 git 就只能使用 SSH 连接对 Git 仓库进行推送和拉取操作，而不能登录机器并取得普通 shell。如果试图登录，就会发现尝试被拒绝。

## 4.5 Git守护进程

对于快速且无需授权的 Git 数据访问，通过“GIt”协议建立一个基于守护进程的仓库时理想之选。但是，因为其不包含授权服务，任何通过该协议管理的内容将在其网络上公开。

如果运行在防火墙之外的服务器上，它应该只对那些公开的只读项目服务。如果运行在防火墙之内的服务器上，它可用于支撑大量参与人员或自动系统（用于持续集成或编译的主机）只读访问的项目，这样可以省去逐一配置 SSH 公钥的麻烦。

通常，只需要以守护进程的形式运行命令，`--reuseaddr`允许服务器在无需等待旧连接超时的情况下重启，`--base-path`选项允许用户在未完全指定路径的条件下克隆项目，结尾的路径将告诉 Git 守护进程从何处寻找仓库来导出。如果有防火墙正在运行，需要开放端口9418的通信权限。

```shell
git daemon --reuseaddr --base-path=/opt/git/ /opt/git/
```

出于安全考虑，强烈建议使用一个对仓库拥有只读权限的用户身份来运行该守护进程 - 你可以创建一个新用户 git-ro 并且以该用户身份来运行守护进程。 为简便起见，我们将像 git-shell 一样，同样使用 git 用户来运行它。

当重启机器时，Git 守护进程将会自动启动，并且如果进程被意外结束它会自动重新运行。 为了在不重启的情况下直接运行，你可以运行以下命令：

```shell
initctl start local-git-daemon
```

在其他系统中，可以使用 sysvinit 系统中的 xinetd 脚本，或者另外的方式来实现，只要你能够将其命令守护进程化并实现监控。

接下来，需要告诉 Git 哪些仓库允许基于服务器的无授权访问。 可以在每个仓库下创建一个名为`git-daemon-export-ok`的文件来实现。

```shell
$ cd /path/to/project.git
$ touch git-daemon-export-ok
```

该文件将允许 Git 提供无需授权的项目访问服务。

# 第六章 GitHub

GitHub 是最大的 Git 版本库托管商，是成千上万的开发者和项目能够合作进行的中心。大部分 Git 版本库都托管在 GitHub，许多开源项目使用 GItHub 实现 Git 托管、问题追踪、代码审查以及其它事情。

## 6.1 账号的创建和配置

GitHub 为免费账户提供了完整功能，限制是你的项目都将被完全公开（每个人都具有读权限）。GitHub 的付费计划可以让你拥有一定数目的私有项目。

如果仅仅克隆公有项目，你甚至不需要注册——刚刚我们创建的账户是为了以后 fork 其它项目，以及推送我们自己的修改。

点击“Add an SSH key”按钮，给你的公钥起一个名字，将你的`~/.ssh/id_rsa.pub`（或者自定义的其它名字）公钥文件的内容粘贴到文本区，然后点击`‘Add key’'。

GitHub 使用用户邮件地址区分 Git 提交。如果在自己的提交中使用了多个邮件地址，希望 GitHub 可以正确地将它们连接起来，需要在管理页面的 Emails 部分添加你拥有的所有邮箱地址。

在添加邮件地址中我们可以看到一些不同的状态。顶部的地址是通过验证的，并且被设置为主要地址，这意味着该地址会接收到所有的通知和回复。第二个地址是通过验证的，如果愿意的话，可以将其设置为主要地址。 最后一个地址是未通过验证的，这意味着你不能将其设置为主要地址。 当 GitHub 发现任意版本库中的任意提交信息包含了这些地址，它就会将其链接到你的账户。

最后，为了额外的安全性，你绝对应当设置两步验证，简写为 “2FA”。 两步验证是一种用于降低因你的密码被盗而带来的账户风险的验证机制，现在已经变得越来越流行。开启两步验证，GitHub 会要求你用两种不同的验证方法，这样，即使其中一个被攻破，攻击者也不能访问你的账户。

## 6.2 对项目做出贡献

派生（Fork）项目。如果想要参与某个项目，但是并没有推送权限，这时可以对这个项目进行“派生”。派生的意思是指，GitHub 将在你的空间中创建一个完全属于你的项目副本，且你对其具有推送权限。

通过这种方式，项目的管理者不再需要忙着把用户添加到贡献者列表并给予他们推送权限。人们可以派生这个项目，将修改推送到派生出的项目副本中，并通过创建合并请求（Pull Request）来让他们的改动进入源版本库。创建了合并请求后，就会开启一个可供审查代码的板块，项目的拥有者和贡献者可以在此讨论相关修改，直到项目拥有者对其感到满意，并且认为这些修改可以被合并到版本库。

GitHub 流程。GitHub 设计了一个以合并请求为中心的特殊合作流程。它基于 Git 分支的特性分支提到的工作流程。不管是在一个紧密的团队中使用单独的版本库，或者使用许多的“Fork”来为一个由陌生人组成的国际企业或网络做出贡献，这种合作流程都能应付。流程通常如下。

- 从 master 分支中创建一个新分支
- 提交一些修改来改进项目
- 将这个分支推送到 GitHub 上
- 创建一个合并请求
- 讨论，根据实际情况继续修改
- 项目的拥有者合并或关闭你的合并请求

创建合并请求。首先，单击“Fork”按钮来获得这个项目的副本。将它克隆到本地，创建一个分支，修改代码，最后再将改动推送到 GitHub。现在到 GitHub 上查看之前的项目副本，可以看到 GitHub 提示我们有新的分支，并且显示了一个大大的绿色按钮让我们可以检查我们的改动，并给源项目创建合并请求。如果点击了那个绿色按钮，就会看到一个新页面，在这里我们可以对改动填写标题和描述，让项目的拥有者考虑一下我们的改动。通常花点时间来编写个清晰有用的描述是个不错的主意，这能让作者明白为什么这个改动可以给他的项目带来好处，并且让他接受合并请求。当单击了“Create pull request”（创建合并请求）的按钮后，这个项目的拥有者将会收到一条包含关改动和合并请求页面的链接的提醒。

利用合并请求。现在，项目的拥有者可以看到你的改动并合并它，拒绝它或是发表评论。在这里我们就当作他喜欢这个点子，但是他想要让灯熄灭的时间比点亮的时间稍长一些。接下来可能会通过电子邮件进行互动。如果贡献者完成了修改并推送，项目的拥有者会再次收到提醒，当他们查看页面时，将会看到最新的改动。

合并请求的进阶用法。将合并请求制作成补丁，大多数的 GitHub 项目将合并请求的分支当作对改动的交流方式，并将变更集合起来统一进行合并；与上游保持同步，GitHub 会对每个提交进行测试，让你知道你的合并请求能否简洁的合并。

Markdown。在议题和合并请求的描述，评论和代码评论还有其他地方，都可以使用“GitHub 风格的Markdown”。Markdown 可以输入纯文本，但是渲染出丰富的内容。GitHub 风格的 Markdown 增加了一些基础的 Markdown 中做不到的东西。它在创建合并请求和议题中的评论和描述时十分有用。

可以这样创建一个任务列表。在合并请求中，任务列表经常被用来在合并之前展示这个分支将要完成的事情。最酷的地方就是，你只需要点击复选框，就能更新评论 —— 你不需要直接修改 Markdown。
```
- [X] 编写代码
- [ ] 编写所有测试程序
- [ ] 为代码编写文档
```

摘录代码。也可以在评论中摘录代码。这在想要展示尚未提交到分支中的代码时会十分有用。它也经常被用在展示无法正常工作的代码或这个合并请求需要的代码。

引用。如果在回复一个很长的评论之中的一小段，只需要复制你需要的片段，并在每行前添加 > 符号即可。事实上，因为这个功能会被经常用到，它也有一个快捷键。只要你把你要回应的文字选中，并按下 r 键，选中的问题会自动引用并填入评论框。

表情符号（EMOJI）。最后，我们可以在评论中使用表情符号。这经常出现在 GitHub 的议题和合并请求的评论中。GitHub 上甚至有表情助手。如果你在输入评论时以`:`开头，自动完成器会帮助你找到你需要的表情。也可以在评论的任何地方使用`:<表情名称>:`来添加表情符号。

图片。从技术层面来说，这并不是 GitHub 风格 Markdown 的功能，但是也很有用。如果不想使用 Markdown 语法来插入图片，GitHub 允许通过拖拽图片到文本区来插入图片。