---
title: Spring笔记01--动态SQL&缓存
categories: Spring
date: 2022-06-26 22:00:00
typora-root-url: ../
---

# Spring笔记01

### 1. Spring

#### 1.1 简介

官网：https://spring.io/projects/spring-framework#overview

github：https://github.com/spring-projects/spring-framework

```xml
<!-- https://mvnrepository.com/artifact/org.springframework/spring-webmvc -->
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-webmvc</artifactId>
    <version>5.3.21</version>
</dependency>
```

#### 1.2 优点

- Spring是一个开源、免费的框架（容器）！
- Spring是一个轻量级的、非入侵式的框架！
- 控制反转（IOC），面向切面编程（AOP）！
- 支持事务的处理，对框架整合的支持！

#### 1.3 组成 

<img src="https://lics-blogs-1258546254.cos.ap-nanjing.myqcloud.com/images/Spring/Spring01-01-7modules.jpg" width="80%" style="center"/>

### 2. IOC理论

之前：

```java
private UserDao userDao =  new UserDaoImpl();
```

利用set进行动态实现值的注入:

```java
public void setUserDao(UserDao userDao) {
    this.userDao = userDao;
}
```

- 之前，程序主动创建对象，控制权在程序员手上；用户变更需求后，需要修改原来的代码
- 使用了set注入后，程序不再具有主动性，而是被动地接受对象！

这种思想，从本质上解决了问题，程序员不需要再去管理对象的创建！系统的耦合性大大降低，可以更加专注于业务的实现。这就是IOC的原型。

### 3. IOC创建对象的方式

1. 使用无参构造创建对象，默认！

2. 若需要用有参构造创建对象，方法：

   - 第一种：下标赋值

     ```xml
     <!-- 有参构造第一种： 下标赋值 -->
     <bean id="user" class="com.lics.pojo.User">
         <constructor-arg index="0" value="lics" />
     </bean>
     ```

   - 第二种：类型匹配 【不建议使用】

     ```xml
     <!-- 有参构造第一种： 下标赋值 -->
     <bean id="user" class="com.lics.pojo.User">
         <constructor-arg type="java.lang.String" value="lics-type" />
     </bean>
     ```

   - 第三种：参数名 【通常使用】

     ```xml
     <bean id="user" class="com.lics.pojo.User">
         <constructor-arg name="name" value="lics-arg-name" />
     </bean>
     ```

3. 在配置文件加载的时候，容器中管理的对象就已经初始化了，无论是否使用。













