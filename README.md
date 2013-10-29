#Learning Git 

###Status:commited modified staged  untracked
1. commited:saved in local repo  
2. modified:not been saved to local repo yet  
3. staged:was in the commit list that will be commited next time  
4. untracked:未跟踪的文件，比如新建的文件
###Files  
1. .git  
	this is the repo
2. 

###基本的 Git 工作流程如下：
1. 在工作目录中修改某些文件  
2. 对修改后的文件进行快照，然后保存到暂存区域。  
3. 提交更新，将保存在暂存区域的文件快照永久转储到 Git 目录中。

###git config
Git 提供了一个叫做 git config 的工具（译注：实际是 git-config 命令，只不过可以通过 git 加一个名字来呼叫此命令。），专门用来配置或读取相应的工作环境变量。而正是由这些环境变量，决定了 Git 在各个环节的具体工作方式和行为。这些变量可以存放在以下三个不同的地方：

1. /etc/gitconfig 文件：系统中对所有用户都普遍适用的配置。若使用 git config 时用--system 选项，读写的就是这个文件。  
2. ~/.gitconfig 文件：用户目录下的配置文件只适用于该用户。若使用 git config 时用--global 选项，读写的就是这个文件。  
3. 当前项目的 git 目录中的配置文件（也就是工作目录中的 .git/config 文件）：这里的配置仅仅针对当前项目有效。  
每一个下层的配置都会覆盖上层的相同配置，所以.git/config 里的配置会覆盖/etc/gitconfig 中的同名变量。   
####初次配置
* 用户信息
``$ git config --global user.name "John Doe"``	    
``$ git config --global user.email johndoe@example.com``  
说明：--global说明修改的是 ~/.gitconfig,  没有global说明是当前目录中的配置文件

* 文本编辑器  
`$ git config --global core.editor emacs`  

* 差异分析工具  
还有一个比较常用的是，在解决合并冲突时使用哪种差异分析工具。比如要改用 vimdiff 的话：
`$ git config --global merge.tool vimdiff`

* 帮助
要学习 config 命令可以怎么用，运行：
`$ git help config`

##创建本地仓库  
两种方法：  

1. 直接创建本地仓库  
当前目录作为项目仓库  
`$ git init`  
就创建了仓库： .git  
`$ git add *.c`  
`$ git add README`  
`$ git commit -m 'initial project version'`  
2. 远程获取别人的仓库  
使用：  
`git clone git://github.com/schacon/grit.git mygrit`  
新建目录 mygrit 来存储仓库  


##文件状态
![status_image](https://www.evernote.com/shard/s323/res/1718ba32-24e1-4d3b-8601-dc396483c58d.png?resizeSmall&width=1024)  
###检查文件状态  
`$ git status`
检查当前目录中的文件都是什么状态 
可能会有如下情况：
`$ git status`
`# On branch master`
`# Untracked files:`
`#   (use "git add .." to include in what will be committed)`
`#`
`#	README`
`nothing added to commit but untracked files present (use "git add" to track)`

###跟踪新文件  
`$git add README.md`  
其实git add 的潜台词就是把目标文件快照放入暂存区域，也就是 add file into staged area，同时，可想而知，未曾跟踪过的文件标记为需要跟踪。 

###修改已 

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
	$git commit  #输入之后会启动默认编辑器输入提交说明
提交之后的输出如下：
	# Please enter the commit message for your changes. Lines starting
	# with '#' will be ignored, and an empty message aborts the commit.
	# On branch master
	# Changes to be committed:
	#   (use "git reset HEAD ..." to unstage)
	#
	#       new file:   README
	#       modified:   benchmarks.rb
	~
	~
	~
	".git/COMMIT_EDITMSG" 10L, 283C
可以看到 暂存区域的文件名没有变
输入：
	$git status  




