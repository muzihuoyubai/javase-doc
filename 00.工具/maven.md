# 引言

## 前置知识

学习本次课需要具备

- 基本的命令行操作技能
- github.com 注册账号

## 学习目的

软件开发或者学习阶段，都需要一款软件版本管理工具。因为编写代码总是需要频繁的修改文件，很可能需要找到修改了很多次以前的某个版本的代码，使用版本管理工具可以存储、追踪目录下文件的修改历史，便于回溯和回退代码，同时便于多个开发者协同开发。因此掌握一个软件版本工具的使用是非常必要的。Git是现在非常流行的一个软件版本管理工具，github上托管着数量巨大的开源代码项目，学习这些内容将对以后编写和学习代码有着巨大的帮助。

## 课程结构

1. 介绍git的工作流，工作区、暂存区、仓库的概念
2. 创建本地的git仓库，添加、提交代码到仓库（以及相关的撤销操作）的相关命令
3. 介绍github和远程仓库的创建
4. 将本地仓库和远程仓库关联进行代码同步

## 学习目标

学习本次课后，学员将

* 能够使用git管理自己的代码
* 能够将代码同步到github

# Mavne安装

mac配置环境变量

```
export JAVA_HOME=$(/usr/libexec/java_home -v 11)
```

## 1. Mac OSX 10.5 or later

In Mac OSX 10.5 or later, Apple recommends to set the `$JAVA_HOME` variable to `/usr/libexec/java_home`, just export `$JAVA_HOME` in file `~/. bash_profile` or `~/.profile`.

```bash
$ vim .bash_profile 

export JAVA_HOME=$(/usr/libexec/java_home)

$ source .bash_profile

$ echo $JAVA_HOME
/Library/Java/JavaVirtualMachines/1.7.0.jdk/Contents/Home


Copy
```

**Why /usr/libexec/java_home?**
This `java_home` can return the Java version specified in Java Preferences for the current user. For examples,

```bash
/usr/libexec/java_home -V
Matching Java Virtual Machines (3):
    1.7.0_05, x86_64:	"Java SE 7"	/Library/Java/JavaVirtualMachines/1.7.0.jdk/Contents/Home
    1.6.0_41-b02-445, x86_64:	"Java SE 6"	/System/Library/Java/JavaVirtualMachines/1.6.0.jdk/Contents/Home
    1.6.0_41-b02-445, i386:	"Java SE 6"	/System/Library/Java/JavaVirtualMachines/1.6.0.jdk/Contents/Home


Copy
```

This Mac OSX has three JDK installed.

```bash
##return top Java version
$ /usr/libexec/java_home
/Library/Java/JavaVirtualMachines/1.7.0.jdk/Contents/Home

## I want Java version 1.6
$ /usr/libexec/java_home -v 1.6
/System/Library/Java/JavaVirtualMachines/1.6.0.jdk/Contents/Home
```



## 2. Older Mac OSX

For older Mac OSX, the `/usr/libexec/java_home` doesn’t exists, so, you should set JAVA_HOME to the fixed path :

```bash
$ vim .bash_profile 

export JAVA_HOME=/System/Library/Java/JavaVirtualMachines/1.6.0.jdk/Contents/Home

$ source .bash_profile

$ echo $JAVA_HOME
/System/Library/Java/JavaVirtualMachines/1.6.0.jdk/Contents/Home
```