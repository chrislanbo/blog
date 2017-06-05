---
title: git命令和terminal命令
date: 2017-04-13 10:01:12
tags: [git,terminal]
---

在Linux命令行terminal上使用gedit直接就可以打开文本文件

那么在mac上面如何操作呢？

使用：open -a TextEdit settings.xml 参数说明：－a指定应用
也可以是：open -e settings.xml 参数说明：－e使用文本编辑器打开
也可以是：open -t settings.xml 参数说明：－t使用默认编辑器打开


# 此为注释 – 将被 Git 忽略
 
*.a       # 忽略所有 .a 结尾的文件
!lib.a    # 但 lib.a 除外
/TODO     # 仅仅忽略项目根目录下的 TODO 文件，不包括 subdir/TODO
build/    # 忽略 build/ 目录下的所有文件
doc/*.txt # 会忽略 doc/notes.txt 但不包括 doc/server/arch.txt

git rm -r --cached .
git add .
git commit -m 'update .gitignore'

目录操作
命令名	功能描述	使用举例
mkdir	创建一个目录	mkdir dirname
rmdir	删除一个目录	rmdir dirname
mvdir	移动或重命名一个目录	mvdir dir1 dir2
cd	改变当前目录	cd dirname
pwd	显示当前目录的路径名	pwd
ls	显示当前目录的内容	ls -la
dircmp	比较两个目录的内容	dircmp dir1 dir2
文件操作
命令名	功能描述	使用举例
cat	显示或连接文件	cat filename
pg	分页格式化显示文件内容	pg filename
more	分屏显示文件内容	more filename
od	显示非文本文件的内容	od -c filename
cp	复制文件或目录	cp file1 file2
rm	删除文件或目录	rm filename
mv	改变文件名或所在目录	mv file1 file2
ln	联接文件	ln -s file1 file2
find	使用匹配表达式查找文件	find . -name "*.c" -print
file	显示文件类型	file filename
open	使用默认的程序打开文件	open filename
<!-- more -->
选择操作
命令名	功能描述	使用举例
head	显示文件的最初几行	head -20 filename
tail	显示文件的最后几行	tail -15 filename
cut	显示文件每行中的某些域	cut -f1,7 -d: /etc/passwd
colrm	从标准输入中删除若干列	colrm 8 20 file2
paste	横向连接文件	paste file1 file2
diff	比较并显示两个文件的差异	diff file1 file2
sed	非交互方式流编辑器	sed "s/red/green/g" filename
grep	在文件中按模式查找	grep "^[a-zA-Z]" filename
awk	在文件中查找并处理模式	awk '{print $1 $1}' filename
sort	排序或归并文件	sort -d -f -u file1
uniq	去掉文件中的重复行	uniq file1 file2
comm	显示两有序文件的公共和非公共行	comm file1 file2
wc	统计文件的字符数、词数和行数	wc filename
nl	给文件加上行号	nl file1 >file2
安全操作
命令名	功能描述	使用举例
passwd	修改用户密码	passwd
chmod	改变文件或目录的权限	chmod ug+x filename
umask	定义创建文件的权限掩码	umask 027
chown	改变文件或目录的属主	chown newowner filename
chgrp	改变文件或目录的所属组	chgrp staff filename
xlock	给终端上锁	xlock -remote
编程操作
命令名	功能描述	使用举例
make	维护可执行程序的最新版本	make
touch	更新文件的访问和修改时间	touch -m 05202400 filename
dbx	命令行界面调试工具	dbx a.out
xde	图形用户界面调试工具	xde a.out
进程操作
命令名	功能描述	使用举例
ps	显示进程当前状态	ps u
kill	终止进程	kill -9 30142
nice	改变待执行命令的优先级	nice cc -c *.c
renice	改变已运行进程的优先级	renice +20 32768
时间操作
命令名	功能描述	使用举例
date	显示系统的当前日期和时间	date
cal	显示日历	cal 8 1996
time	统计程序的执行时间	time a.out
网络与通信操作
命令名	功能描述	使用举例
telnet	远程登录	telnet hpc.sp.net.edu.cn
rlogin	远程登录	rlogin hostname -l username
rsh	在远程主机执行指定命令	rsh f01n03 date
ftp	在本地主机与远程主机之间传输文件	ftp ftp.sp.net.edu.cn
rcp	在本地主机与远程主机 之间复制文件	rcp file1 host1:file2
ping	给一个网络主机发送 回应请求	ping hpc.sp.net.edu.cn
mail	阅读和发送电子邮件	mail
write	给另一用户发送报文	write username pts/1
mesg	允许或拒绝接收报文	mesg n
Korn Shell 命令
命令名	功能描述	使用举例
history	列出最近执行过的 几条命令及编号	history
r	重复执行最近执行过的 某条命令	r -2
alias	给某个命令定义别名	alias del=rm -i
unalias	取消对某个别名的定义	unalias del
其它命令
命令名	功能描述	使用举例
uname	显示操作系统的有关信息	uname -a
clear	清除屏幕或窗口内容	clear
env	显示当前所有设置过的环境变量	env
who	列出当前登录的所有用户	who
whoami	显示当前正进行操作的用户名	whoami
tty	显示终端或伪终端的名称	tty
stty	显示或重置控制键定义	stty -a
du	查询磁盘使用情况	du -k subdir
df	显示文件系统的总空间和可用空间	df /tmp
w	显示当前系统活动的总信息	w


查看、添加、提交、删除、找回，重置修改文件

git help <command> # 显示command的help

git show # 显示某次提交的内容 git show $id

git co -- <file> # 抛弃工作区修改

git co . # 抛弃工作区修改

git add <file> # 将工作文件修改提交到本地暂存区

git add . # 将所有修改过的工作文件提交暂存区

git rm <file> # 从版本库中删除文件

git rm <file> --cached # 从版本库中删除文件，但不删除文件

git reset <file> # 从暂存区恢复到工作文件

git reset -- . # 从暂存区恢复到工作文件

git reset --hard # 恢复最近一次提交过的状态，即放弃上次提交后的所有本次修改

git ci <file> git ci . git ci -a # 将git add, git rm和git ci等操作都合并在一起做　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　　git ci -am "some comments"

git ci --amend # 修改最后一次提交记录

git revert <$id> # 恢复某次提交的状态，恢复动作本身也创建次提交对象

git revert HEAD # 恢复最后一次提交的状态

查看文件diff

git diff <file> # 比较当前文件和暂存区文件差异 git diff

git diff <id1><id1><id2> # 比较两次提交之间的差异

git diff <branch1>..<branch2> # 在两个分支之间比较

git diff --staged # 比较暂存区和版本库差异

git diff --cached # 比较暂存区和版本库差异

git diff --stat # 仅仅比较统计信息

查看提交记录

git log git log <file> # 查看该文件每次提交记录

git log -p <file> # 查看每次详细修改内容的diff

git log -p -2 # 查看最近两次详细修改内容的diff

git log --stat #查看提交统计信息

tig

Mac上可以使用tig代替diff和log，brew install tig

Git 本地分支管理

查看、切换、创建和删除分支

git br -r # 查看远程分支

git br <new_branch> # 创建新的分支

git br -v # 查看各个分支最后提交信息

git br --merged # 查看已经被合并到当前分支的分支

git br --no-merged # 查看尚未被合并到当前分支的分支

git co <branch> # 切换到某个分支

git co -b <new_branch> # 创建新的分支，并且切换过去

git co -b <new_branch> <branch> # 基于branch创建新的new_branch

git co $id # 把某次历史提交记录checkout出来，但无分支信息，切换到其他分支会自动删除

git co $id -b <new_branch> # 把某次历史提交记录checkout出来，创建成一个分支

git br -d <branch> # 删除某个分支

git br -D <branch> # 强制删除某个分支 (未被合并的分支被删除的时候需要强制)

 分支合并和rebase

git merge <branch> # 将branch分支合并到当前分支

git merge origin/master --no-ff # 不要Fast-Foward合并，这样可以生成merge提交

git rebase master <branch> # 将master rebase到branch，相当于： git co <branch> && git rebase master && git co master && git merge <branch>

 Git补丁管理(方便在多台机器上开发同步时用)

git diff > ../sync.patch # 生成补丁

git apply ../sync.patch # 打补丁

git apply --check ../sync.patch #测试补丁能否成功

 Git暂存管理

git stash # 暂存

git stash list # 列所有stash

git stash apply # 恢复暂存的内容

git stash drop # 删除暂存区

Git远程分支管理

git pull # 抓取远程仓库所有分支更新并合并到本地

git pull --no-ff # 抓取远程仓库所有分支更新并合并到本地，不要快进合并

git fetch origin # 抓取远程仓库更新

git merge origin/master # 将远程主分支合并到本地当前分支

git co --track origin/branch # 跟踪某个远程分支创建相应的本地分支

git co -b <local_branch> origin/<remote_branch> # 基于远程分支创建本地分支，功能同上

git push # push所有分支

git push origin master # 将本地主分支推到远程主分支

git push -u origin master # 将本地主分支推到远程(如无远程主分支则创建，用于初始化远程仓库)

git push origin <local_branch> # 创建远程分支， origin是远程仓库名

git push origin <local_branch>:<remote_branch> # 创建远程分支

git push origin :<remote_branch> #先删除本地分支(git br -d <branch>)，然后再push删除远程分支

Git远程仓库管理

GitHub

git remote -v # 查看远程服务器地址和仓库名称

git remote show origin # 查看远程服务器仓库状态

git remote add origin git@ github:robbin/robbin_site.git # 添加远程仓库地址

git remote set-url origin git@ github.com:robbin/robbin_site.git # 设置远程仓库地址(用于修改远程仓库地址) git remote rm <repository> # 删除远程仓库

创建远程仓库

git clone --bare robbin_site robbin_site.git # 用带版本的项目创建纯版本仓库

scp -r my_project.git git@ git.csdn.net:~ # 将纯仓库上传到服务器上

mkdir robbin_site.git && cd robbin_site.git && git --bare init # 在服务器创建纯仓库

git remote add origin git@ github.com:robbin/robbin_site.git # 设置远程仓库地址

git push -u origin master # 客户端首次提交

git push -u origin develop # 首次将本地develop分支提交到远程develop分支，并且track

git remote set-head origin master # 设置远程仓库的HEAD指向master分支

也可以命令设置跟踪远程库和本地库

git branch --set-upstream master origin/master

git branch --set-upstream develop origin/develop

git相关使用命令
根目录创建.gitignore文件
示例：
.gitignore文件中键入
bin/
gen/
等


git status 代码变动
git diff 查看不同
git checkout src/com/.../xxx.java 撤销未提交(commit)的代码
git reset HEAD src/com/.../xxx.java 取消已经提交的代码(commit)的代码
git log 查看提交的log
git log 2e7c076... -1 -p 查看本次提交修改的内容
