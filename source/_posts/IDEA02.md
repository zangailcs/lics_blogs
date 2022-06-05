---
title: IDEA使用技巧笔记02
categories: IDEA使用技巧
mathjax: false
typora-root-url: ../
---

### IDEA（windows）使用技巧笔记02

<font color="red">注：本博客整理的是windows下的IDEA相关的快捷键，MAC等系统可能有所差异。</font>

##### 1. 编写高质量代码

| 使用场景        | 快捷键                                                      | 说明             | 所属菜单 |
| --------------- | ----------------------------------------------------------- | :--------------- | -------- |
| **1. 代码重构** | shift + F6                                                  | 重构变量名       | Refactor |
|                 | ctrl  + F6<br />(更方便的方式：<br />修改后按 alt + Enter ) | 重构方法签名     | Refactor |
| **2. 抽取**     | ctrl + alt + V                                              | 抽取为变量       | Refactor |
|                 | ctrl + alt + C                                              | 抽取为静态变量   | Refactor |
|                 | ctrl + alt + F                                              | 抽取为成员变量   | Refactor |
|                 | ctrl + alt + P                                              | 抽取为方法参数   | Refactor |
|                 | ctrl + alt + M                                              | 抽取代码段为方法 | Refactor |

##### 2. 寻找修改轨迹（git的集成）

+ **annotate**：在代码行数上右键可以看到此选项，打开后能看到commit信息、作者等。
+ 