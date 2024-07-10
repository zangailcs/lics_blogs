---
title: OnJava06--异常
categories: 
- EnjoyCoding
- Java
- OnJava笔记
date: 2024-07-08 22:20:00
typora-root-url: ../ 
---

# OnJava06--异常

### 1、异常

#### （1）抛出异常时：

1. 创建异常对象，也是new创建，并放在堆上
2. 停止当前执行路径
3. 异常处理机制接管控制，找到“异常处理程序” （catch块）
   - 异常处理程序用来从问题中恢复，要么尝试另一条路径，要么继续执行

#### （2）终止与恢复

- 终止模型--Java支持的模型
- 恢复模型--重试
  - Java中需要通过把try块放在while循环里，不断重新进入这个try块，才能获得类似恢复模型的行为。

#### （3）异常说明

也即方法签名中的throws部分

```java
    void f() throws Exception1, Exception2 {
        // ...
    }
```

#### （4）特例：RuntimeException

- 无法预料的错误，eg：空指针异常
- RuntimeException无法放在throws后面，因为它们是**非检查型异常**。
- 编译器会强制实施对所有非RuntimeException运行时异常的检查型异常的处理。

#### （5）异常的约束

- 重写一个方法时，只能抛出该方法的基类版本中说明的异常

