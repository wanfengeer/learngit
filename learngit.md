# 关于git的学习。

标签（空格分隔）： git

---
###相关文章链接：
[添加SSH密钥到GitHub][1]
###仓库创建
* 关于仓库的创建：
  1.创建一个文件（windows中目录文件路径最好全是英文）
  2.右键点击然后选择git Bash Here，输入命令git init。目录下多了一个.git的目录，即创建成功。
* 把文件添加到版本库
1.第一步，用命令git add告诉Git，把文件添加到仓库：
`` $ git add 文件名称``
2.第二步，用命令git commit告诉Git，把文件提交到仓库：
``$ git commit -m "对本次提交的说明" ``
ps: commit可以一次提交很多文件，所以你可以多次add不同的文件
###版本回退
* ``git status``
可以让我们时刻掌握仓库当前的状态，可以告诉我们文件是否被修改却没有提交
* ``git diff``
顾名思义就是查看difference，显示的格式正是Unix通用的diff格式。让我们可以看到被修改的具体内容
* ``git log``
命令显示从最近到最远的提交日志
命令后面可以加--pretty=oneline参数，运行后能看到commit id。
* ``git reset``
 * 将当前版本回退到之前的版本。
 * 首先，Git必须知道当前版本是哪个版本，在Git中，用HEAD表示当前版本，也就是最新的提交，上一个版本就是HEAD^，上上一个版本就是HEAD^^，当然往上100个版本写100个^比较容易数不过来，所以写成HEAD~100。
举例：
>$ git reset --hard HEAD^
HEAD is now at e475afc add distributed
 * 若已经回退上一版本，又想回到当前版本，只要之前用git log --pretty=oneline参数查看过版本的commit id（版本号），便可以用``$ git reset --hard commit_id``回去。
 若找不到版本号，可以用``$ git reflog``命令，它记录了你每一次命令
###工作区和暂存区
* 工作区：就是你在电脑里能看到的目录
* 版本库：工作区有一个隐藏目录.git，这个不算工作区，而是Git的版本库。
Git的版本库里存了很多东西，其中最重要的就是称为stage（或者叫index）的暂存区，还有Git为我们自动创建的第一个分支master，以及指向master的一个指针叫HEAD。
* git add命令实际上就是把要提交的所有修改放到暂存区（Stage），然后，执行git commit就可以一次性把暂存区的所有修改提交到分支。
###管理修改
每次修改，如果不用git add到暂存区，那就不会加入到commit中。
###撤销修改
* 场景1：当你改乱了工作区某个文件的内容，想直接丢弃工作区的修改时，用命令git checkout -- file。
* 场景2：当你不但改乱了工作区某个文件的内容，还添加到了暂存区时，想丢弃修改，分两步，第一步用命令git reset HEAD <file>，就回到了场景1，第二步按场景1操作。
* 场景3：已经提交了不合适的修改到版本库时，想要撤销本次提交，版本回退一节，不过前提是没有推送到远程库。
###删除文件
* ``git checkout``其实是用版本库里的版本替换工作区的版本，无论工作区是修改还是删除，都可以“一键还原”。
* ``git rm``用于删除一个文件,并且git commit，就能删除版本库里的文件；如果一个文件已经被提交到版本库，那么你永远不用担心误删，但是要小心，你只能恢复文件到最新版本，你会丢失最近一次提交后你修改的内容。
###远程仓库
1.先有本地仓库再关联远程仓库：

* 本地仓库与远程仓库关联：
``$ git remote add origin git@github.com:GitHup账户名/learngit.git``
* 把本地库的内容推送到远程，用git push命令，实际上是把当前分支master推送到远程
``$ git push -u origin master``
由于远程库是空的，我们第一次推送master分支时，加上了-u参数，Git不但会把本地的master分支内容推送的远程新的master分支，还会把本地的master分支和远程的master分支关联起来，在以后的推送或者拉取时就可以简化命令，只用git push就好。
（嗯，弄了很久发现push不上去，后来发现是没有创建添加密钥2333333，具体操作看文章开头。)


  [1]: https://www.cnblogs.com/xixihuang/p/5522424.html