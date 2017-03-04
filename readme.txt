Git:分布式版本控制系统

local repository

1.git init 初始化库
2.git add 添加文件到库 （可反复添加，一次添加多个）
3.git commit -m “xxx” 提交文件到库 （添加多次后一次提交即可）
4.git status （显示现在库的状态）
5.git diff （显示改动的文件的具体变化）
6.git log （查看提交历史） —-pretty=oneline（使之显示在一行的参数）注：已改为别名lg
7.commit id (版本号,一个SHA1计算出来的一个非常大的数字，用十六进制表示)
8.git用HEAD表示当前版本,上一个版本就是HEAD^，上上一个版本就是HEAD^^，往上100个版本写100个^比较容易数不过来，所以写成HEAD~100
9.git reset --hard HEAD^ (表示回退到上一个版本，-—hard是参数)
10.git reset --hard 3628164（版本号）
(可以指定回到未来的某个版本,关键是记得版本号。版本号没必要写全，前几位就可以了，Git会自动去找。当然也不能只写前一两位，因为Git可能会找到多个版本号，就无法确定是哪一个了。)
11.git reflog (查看命令历史)

12.
工作区（Working Directory） 工作区有一个隐藏目录.git
版本库（Repository）Git的版本库里存了很多东西，其中最重要的就是称为stage（或者叫index）的暂存区，还有Git为我们自动创建的第一个分支master，以及指向master的一个指针叫HEAD。

13.
前面讲了我们把文件往Git版本库里添加的时候，是分两步执行的：
第一步是用git add把文件添加进去，实际上就是把文件修改添加到暂存区；
第二步是用git commit提交更改，实际上就是把暂存区的所有内容提交到当前分支。
因为我们创建Git版本库时，Git自动为我们创建了唯一一个master分支，所以，现在，git commit就是往master分支上提交更改。你可以简单理解为，需要提交的文件修改通通放到暂存区，然后，一次性提交暂存区的所有修改。

14.Git跟踪修改:每次修改，如果不add到暂存区，那就不会加入到commit中。

15.git checkout -- readme.txt （丢弃工作区的修改）
把readme.txt文件在工作区的修改全部撤销，这里有两种情况：
一种是readme.txt自修改后还没有被放到暂存区，现在，撤销修改就回到和版本库一模一样的状态；
一种是readme.txt已经添加到暂存区后，又作了修改，现在，撤销修改就回到添加到暂存区后的状态。
总之，就是让这个文件回到最近一次git commit或git add时的状态。（git checkout -- file命令中的--很重要，没有--，就变成了“切换到另一个分支”的命令）

16.git reset HEAD readme.txt（可以把暂存区的修改撤销掉（unstage），重新放回工作区，就是撤销了git add的操作）
git reset命令既可以回退版本，也可以把暂存区的修改回退到工作区。当我们用HEAD时，表示最新的版本。再用git status查看一下，现在暂存区是干净的，工作区有修改。

17.
git rm read.txt (从版本库里删除)
git checkout —read.txt (把误删的文件恢复到最新版本,但会丢失最近一次提交后你修改的内容。其实是用版本库里的版本替换工作区的版本，无论工作区是修改还是删除，都可以“一键还原”。)


remote repository

1.把一个已有的本地仓库与之关联,然后，把本地仓库的内容推送到GitHub仓库:
git remote add origin https://github.com/YINNNER/Git.git
git push -u origin master 
(用git push命令，实际上是把当前分支master推送到远程。
由于远程库是空的，我们第一次推送master分支时，加上了-u参数，Git不但会把本地的master分支内容推送的远程新的master分支，还会把本地的master分支和远程的master分支关联起来，在以后的推送或者拉取时就可以简化命令。)

2.git push origin master(把本地master分支的最新修改推送至GitHub)

小结：
关联一个远程库，使用命令
     git remote add origin git@server-name:path/repo-name.git;
E.G. git remote add origin git@github.com:YINNNER/Git.git
OR   git remote add origin https://github.com/YINNNER/Git.git;
关联后，使用命令git push -u origin master第一次推送master分支的所有内容；
此后，每次本地提交后，只要有必要，就可以使用命令git push origin master推送最新修改；
分布式版本系统的最大好处之一是在本地工作完全不需要考虑远程库的存在，也就是有没有联网都可以正常工作。当有网络的时候，再把本地提交推送一下就完成了同步！

3. git clone git@github.com:YINNNER/Git.git;
Git支持多种协议，默认的git://使用ssh，但也可以使用https等其他协议。使用https除了速度慢以外，还有个最大的麻烦是每次推送都必须输入口令，但是在某些只开放http端口的公司内部就无法使用ssh协议而只能用https。通过ssh支持的原生git协议速度最快。

4.使用分支：
查看分支：git branch
创建分支：git branch <name>
切换分支：git checkout <name>（切换成为当前分支）
创建+切换分支：git checkout -b <name>
合并某分支到当前分支：git merge <name>
删除分支：git branch -d <name>

5.当Git无法自动合并分支时，就必须首先解决冲突。
解决冲突后，再提交，合并完成。

5.5解决冲突：
s1:git status 显示冲突的文件是什么
s2:打开冲突文件
s3:Git用<<<<<<<，=======，>>>>>>>标记出不同分支的内容:

<<<<<<< HEAD
Creating a new branch is quick & simple.
=======
Creating a new branch is quick AND simple.
>>>>>>> feature1

s4:将<<<<<<<，=======，>>>>>>>之间的内容都删去，修改如下后保存：

Creating a new branch is quick and simple.

s5:提交：

$ git add readme.txt 
$ git commit -m "conflict fixed"
[master 59bc1cb] conflict fixed

6.git log --graph （显示分支合并图）

用带参数的git log也可以看到分支的合并情况：

$ git log --graph --pretty=oneline --abbrev-commit
*   59bc1cb conflict fixed
|\
| * 75a857c AND simple
* | 400b400 & simple
|/
* fec145a branch test
...


7.git merge --no-ff -m "merge with no-ff" dev
通常，合并分支时，如果可能，Git会用Fast forward模式，但这种模式下，删除分支后，会丢掉分支信息。
如果要强制禁用Fast forward模式，Git就会在merge时生成一个新的commit，这样，从分支历史上就可以看出分支信息。
准备合并dev分支，请注意--no-ff参数，表示禁用Fast forward：
git merge --no-ff -m "merge with no-ff" dev
因为本次合并要创建一个新的commit，所以加上-m参数，把commit描述写进去。
可以看到，不使用Fast forward模式，merge后就像这样：
git-no-ff-mode
注：合并分支时，加上--no-ff参数就可以用普通模式合并，合并后的历史有分支，能看出来曾经做过合并，而fast forward合并就看不出来曾经做过合并。

8.分支策略
在实际开发中，我们应该按照几个基本原则进行分支管理：
首先，master分支应该是非常稳定的，也就是仅用来发布新版本，平时不能在上面干活；
干活都在dev分支上，也就是说，dev分支是不稳定的，到某个时候，比如1.0版本发布时，再把dev分支合并到master上，在master分支发布1.0版本；
你和你的小伙伴们每个人都在dev分支上干活，每个人都有自己的分支，时不时地往dev分支上合并就可以了。

9.bug分支
修复bug时，我们会通过创建新的bug分支进行修复，然后合并，最后删除

10.
git stash临时储存当前工作现场
git stash pop回到工作现场（恢复的同时把stash内容也删了）
git stash apply恢复且stash内容并不删除，用git stash drop再来删除
git stash list查看存储的工作现场
（可多次stash，恢复的时候，先用git stash list查看，然后恢复指定的stash，用命令：
git stash apply stash@{0}）

11.git branch -D <name>（大写D,强行删除一个没有被合并过的分支）

12.开发一个新feature，最好新建一个分支；

13.
当你从远程仓库克隆时，实际上Git自动把本地的master分支和远程的master分支对应起来了，并且，远程仓库的默认名称是origin。
git remote（查看远程库的信息）；
git remote -v显示更详细的信息；
如：
$ git remote -v
origin  git@github.com:michaelliao/learngit.git (fetch)
origin  git@github.com:michaelliao/learngit.git (push)
上面显示了可以抓取和推送的origin的地址。如果没有推送权限，就看不到push的地址。

14.推送本地分支到远程库：
git push origin dev
         远程库某分支／本地库某分支
（本地新建的分支如果不推送到远程，对其他人就是不可见的）

15.多人协作策略：
master分支是主分支，因此要时刻与远程同步；
dev分支是开发分支，团队所有成员都需要在上面工作，所以也需要与远程同步；
bug分支只用于在本地修复bug，就没必要推到远程了，除非老板要看看你每周到底修复了几个bug；
feature分支是否推到远程，取决于你是否和你的小伙伴合作在上面开发。

16.抓取分支：
推送失败，因为你的小伙伴的最新提交和你试图推送的提交有冲突，解决办法也很简单，Git已经提示我们，先用git pull把最新的提交从origin/dev抓下来，然后，在本地合并，解决冲突，再推送：

多人协作的工作模式：
git push origin branch-name 首先可以试图推送自己的修改；
git pull 如果推送失败，则因为远程分支比你的本地更新，需要先试图合并；如果合并有冲突，则解决冲突，并在本地提交；
git branch --set-upstream branch-name origin/branch-name 创建本地分支和远程分支的联系
（如果git pull提示“no tracking information”，则说明本地分支和远程分支的链接关系没有创建）
git push origin branch-name 没有冲突或者解决掉冲突后，再推送就能成功！

小结：
从本地推送分支，使用git push origin branch-name，如果推送失败，先用git pull抓取远程的新提交；
在本地创建和远程分支对应的分支，使用git checkout -b branch-name origin/branch-name，本地和远程分支的名称最好一致；
建立本地分支和远程分支的关联，使用git branch --set-upstream branch-name origin/branch-name；
从远程抓取分支，使用git pull，如果有冲突，要先处理冲突。

17.标签管理：
发布一个版本时，我们通常先在版本库中打一个标签（tag），这样，就唯一确定了打标签时刻的版本。将来无论什么时候，取某个标签的版本，就是把那个打标签的时刻的历史版本取出来。所以，标签也是版本库的一个快照。
Git的标签虽然是版本库的快照，但其实它就是指向某个commit的指针（跟分支很像对不对？但是分支可以移动，标签不能移动），所以，创建和删除标签都是瞬间完成的。

tag就是一个让人容易记住的有意义的名字，它跟某个commit绑在一起。
如：按照tag v1.2查找commit就行

17.1.
git tag <name>   新建一个标签，默认为HEAD；
git tag <name> commit id  可以指定一个commit id；
git tag -a <tagname> -m "blablabla..."  可以指定标签信息；
git tag -s <tagname> -m "blablabla..."  可以用PGP签名标签；
git tag  查看所有标签； 注：标签不是按时间顺序列出，而是按字母排序的。
git show <tagname> 查看标签信息。
git push origin <tagname>  可以推送一个本地标签；
git push origin --tags  可以推送全部未推送过的本地标签；
git tag -d <tagname>  可以删除一个本地标签；
如果标签已经推送到远程，要删除远程标签就麻烦一点，先从本地删除即上面的命令，再用下面的命令：
git push origin :refs/tags/<tagname>  可以删除一个远程标签。

18.使用GitHub
在GitHub上，可以任意Fork开源仓库到自己的账号仓库；
自己拥有Fork后的仓库的读写权限；
（一定要从自己的账号下clone仓库，这样你才能推送修改。如果从bootstrap的作者的仓库地址克隆，因为没有权限，你将不能推送修改。）
可以推送pull request给官方仓库来贡献代码。

19.自定义git
让Git显示颜色，会让命令输出看起来更醒目：
git config --global color.ui true

20.忽略特殊文件
忽略某些文件时，需要编写.gitignore；
.gitignore文件本身要放到版本库里，并且可以对.gitignore做版本管理！

21.配置别名
git config --global alias.st status
	     全局参数	  别名／原名
（全局参数，使命令在这台电脑的所有Git仓库下都有用，不加全局参数就是只对当前仓库有用。）

每个仓库的Git配置文件都放在.git/config文件中：

$ cat .git/config 

别名就在[alias]后面，要删除别名，直接把对应的行删掉即可。
而当前用户的Git配置文件放在用户主目录(~)下的一个隐藏文件.gitconfig中：

$ cat .gitconfig

配置别名也可以直接修改这个文件，如果改错了，可以删掉文件重新通过命令配置。



附录：
1.Git教程：http://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000

*2.搭建Git服务器


在远程仓库一节中，我们讲了远程仓库实际上和本地仓库没啥不同，纯粹为了7x24小时开机并交换大家的修改。
GitHub就是一个免费托管开源代码的远程仓库。但是对于某些视源代码如生命的商业公司来说，既不想公开源代码，又舍不得给GitHub交保护费，那就只能自己搭建一台Git服务器作为私有仓库使用。
搭建Git服务器需要准备一台运行Linux的机器，强烈推荐用Ubuntu或Debian，这样，通过几条简单的apt命令就可以完成安装。
假设你已经有sudo权限的用户账号，下面，正式开始安装。

第一步，安装git：

$ sudo apt-get install git
第二步，创建一个git用户，用来运行git服务：

$ sudo adduser git
第三步，创建证书登录：

收集所有需要登录的用户的公钥，就是他们自己的id_rsa.pub文件，把所有公钥导入到/home/git/.ssh/authorized_keys文件里，一行一个。

第四步，初始化Git仓库：

先选定一个目录作为Git仓库，假定是/srv/sample.git，在/srv目录下输入命令：

$ sudo git init --bare sample.git
Git就会创建一个裸仓库，裸仓库没有工作区，因为服务器上的Git仓库纯粹是为了共享，所以不让用户直接登录到服务器上去改工作区，并且服务器上的Git仓库通常都以.git结尾。然后，把owner改为git：

$ sudo chown -R git:git sample.git
第五步，禁用shell登录：

出于安全考虑，第二步创建的git用户不允许登录shell，这可以通过编辑/etc/passwd文件完成。找到类似下面的一行：

git:x:1001:1001:,,,:/home/git:/bin/bash
改为：

git:x:1001:1001:,,,:/home/git:/usr/bin/git-shell
这样，git用户可以正常通过ssh使用git，但无法登录shell，因为我们为git用户指定的git-shell每次一登录就自动退出。

第六步，克隆远程仓库：

现在，可以通过git clone命令克隆远程仓库了，在各自的电脑上运行：

$ git clone git@server:/srv/sample.git
Cloning into 'sample'...
warning: You appear to have cloned an empty repository.
剩下的推送就简单了。

管理公钥

如果团队很小，把每个人的公钥收集起来放到服务器的/home/git/.ssh/authorized_keys文件里就是可行的。如果团队有几百号人，就没法这么玩了，这时，可以用Gitosis来管理公钥。
这里我们不介绍怎么玩Gitosis了，几百号人的团队基本都在500强了，相信找个高水平的Linux管理员问题不大。

管理权限

有很多不但视源代码如生命，而且视员工为窃贼的公司，会在版本控制系统里设置一套完善的权限控制，每个人是否有读写权限会精确到每个分支甚至每个目录下。因为Git是为Linux源代码托管而开发的，所以Git也继承了开源社区的精神，不支持权限控制。不过，因为Git支持钩子（hook），所以，可以在服务器端编写一系列脚本来控制提交等操作，达到权限控制的目的。Gitolite就是这个工具。
这里我们也不介绍Gitolite了，不要把有限的生命浪费到权限斗争中。

小结
搭建Git服务器非常简单，通常10分钟即可完成；
要方便管理公钥，用Gitosis；
要像SVN那样变态地控制权限，用Gitolite。
