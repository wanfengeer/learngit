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
使用``git add .``可以将已有文件的所有改动直接添加到仓库。
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
``$ git log --pretty=oneline --abbrev-commit``
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

2.只知道远程仓库

* 在GitHub上面找到仓库地址，在本地用``git clone``命令克隆仓库再执行操作。
一般来说，用ssh支持的原生git协议速度最快。
###关于分支
* Git创建一个分支很快，因为它只增加一个指针，改改HEAD的指向，工作区的文件都没有任何变化。
* Git鼓励大量使用分支：
 * 查看分支：git branch
 * 创建分支：git branch <name>
 * 切换分支：git checkout <name>
 * 创建+切换分支：git checkout -b <name>
 * 合并某分支到当前分支：git merge <name>
 * 删除分支：git branch -d <name>
* 当不同的分支出现了不同的提交后，直接合并就会出现问题。用git status也可以查看冲突。
``$ git log --graph --pretty=oneline --abbrev-commit``可查看分支的合并情况
当Git无法自动合并分支时，就必须首先解决冲突。解决冲突后，再提交，合并完成。
解决冲突就是把Git合并失败的文件手动编辑为我们希望的内容，再提交。
* 合并分支时，加上--no-ff参数就可以用普通模式合并，合并后的历史有分支，能看出来曾经做过合并，而fast forward合并就看不出来曾经做过合并。
* ``git stash``可以把当前工作现场“储藏”起来，等以后恢复现场后继续工作。使用后使用``git status``看工作区是干净的
之后恢复：一是用``git stash apply``恢复，但是恢复后，stash内容并不删除，你需要用``git stash drop``来删除；
另一种方式是用``git stash pop``，恢复的同时把stash内容也删了：
``git stash list``查看stash内容
* 开发一个新feature，最好新建一个分支；
如果要丢弃一个没有被合并过的分支，可以通过``git branch -D <name>``强行删除。
* 多人协作的工作模式通常是：
首先，可以试图用``git push origin <branch-name>``推送自己的修改；
如果推送失败，则因为远程分支比你的本地更新，需要先用``git pull``试图合并；
如果合并有冲突，则解决冲突，并在本地提交；
没有冲突或者解决掉冲突后，再用``git push origin <branch-name>``推送就能成功！
如果``git pull``提示``no tracking information``，则说明本地分支和远程分支的链接关系没有创建，用命令``git branch --set-upstream-to <branch-name> origin/<branch-name>``。
###标签管理
tag就是一个让人容易记住的有意义的名字，它跟某个commit绑在一起。

* ``git tag <name>``创建标签（默认打在当前分支最新的commit上）
``git tag <name> <commit_id>``给版本号为commit_id的提交打上标签
``git tag``查看所有标签，按字母排序不是时间。
``git show <tagname>``查看标签信息
``$ git tag -a <tagname> -m "说明文字" <commit_id>``创建带有说明的标签，用-a指定标签名，-m指定说明文字：
注：标签总是和某个commit挂钩。如果这个commit既出现在master分支，又出现在dev分支，那么在这两个分支上都可以看到这个标签。
* 命令``git push origin <tagname>``可以推送一个本地标签；
命令``git push origin --tags``可以推送全部未推送过的本地标签；
命令``git tag -d <tagname>``可以删除一个本地标签；
命令``git push origin :refs/tags/<tagname>``可以删除一个远程标签。

  [1]: https://www.cnblogs.com/xixihuang/p/5522424.html