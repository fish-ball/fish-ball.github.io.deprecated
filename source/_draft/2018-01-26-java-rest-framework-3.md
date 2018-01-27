---
title: Java-Rest-Framework实践实录（三）——MyBatis学习
date: 2018/1/26 0:28
categories:
- [技术, 后台, Java]
tags:
- Java
- 编程
- REST
- RESTful
- 后台
- MyBatis
---

## 从模型层开始入手

在Django的世界，Model层其实为我们做的非常多，一切用起来就像呼吸空气一般自然。然而在Java的世界似乎并不是这样。一套ORM似乎并没有承载可以像Django Model如此完整的功能集合。

不过无论如何，现在要做一个项目，还是必须从数据模型开始，所以还是从MyBatis开始研究。

下面的研究从官方的文档（中文）开始：<http://www.mybatis.org/mybatis-3/zh>

另外，我们使用MariaDB作为数据库服务。

### 构建项目

