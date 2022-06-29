---
title: Spring笔记03--AOP
categories: Spring
date: 2022-06-28 20:00:00
typora-root-url: ../
---

# Spring笔记03--AOP

### 1. 代理模式

代理模式就是Spring AOP的底层！

代理模式的分类：

- 静态代理
- 动态代理

### 2. AOP

#### 2.1 什么是AOP

AOP（Aspect Oriented Programming），面向切面编程。通过预编译方式和运行期动态代理实现程序功能的统一维护的一种技术。

<img src="https://lics-blogs-1258546254.cos.ap-nanjing.myqcloud.com/images/Spring/03-01-AOP-example.png" width="80%" style="center"/>

#### 2.2 AOP在Spring中的作用

提供声明式事务，允许用户自定义切面。

<img src="https://lics-blogs-1258546254.cos.ap-nanjing.myqcloud.com/images/Spring/03-02-AOP-usage.png" width="80%" style="center"/>

#### 2.3 使用Spring实现AOP

【重点】使用AOP，需要导入一个依赖包：

```xml
<!-- https://mvnrepository.com/artifact/org.aspectj/aspectjweaver -->
<dependency>
    <groupId>org.aspectj</groupId>
    <artifactId>aspectjweaver</artifactId>
    <version>1.9.9.1</version>
    <scope>runtime</scope>
</dependency>
```



方式一：使用Spring的API接口

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:aop="http://www.springframework.org/schema/aop"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
        https://www.springframework.org/schema/beans/spring-beans.xsd
        http://www.springframework.org/schema/aop
        https://www.springframework.org/schema/aop/spring-aop.xsd">

    <!-- 注册bean -->
    <bean id="userService" class="com.lics.service.UserServiceImpl" />
    <bean id="log" class="com.lics.log.Log" />
    <bean id="logAfter" class="com.lics.log.LogAfter" />

    <!-- 配置aop -->
    <aop:config>
        <!-- 切入点 -->
        <aop:pointcut id="pointcut" expression="execution(* com.lics.service.UserServiceImpl.*(..))"/>
        <!-- 执行环绕增加！ -->
        <aop:advisor advice-ref="log" pointcut-ref="pointcut" />
        <aop:advisor advice-ref="logAfter" pointcut-ref="pointcut" />
    </aop:config>
    
</beans>
```

方式二：自定义类







