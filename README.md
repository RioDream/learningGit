#Learning Git 

##Init
###git config
Git 提供了一个叫做 git config 的工具（译注：实际是 git-config 命令，只不过可以通过 git 加一个名字来呼叫此命令。），专门用来配置或读取相应的工作环境变量。而正是由这些环境变量，决定了 Git 在各个环节的具体工作方式和行为。这些变量可以存放在以下三个不同的地方：

1. /etc/gitconfig 文件：系统中对所有用户都普遍适用的配置。若使用 git config 时用--system 选项，读写的就是这个文件。  
2. ~/.gitconfig 文件：用户目录下的配置文件只适用于该用户。若使用 git config 时用--global 选项，读写的就是这个文件。  
3. 当前项目的 git 目录中的配置文件（也就是工作目录中的 .git/config 文件）：这里的配置仅仅针对当前项目有效。  
每一个下层的配置都会覆盖上层的相同配置，所以.git/config 里的配置会覆盖/etc/gitconfig 中的同名变量。   
####初次配置

* 用户信息  
	$ git config --global user.name "John Doe"    
	$ git config --global user.email johndoe@example.com  
说明：--global说明修改的是 ~/.gitconfig,  没有global说明是当前目录中的配置文件

* 文本编辑器  
	$ git config --global core.editor emacs

* 差异分析工具  
还有一个比较常用的是，在解决合并冲突时使用哪种差异分析工具。比如要改用 vimdiff 的话：  
	$ git config --global merge.tool vimdiff

* 帮助
要学习 config 命令可以怎么用，运行：  
	$ git help config

##开始：创建本地仓库  
两种方法：  

1. 直接创建本地仓库  
当前目录作为项目仓库
	$ git init
就创建了仓库： .git  
	$ git add *.c
	$ git add README
	$ git commit -m 'initial project version'
2. 远程获取别人的仓库  
使用：  
	git clone git://github.com/schacon/grit.git mygrit
新建目录 mygrit 来存储仓库  

###本地目录中的文件状态:commited,modified,staged,untracked
1. commited:saved in local repo  
2. modified:not been saved to local repo yet  
3. staged:was in the commit list that will be commited next time  
4. untracked:未跟踪的文件，比如新建的文件

###本地的工作目录中一般都包含哪些文件  
1. .git  
	this is the repo
2. 





##本地工作
![status_image](https://www.evernote.com/shard/s323/res/1718ba32-24e1-4d3b-8601-dc396483c58d.png?resizeSmall&width=1024)

###基本的 Git 工作流程如下：
1. 在工作目录中修改某些文件  
2. 对修改后的文件进行快照，然后保存到暂存区域。  
3. 提交更新，将保存在暂存区域的文件快照永久转储到 Git 目录中。

###检查文件状态  
	$ git status
检查当前目录中的文件都是什么状态  
可能会有如下情况：  
	$ git status
	# On branch master
	# Untracked files:
	#   (use "git add .." to include in what will be committed)
	#
	#	README
	nothing added to commit but untracked files present (use "git add" to track)

###跟踪新文件  
	$git add README.md
其实git add 的潜台词就是把目标文件快照放入暂存区域，也就是 add file into staged area，同时，可想而知，未曾跟踪过的文件标记为需要跟踪。 

###修改文件

	$ git status
	# On branch master
	# Changes to be committed:
	#   (use "git reset HEAD ..." to unstage)
	#
	#	new file:   README
	#
	# Changed but not updated:
	#   (use "git add ..." to update what will be committed)
	#
	#	modified:   benchmarks.rb
	#
看到 benchmarks,rb 已经被修改，但是没有放到暂存区
使用：  
	$git status  
将其放到暂存区

####注意：如果这时候再去修改 benchmarks.rb 文件
	$ git status
	# On branch master
	# Changes to be committed:
	#   (use "git reset HEAD ..." to unstage)
	#
	#	new file:   README
	#	modified:   benchmarks.rb
	#
	# Changed but not updated:
	#   (use "git add ..." to update what will be committed)
	#
	#	modified:   benchmarks.rb
	#
可以看到 benchmarks.rb文件有两种状态，这时候需要重新运行git add，将修改之后的文件暂存起来

###忽略某些文件  
	$cat .gitignore 
	*.[oa] #忽略以.o 或者.a结尾的文件，不对其跟踪
	*~     #忽略以～结尾的文件，经常是Vim等编辑器的swap文件

###查看更新内容  
查看处于modified状态的文件都更新了什么，输入：
	$git diff

###提交更新
提交到本地仓库  
	$git commit  #输入之后会启动默认编辑器输入提交说明
	$git commit -m 'first commt' #直接加入说明

###移除文件  
要从 Git 移除某个文件，就必须从跟踪文件清单中移除（确切地说，是从暂存区域移除），然后提交。   
可以使用git rm  命令完成此项工作，并连带从工作目录中删除指定的文件，这样以后就不会出现在未跟踪文件清单中了。 
如果只是删除了目录下的文件，就会被记录在status中，如：  
	$ rm grit.gemspec  

	$ git status
	# On branch master
	#
	# Changed but not updated:
	#   (use "git add/rm ..." to update what will be committed)
	#
	#       deleted:    grit.gemspec
	#
再使用 git rm 命令移除暂存列表中的grit.gemspec  

###移动和重命名  
	$ git mv file_from file_to
它会恰如预期般正常工作。实际上，即便此时查看状态信息，也会明白无误地看到关于重命名操作的说明：  
	$ git mv README.txt README
	$ git status
	# On branch master
	# Your branch is ahead of 'origin/master' by 1 commit.
	#
	# Changes to be committed:
	#   (use "git reset HEAD ..." to unstage)
	#
	#       renamed:    README.txt -> README
	#

其实，运行 git mv 就相当于运行了下面三条命令：  
	$ mv README.txt README
	$ git rm README.txt
	$ git add README

##提交远程仓库

###查看当前的远程仓库
	$git remote #查看当前有哪些远程仓库
在克隆完某个项目后，至少可以看到一个名为 origin 的远程库，Git 默认使用这个名字来标识你所克隆的原始仓库：  
	$ git clone git://github.com/schacon/ticgit.git
	Initialized empty Git repository in /private/tmp/ticgit/.git/
	remote: Counting objects: 595, done.
	remote: Compressing objects: 100% (269/269), done.
	remote: Total 595 (delta 255), reused 589 (delta 253)
	Receiving objects: 100% (595/595), 73.31 KiB | 1 KiB/s, done.
	Resolving deltas: 100% (255/255), done.
	$ cd ticgit
	$ git remote
	origin

也可以加上 -v 选项（译注：此为 --verbose 的简写，取首字母），显示对应的克隆地址：  
	$ git remote -v
	origin	git://github.com/schacon/ticgit.git

###添加远程仓库
要添加一个新的远程仓库，可以指定一个简单的名字，以便将来引用，运行 git remote add [shortname] [url]：  
	$ git remote
	origin
	$ git remote add pb git://github.com/paulboone/ticgit.git
	$ git remote -v
	origin	git://github.com/schacon/ticgit.git
	pb	git://github.com/paulboone/ticgit.git
于是你就有了一个仓库名叫pb

###从远程仓库更新数据
可以用下面的命令从远程仓库抓取数据到本地：  
	$ git fetch [remote-name]
此命令会到远程仓库中拉取所有你本地仓库中还没有的数据。运行完成后，你就可以在本地访问该远程仓库中的所有分支，将其中某个分支合并到本地，或者只是取出某个分支，一探究竟。

如果是克隆了一个仓库，此命令会自动将远程仓库归于 origin 名下。所以，git fetch origin 会抓取从你上次克隆以来别人上传到此远程仓库中的所有更新（或是上次 fetch 以来别人提交的更新）。有一点很重要，需要记住，fetch 命令只是将远端的数据拉到本地仓库，并不自动合并到当前工作分支，只有当你确实准备好了，才能手工合并。    

设置了某个分支用于跟踪某个远端仓库的分支，可以使用 git pull 命令自动抓取数据下来，然后将远端分支自动合并到本地仓库中当前分支。在日常工作中我们经常这么用，既快且好。实际上，默认情况下git clone 命令本质上就是自动创建了本地的 master 分支用于跟踪远程仓库中的 master 分支（假设远程仓库确实有 master 分支）。所以一般我们运行git pull，目的都是要从原始克隆的远端仓库中抓取数据后，合并到工作目录中的当前分支。  

###推送数据到远程仓库
项目进行到一个阶段，要同别人分享目前的成果，可以将本地仓库中的数据推送到远程仓库。实现这个任务的命令很简单： git push [remote-name] [branch-name]。如果要把本地的 master 分支推送到origin 服务器上（再次说明下，克隆操作会自动使用默认的 master 和 origin 名字），可以运行下面的命令：  
	$ git push origin master
只有在所克隆的服务器上有写权限，或者同一时刻没有其他人在推数据，这条命令才会如期完成任务。如果在你推数据前，已经有其他人推送了若干更新，那 你的推送操作就会被驳回。你必须先把他们的更新抓取到本地，合并到自己的项目中，然后才可以再次推送。有关推送数据到远程仓库的详细内容见第三章。