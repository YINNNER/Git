Git is a distributed version control system.
Git is a free software.

Git:

local repository

1.git init 初始化库
2.git add 添加文件到库 （可反复添加，一次添加多个）
3.git commit -m “xxx” 提交文件到库 （添加多次后一次提交即可）
4.git status （显示现在库的状态）
5.git diff （显示改动的文件的具体变化）
6.git log （显示提及日志）—-pretty=oneline
7.commit id (版本号,一个SHA1计算出来的一个非常大的数字，用十六进制表示)
8.git用HEAD表示当前版本,上一个版本就是HEAD^，上上一个版本就是HEAD^^，往上100个版本写100个^比较容易数不过来，所以写成HEAD~100
9.git reset --hard HEAD^ (表示回退到上一个版本，-—hard是参数)
10.git reset --hard 3628164（版本号）
(可以指定回到未来的某个版本,关键是记得版本号。版本号没必要写全，前几位就可以了，Git会自动去找。当然也不能只写前一两位，因为Git可能会找到多个版本号，就无法确定是哪一个了。)
