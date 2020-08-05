# `MAC`下逆向`Adnroid`应用

[TOC]

## 一、安装工具

`Android`逆向需要三个工具`apktool`、`dex2jar`和`JD-GUI`，其作用如下：

> 1. `apktool`：解析`apk`包，获取资源文件和`smail`代码；
> 2. `dex2jar`：逆向`class.dex`文件，得到`jar`包；
> 3. `JD-GUI`：查看`jar`包文件里的`Java`代码。

### 1.1 安装`apktool`

#### 1.1.1 下载

`apktool`官网地址：[http://ibotpeaches.github.io/Apktool/](http://ibotpeaches.github.io/Apktool/) 。

悲剧的是现在打不开了，可以从我这里直接下载 [apktool_2.1.0.jar](https://github.com/tianyalu/github-doc/raw/master/android/apk_reverse/tools/apktool_2.1.0.jar) 。

此外还需要创建一个启动文件`apktool`(没有后缀名)，其内容如下：

```bash
#!/bin/bash
#
# Copyright (C) 2007 The Android Open Source Project
#
# Licensed under the Apache License, Version 2.0 (the "License");
# you may not use this file except in compliance with the License.
# You may obtain a copy of the License at
#
#     http://www.apache.org/licenses/LICENSE-2.0
#
# Unless required by applicable law or agreed to in writing, software
# distributed under the License is distributed on an "AS IS" BASIS,
# WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
# See the License for the specific language governing permissions and
# limitations under the License.

# This script is a wrapper for smali.jar, so you can simply call "smali",
# instead of java -jar smali.jar. It is heavily based on the "dx" script
# from the Android SDK

# Set up prog to be the path of this script, including following symlinks,
# and set up progdir to be the fully-qualified pathname of its directory.
prog="$0"
while [ -h "${prog}" ]; do
    newProg=`/bin/ls -ld "${prog}"`

    newProg=`expr "${newProg}" : ".* -> \(.*\)$"`
    if expr "x${newProg}" : 'x/' >/dev/null; then
        prog="${newProg}"
    else
        progdir=`dirname "${prog}"`
        prog="${progdir}/${newProg}"
    fi
done
oldwd=`pwd`
progdir=`dirname "${prog}"`
cd "${progdir}"
progdir=`pwd`
prog="${progdir}"/`basename "${prog}"`
cd "${oldwd}"

jarfile=apktool.jar
libdir="$progdir"
if [ ! -r "$libdir/$jarfile" ]
then
    echo `basename "$prog"`": can't find $jarfile"
    exit 1
fi

javaOpts=""

# If you want DX to have more memory when executing, uncomment the following
# line and adjust the value accordingly. Use "java -X" for a list of options
# you can pass here.
# 
javaOpts="-Xmx512M"

# Alternatively, this will extract any parameter "-Jxxx" from the command line
# and pass them to Java (instead of to dx). This makes it possible for you to
# add a command-line parameter such as "-JXmx256M" in your ant scripts, for
# example.
while expr "x$1" : 'x-J' >/dev/null; do
    opt=`expr "$1" : '-J\(.*\)'`
    javaOpts="${javaOpts} -${opt}"
    shift
done

if [ "$OSTYPE" = "cygwin" ] ; then
    jarpath=`cygpath -w  "$libdir/$jarfile"`
else
    jarpath="$libdir/$jarfile"
fi

# add current location to path for aapt
PATH=$PATH:`pwd`;
export PATH;
exec java $javaOpts -Djava.awt.headless=true -jar "$jarpath" "$@"
```

#### 1.1.2 配置

将下载的`apktool_2.1.0.jar`重命名为`apktool.jar`:

```bash
mv apktool_2.1.0.jar apktool.jar
```

将`apktool`和`apktool.jar`文件复制到`/usr/local/bin`目录下并给`apktool`添加可执行权限：

```bash
cp apktool /usr/local/bin
cp apktool.jar /usr/local/bin
chmod +x apktool
```

可以通过运行如下命令判断是否配置成功：

```bash
apktool -version
# 2.1.0
```

### 1.2 安装`dex2jar`

#### 1.2.1 下载

`dex2jar`官方`github`地址：[https://github.com/pxb1988/dex2jar](https://github.com/pxb1988/dex2jar) ；

官方下载地址：[https://sourceforge.net/projects/dex2jar/](https://sourceforge.net/projects/dex2jar/) ；

也可以从我这里直接下载 [dex2jar-2.0.zip](https://github.com/tianyalu/github-doc/raw/master/android/apk_reverse/tools/dex2jar-2.0.zip) 。

#### 1.2.2 配置

下载`dex2jar-2.0.zip`文件后解压得到`dex2jar-2.0`文件夹，进入该文件夹并给所有子文件添加可执行权限：

```bash
cd dex2jar-2.0
chmod +x ./*
```

### 1.3 安装`JD-GUI`

#### 1.3.1 下载

`JD-GUI`官方地址：[http://jd.benow.ca/](https://link.jianshu.com/?t=http://jd.benow.ca/) ；

悲剧的是现在也打不开了，可以从我这里直接下载 [jd-gui-osx-1.4.0.tar](https://github.com/tianyalu/github-doc/raw/master/android/apk_reverse/tools/jd-gui-osx-1.4.0.tar) 。

#### 1.3.2 安装

首先解压`jd-gui-osx-1.4.0.tar`，然后进入解压后的文件夹，双击运行`JD-GUI.app`，然后拖拽到“应用程序”中即可。

## 二、实施逆向

假设我们逆向的文件为`demo.apk`。

### 1.1 获取`xml`反编译文件和`smail`代码

```bash
apktool d demo.apk
```

命令跑完后我们就可以看到资源文件和`samil`源码了。

### 1.2 反编译`class.dex`文件

#### 1.2.1 分块反编译

将`demo.apk`该后缀名为`demo.zip`并解压，将获取到的`.dex`文件复制到`dex2jar-2.0`目录下，然后反编译：

```bash
cd dex2jar-2.0
./d2j-dex2jar.sh classes.dex
```

完成后会得到`classes-dex2jar.jar`文件。

#### 1.2.2 全量反编译

如果项目大的话，1.2.1会得到多个`.dex`文件，这样不便于我们阅读代码。

此时可以直接对`demo.apk`反编译，首先将`demo.apk`复制到`dex2jar-2.0`目录下，然后反编译：

```bash
cd dex2jar-2.0
./d2j-dex2jar.sh demo.apk
```

完成后会得到`quan-dex2jar.jar`文件。

### 1.3 查看`jar`文件代码

打开`JD-GUI`，然后选择第二步生成的`.jar`文件就可以看到`Java`代码了。

## 三、后记

本文参考：[Mac上简单的Android逆向](https://www.jianshu.com/p/7696c7e33eac)

本文仅浅层次地实现了逆向，若有更深层次的需求可参考：[Android逆向](https://www.jianshu.com/p/da81e7e9aff4)

