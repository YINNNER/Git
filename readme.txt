Git is a distributed version control system.
Git is a free software.

Git:

local repository

1.git init 初始化库
2.git add 添加文件到库 （可反复添加，一次添加多个）
3.git commit -m “xxx” 提交文件到库 （添加多次后一次提交即可）
4.git status （显示现在库的状态）
5.git diff （显示改动的文件的具体变化）
6.git log （查看提交历史） —-pretty=oneline（使之显示在一行的参数）
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
总之，就是让这个文件回到最近一次git commit或git add时的状态。