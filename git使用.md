## git 使用

* [解释几个定义](解释几个定义)
* [版本回退](版本回退)
* [撤销操作](撤销操作)
* [git reset揭秘](git reset揭秘)
* [git rm与git rm --cached](git rm与git rm --cached)
* [git add -u与-A .三者的区别](git add -u与-A .三者的区别)

### 解释几个定义
- 版本库  
工作区有一个隐藏目录.git，这个不算工作区，而是Git的版本库。
Git的版本库里存了很多东西，其中最重要的就是称为stage（或者叫index）的暂存区，还有Git为我们自动创建的第一个分支master，以及指向master的一个指针叫HEAD。

 ![](https://i.imgur.com/tsUowzN.jpg)

- HEAD（头）  
指向当前branch最顶端的一个commit，该分支上一次commit后的节点

- Index（索引）  
 The index, 也可以被认为是staging area（暂存区）, 是一堆将在下一次commit中提交的文件，提交之后它就是HEAD的父节点. （译注：git add把文件添加进去，实际上就是把文件修改添加到暂存区,git commit提交更改，实际上就是把暂存区的所有内容提交到当前分支）
 
- Working Copy（工作副本）
当前工作目录下的文件，（译注：一般指，有修改，没有git add，没有git commit的文件）
 
- Flow（流程如下）
	- 当你第一次checkout一个新的分支，HEAD指向该分支上最近一次commit。它和index和working copy是一样一样的。
	- 当你修改了一个文件，Git注意到了会说“哦，有些东西被改了”，你的working copy不再和index和HEAD相同了，所以当文件有改动，它会标记这些文件。
	- 然后，你执行git add命令，这条命令会将上面修改的文件缓存在index中，Git又说了“哦，你的working copy和index相同了，而他们俩和HEAD不同了”。
	- 当你执行git commit，Git创建了一个新的commit，HEAD这时指向这个新的commit，此时，HEAD & index & working copy又相同了，Git又开心了一次。

### 版本回退
- [版本回退详解](https://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000/0013744142037508cf42e51debf49668810645e02887691000)
- HEAD指向的版本就是当前版本，因此，Git允许我们在版本的历史之间穿梭，使用命令

```
git reset --hard commit_id。
```

- 穿梭前，用git log可以查看提交历史，以便确定要回退到哪个版本。
- 要重返未来，用git reflog查看命令历史，以便确定要回到未来的哪个版本。

### 撤销操作
- 修改最后一次提交  

有时候我们提交完了才发现漏掉了几个文件没有加，或者提交信息写错了。想要撤消刚才的提交操作，可以使用 --amend 选项重新提交：

```
git commit --amend
```

此命令将使用当前的暂存区域快照提交。如果刚才提交完没有作任何改动，直接运行此命令的话，相当于有机会重新编辑提交说明，但将要提交的文件快照和之前的一样。
启动文本编辑器后，会看到上次提交时的说明，编辑它确认没问题后保存退出，就会使用新的提交说明覆盖刚才失误的提交。
如果刚才提交时忘了暂存某些修改，可以先补上暂存操作，然后再运行 --amend 提交：

```
$ git commit -m 'initial commit'
$ git add forgotten_file
$ git commit --amend
```

上面的三条命令最终只是产生一个提交，第二个提交命令修正了第一个的提交内容。

- 取消已经暂存的文件

来看下面的例子，有两个修改过的文件，我们想要分开提交，但不小心用 git add . 全加到了暂存区域。该如何撤消暂存其中的一个文件呢？其实，git status 的命令输出已经告诉了我们该怎么做：

```
$ git add .
$ git status
On branch master
Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)

        modified:   README.txt
        modified:   benchmarks.rb
```

就在 “Changes to be committed” 下面，括号中有提示，可以使用 git reset HEAD <file>... 的方式取消暂存。好吧，我们来试试取消暂存 benchmarks.rb 文件:  

```
$ git reset HEAD benchmarks.rb
Unstaged changes after reset:
M       benchmarks.rb
$ git status
On branch master
Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)

        modified:   README.txt
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

        modified:   benchmarks.rb
```

现在 benchmarks.rb 文件又回到了之前已修改未暂存的状态。
- 取消对文件的修改  

如果觉得刚才对 benchmarks.rb 的修改完全没有必要，该如何取消修改，回到之前的状态（也就是修改之前的版本）呢？git status 同样提示了具体的撤消方法，接着上面的例子，现在未暂存区域看起来像这样：  

```
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

        modified:   benchmarks.rb
```  
在第二个括号中，我们看到了抛弃文件修改的命令,让我们试试看：

```
$ git checkout -- benchmarks.rb
$ git status
On branch master
Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)

        modified:   README.txt
```

该文件已经恢复到修改前的版本。你可能已经意识到了，这条命令有些危险，所有对文件的修改都没有了，因为我们刚刚把之前版本的文件复制过来重写了此文件。所以在用这条命令前，请务必确定真的不再需要保留刚才的修改。
- 小结
场景1：当你改乱了工作区某个文件的内容，想直接丢弃工作区的修改时，用命令**git checkout -- file**。
场景2：当你不但改乱了工作区某个文件的内容，还添加到了暂存区时，想丢弃修改，分两步，第一步用命令git reset HEAD <file>，就回到了场景1，第二步按场景1操作。
场景3：已经提交了不合适的修改到版本库时，想要撤销本次提交，参考版本回退，不过前提是没有推送到远程库。  

### git reset揭秘

git reset 命令是git中最常用的命令，但也是最危险，最容易被误用的命令。reset命令本身很简单，但是它的参数让人迷惑，主要的参数有soft, hard and mixed，它们告诉Git，当执行reset时，要对index和working copy做什么。
- Soft
The --soft参数只告诉Git将其他的commit重置到HEAD，就仅此而已。index和working copy中的文件都不改变。

```
实例:
git reset --soft 回退到某个版本，只回退了commit的信息，不会恢复到index file一级（即git add一级）。如果还要提交，直接commit即可
```

- Mixed (default)
The --mixed 改变HEAD和index，指向那个你要reset到的commit上。而working copy文件不被改变。当然会显示工作目录下有修改，但没有缓存到index中。

```
实例:
git reset --mixed 此为默认方式，不带任何参数的git reset，即时这种方式，它回退到某个版本，只保留源码，回退commit和index信息
``` 

- Hard
The --hard HEAD & index & working copy同时改变到你要reset到的那个commit上。这个参数很危险，执行了它，你的本地修改可能就丢失了。

```
实例: 
git reset --hard：彻底回退到某个版本，本地的源码也会变为上一个版本的内容，此命令慎用！
```

- **命令后面就跟的是我们需要reset的位置**，比如  origin/develop 、 origin/master 当然也可以使上几次commit的id。
id的话我们可以通过git log查看

### git rm与git rm --cached
当我们需要删除暂存区或分支上的文件, 同时工作区也不需要这个文件了, 可以使用
```
git rm file_path
git commit -m 'delete somefile'
git push
```
当我们需要删除暂存区或分支上的文件, 但本地又需要使用, 只是不希望这个文件被版本控制, 可以使用
```
git rm --cached file_path
git commit -m 'delete remote somefile'
git push
```

### git add -u与-A .三者的区别
将文件的修改、文件的删除，添加到暂存区(update)
```
git add -u
```
将文件的修改，文件的新建，添加到暂存区
```
git add .
```
将文件的修改，文件的删除，文件的新建，添加到暂存区
```
git add -A
```

### git命令速查表
![](https://i.imgur.com/niekHES.jpg)

