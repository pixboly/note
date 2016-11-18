> Git使用笔记

# 三、Git分支

Url:http://www.open-open.com/lib/view/open1328069889514.html

​	几乎每一种版本控制系统都以某种形式支持分支。使用分支意味着你可以从开发主线上分离开来，然后在不影响主线的同时继续工作。在很多版本控制系统中，这是个昂贵的过程，常常需要创建一个源代码目录的完整副本，对大型项目来说会花费很长时间。
有人把 Git 的分支模型称为“必杀技特性”，而正是因为它，将 Git 从版本控制系统家族里区分出来。Git 有何特别之处呢？Git 的分支可谓是难以置信的轻量级，它的新建操作几乎可以在瞬间完成，并且在不同分支间切换起来也差不多一样快。和许多其他版本控制系统不同，Git 鼓励在工作流程中频繁使用分支与合并，哪怕一天之内进行许多次都没有关系。理解分支的概念并熟练运用后，你才会意识到为什么 Git 是一个如此强大而独特的工具，并从此真正改变你的开发方式。

## 1、何谓分支

为了理解Git分支的实现方式，我们需要回顾一下Git是如何储存数据的。或许你还记得第一章的内容，Git保存的不是文件差异或者变化量，而只是一系列文件快照。

在 Git中提交时，会保存一个提交（commit）对象，该对象包含一个指向暂存内容快照的指针，包含本次提交的作者等相关附属信息，包含零个或多个指向该提交对象的父对象指针：首次提交是没有直接祖先的，普通提交有一个祖先，由两个或多个分支合并产生的提交则有多个祖先。

为直观起见，我们假设在工作目录中有三个文件，准备将它们暂存后提交。暂存操作会对每一个文件计算校验和（即第一章中提到的SHA-1哈希字串），然后把当前版本的文件快照保存到Git仓库中（Git使用blob类型的对象存储这些快照），并将校验和加入暂存区域：

```shell
$ git add README test.rb LICENSE
$ git commit -m 'initial commit of my project'
```

当使用 git commit 新建一个提交对象前，Git 会先计算每一个子目录（本例中就是项目根目录）的校验和，然后在 Git 仓库中将这些目录保存为树（tree）对象。之后 Git 创建的提交对象，除了包含相关提交信息以外，还包含着指向这个树对象（项目根目录）的指针，如此它就可以在将来需要的时候，重现此次快照的内容了。

现在，Git 仓库中有五个对象：三个表示文件快照内容的 blob 对象；一个记录着目录树内容及其中各个文件对应 blob 对象索引的 tree 对象；以及一个包含指向 tree 对象（根目录）的索引和其他提交信息元数据的 commit 对象。概念上来说，仓库中的各个对象保存的数据和相互关系看起来如图 3-1 所示：

![]()

作些修改后再次提交，那么这次的提交对象会包含一个指向上次提交对象的指针（译注：即下图中的 parent 对象）。两次提交后，仓库历史会变成图 3-2 的样子：

![]()

现在来谈分支。Git 中的分支，其实本质上仅仅是个指向 commit 对象的可变指针。Git 会使用 master 作为分支的默认名字。在若干次提交后，你其实已经有了一个指向最后一次提交对象的 master 分支，它在每次提交的时候都会自动向前移动。

![]()

图 3-3. 分支其实就是从某个提交对象往回看的历史

那么，Git 又是如何创建一个新的分支的呢？答案很简单，创建一个新的分支指针。比如新建一个 testing 分支，可以使用

 git branch命令：

```shell
$ git branch testing
```

这会在当前 commit 对象上新建一个分支指针（见图 3-4）。

![]()

图 3-4. 多个分支指向提交数据的历史

那么，Git 是如何知道你当前在哪个分支上工作的呢？其实答案也很简单，它保存着一个名为 HEAD 的特别指针。请注意它和你熟知的许多其他版本控制系统（比如Subversion 或 CVS）里的 HEAD 概念大不相同。在 Git 中，它是一个指向你正在工作中的本地分支的指针（译注：将 HEAD 想象为当前分支的别名。）。运行git branch 命令，仅仅是建立了一个新的分支，但不会自动切换到这个分支中去，所以在这个例子中，我们依然还在 master 分支里工作（参考图 3-5）。

![]()

图 3-5.HEAD 指向当前所在的分支

要切换到其他分支，可以执行`gitcheckout`命令。我们现在转换到新建的 testing 分支：

```shell
$ git checkout testing
```

这样 HEAD 就指向了 testing 分支（见图3-6）。

![]()

图 3-6.HEAD 在你转换分支时指向新的分支

这样的实现方式会给我们带来什么好处呢？好吧，现在不妨再提交一次：

```Shell
$ vim test.rb
$ git commit -a -m 'made a change'
```

图 3-7 展示了提交后的结果。

![]()

图 3-7. 每次提交后 HEAD 随着分支一起向前移动

非常有趣，现在 testing 分支向前移动了一格，而master 分支仍然指向原先

 `gitcheckout`时所在的 commit 对象。现在我们回到 master 分支看看：

```Shell
$ git checkout master
```

图 3-8 显示了结果。

![]()

图 3-8.HEAD 在一次 checkout 之后移动到了另一个分支

这条命令做了两件事。它把 HEAD 指针移回到 master 分支，并把工作目录中的文件换成了 master 分支所指向的快照内容。也就是说，现在开始所做的改动，将始于本项目中一个较老的版本。它的主要作用是将 testing 分支里作出的修改暂时取消，这样你就可以向另一个方向进行开发。

我们作些修改后再次提交：

```Shell
$ vim test.rb
$ git commit -a -m 'made other changes'
```

现在我们的项目提交历史产生了分叉（如图 3-9 所示），因为刚才我们创建了一个分支，转换到其中进行了一些工作，然后又回到原来的主分支进行了另外一些工作。这些改变分别孤立在不同的分支里：我们可以
在不同分支里反复切换，并在时机成熟时把它们合并到一起。而所有这些工作，仅仅需要branch 和 checkout 这两条命令就可以完成。

![]()

图 3-9. 不同流向的分支历史

由于 Git 中的分支实际上仅是一个包含所指对象校验和（40 个字符长度 SHA-1 字串）的文件，所以创建和销毁一个分支就变得非常廉价。说白了，新建一个分支就是向一个文件写入 41 个字节（外加一个换行符）那么简单，当然也就很快了。

这和大多数版本控制系统形成了鲜明对比，它们管理分支大多采取备份所有项目文件到特定目录的方式，所以根据项目文件数量和大小不同，可能花费的时间 也会有相当大的差别，快则几秒，慢则数分钟。而 Git 的实现与项目复杂度无关，它永远可以在几毫秒的时间内完成分支的创建和切换。同时，因为每次提交时都记录了祖先信息（译注：即`parent`对象），将来要合并分支时，寻找恰当的合并基础（译注：即共同祖先）的工作其实已经自然而然地摆在那里了，所以实现起来非常容易。Git 鼓励开发者频繁使用分支，正是因为有着这些特性作保障。

接下来看看，我们为什么应该频繁使用分支。

## 2、分支的新建与合并

现在让我们来看一个简单的分支与合并的例子，实际工作中大体也会用到这样的工作流程：

1. 开发某个网站。 
2. 为实现某个新的需求，创建一个分支。 
3. 在这个分支上开展工作。

假设此时，你突然接到一个电话说有个很严重的问题需要紧急修补，那么可以按照下面的方式处理：

1. 返回到原先已经发布到生产服务器上的分支。 
2. 为这次紧急修补建立一个新分支，并在其中修复问题。 
3. 通过测试后，回到生产服务器所在的分支，将修补分支合并进来，然后再推送到生产服务器上。 
4. 切换到之前实现新需求的分支，继续工作。

### 2.1、分支的新建与切换

#### 2.1.1、创建分支

首先，我们假设你正在项目中愉快地工作，并且已经提交了几次更新（见图 3-10）。

![]()

图 3-10. 一个简短的提交历史
现在，你决定要修补问题追踪系统上的 #53 问题。顺带说明下，Git 并不同任何特定的问题追踪系统打交道。这里为了说明要解决的问题，才把新建的分支取名为 iss53。要新建并切换到该分支，运行git checkout 并加上-b 参数：

```shell
$ git checkout -b iss53
Switched to a new branch "iss53"
```

这相当于执行下面这两条命令：

```Shell
$ git branch iss53
$ git checkout iss53
```

图 3-11 示意该命令的执行结果。

![]()

图 3-11. 创建了一个新分支的指针
接着你开始尝试修复问题，在提交了若干次更新后，iss53 分支的指针也会随着向前推进，因为它就是当前分支（换句话说，当前的 HEAD 指针正指向 iss53，见图 3-12）

```Shell
$ vim index.html
$ git commit -a -m 'added a new footer [issue 53]'
```

![]()

图3-12. iss53 分支随工作进展向前推进

现在你就接到了那个网站问题的紧急电话，需要马上修补。有了 Git ，我们就不需要同时发布这个补丁和`iss53` 里作出的修改，也不需要在创建和发布该补丁到服务器之前花费大力气来复原这些修改。唯一需要的仅仅是切换回`master` 分支。

不过在此之前，留心你的暂存区或者工作目录里，那些还没有提交的修改，它会和你即将检出的分支产生冲突从而阻止 Git 为你切换分支。切换分支的时候最好保持一个清洁的工作区域。稍后会介绍几个绕过这种问题的办法（分别叫做 stashing 和 commit amending）。目前已经提交了所有的修改，所以接下来可以正常转换到master分支：

```shell
$ git checkout master
Switched to branch "master"
```

此时工作目录中的内容和你在解决问题 #53 之前一模一样，你可以集中精力进行紧急修补。这一点值得牢记：Git 会把工作目录的内容恢复为检出某分支时它所指向的那个提交对象的快照。它会自动添加、删除和修改文件以确保目录的内容和你当时提交时完全一样。
接下来，你得进行紧急修补。我们创建一个紧急修补分支 hotfix 来开展工作，直到搞定（见图 3-13）：

```shell
$ git checkout -b 'hotfix'
Switched to a new branch "hotfix"
$ vim index.html
$ git commit -a -m 'fixed the broken email address'
[hotfix]: created 3a0874c: "fixed the broken email address"
 1 files changed, 0 insertions(+), 1 deletions(-)
```

![]()

图 3-13. hotfix 分支是从 master 分支所在点分化出来的

#### 2.1.2、分支合并

有必要作些测试，确保修补是成功的，然后回到 master 分支并把它合并进来，然后发布到生产服务器。用git merge 命令来进行合并：

```Shell
$ git checkout master
$ git merge hotfix
Updating f42c576..3a0874c
Fast forward
 README |    1 -
 1 files changed, 0 insertions(+), 1 deletions(-)
```

请注意，合并时出现了“Fast forward”的提示。由于当前 master 分支所在的提交对象是要并入的 hotfix分支的直接上游，Git 只需把master 分支指针直接右移。换句话说，如果顺着一个分支走下去可以到达另一个分支的话，那么 Git 在合并两者时，只会简单地把指针右移，因为这种单线的历史分支不存在任何需要解决的分歧，所以这种合并过程可以称为快进（Fast forward）。

现在最新的修改已经在当前 `master` 分支所指向的提交对象中了，可以部署到生产服务器上去了（见图 3-14）。

![]()

图 3-14. 合并之后，master 分支和 hotfix 分支指向同一位置。

####  2.1.3、删除分支

在那个超级重要的修补发布以后，你想要回到被打扰之前的工作。由于当前 hotfix 分支和 master 都指向相同的提交对象，所以hotfix 已经完成了历史使命，可以删掉了。使用 git branch 的 -d 选项执行删除操作：

```shell
$ git branch -d hotfix
Deleted branch hotfix (3a0874c).
```

现在回到之前未完成的 #53 问题修复分支上继续工作（图
3-15）：

```Shell
$ git checkout iss53
Switched to branch "iss53"
$ vim index.html
$ git commit -a -m 'finished the new footer [issue 53]'
[iss53]: created ad82d7a: "finished the new footer [issue 53]"
 1 files changed, 1 insertions(+), 0 deletions(-)
```

![图 3-15. iss53 分支可以不受影响继续推进。]()

不用担心之前 hotfix 分支的修改内容尚未包含到 iss53 中来。如果确实需要纳入此次修补，可以用git merge master 把 master 分支合并到 iss53；或者等 iss53 完成之后，再将iss53 分支中的更新并入master。

### 2.2、分支的合并

在问题 #53 相关的工作完成之后，可以合并回 master 分支。实际操作同前面合并 hotfix 分支差不多，只需回到master 分支，运行 git merge 命令指定要合并进来的分支：

```shell
$ git checkout master
$ git merge iss53
Merge made by recursive.
 README |    1 +
 1 files changed, 1 insertions(+), 0 deletions(-)
```

请注意，这次合并操作的底层实现，并不同于之前 `hotfix` 的并入方式。因为这次你的开发历史是从更早的地方开始分叉的。由于当前 `master` 分支所指向的提交对象（C4）并不是 `iss53` 分支的直接祖先，Git 不得不进行一些额外处理。就此例而言，Git 会用两个分支的末端（C4 和 C5）以及它们的共同祖先（C2）进行一次简单的三方合并计算。图 3-16 用红框标出了 Git 用于合并的三个提交对象：

![图 3-16. Git 为分支合并自动识别出最佳的同源合并点。]()

这次，Git 没有简单地把分支指针右移，而是对三方合并后的结果重新做一个新的快照，并自动创建一个指向它的提交对象（C6）（见图 3-17）。这个提交对象比较特殊，它有两个祖先（C4 和 C5）。

值得一提的是 Git 可以自己裁决哪个共同祖先才是最佳合并基础；这和 CVS 或 Subversion（1.5以后的版本）不同，它们需要开发者手工指定合并基础。所以此特性让 Git 的合并操作比其他系统都要简单不少。

![图 3-17. Git 自动创建了一个包含了合并结果的提交对象。]()

既然之前的工作成果已经合并到 `master` 了，那么 `iss53` 也就没用了。你可以就此删除它，并在问题追踪系统里关闭该问题。

```shell
$ git branch -d iss53
```

### 2.3、遇到冲突时的分支合并

有时候合并操作并不会如此顺利。如果在不同的分支中都修改了同一个文件的同一部分，Git 就无法干净地把两者合到一起（译注：逻辑上说，这种问题只能由人来裁决。）。如果你在解决问题 #53 的过程中修改了hotfix 中修改的部分，将得到类似下面的结果：

```Shell
$ git merge iss53
Auto-merging index.html
CONFLICT (content): Merge conflict in index.html
Automatic merge failed; fix conflicts and then commit the result.
```

Git 作了合并，但没有提交，它会停下来等你解决冲突。要看看哪些文件在合并时发生冲突，可以用 git status 查阅:

```shell
[master*]$ git status
index.html: needs merge
# On branch master
# Changed but not updated:
#   (use "git add 
   ..." to update what will be committed)
#   (use "git checkout -- 
      ..." to discard changes in working directory)
#
#	unmerged:   index.html
#
```

任何包含未解决冲突的文件都会以未合并（unmerged）的状态列出。Git 会在有冲突的文件里加入标准的冲突解决标记，可以通过它们来手工定位并解决这些冲突。可以看到此文件包含类似下面这样的部分：

```Shell
<<<<<<< HEAD:index.html
     
     contact : email.support@github.com
=======
      									please contact us at support@github.com
>>>>>>> iss53:index.html
```

可以看到 `=======` 隔开的上半部分，是 `HEAD`（即 `master` 分支，在运行`merge` 命令时所切换到的分支）中的内容，下半部分是在 `iss53` 分支中的内容。解决冲突的办法无非是二者选其一或者由你亲自整合到一起。比如你可以通过把这段内容替换为下面这样来解决：

```Shell
please contact us at email.support@github.com
```

这个解决方案各采纳了两个分支中的一部分内容，而且我还删除了 `<<<<<<<`，`=======` 和 `>>>>>>>` 这些行。在解决了所有文件里的所有冲突后，运行 `git add` 将把它们标记为已解决状态（译注：实际上就是来一次快照保存到暂存区域。）。因为一旦暂存，就表示冲突已经解决。如果你想用一个有图形界面的工具来解决这些问题，不妨运行`git mergetool`，它会调用一个可视化的合并工具并引导你解决所有冲突：

```Shell
$ git mergetool
merge tool candidates: kdiff3 tkdiff xxdiff meld gvimdiff opendiff emerge vimdiff
Merging the files: index.html

Normal merge conflict for 'index.html':
  {local}: modified
  {remote}: modified
Hit return to start merge resolution tool (opendiff):

```

如果不想用默认的合并工具（Git 为我默认选择了 opendiff，因为我在 Mac 上运行了该命令），你可以在上方”merge tool candidates”里找到可用的合并工具列表，输入你想用的工具名。我们将在第七章讨论怎样改变环境中的默认值。
退出合并工具以后，Git 会询问你合并是否成功。如果回答是，它会为你把相关文件暂存起来，以表明状态为已解决。
再运行一次 git status 来确认所有冲突都已解决：

```shell
$ git status
# On branch master
# Changes to be committed:
#   (use "git reset HEAD 
     
     ..." to unstage)
#
#	modified:   index.html
#
```

如果觉得满意了，并且确认所有冲突都已解决，也就是进入了暂存区，就可以用 `git commit` 来完成这次合并提交。提交的记录差不多是这样：

```shell
Merge branch 'iss53'

Conflicts:
  index.html
#
# It looks like you may be committing a MERGE.
# If this is not correct, please remove the file
# .git/MERGE_HEAD
# and try again.
#
```

如果想给将来看这次合并的人一些方便，可以修改该信息，提供更多合并细节。比如你都作了哪些改动，以及这么做的原因。有时候裁决冲突的理由并不直接或明显，有必要略加注解.

## 3、分支的管理

