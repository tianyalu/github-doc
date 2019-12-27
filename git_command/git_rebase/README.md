## 一、前言
### 1.1 `rebase`与`merge`
`git merge`和`git rebase`都是用来将一个分支合并到另一个分支的，只不过方式不同。
* `git merge`: 通过将两个分支中更新的内容合并提交（会生成新的commit）来将两个分支的历史连接在一起，是安全操作；  
* `git rebase`: 变基，可以把当前分支的`commit`放到公共分支的最后面，就好像是从公共分支又重新拉出来这个分支一样。  

### 1.2 `rebase`优缺点
**优点：**  
* `rebase`最大的好处是项目历史会非常整洁；  
* `rebase`导致最后的项目历史呈现完美的线性--可以更容易地使用`git log`, `git bisect`和`gitk`来查看项目历史。  

**缺点：**  
* 安全性：如果违反了`rebase`黄金法则，重写项目历史可能会给协调工作流带来灾难性的影响；  
* 可跟踪性：`rebase`不会有合并提交中附带的信息--你看不到`feature`分支中并入了上游的那些更改。  

### 1.3 `rebase`黄金法则
> 绝不要在公共的分支上使用它！  

在你运行`git rebase`之前，一定要问问自己「有没有别人正在这个分支上工作？」，如果答案是肯定的，那就不能`rebase`。


## 二、`rebase`应用场景
### 2.1 合并同一分支的多次commit
合并最近两次提交的commit：  
```base
git rebase -i HEAD~2
```
该命令进入的操作界面是针对commit二不是commit Message的，仅可修改命令。  

![image](https://github.com/tianyalu/github-doc/raw/master/git_command/git_rebase/show/rebase_multi_commit.png)

命令连用：  
* `pick squash`连用：合并多次commit，显示编辑提交信息界面（列出多条commit Message供修改
）  
* `pick fixup`连用：合并多次commit，不显示编辑提交信息界面（直接采用pick的那条commit message）  
* `reword fixup`连用：合并多次commit，显示编辑提交信息界面（仅列出reword的那条commit Message供修改）  


### 2.2 本地与远端同一分支提交历史不一致
**场景：**  
假如我和A在同一个分支`new`上开发，当我修复bug，打算`push`时，发现A已经提交过了，此时我`push`失败，只能先`pull`代码，解决冲突然后再次提交，此时会多出一个“分叉”，如下图所示：  
![image](https://github.com/tianyalu/github-doc/raw/master/git_command/git_rebase/show/one_branch_merge.png)

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
![image](https://github.com/tianyalu/github-doc/raw/master/git_command/git_rebase/show/one_branch_rebase.png)


### 2.3 不同分支之间的合并
**场景：**  
从dev分支拉出`feature`分支开发新的功能，开发完成后想合并到dev分支，此时有人已经`push`了新的内容到了dev上，我再合并分支必然失败，所以只能切换到dev分支执行`pull`命令拉取最新内容，然后进行合并操作。  
如果采用`merge`命令合并，dev分支会保存合并历史，出现“分叉”，效果如下图所示：  

![image](https://github.com/tianyalu/github-doc/raw/master/git_command/git_rebase/show/two_branch_merge.png)

采用`git rebase`： 
切到dev分支并拉取最新代码：
```bash
git pull
```
切到`feature`分支执行`rebase`：
```bash
# 以dev为基础，将feature分支上的修改增加到dev分支上，并生成新的版本
git checkout feature
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

![image](https://github.com/tianyalu/github-doc/raw/master/git_command/git_rebase/show/two_branch_rebase.png)


