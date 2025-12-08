---
title: OnJava07--正则表达式、反射
categories: 
- EnjoyCoding
- Java
- OnJava笔记
date: 2024-07-14 23:20:00
typora-root-url: ../ 
---

# OnJava07--正则表达式、反射

### 1、正则表达式

#### 1.1 基础

- ？：有，也可能没有
  
- +：有一个或多个

- |：或

- W：非单词字符，w：单词字符

- java中，“\\\”代表正在插入一个正则表达式斜杠，后面的字符有特殊含义

- eg：

  ```java
  -?  //代表一个数前面可能有也可能没有减号
  -?\\d+   // 代表可选的减号后有一个或多个数字
  (-|\\+)?   // 代表 - 、 + 或什么都没有， + 在正则中是特殊字符，所以需要\\转义
  \\W+    // 不是单词的字符
  \\w+    // 单词
  \s      // 一个空白字符（空格 制表符 换行符 换页 回车）
  \S      // 非空白字符
  \d      // 数字
  \D      // 非数字
  ```

  

### 2、反射

#### 2.1 Class对象

（1）Java使用Class对象执行反射。每个类都有一个Class对象，并存储在同名的.class文件中。JVM使用类加载器生成这个对象。

（2）类在首次使用时才被动态加载到JVM。程序第一次引用该类的静态成员（构造函数是类的一个静态方法，因此new创建类的新对象也算）时，触发这个类的加载。

（3）类字面量：比forName效率高，且更安全（会进行编译时检查）

```java
    ClassName.class
```

（4）非具体的类引用：

```java
    Class<?>
```

（5）cast()：类型转换，大部分情况下可以用 (ClassName) obj 转型替换。

（6）isInstance() 、instanceof

（7）运行时的类信息：java.lang.reflect库中包含Field、Method、Constructor类

- getFields结合get、set方法读取和修改Field对象关联的字段
- invoke方法调用Method对象关联的方法
- getConstructors获取构造器的对象数组

