---
title: Java-Rest-Framework实践实录（二）——环境准备
date: 2018/1/25 23:12
categories:
- [技术, 后台, Java]
tags:
- Java
- 编程
- REST
- RESTful
- 后台
---

## 环境准备

说明一下，我以下的实践环境基于Ubuntu16.04+Java8。

我们使用Maven进行项目构建，因为正如我自己，其实是没有正式搞过Java Web项目的，因此我觉得也有必要说明一下各项环境的准备。

当然，构建工具也可以使用Gradle，关于构建工具，目前流行的两个基本上就是Maven和Gradle，作用说白了就是下面几个：

* 依赖包管理，可以直接引入一些第三方库而不用手动下载引入jar包
* 将项目所有相关的代码集合执行构建，其实java只是编译一堆java类文件并且指定入口而已，包管理器帮我们自动完成这个过程，否则比较麻烦的

### 1. 安装JDK

现在一般JDK有两个版本：

第一个是`openjdk`

```
sudo apt install -y openjdk-8-jdk
# 如果是安装java9
# sudo apt install -y openjdk-9-jdk
```

另一个是`java-8-oracle`

```
sudo add-apt-repository ppa:webupd8team/java
sudo apt-get update
sudo apt-get install -y oracle-java8-installer
# 如果是安装java9
# sudo apt-get install -y oracle-java9-installer
```

可以都装，然后用下面这个命令更改版本：

```
sudo update-alternatives --config java
```

### 2. 安装Maven和Gradle

在Ubuntu下其实相当好办：

```
sudo apt install -y maven gradle
```

### 3. 安装我们的开发环境IntelliJ IDEA

下载链接：<https://www.jetbrains.com/idea/download/>

至于如何科学使用，那就只有自己想办法了。


