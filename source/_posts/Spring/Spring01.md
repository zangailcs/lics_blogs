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

![Spring的七大模块](/images/Spring/01-01-7module.bmp)