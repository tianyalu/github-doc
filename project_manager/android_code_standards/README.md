# Android 代码命名规范
## 一、前言
### 1.1 为什么规范Android代码命名？
* 增强代码的可读性  
* 增强代码的可维护性  
通过上述作用，可以使**开发效率**和**维护效率**得到大幅提升。
### 1.2 Android需要命名的代码（对象）有哪些？
![image](https://github.com/tianyalu/github-doc/blob/master/project_manager/android_code_standards/show/rename_objects.png?raw=true)

## 二、具体命名规范
由于`Android`主要由`Java`实现，所以`Android`规范会涵盖部分`Java`规范。  
更详细的Java规范请参考：[阿里巴巴Java开发手册](https://github.com/tianyalu/github-doc/blob/master/project_manager/android_code_standards/%E9%98%BF%E9%87%8C%E5%B7%B4%E5%B7%B4Java%E5%BC%80%E5%8F%91%E6%89%8B%E5%86%8C1.5.0.pdf)。
### 2.1 包命名
* 基础规则：小写、单词间连续五间隔、反域名法（分为4级，具体如下图）  

![image](https://github.com/tianyalu/github-doc/blob/master/project_manager/android_code_standards/show/package_name_standard.png?raw=true)  

* 第4级包名会随着功能的不同而不同。下面列举一些常见&需要规范的4级功能包名  

![image](https://github.com/tianyalu/github-doc/blob/master/project_manager/android_code_standards/show/package_name.png?raw=true)  

### 2.2 类命名
* 基础规则
    - 类型 = 名词 / 名词短语  
    - 形式 = 驼峰形式中的 **大骆驼拼写法**（`UpperCamelCase`）  
> 即名称中的每个词的首字母都大写，如`AndroidStudio`  
* 在具体命名类时，会**根据该类的类型不同而附加额外的命名规则**。具体如下图所示：  

![image](https://github.com/tianyalu/github-doc/blob/master/project_manager/android_code_standards/show/class_name.png?raw=true)  

### 2.3 变量命名
* 基础规则  
    - 类型 = 名词 / 名词短语  
    - 形式 = 驼峰形式中的**小驼峰写法**（`LowerCamelCase`）  
> 即名称中的第一个词的首字母小写，后面每个词的首字母大写，如`androidStudioTool`  
* 在具体命名变量时，会**根据该变量的类型不同而附加额外的命名规则**。具体如下图所示：  

![image](https://github.com/tianyalu/github-doc/blob/master/project_manager/android_code_standards/show/variable_name.png?raw=true)  

### 2.4 方法命名
* 基础规则
    - 类型 = 动词 / 动词短语  
    - 形式 = 驼峰形式中的小骆驼拼写法（`LowerCamelCase`）  
> 即名称中的第一个词的首字母小写，后面每个词的首字母大写，如`androidStudioTool`  
* 在具体命名方法名时，会**根据该变量的类型不同而附加额外的命名规则**。具体如下图所示：  

![image](https://github.com/tianyalu/github-doc/blob/master/project_manager/android_code_standards/show/function_name.png?raw=true)  

### 2.5 参数名命名
* 基础规则：驼峰形式中的**小驼峰写法**（`LowerCamelCase`）  
> 即名称中的第一个词的首字母小写，后面每个词的首字母大写，如`androidStudioTool`  
* 附加命名规则：功能名，如`userName`  
### 2.6 资源
* Android的资源包括  

![image](https://github.com/tianyalu/github-doc/blob/master/project_manager/android_code_standards/show/resource_dir.png?raw=true)  
![image](https://github.com/tianyalu/github-doc/blob/master/project_manager/android_code_standards/show/resource_structure.png?raw=true)  

下面对`Android`资源的命名规矩进行详细讲解  

#### 2.6.1 布局文件资源
![image](https://github.com/tianyalu/github-doc/blob/master/project_manager/android_code_standards/show/resource_layout.png?raw=true)  
#### 2.6.2 图片资源
![image](https://github.com/tianyalu/github-doc/blob/master/project_manager/android_code_standards/show/resource_picture.png?raw=true)  
#### 2.6.3 参数值资源
![image](https://github.com/tianyalu/github-doc/blob/master/project_manager/android_code_standards/show/resource_value.png?raw=true)  
#### 2.6.4 动画资源
![image](https://github.com/tianyalu/github-doc/blob/master/project_manager/android_code_standards/show/resource_animator.png?raw=true)  

### 2.7 分包
分包原则：按功能模块分包，即相似的基础功能放在一个包下，不同的业务功能放在不同的包下。  
示例如下：  

         java
           |--- com
                |---example
                    |--- base
                    |    |--- BaseActivity.java
                    |    |--- BaseFragment.java
                    |    |--- xxx.java
                    |
                    |--- network
                    |    |--- HttpClient.java
                    |    |--- xxx.java
                    |
                    |--- image
                    |    |--- ImageManager.java
                    |    |--- xxx.java
                    |
                    |--- db
                    |    |--- DbManager.java
                    |    |--- xxx.java
                    |
                    |--- news
                    |    |--- NewsActivity.java
                    |    |--- NewsFragment.java
                    |    |--- xxx.java
                    |
                    |--- movie
                    |    |--- MovieActivity.java
                    |    |--- MovieFragment.java
                    |    |--- xxx.java
                    |
                    |--- music
                    |    |--- MusicActivity.java
                    |    |--- MusicFragment.java
                    |    |--- xxx.java
                    |
                    |--- entity
                    |    |--- Movie.java
                    |    |--- News.java
                    |    |--- xxx.java
                    |
                    |--- adapter
                    |    |--- AbsAdapter.java
                    |    |--- MovieAdapter.java
                    |
                    |--- widget
                    |    |--- CircleImageView.java
                    |    |--- xxx.java
                    |    
                    |--- util
                         |--- ToastUtil.java
                         |--- xxx.java

### 2.8 额外
除了上述的命名规范外，`Android`中还有一些全局通用的命名规范：  

![image](https://github.com/tianyalu/github-doc/blob/master/project_manager/android_code_standards/show/global_standard.png?raw=true)  

## 三、附录
### 3.1 常见使用单词缩写
使用单词缩写的原则：只使用约定俗成的单词缩写。  
**注意：**  
严禁自由缩写单词  

![image](https://github.com/tianyalu/github-doc/blob/master/project_manager/android_code_standards/show/word_abbreviation.png?raw=true)  


     