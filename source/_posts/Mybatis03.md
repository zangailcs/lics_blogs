---
title: Mybatis笔记03
categories: Mybatis
typora-root-url: ../
---

### Mybatis笔记03

#### 1. 日志

##### 1.1 日志工厂

当数据库出现异常时，需要排错。日志是最好的助手！

曾经：sout、debug

现在：日志工厂！

![image-03-settings-log](/images/MyBatis/03-settings-log.png)

- SLF4J 
- LOG4J 【掌握】 
- LOG4J2 
- JDK_LOGGING 
- COMMONS_LOGGING 
- STDOUT_LOGGING 【掌握】 
- NO_LOGGING

在设置中设定MyBatis中具体使用哪一个日志！

**STDOUT_LOGGING 标准日志输出**
