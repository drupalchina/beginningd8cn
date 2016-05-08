# 使用 Git

本章提供关于 Git 的基本概述。 Git 是 Drupal 社区自 Drupal 7 开发时就采用的一个源代码控制系统。你也许会问，什么是源代码控制系统。其实源代码控制系统，就是能够帮助开发者管理文档，源代码以及其他信息集合变更的工具。比方说你之前修改过一个文档或一段代码，后来又希望它及时返回到原来的状态，撤销所有的变更，那么源代码控制系统可以挽回局面。在某一时刻对数字文档进行快照存储，并且在随后的时间里可以通过这个快照使文档恢复到之前的状态，是源代码控制系统的一个关键特性。

其他有关源代码控制系统的关键环节还包括：给开发者或系统分发变更的能力；采用单一基础代码库，并在此基础上为开发者们在不相互影响的情况下进行同步开发而创建副本（或分支，技术上更正确的术语），然后在某个时间点合并所有分支，并解决由于同一文件区块（例如，一行代码）被两人修改而引起的冲突问题的能力。

在市场存在着几个不同的源代码控制系统。 Drupal 社区选择并采用的是 Git。Linux 操作系统的创作者 Linus Torvalds 在他创作 Linux 的时候想要寻找一个真正开源、快速、功能强大还容易使用的（源代码控制系统）。就在这情况下，他创作了 Git。在本章节，我将会讲述一些 Git 的基础功能，你也许会你的 Drupal 8 网站上使用这些功能。

## 安装 Git


使用 Git 的第一步就是安装它。但是安装它之前，你可查看一下它是否已经安装过了。在终端窗口的命令提示行中键入`git`后按下<kbd>回车键</kbd>，如果你看到一系列 Git 命令，那恭喜你，你的 Git 早就安装好了。如果你看到的一些以 "command no found"（命令未找到）结尾的内容，那么是时候安装 Git 了。

## 在 Linux 上安装 Git

在 Linux 中安装 Git 只需一步简单的操作。 如果使用的系统是基于 Debian 的发行版，譬如说 Ubuntu，那么只需在终端窗口中输入`apt-get install git`。如果你使用的不是基于 Debian 的发行版，你可以使用 yum 来安装 Git，在终端窗口中输入 `yum install git-core`。 安装完成后，在命令提示符后键入`git`，按下<kbd>回车键</kbd>，你现在应该会看到一系列 Git 命令。 如果不是这样的话，访问 Git 的官方网站 http://git-scm.com 并查询帮助文档。

## 在 OS X 上安装 Git

在 OS X 上安装 Git 可以通过图形界面来完成。安装包在 http://sourceforge.net/projects/git-osx-installer 上获取。这个简单易用的工具提供一个快速的方法让你在你的 Mac 上安装 Git。下载这个 dmg 文件，点击启动这个安装工具，并跟随指引操作。一旦完成安装，打开一个终端窗口，在命令提示符后键入`git`，按下<kbd>回车键</kbd>，已确保 Git 已经安装好了。如果 Git 已经正确地安装了，你应该会看到一系列 Git 命令，如果不是这样的话，访问 http://git-scm.com 来寻找帮助。

## 在 Windows 上安装 Git

要在 Windows 上安装 Git，需要从 http://msygit.github.io 下载安装 Git 的 exe 文件。 这个Windows安装包会为你安装 Git 的工具，使你能够在终端窗口中运行 Git 并且提供图形界面工具来管理你的 Git 仓库。 安装完成之后，启动一个终端窗口，在命令行中键入 `git`，按下<kbd>回车键</kbd>。 你应该会看到一系列的 Git 命令。 如果你没看到一系列命令，访问 http://msygit.github.io 寻求帮助。

## 使用 Git

在你使用 Git 的过程中，会有一些基本的 Git 命令一直刺激你，让你逐渐喜欢上 Git 。使用 Git 的第一步是创建一个 Git 仓库。这个仓库里存放着你想要对它进行版本控制的所有东西。让我们使用安装完成的 Drupal 8 作为我们第一个 Git 项目，将它置于源代码控制状态下。使用终端，导航到你的 Drupal 8 项目的根目录下。 在终端窗口中，键入 `git init`，然后按下<kbd>回车键</kbd>。

------

**注意** 如果你已经为这个网站创建了一个 Git 仓库，那么 `git init` 命令会返回一条错误信息。

------

你应该会看到类似于此消息的内容：

Initialized empty Git repository in /Applications/MAMP/htdocs/drupal8/.git/

如果仓库未能成功创建，访问 Git 官方网站，寻找关于你所遇到的错误的帮助。

仓库创建完成之后，下一步需要进行的是添加元素到仓库里。既然我们还没有添加任何文件，我们将会把 Drupal 8 目录下的所有文件添加到 Git 仓库中。完成这个任务，只需键入一下命令：

```bash
git add -A .
```

确保你输入的命令最后有那个小点点，因为它代表着当前目录。如果你成功地将所有的文件添加到仓库里，你将会返回到命令提示符状态下，并且没有错误信息显示。当你键入 `git status`时并按下<kbd>回车键</kbd>，你应该会看到一个长长的，包含着新增到仓库里但还未提交的文件列表。

将刚刚添加的文件提交的操作给你提供一个快照。在未来当你遇到进行变更后需要恢复到之前状态的情况，你可以回滚到这个快照。 如何频繁添加和提交文件取决于你或者你的项目团队，关键点是只有那些已经添加并提交的文件才能够及时回滚到之前的状态。那么现在让我们使用下面的命令将 Drupal 8 的文件提交到我们的 Git 仓库中。

```bash
git commit –m "initial commit to the repository"
```

运行提交命令之后，你应该会看到一串关于新节点在你的 Git 仓库中创建的消息，每个提交的文件都会有一条消息。这时如果你运行`git status`命令，你应该会看到"everything is up to date"（所用文件都是最新的）：

<pre>
On branch master
nothing to commit, working directory clean
</pre>

进行到这里，我们已经完成文件提交，并且获得了将文件恢复到刚刚提交时的状态的能力。接下来要进行的是对文件进行一些变更，并提交这些变更。让我们对已有的文件做一些修改，并且添加一个新文件，来看看 Git 对这两种情况是如何作出响应的。首先，在你的 sites/default/files 目录下创建一个文档。 为了演示，我们新建一个名为 test.txt 的文件，里面包含几行信息，这样我们就能看到 Git 在运转。文件创建完成之后，运行`git status`来验证 Git 已经检测到这个新文件。你应该会看到类似的输出：

<pre>
# On branch master
# Untracked files:
# (use "git add <file>..." to include in what will be committed)
#
# test.txt
</pre>

So let’s follow the instruction and use git add test.txt to add the file to Git. After adding the file, use git status to check to see that the file was added. You should see output similar to

那么让我们跟随指引，使用`git add test.txt` 来将文件添加到 Git. 添加文件后，使用`git status`来看看文件是否已经被添加了。你应该会看到类似的输出：

<pre>
# On branch master
# Changes to be committed:
# (use "git reset HEAD <file>..." to unstage)
#
# new file: test.txt
#
</pre>




