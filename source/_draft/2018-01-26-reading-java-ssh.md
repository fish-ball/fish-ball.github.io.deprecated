---
title: 读书笔记——轻量级JavaEE企业应用实战
date: 2018/1/26 23:37
categories:
- [读书笔记, 计算机]
- [技术, 后台, Java]
tags:
- Java
- 编程
- REST
- RESTful
- 后台
- 读书笔记
---

## 简述

阅读书籍：《轻量级JavaEE企业应用实战Spring+MyBatis企业应用实战》

ISBN：978-7-121-30421-7

这是Java入门之后（前面学的是《Java“变成死相”》）学的第一本应用类Java教材，旨在迅速扫盲并且介入生产。

阅读本书我比较渴望得到的是具备实践意义的，一般性以及普及性的Java WEB开发手段，以此作为Java开发的第一个台阶。

因此，在这个读书笔记中，我记录的可能知识一些主要的概念名词解释，用于快速扫盲以及概念理解的强化。

## 笔记内容

### 概念解释

#### Maven

一个构建工具，用于构建一个Java项目，简单说就是将所有的java文件链接打包到一个`.jar`包当中。

Maven是一个约定大于配置的构建工具，只要少量配置即可让项目正常运行。

同时Maven提供一系列的命令行工具以实现对项目进行各类操作。

##### Maven的主要约定：

* 源代码位于`/src/main/java`目录下
* 资源文件位于`/src/main/resources`目录下
* 测试代码位于`/src/test`目录下
* 编译生成的class文件位于`/target/classes`目录下

##### pom.xml

Project Object Model -- 项目对象模型的描述文件

用于为Maven定义当前项目相关的配置参数，例如项目名称、依赖、插件等。

POM文件的主要元素包括：

* properties: 用于定义全局属性
* dependencies: 用于定义依赖关系，可包含0-N个`<dependency/>`节点
* dependencyManagement: 用于定义依赖管理
* build: 定义构建信息
* reporting: 定义站点报告的信息
* licenses: 略
* organization: 略
* developers: 略
* contributors: 略
* issueManagement: 定义项目的bug跟踪系统
* mailingLists: 定义项目的邮件列表
* scm: 定义项目的源代码管理工具：CVS/SVN/GIT等
* repositories: 远程资源库的位置（例如修改maven镜像地址在这里配置）
* pluginRepositories: 插件远程资源库的位置
* distributionManagement: 部署管理
* profiles: 根据环境调整的构建配置

##### 常用命令：

* mvn archetype:generate -- 使用指定的原型创建一个Maven项目
* mvn archetype:create-from-project -- 使用已有项目创建一个Maven项目
* mvn archetype:crawl -- 从仓库搜索原型
* mvn install -- 下载依赖
* mvn compile -- 编译项目的java文件成为class文件
* mvn exec:java -DmainClass="xxx.xxx" -- 指定主类运行Java项目

##### Maven的生命周期：

* clean: 构建项目前做一些清理工作
  * pre-clean: 预清理
  * clean: 执行清理
  * post-clean: 最后清理
* default: 包含了项目构建的核心部分
  * compile: 编译项目
  * test: 单元测试
  * package: 项目打包
  * install: 安装到本地仓库
  * deploy: 部署到远程仓库
* site
  * pre-site: 生成站点之前做验证
  * site: 生成站点
  * post-site: 生成站点之后做验证
  * site-deploy: 发布站点到远程服务器

也就是说如果执行`mvn install`实际经历的生命周期为：

```
  pre-clean
> clean
> post-clean
> compile
> test
> package
> install
```

##### Maven插件的调用方式：

```
mvn <plugin-prefix>:<goal> -D<属性名>=<属性值> ...
```

在`pom.xml`引入插件的时候可以将插件绑定到指定的生命周期以及制定运行插件的java目标（goal）：

```
<plugin>
    <!-- ... -->
    <executions>
        <!-- 事实上可以绑定到多个阶段 -->
        <execution>
            <!-- 指定绑定到compile阶段 -->
            <phase>compile</phase>
            <!-- 指定运行exec的java目标 -->
            <goals>
                <goal>java</goal>
            </goals>
        </execution>
    </executions>
    <!-- ... -->
</plugin>
```

##### Maven坐标

每个项目是一个具备唯一标识符的Java包，这个标识符被称为Maven坐标，由如下几个属性唯一确定：

* groupId: 该项目开发者的域名
* artifactId: 指定项目名
* packaging: 指定项目的打包类型
* version: 指定项目的版本

在项目主`pom.xml`中定义本项目的坐标、引用插件、引用依赖包等的时候，均需要定义对应Maven包的坐标，例如：

```xml
<dependency>
    <groupId>junit</group>
    <artifactId>junit</artifactId>
    <version>3.8.1</version>
</dependency>
```

Maven目标一般简写为：`groupId:artifactId:packaging:version`

#### 项目结构

作为一个运行环境下的tomcat项目，我们应该维护一个这样的基本项目结构：

```
<ProjectName>/
|- WEB-INF/
|    |- classes/
|    |- lib/
|    |- web.xml
|- page1.jsp
|- page2.jsp
```

其中`classes`目录放置源代码编译获得的`.class`类文件，`lib`目录放置编译好的`.jar`包。

注意`WEB-INF`是一个必须的特殊文件夹，用户浏览器不可以访问当这里面的内容。

> 满足这样结构的项目，放在tomcat的webapps目录下即可自动运行。

##### Maven项目的Tomcat项目结构

```
<ProjectName>/
|- src
|    |- main/
|    |   |- java/
|    |       |- com/
|    |           |- example/
|    |               |- myapp/
|    |                   |- MyClass.java
|    |                   |- MyServlet.java
|    |- webapp/
|        |- WEB-INF/
|        |    |- classes/
|        |    |- lib/
|        |    |- web.xml
|        |- page1.jsp
|        |- page2.jsp
|- target/
|- pom.xml
```

##### web.xml

作为一个WEB项目，有我们必须要有一个最重要的东西，就是这个`web.xml`。

这个文件应该放置在项目路径下的`/WEB-INF/web.xml`路径。

`web.xml`被称作配置描述符，实现如下功能：

**常用的**

* 配置JSP
* 配置和管理Servlet
* 配置和管理Listener
* 配置和管理Filter
* 配置标签库
* 配置JSP属性

**另外的**

* 配置和管理JAAS授权认证
* 配置和管理资源引用
* Web应用首页

给个例子：

```xml
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
        xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
        xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee
          http://xmlns.jcp.org/xml/ns/javaee/web-app_3_1.xsd"
        version="1.1">
    <!-- 类似nginx的index命令指定默认首页文件次序 -->
    <welcome-file-list>
        <welcome-file>index.html</welcome-file>
        <welcome-file>index.htm</welcome-file>
        <welcome-file>index.jsp</welcome-file>
    </welcome-file-list>
</web-app>
```

> 注意：`web.xml`是级联控制的，例如使用tomcat可以在tomcat的`conf`目录下修改公用的`web.xml`配置。

#### Servlet

Servlet的意思就是一个独立的处理HTTP请求产生响应的工作单元。

意思是说，可以有多个Servlet实例，Java的服务可以Hold住若干个这样的Servlet对象实例，然后监听HTTP请求提供服务。

既然如此，每个Servlet相当于一般理解的MVC的Controller一样，都会获取到诸如Request对象、GET/POST参数，Session等输入，并且最终返回一个Response对象。

Servlet必须继承自`HttpServlet`类，一般重载这几个方法：

* init()
* service()
* destroy()
* doGet()
* doPost()
* doPut()
* doDelete()

init和destroy是生命周期的函数，可以用于建立/关闭数据库链接之类的动作。

至于后面四个熟悉Http的话字面上应该秒懂了。

##### 最简单的案例

给一个最简单的例子`MyServlet.java`：

注意这个文件应当放在当前maven项目目录下的`/src/main/java/com/example/hello`目录下。

```java
package com.example.hello;

import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import java.io.IOException;
import java.io.PrintWriter;

@WebServlet(name = "myServlet", urlPatterns = {"/hello"})
public class MyServlet extends HttpServlet {
    public void service(HttpServletRequest request,
                        HttpServletResponse response) throws IOException {
        PrintWriter out = response.getWriter();
        out.println("Hello world!");
    }
}
```

> 注意这里有个小坑，`HttpServlet`的依赖包在maven的`pom.xml`应该这样引入，如果引入了Servlet2.4而不是3.1的话就会找不到`WebServlet`这个修饰器类。

```xml
<dependency>
    <groupId>javax.servlet</groupId>
    <artifactId>javax.servlet-api</artifactId>
    <version>3.1.0</version>
    <scope>provided</scope>
</dependency>
```

反正大概长这么个样子，最关键的是`service`方法，我们通过在`service`方法内部获取请求参数，并且通过`response`对象获取输入流将响应内容输出就可以实现一个普通的HTTP服务功能。

###### 运行这个Servlet

注意，项目的`web.xml`应当放在项目目录下的`/src/main/webapp/WEB-INF`下，另外还可以在`/src/main/webapp`下放置其他的jsp文件之类。

我们可以在`pom.xml`里面加入tomcat7插件：

```xml
<project ...>
    <!-- ... -->
    <build>
        <finalName>webDemo</finalName>
        <plugins>
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-compiler-plugin</artifactId>
                <version>2.5.1</version>
                <inherited>true</inherited>
                <configuration>
                    <source>1.7</source>
                    <target>1.7</target>
                </configuration>
            </plugin>
            <plugin>
                <groupId>org.apache.tomcat.maven</groupId>
                <artifactId>tomcat7-maven-plugin</artifactId>
                <version>2.2</version>
                <configuration>
                    <port>8082</port>
                    <uriEncoding>UTF-8</uriEncoding>
                    <server>tomcat7</server>
                    <contextReloadable>true</contextReloadable>
                </configuration>
            </plugin>
        </plugins>
    </build>
    <!-- ... -->
</project>
```

然后执行`mvn clean && mvn tomcat7:run`，就可以在`http://127.0.0.1:8082/webDemo/hello`就可以访问到这个Servlet，并且得到`Hello World!`的输出。

##### Servlet的配置

###### 第一种：使用`@WebServlet`注解修饰

可以加入如下参数：

* asyncSupported
* displayName
* initParams
* loadOnStartup
* name
* urlPatterns/value

> 详细使用请查阅文档

###### 第二种：在`web.xml`加入相应的`<servlet-name>`元素

举个例子好了：

```
<?xml version="1.0" encoding="UTF-8"?>
<web-app version="3.1"
    xmlns="http://java.sun.com/xml/ns/javaee"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xsi:schemaLocation="http://java.sun.com/xml/ns/javaee http://java.sun.com/xm
    <sevlet>
        <servlet-name>webDemo</servlet-name>
        <servlet-class>com.example.hello.MyServlet</servlet-class>
    </sevlet>
    <servlet-mapping>
        <servlet-name>webDemo</servlet-name>
        <url-pattern>/hello</url-pattern>
    </servlet-mapping>
</web-app>
```

##### Servlet的生命周期

Servlet在第一次收到请求的时候调用`init()`方法，然后获取的请求根据方法调用`doGet()`、`doPost()`或者统一调用`service()`方法，当Web容器销毁，一般是关闭Web应用（tomcat）的时候触发。

#### JSP

如上所示，项目目录除了`WEB-INF`目录以外可以维护一个目录结构，放置若干个`.jsp`文件。

每个`.jsp`文件就会实质上产生一个`Servlet`，可以类似PHP文件的混编方式来运行。

举个例子：

```jsp
<%@Page contentType="text/html; charset=UTF-8" language="java" errorPage="" %>
<html>
<head>
    <title>Hello JSP!</title>
</head>
<body>
    <p>下面一行包含了动态内容</p>
    <p>当前日期：<%out.println(new java.util.Date());%></p>
</body>
</html>
```

> 实际上访问到这个jsp文件对应的页面的时候，(tomcat)会自动产生一个Servlet（也就是实现了某个上面提及的`service()`）方法，至于这个Servlet长什么样，教材有讲，可以查阅或者自行脑补。

补充说明几个事实：

* JSP文件必须放在JSP服务（例如tomcat）内运行
* JSP文件必须生成Servlet才能运行
* 每个JSP页面的第一个访问者速度很慢，因为必须等待JSP编译成Servlet

##### JSP的4种基本语法

* 注释：`<%-- --%>`，注意注释内容不会输出响应流
* 声明：`<%! %>`，例如声明一个变量
* 输出表达式：`<%= %>`，例如输出某个变量或者字符串
* JSP脚本：`<% %>`，随意跑

##### JSP编译指令

放在jsp文件头部，指定一些Header之类的

###### page 指令

直接上例子

```jsp
<%@page
    language="Java"
    extends="package.class"
    import="package.class | package.*,..."
    session="true | false"
    buffer="none | 8KB | size Kb"
    autoFlush="true | false"
    isThreadSafe="true | false"
    info="text"
    errorPage="relativeURL"
    contentType="text/html;charset=GBK"
    pageEncoding="ISO-8859-1"
    isErrorPage="true | false"
%>
```

有什么用，顾名思义脑补一下就知道大概了，可以想象到的是，这一部分参数会在生成的Servlet类里面动了一些小手脚，也很好理解。

###### include 指令

```jsp
<%@include file="scriptlet.jsp"%>
```

导入一个外部文件，嵌入到当前JSP文件中，可以放在中间的任何位置。直接融合之后编译，类似C/C++的宏include。

所以被嵌入的文件只需要是一个片段即可，甚至无需是一个合法的jsp文件。

##### JSP的7个动作指令

* jsp:forward -- 执行页面转向，将请求处理转发到下一个页面（注意不产生跳转）
* jsp:param -- 用于传递参数，嵌入在其他动作标签中使用
* jsp:include -- 动态导入文件，执行目标文件，并且将产生的body注入
* jsp:plugin -- 下载服务器端的JavaBean或Applet到客户端执行（少用）
* jsp:useBean -- 初始化一个Java实例
* jsp:setProperty -- 设置JavaBean实例的属性值
* jsp:getProperty -- 获取JavaBean实例的属性值

内容太多，具体要查还是看书吧。

##### JSP脚本中的9个内置对象

* application
* config
* exception
* out
* page
* pageContext
* request
* response
* session

内容也是自己脑补和查阅好了，这里只列个索引。



### 特殊要点

#### 创建一个Struts2项目

```
# 注意下面 {} 里面的是要创建的目标项目的属性，根据情况指定
mvn archetype:generate \
-DgroupId={org.crazyit} \
-DartifactId={struts2qs} \
-Dpackage={org.crazyit.struts2qs} \
-DarchetypeArtifactId=maven-archetype-webapp \
-DinteractMode=false
```



