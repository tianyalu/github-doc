## 一、合并同一分支的多次commit
合并最近两次提交的commit：  
```base
git rebase -i HEAD~2
```
该命令进入的操作界面是针对commit二不是commit Message的，仅可修改命令。  

![image](https://github.com/tianyalu/github-doc/blob/master/git_command/git_rebase/rebase_multi_commit.png)

命令连用：  
* `pick squash`连用：合并多次commit，显示编辑提交信息界面（列出多条commit Message供修改
）  
* `pick fixup`连用：合并多次commit，不显示编辑提交信息界面（直接采用pick的那条commit message）  
* `reword fixup`连用：合并多次commit，显示编辑提交信息界面（仅列出reword的那条commit Message供修改）  


## 二、本地与远端同一分支提交历史不一致
**场景：**  
假如我和A在同一个分支`new`上开发，当我修复bug，打算`push`时，发现A已经提交过了，此时我`push`失败，只能先`pull`代码，解决冲突然后再次提交，此时会多出一个“分叉”，如下图所示：  
![4142a21f6f2a2b75aaf56bd71777a266.png](evernotecid://F56AC6C6-8A58-4539-A2EC-9B91A02F1B29/appyinxiangcom/17862382/ENResource/p2518)

采用`git rebase`：  
```bash
git rebase
# 或者
git pull --rebase
```
如果有冲突，可以通过如下命令查看冲突文件：  
```bash
git status
```
解决冲突后将冲突文件添加到暂存区：  
```bash
git add .
```
继续执行`git rebase`:  
```bash
git rebase --continue
git push
```
此时的线路图就是一条直线，如下图所示：  
![cfc4b019c52e9761cde9aea0cb65f6ae.png](evernotecid://F56AC6C6-8A58-4539-A2EC-9B91A02F1B29/appyinxiangcom/17862382/ENResource/p2519)


## 三、不同分支之间的合并
**场景：**  
从dev分支拉出`feature`分支开发新的功能，开发完成后想合并到dev分支，此时有人已经`push`了新的内容到了dev上，我再合并分支必然失败，所以只能切换到dev分支执行`pull`命令拉取最新内容，然后进行合并操作。  
如果采用`merge`命令合并，dev分支会保存合并历史，出现“分叉”，效果如下图所示：  

![7e6f6953f41757ed32b3cbb6fe389dc0.png](evernotecid://F56AC6C6-8A58-4539-A2EC-9B91A02F1B29/appyinxiangcom/17862382/ENResource/p2520)  

采用`git rebase`： 
切到dev分支并拉取最新代码：
```bash
git pull
```
切到dev分支执行`rebase`：
```bash
# 以dev为基础，将feature分支上的修改增加到dev分支上，并生成新的版本
git rebase dev
```
如果有冲突通过如下命令查看冲突的文件：  
```bash
git status
```
解决冲突后将冲突文件添加到缓存区：  
```bash
git add .
```
继续执行`rebase`操作：  
```bash
git rebase --continue
```
切到dev分支进行合并操作：  
```bash
git merge feature
git push
```
此时的线路图就是一条直线，如下图所示：  

![0ba455698e78f010033feff7e5d86cfa.png](evernotecid://F56AC6C6-8A58-4539-A2EC-9B91A02F1B29/appyinxiangcom/17862382/ENResource/p2521)


