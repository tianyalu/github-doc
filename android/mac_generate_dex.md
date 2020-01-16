### 1.1 配置Java环境变量
详情可参考：[Mac下环境变量详解](https://github.com/tianyalu/github-doc/blob/master/android/mac_environment.md)
### 1.2 配置DX环境变量
编辑`~/.bash_profile`文件并追加如下内容
```bash
export DX_HOME=/Users/tian/Library/Android/sdk/build-tools/29.0.2
export PATH=$PATH:$DX_HOME
```
使环境变量立即生效：  
```bash
source ~/.bash_profile
```
### 1.3 `.java`文件生成`.class`文件
```bash
# 生成.class文件
javac Calculator.java 
```
在实际使用过程中，`.java`文件中的引用可能会有依赖关系，此时可以借助`AndroidStudio`, `build`之后在`app/build/intermediates/javac/debug/classes/...`下找到相应的`.class`文件。  

### 1.4 `.class`文件生成`.dex`文件
```bash
# cd 到Calculator.class所在的目录下
dx --dex --no-strict --output Calculator.dex Calculator.class
```
如果不加`--no-strict`的话可能会报如下错误：  
```text
PARSE ERROR:
class name (com/sty/ne/andfix/fix/Calculator) does not match path (Calculator.class)
...while parsing Calculator.class
```
完成之后可以把`.dex`文件放到手机上：  
```bash
adb push Calculator.dex /storage/emulated/0
```
