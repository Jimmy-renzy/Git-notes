# My first git repository to record the process of learning git. #
---
***write notes here for review.***

>学习资源：廖雪峰的官方网站
>https://www.liaoxuefeng.com/wiki/896043488029600

`Hello,git!`

## git常用命令 ##

`git init`：新建目录A，在A路径下打开Git Bash，通过`git init`初始化A使之成为Git可以管理的仓库

`git status`：查看工作区的状态

![git_status.png](http://ww1.sinaimg.cn/large/006dcww6ly1ghoe6s0jr0j30iu04vt8y.jpg)

`git add`：将代码的改动提交到本地的缓存区

`git diff`：一般在`git add`前可以用`git diff`查看工作区的修改变动；`git diff`是比较的工作区与暂存区的区别，`git diff HEAD`比较的是工作区和本地仓库的区别，`git diff HEAD --filexxx`，可以用来指定查看filexxx在工作区和本地仓库中的区别。

![git_diff.png](http://ww1.sinaimg.cn/large/006dcww6ly1ghogwytmmwj30j109raak.jpg)

`git rm`：将工作区删除文件的操作提交至本地的缓存区，本质上是rm fileA + git add fileA

`git commit`：一般格式为*git commit -m "commit信息"*，用于将缓存区的改动更新至repository的本地版本

`git push`：将本地版本更新至remote远程git仓库

***结合工作区、缓存区、本地分支和远程分支去理解git add, git commit, git push***

![工作区缓存区.png](http://ww1.sinaimg.cn/large/006dcww6ly1ghp939nf6zj30lo0b640p.jpg)

工作区（Working Directory）就是我们最开始为了存储项目在电脑中新建的目录A

工作区有一个隐藏目录.git，这是Git的版本库，Git的版本库中最重要的就是称为stage（或者叫index）的暂存区，还有Git为我们自动创建的第一个分支master，以及指向master的一个指针叫HEAD。
 
---

### 删除文件 ###

如果在工作区删掉一个文件filexxx，此时filexxx在工作区不存在了，但是在版本库中还存在。

如果确认删除，则`git rm filexxx`后`git commit`，文件就从版本库中被删除了。

如果filexxx属于手残误删，因为版本库里还有，所以可以执行`git checkout -- filexxx`，将误删的文件恢复。

`git checkout`其实是用版本库里的版本替换工作区的版本，无论工作区是修改还是删除，都可以“一键还原”。

---

### 撤销修改 ###

场景1：当你改乱了工作区某个文件的内容，想直接丢弃工作区的修改时，用命令`git checkout --file`，把file文件在工作区的修改全部撤销。

场景2：当你不但改乱了工作区某个文件的内容，还git add添加到了暂存区时，想丢弃修改，分两步，第一步用命令`git reset HEAD <file>`将暂存区的修改撤销掉（unstage），重新放回工作区，就回到了场景1，第二步按场景1操作。

场景3：改乱工作区某个文件后，不仅git add，还git commit到本地仓库，此时需要执行`git reset --hrad Head^`进行版本回退，回退到上一次commit的版本。

---

### 版本回退和恢复 ###

**如果把文件改乱了或者误删了某个文件，可以从最近的一个commit恢复** 

`git log`：查看历史记录，主要是commit的记录

`git reset`：回退到上一个版本

![git_reset.png](http://ww1.sinaimg.cn/large/006dcww6ly1ghp6kqiamcj30iw0b8my0.jpg)

Git必须知道当前版本是哪个版本，在Git中，用HEAD表示当前版本，也就是最新的提交929dfcb...，上一个版本就是HEAD^，上上一个版本就是HEAD^^，当然往上100个版本写100个^比较容易数不过来，所以写成HEAD~100。

再用git log查询，只有两条commit信息了

![git_reset2.png](http://ww1.sinaimg.cn/large/006dcww6ly1ghp6j6rkaxj30hi08d0tb.jpg)

**回退到了某个版本，后悔了，又想恢复到新版本怎么办？** 

1. `git reflog`:用来记录我们的每一次git命令
2. 从中找到想要回退的版本的commit id
3. git reset --hard commit—id，搞定

---


## git拉取远程分支到本地 ##

有过把项目改崩了的经历，将改崩的项目在本地删除，然后需要把远程仓库中的dev分支重新拉到本地进行开发。

*反思：当时对git操作不熟悉，其实也可以用 `git reset --hard HEAD^`，将改崩的仓库回退到上一次commit的版本，不过将远程仓库中的某一分支拉到本地进行开发也是开发需要掌握的*

>引用博客：
>https://blog.csdn.net/carfge/article/details/79691360

1. 新建文件夹A
2. git init A 初始化文件夹A，使之成为Git可以管理的仓库
3. 本地仓库A和远程仓库建立连接，在本地仓库路径下执行命令：`git remote add origin http://（远程仓库链接）`
4. 把远程分支拉到本地：`git fetch origin dev（dev为远程仓库的分支名）`
5. 在本地创建分支local/dev并切换到该分支：`git checkout -b local/dev(本地分支名称) origin/dev(远程分支名称)`
6. 把某个分支上的内容都拉取到本地：`git pull origin dev(远程分支名称)`
7. 将本地的local/dev分支与远程仓库的origin/dev分支关联，`git branch --set-upstream-to=origin/dev local/dev`，关联之后，在本地分支下进行pull 和push操作时，便不需要指定远程的分支。
7. 最后在本地A目录下检查分支是否成功拉取，大功告成！
8. git branch --set-upstream-to=origin/<branch> pj-risk

![拉取远程分支到本地.png](http://ww1.sinaimg.cn/large/006dcww6ly1ghp7u8109pj30tn0wm418.jpg)

---

## github的使用 ##

### 关联并添加远程仓库 ###

***本地Git仓库和GitHub仓库之间的传输是通过SSH加密的，所以需要在github上添加ssh key。***

- 在.ssh文件夹里创建config文件，并写入以下内容：

```

	#github server
	Host github.com 
	RSAAuthentication yes 
	IdentityFile ~/.ssh/github_id_rsa
```


- 本地生成ssh key

	打开终端使用命令： (ssh-keygen -t rsa -C "user.email" -f github_id_rsa ) 生成ssh key.
	
	命令行进入到生成的.ssh 文件夹里： cd ~/.ssh
	
	文件夹.ssh里面有两个文件 github_id_rsa 和 github_id_rsa.pub


-  将gitbug_id_rsa.pub里的内容拷贝到github中账号管理的SSH keys。


-  在github新建repo, 命名Git-notes。


-  然后在本地的仓库下运行命令：
`git remote add origin git@github.com:michaelliao/learngit.git`

	将本地仓库与远程仓库Git-notes关联

	![git_remote_add_origin.png](http://ww1.sinaimg.cn/large/006dcww6ly1ghstdk31n0j30gl01pa9z.jpg)

- 关联后，使用命令git push -u origin master第一次推送master分支的所有内容

	![git_push_u_origin_master.png](http://ww1.sinaimg.cn/large/006dcww6ly1ghsswu28rsj30pu08bdgu.jpg)
 
- 此后，每次本地提交后，只要有必要，就可以使用命令git push origin master推送最新修改

---

## 分支管理 ##

### 创建与合并分支 ###

查看分支：`git branch`

创建分支：`git branch <name>`

切换分支：`git checkout <name>` 或者 `git switch <name>`

创建+切换分支：`git checkout -b <name>` 或者 `git switch -c <name>`

合并某分支到当前分支：`git merge <name>` [比如在master分支下执行`git merge dev`，就是把dev分支的工作成果合并到master分支上]

删除分支：`git branch -d <name>` **[注意是branch -d, 不是checkout -d]**

强制删除没有已做修改但没被合并过的分支：`git branch -D <name>`


### 分支管理策略 ###

`git merge --no-ff -m "commit信息" dev`：合并分支时，Git会用Fast forward模式，但这种模式下，删除分支后，会丢掉分支信息。如果要强制禁用Fast forward模式，Git就会在merge时生成一个新的commit，这样，从分支历史上就可以看出分支信息。

![git_merge_no-ff.png](http://ww1.sinaimg.cn/large/006dcww6ly1ghtrft4omgj30gy029dfq.jpg)

`git log --graph`：查看分支合并图

![git_log_--graph.png](http://ww1.sinaimg.cn/large/006dcww6ly1ghtr6ndppjj30jt0c6756.jpg)


**在实际开发中，进行分支管理的基本原则：**

- 首先，master分支应该是非常稳定的，也就是仅用来发布新版本，平时不能在上面干活；

- 那在哪干活呢？干活都在dev分支上，也就是说，dev分支是不稳定的，到某个时候，比如1.0版本发布时，再把dev分支合并到master上，在master分支发布1.0版本；

- 你和你的小伙伴们每个人都在dev分支上干活，每个人都有自己的分支，时不时地往dev分支上合并就可以了。

- 所以，团队合作的分支看起来就像这样：

![分支管理策略.png](http://ww1.sinaimg.cn/large/006dcww6ly1ghtqg7pkqnj30na06l0u7.jpg)


### 创建分支修复bug ###

1. 修复bug时，我们会通过创建新的bug分支进行修复，然后合并，最后删除；

2. 当手头工作没有完成时，先把工作现场`git stash`一下，然后去修复bug，修复后，再`git stash pop`，回到工作现场；

3. 在master分支上修复的bug，想要合并到当前dev分支，可以用`git cherry-pick <commit>`命令，把bug提交的修改“复制”到当前分支，避免重复劳动。


### 分支开发和提交 ###

`git remote -v`：查看远程库信息


从本地推送分支，使用git push origin branch-name，如果推送失败，先用git pull抓取远程的新提交；

在本地创建和远程分支对应的分支，使用git checkout -b branch-name origin/branch-name，本地和远程分支的名称最好一致；

建立本地分支和远程分支的关联，使用git branch --set-upstream branch-name origin/branch-name；

从远程抓取分支，使用git pull，如果有冲突，要先处理冲突。


### 多人协作开发的工作模式 ###

	首先，可以试图用git push origin <branch-name>推送自己的修改；
	
	如果推送失败，则因为远程分支比你的本地更新，需要先用git pull试图合并；
	
	如果合并有冲突，则解决冲突，并在本地提交；
	
	没有冲突或者解决掉冲突后，再用git push origin <branch-name>推送就能成功！


---

## 标签管理 ##

	发布一个版本时，我们通常先在版本库中打一个标签（tag），这样，就唯一确定了打标签时刻的版本。
	
	将来无论什么时候，取某个标签的版本，就是把那个打标签的时刻的历史版本取出来。所以，标签也是版本库的一个快照。
	
	Git的标签虽然是版本库的快照，但其实它就是指向某个commit的指针，是跟某个commit绑在一起的。

### 创建标签 ###

首先，切换到需要打标签的分支上：

	$ git branch
	* dev
	  master
	$ git checkout master
	Switched to branch 'master'

然后，敲命令 `git tag <name>` 就可以打一个新标签：

	$ git tag v1.0

可以用命令git tag查看所有标签：

	$ git tag
	v1.0

默认标签是打在最新提交的commit上的。

如果需要对某次特定的提交打标签，找到它对应的commit id："12axxx"，敲入命令：

	$ git tag v0.9 12axxx

可以用git show <tagname>查看标签的具体信息：

	$ git show v0.9
	commit f52c63349bc3c1593499807e5c8e972b82c8f286 (tag: v0.9)
	Author: Michael Liao <askxuefeng@gmail.com>
	Date:   Fri May 18 21:56:54 2018 +0800
	
	    add merge
	
	diff --git a/readme.txt b/readme.txt
	...

还可以创建带有说明的标签，用-a指定标签名，-m指定说明文字：

	$ git tag -a v0.1 -m "version 0.1 released" 1094adb


### 操作标签 ###

- 命令git push origin <tagname>可以推送一个本地标签；

- 命令git push origin --tags可以推送全部未推送过的本地标签；

- 命令git tag -d <tagname>可以删除一个本地标签；

- 命令git push origin :refs/tags/<tagname>可以删除一个远程标签。

---

## 实际开发中遇到的问题 ##

- git clone 后在远程仓库新建了dev分支，但是在本地用 git branch -r 看不到远程新建的分支？

	远程版本库更新，但git不会自动联网去同步更新，所以git在本地同步版本库是手动操作的，对应的就是 git fetch 和 git push。这里就需要git fetch之后，就可以顺利查看远程仓库新建了dev分支。

- `git rebase`???