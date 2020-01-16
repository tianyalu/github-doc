[toc]
### 1.1 环境变量的作用
可以使用户在操作系统的各个目录下，都能访问到需要的工具目录内的内容（可执行文件）。
### 1.2 配置文件优先级
a: `/etc/profile`
b: `/etc/paths`
c: `~/.bash_profile`
d: `~/.bash_login`
e: `~/.profile`
f: `~/.bashrc` 

> a和b是系统级别的，系统启动就会加载；  
> c,d,e是用户级别的,按照从前往后的顺序读取，如果c文件存在，则后面的几个文件就会被忽略不读了，以此类推；  
> f没有上述规则限制，它是`bash shell`打开的时候载入的。  

这里建议在`~/.bash_profile`即c中添加环境变量。
### 1.3 Mac配置环境变量的地方
`/etc/profile`: 全局公有配置，不管是哪个用户，登录时都会读取该文件(建议不修改这个文件）。  
`/etc/bashrc`: 全局公有配置，`bash shell`执行时，不管是何种方式，都会读取此文件（一般在这个文件中添加系统级环境变量）。  
`~/.bash_profile`: 每个用户都可以使用该文件，在此文件中输入专用于自己使用的shell信息，当用户登录时，该文件仅仅执行一次）。 
### 1.4 示例
以`~/.bash_profile`文件配置Java和Android环境变量为例： 
#### 1.4.1 编辑目标文件
```bash
vim ~/.bash_profile
```
#### 1.4.2 添加如下内容
```bash
export JAVA_HOME=/Library/Java/JavaVirtualMachines/jdk1.8.0_162.jdk/Contents/Home
export PATH=$JAVA_HOME/bin:$PATH
export CLASSPATH=.:$JAVA_HOME/lib/dt.jar:$JAVA_HOME/lib/tools.jar

export ANDROID_HOME=/Users/tian/Library/Android/sdk
export PATH=$PATH:$ANDROID_HOME/tools:$ANDROID_HOME/platform-tools
export PATH=$PATH:$ANDROID_HOME/tools/bin
export PATH=$PATH:$ANDROID_HOME/emulator
```

**补充：**  
* Mac中环境变量是添加到`PATH`路径下的，系统运行的时候回直接去找`/usr/libexec/path_helper`这个文件，里面的内容就是通过环境变量设置的`PATH`，所示设置环境变量是通过`PATH`设置的；  
* 在一个`PATH`下添加多个环境变量，后面用 `:` 把路径拼接下来；  
* 在`PATH`中添加`$PATH:`是为了引用一些系统基础命令，否则可能报`command not found`错误。

#### 1.4.3 使配置文件立即生效
```bash
source ~/.bash_profile
```

#### 1.4.4 查看是否真的添加到环境变量中去了
```bash
# 查看全部环境变量
echo $PATH
# 查看单独配置的某个环境变量
echo $JAVA_HOME
```
### 补充：终端编辑完成之后如何保存
vi/vim编辑器，Esc退出编辑模式，输入以下命令：  
:wq 保存后退出  
:wq! 强制存储后退出  
:w 保存但不退出  
:w! 若文件属性为【只读】时，强制写入该文档  
:q 离开vi编辑  
:q! 若曾修改过文件，又不想存储，使用!为强制离开而不存储文档  
:e! 将文档还原到最原始的状态  


本文参考：  
[mac下添加环境变量](https://www.jianshu.com/p/463244ec27e3)
