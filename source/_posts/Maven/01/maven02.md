---
title: maven入门笔记02--根据坐标创建Maven工程
categories: Maven
date: 2022-09-04 23:00:00
typora-root-url: ../
---

# Maven入门02-实验一（根据坐标创建Maven工程）

### 1. Maven核心概念：坐标

#### ① 数学中的坐标：

以三维空间坐标系为例，使用x、y、z三个向量，可以在空间中唯一的定位到一个点。

#### ② Maven中的坐标

##### [1] 向量说明

使用三个<font color='cornflowerblue'>向量</font>在<font color='cornflowerblue'>Maven的仓库</font>中<font color='cornflowerblue'>唯一</font>的定位到一个<font color='cornflowerblue'>jar</font>包。

- <font color='cornflowerblue'>groupId</font>：公司或组织的id
- <font color='cornflowerblue'>artifactId</font>：一个项目或者是项目中的一个模块的id
- <font color='cornflowerblue'>version</font>：版本号

##### [2] 三个向量的取值方式

- groupId：公司或者组织域名的倒序，通常也会加上项目名称
  - 例如：com.lics.helloMaven
- artifactId：模块的名称，将来作为Maven工程的工程名
  - 例如：pro01-lics-maven
- version：模块的版本号，根据需要设定
  - 例如：SNAPSHOT表示快照版本，正在迭代过程中，不稳定的版本
  - 例如：RELEASE表示正式版本

### 2. 坐标和仓库中jar包的存储路径之间的对应关系

坐标：

```xml
<groupId>javax.servlet</groupId>
    <artifactId>javax.servlet-api</artifactId>
    <version>4.0.1</version>
<scope>provided</scope>
```

对应的jar包在maven本地仓库中的位置：

```text
${maven本地仓库根目录}$\javax\servlet\javax.servlet-api\4.0.1\javax.servlet-api-4.0.1.jar
```

### 3. 实验操作

#### ① 创建目录作为后面操作的工作空间

例如：D:\codes\maven-workspace

::: notice

此时我们已经有了三个目录，分别是：

- Maven核心程序：中军大帐
- Maven本地仓库：兵营
- 本地工作空间：战场

:::

#### ② 在工作空间目录下打开cmd

#### ③ 使用命令生成Maven工程

运行 <font color='cornflowerblue'>**mvn archetype:generate**</font> 命令

