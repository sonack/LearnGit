Git:
分布式版本控制系统
GitHub网站
为开源项目免费提供Git存储
CSV SVN 免费的版本控制系统
集中式 速度慢 还必须联网才能使用
BitKeeper(BitMover公司) 授予Linux社区免费试用
集中式 vs. 分布式
集中式：版本库集中放在中央服务器中的，需要从中央服务器中取得最新的版本，在本机上进行修改，然后推送到远程的中央服务器上。中央服务器就好比是一个图书馆。必须联网才能工作。
分布式：没有中央服务器，每个人的电脑上都是一个完整的版本库，只互相推送各自的修改。因为两人电脑通常不在一个局域网内，两台电脑互相访问不了或者没有开机等等，导致在两人之间的电脑上无法推送版本库的修改，所以通常也有一台充当“中央服务器”的电脑，方便“交换”大家的修改。


CVS：免费、开源、集中式版本控制系统。由于CVS自身设计问题，会造成提交文件不完整，版本库莫名其妙损坏的情况。同样开源免费的SVN就修正了CVS的一些稳定性问题，是目前用的最多的集中式版本库控制系统。

收费的集中式版本控制系统：IBM的ClearCase 微软的VSS(集成在Visual Studio中)。
分布式版本控制系统:Git BitKeeper Mercurial Bazaar

sudo apt-get install git
在老版本的debian或者ubuntu linux,要用 sudo apt-get install git-core
因为有个软件(GNU Interactive Tools)也叫GIT,所以git叫git-core
源码安装:
./config	make	sudo make install

安装完成后配置：
git config --global user.name "snk"
git config --global user.email "1243535683@qq.com"
--global表示在本机上的所有Git仓库都会使用这个配置

创建版本库(仓库 repository)
$ mkdir learngit
$ cd learngit
$ pwd
$ git init	把当前目录变成Git可以管理的Git仓库
.git目录是git来跟踪管理版本库的
也可以在非空目录下建立git仓库

所有的版本控制系统，其实只能跟踪文本文件的改动，比如TXT文件、网页、程序代码等等。
图片视频等二进制文件，虽然也能由版本控制系统管理，但无法跟踪文件的变化，只能把二进制文件每次的改动串起来，比如只能知道图片从100KB改成了120KB，但到底改了啥，VCS无法知道。

$ git add readme.txt
$ git commit -m "wrote a readme file"

commit一次可以提交很多文件，所以可以多次add不同的文件，比如
$ git add file1.txt
$ git add file2.txt file3.txt
$ git commit -m "add 3 files."

$ git diff file  可以查看修改
$ git status 查看仓库当前的状态

版本回退:
git log 命令显示从最近到最远的提交日志.
--pretty=oneline	一行显示 更好看
commit id(版本号)：由SHA1计算出来的一个非常大的数字
利用git可视化工具:
安装 sudo apt-get install gitg
版本回退，用HEAD表示当前版本 上一个版本是HEAD^，上上版本是HEAD^^
往上100个版本写成HEAD~100

git reset 命令
$ git reset --hard HEAD^
$ git reset --hard cce56e9

$ git reflog	记录每一次命令

工作区 && 暂存区
工作区：在电脑里能看到的目录
版本库：存放了很多东西，最重要的是暂存区(stage、index)
	还有Git自动创建的第一个分支 master 以及 指向 master 的一个指针 HEAD

git add 把文件修改添加到暂存区
git commit 把暂存区的所有内容提交到当前分支

管理修改:
git diff HEAD -- readme.txt	查看工作区和版本库里面最新版本的区别
每次修改，如果不add到暂存区，那就不会加入到commit中。

撤销修改
$ git checkout -- readme.txt
1.readme.txt自从修改后还没有被放到暂存区，此时撤销修改就回到和版本库一模一样的状态；
2.readme.txt已经添加到暂存区，又做了修改，此时撤销修改就回到添加到暂存区时的状态。

git reset HEAD file
可以把送到暂存区的修改撤销掉(unstaged)，重新放回工作区。

删除文件
$ git rm file  来提交删除到暂存区
$ git commit -m "remove sth."

或者误删恢复:
$ git checkout -- file

远程仓库:
github网站 提供Git仓库托管服务
本地Git仓库与GitHub仓库之间的传输是通过SSH加密的。
SSH：
Secure Shell的缩写
安全外壳协议 是建立在应用层和传输层基础上的安全协议。
验证:
1.基于口令的安全验证 容易受到中间人攻击
2.基于密钥的安全验证 登录过程较慢

1.用户主目录的.ssh目录下生成id_rsa和id_rsa.pub文件
id_rsa是私钥(不能泄露)	id_rsa.pub是公钥(可以放心地告诉任何人)
2.登陆github,在Account Settings里面设置SSH公钥。
允许添加多个key,可以用若干台电脑(公司、家里等)来往Github推送。

在Github上免费托管的Git仓库，都是公开的，但只有自己可以修改。
如果不想让别人看到Git库：
1.付费使用GitHub的私有仓库。
2.自己搭建Git服务器。

添加远程库:
$ git remote add origin git@github.com:sonack/learngit.git

把本地库的内容推送到远程库上:
$ git push -u origin master
由于远程库是空的，第一次推送master分支时，加上了-u参数，Git不但会把本地master分支内容推送到远程新的master分支，还会把本地的master分支和远程的master分支关联起来，为以后的推送或者拉取时就可以简化命令。
以后，只要本地有了新的commit，就可以通过命令:
$ git push origin master
把本地master分支的最新修改推送至GitHub。

SSH警告:
当第一次使用Git的clone或者push命令连接GitHub时，会得到一个警告。
这是因为Git使用SSH连接，而SSH连接在第一次验证GitHub服务器的Key时，需要你确认GitHub的Key的指纹信息是否真的来自GitHub的服务器，输入yes回车即可。
如果担心有人冒充GitHub服务器，输入yes前可以对照
https://help.github.com/articles/what-are-github-s-ssh-key-fingerprints/
中的GitHub的RSA Key的指纹信息是否与SSH连接给出的一致。

从远程库克隆
从GitHub网站上首先创建一个远程库
利用命令git clone克隆到本地库
$ git clone git@github.com:sonack/RemoteRepo.git

如果有多个人协作开发，那么每个人各自从远程克隆一份就可以了。
Git支持多种协议:默认的git://使用ssh,但也可以使用https等其他协议。
https 速度慢 每次需要输入口令 但在某些只开放http端口的公司内部就只能使用https.

分支管理
主分支	master
dev分支
严格来说: HEAD指向master,master指向提交
小结:
查看分支: git branch
创建分支: git branch <name>
切换分支: git checkout <name>
创建+切换分支:	git checkout -b <name>
合并某分支到当前分支:	git merge <name>
删除分支:	git branch -d <name>
合并方式: Fast-forward	快速合并(直接把master指向dev的当前commit，速度非常快 O(1))

解决冲突:
git无法执行快速合并，只能试图把各自的修改合并起来，但这种合并就可能会有冲突。
git merge feature1
提示冲突 也可以用git status查看冲突的文件
手动解决冲突后
git add file
git commit -m "conflict fixed"
git branch -d feature1

git使用 <<<<<<<、=======、>>>>>>>标记出不同分支的内容
用带参数的 git log 也可以看到分支的合并情况:
git log --graph --pretty=oneline --abbrev-commit

分支管理策略:
通常，合并分支时，如果可能，Git会用Fast forward模式，但这种模式下，删除分支后，会丢掉分支信息。
如果强制禁用Fast forward模式，那么Git就会在merge时生成一个新的commit，这样，从分支历史上就可以看出分支信息。

git merge --no-ff -m "merge with no-ff" dev
因为这种合并要创建一个新的commit，所以加上-m参数，把commit描述写进去。

分支策略:
原则:
1.master分支应该是非常稳定的，也就是仅用来发布新版本，平时不在上面干活；
2.在dev分支上干活，dev分支是不稳定的，到某个时候，再把dev分支合并到master分支上。
3.每个人都在各自的dev分支上干活，每个人都有自己的分支，时不时地往dev分支上合并。

BUG分支:
工作还没完成，不能commit,必须要先解决bug.
可以用stash功能，把当前工作现场"储藏"起来，等以后恢复现场后继续工作。
在dev分支上没有被git管理的文件是不能被stash的(只有修改和暂存区等能被管理)
$ git stash

恢复
$ git stash apply	恢复但不删除stash内容
$ git stash drop	删除stash内容
或者用一条语句
$ git stash pop		恢复的同时把stash内容也删除

恢复指定的stash
$ git stash apply stash@{0}

多人协作
当从远程仓库克隆式，实际上Git自动地把本地的master分支和远程的master分支对应起来了，并且远程仓库的默认名称为origin.
查看远程库的信息,用git remote
$ git remote
或者用git remote -v 显示更详细的信息
显示可以抓取和推送的origin的地址，如果没有推送权限，就看不到push的地址。

推送分支:
推送分支，就是把该分支上的所有本地提交推送到远程库，推送时，要指定本地分支，这样Git就会把该分支推送到远程库对应的远程分支上。
$ git push origin master
$ git push origin dev
或者用
$ git push --set-upstream origin dev
需要推送的分支:
1.master是主分支，因此要时刻与远程同步；
2.dev是开发分支，团队所有成员都需要在上面工作，所以也需要与远程同步；
3.bug分支只用于在本地修复bug，没必要推送到远程。
4.feature分支是否推到远程，取决于是否需要合作开发。

抓取分支:
克隆别人的库只能看到master分支
要在dev分支上开发，必须创建远程origin的dev分支到本地，用下面的命令创建本地dev分支
$ git checkout -b dev origin/dev
解决冲突
$ git pull
如果失败,没有指定本地dev分支和远程origin/dev分支的链接，使用:
$ git branch --set-upstream-to origin/dev dev
或者用--track
然后再次git pull
手动解决冲突后，再提交，push
$ git add conflictFile
$ git commit -m "fix the conflict on dev branch"
$ git push
多人协作的工作模式:
1.试图用git push origin branch-name 推送自己的修改
2.如果推送失败，则因为远程分支比你的本地更新，需要先用git pull试图合并
3.如果合并有冲突，则解决冲突，并在本地提交
4.没有冲突或者解决掉冲突后，再用 git push origin branch-name 推送分支

标签管理:
标签是版本库的一个快照，本质上是指向某个commit的指针(和分支类似，但分支可以移动，标签不能移动)
创建标签
切换到需要打标签的分支上
$ git branch
$ git checkout master
$ git tag v1.0
git tag <tagName>
默认标签是打在最新提交的commit上的。
git tag <tagName> <commit id>

$ git tag	#查看所有标签
标签不是按照时间顺序列出的，而是按照字母排序的。
$ git show <tagName>	#查看标签信息

创建带有说明的标签，用-a指定标签名，-m指定说明文字
创建用私钥签名一个标签，用-s
签名采用PGP签名，因此，必须首先安装gpg(GnuPG)，如果没有找到gpg，或者没有gpg密钥对，就会报错。

操作标签:
删除标签
$ git tag -d v0.1
推送标签到远程
$ git push origin <tagname>
一次性推送全部尚未推送到远程的本地标签
$ git push origin --tags

删除远程标签
1.先从本地删除
$ git tag -d v0.9
2.然后从远程删除，删除命令也是push:
$ git push origin:refs/tags/v0.9

GitHub
Fork开源仓库后拥有其读写权限，可以通过推送pull request给官方仓库贡献代码

自定义Git
1.让Git显示颜色
$ git config --global color.ui true

忽略特殊文件
git工作区的根目录下创建一个特殊的文件--- .gitignore
可以参考	https://github.com/github/gitignore
忽略文件的原则:
1.忽略操作系统自动生成的文件，比如缩略图等；
2.忽略编译生成的中间文件，可执行文件等。
3.忽略自己带有敏感信息的配置文件，比如存放口令的配置文件
然后把.gitignore文件放到版本库里，并且可以对.gitignore文件做版本管理。

配置别名:
$ git config --global alias.st status
$ git config --global alias.co checkout
$ git config --global alias.ci commit
$ git config --global alias.br branch
--global 参数是全局参数 也就是说这些配置在这台电脑的所有Git仓库下都可用。

git config --global alias.unstage 'reset HEAD'

配置git last,让其能显示最近一次的提交:
$ git config --global alias.last 'log -1'

$ git config --global alias.lg "log --color --graph --pretty=format:'%Cred%h%Creset -%C(yellow)%d%Creset %s %Cgreen(%cr) %C(bold blue)<%an>%Creset' --abbrev-commit"

配置文件:
配置Git的时候，加上--global是针对当前用户起作用的，如果不加，那只针对当前的仓库起作用。
每个仓库的配置文件放在./git/config文件中
当前用户的配置文件放在用户主目录下的一个隐藏文件.gitconfig中
配置别名也可以直接修改这个文件，如果改错了，可以删掉文件重新通过命令配置。

搭建Git服务器
1.安装git
$ sudo apt-get install git
2.创建一个git用户,用来运行git服务
# sudo adduser git
3.创建证书登陆
收集所有需要登陆的用户的公钥(id_ras.pub文件)，把所有公钥导入到/home/git/.ssh/authorized_keys文件里，一行一个。
4.初始化Git仓库:
先选定一个目录作为Git仓库，假定是/srv/sample.git
在/srv目录下输入:
$ sudo git init --bare sample.git
Git就会创建一个裸仓库，没有工作区。
因为服务器上的Git仓库纯粹是为了共享，所以不让用户直接登陆到服务器上去改工作区，并且服务器上的Git仓库通常都以.git结尾。然后把owner改为git:
$ sudo chown -R git:git sample.git
5.禁用shell登陆
6.克隆远程仓库
$ git clone git@server:/srv/sample.git

管理公钥:如果团队人员很多，可以用Gitosis
管理权限:Git不支持权限控制，但支持钩子(hook)，所以可以在服务器端编写一系列脚本来控制提交等操作，达到权限控制的目的。Gitolite就是这个工具。

GitCheatSheet:
http://pan.baidu.com/s/1jGxjQL4#path=%252Fgit

