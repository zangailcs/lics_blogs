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

使用三个<font color='cornflowerblue'>向量</font>在<font color='cornflowerblue'>Maven的仓库</font>中唯一的定位到一个<font color='cornflowerblue'>jar</font>包。

- <font color='cornflowerblue'>groupId</font>：公司或组织的id
- <font color='cornflowerblue'>artifactId</font>：一个项目或者是项目中的一个模块的id
- <font color='cornflowerblue'>version</font>：版本号

