## 一、Git放弃本地文件修改
### 1.1 未使用`git add`缓存代码（working tree）
* 放弃某个文件修改（包括删除不包括新增）  
```bash
git checkout --filename
```
* 放弃所有文件修改（包括删除不包括新增）
```bash
git checkout .
```

**说明：**  
* 此命令用来放弃所有还没有加入到缓存区（`git add`命令）的修改：内容修改与整个文件删除；  
* 此命令不会删除新建的文件，因为新建的文件还没加入Git管理系统中，所以对Git来说是未知的，只需手动删除即可。

### 1.2 已使用`git add`缓存代码，未使用`git commit`（index暂存区）
* 放弃某个文件修改  
```bash
git reset HEAD filename
```
* 放弃所有文件修改  
```bash
git reset HEAD
```  

**说明：**  
此命令用来清除git对于文件修改的缓存，相当于撤销`git add`命令所做的工作。在使用本命令后，本地的修改并不会消失。若想要放弃本地修改，请参考1.1。  

### 1.3 已经用`git commit`提交了代码（repository）
* 彻底回退到上一次commit的状态  
```bash
git reset --hard HEAD^
```
* 彻底回退到任意commit id版本的状态  
```bash
git reset --hard commit id
```

## 二、`git reset`命令
* --hard（彻底回退：清除repository，index暂存区以及working tree工作目录中修改的内容）
```bash
# 彻底回退到上一次commit的状态
git reset --hard HEAD~1
# 或者
git reset --hard HEAD^

# 彻底回退到任意commit id版本的状态
git reset --hard commit id
```

* --soft（回退HEAD：清除repository中的内容，保留index暂存区以及working tree中修改的内容）
```bash
# repository回退HEAD到上一次commit，index暂存区以及working tree中的修改仍然保留
git reset --soft HEAD^
# 或
git reset --soft HEAD~1
```

* --mixed（默认：清除repository和index暂存区中的内容，仅保留working tree中修改的内容） 
```bash
# repository回退HEAD到上一次commit，清除repository和index暂存区中的内容，working tree中修改的内容仍保留
git reset HEAD^
# 或
git reset --mixed HEAD^
```

## 三、远程分支回退
本地分支回退之后用`push --force`命令
```bash
# 本地分支回退
git reset --hard commit id

# 远程分支回退
git push --force
# 或者
git push origin HEAD --force
```