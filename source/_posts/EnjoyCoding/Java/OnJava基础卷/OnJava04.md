---
title: OnJava04--接口、内部类
categories: 
- EnjoyCoding
- Java
- OnJava笔记
date: 2024-06-19 22:20:00
typora-root-url: ../ 
---

# OnJava04--接口、内部类

### 1、接口

#### 1.1 抽象类、抽象方法

- 抽象方法：

```java
    abstract void f();
```

- 抽象类：包含抽象方法的类（不是必需，只要abstract修饰的class都是抽象类，可以阻止对该类的任何实例化）
- 抽象类中不能包含private的抽象方法（子类无法重写该方法进行实现）

#### 1.2 接口定义（interface）

（1）Java8之前：

- interface创建了一个完全抽象的类，不包含任何实现；
- 只确定了方法名、参数列表和返回值，但不提供任何方法主体 -- 接口描述的是一个类应该做什么，而不是应该如何做
- 接口用来在类之间建立“协议”

（2）Java8及之后：

- 允许默认方法和静态方法
  - 原因：允许向现有接口中添加方法，而不会破坏已经使用该接口的所有代码

（3）接口可以包含字段，隐式的static和final

#### 1.3 接口和抽象类

![接口和抽象类区别](https://lics-blogs-1258546254.cos.ap-nanjing.myqcloud.com/images/OnJava/image-20240619225139980.png)

#### 1.4 组合多个接口

一个类可以实现任意数量的接口，并可以向上转型到每个接口。

#### 1.5 用继承来拓展接口

- 可以使用继承向接口中添加新的方法声明
- 可以使用继承将多个接口组合成一个新接口



### 2、内部类

- 将逻辑上存在关联的类组织在一起，并能控制一个类在另一个类内的可见性。
- 在外部类的非静态方法之外的地方使用内部类时，类型需要写作：

```java
    OuterClassName.InnerClassName
```

#### （1）到外部类的链接

- 创建一个内部类时，内部类的对象中会隐含一个链接指向创建该对象的外围对象。通过此链接，内部类对象可以访问外围对象的成员。
- 内部类拥有对外围对象所有元素的访问权。

#### （2）.this和.new

- 在内部类中获取外部类对象的引用：

```java
    OuterClassName.this
```

- 外部类的对象创建它的某个内部类的对象：

```java
    obj.new InnerClassName()
```

- 必须先拥有外部类的对象，才能创建内部类对象：因为内部类的对象会隐式连接到用于创建它的外部类对象。
  - 静态内部类（static修饰的内部类）并不需要，可以直接创建new OuterClassName.StaticInnerClassName()。

#### （3）匿名内部类

接口：Contents.java

```java
public interface Contents {
  int value();
}
```

示例：Demo.java

```java
public class Demo {
  public Contents contents() {
    return new Contents() { // 内部类定义
      private int i = 11;
      @Override public int value() { return i; }
    }; // 需要分号
  }
  public static void main(String[] args) {
    Demo p = new Demo();
    Contents c = p.contents();
  }
}
```

contents()方法创建并返回了一个实现了Contents接口的内部类的对象，而这个类没有名字 -- 匿名的。

上述语法的意思是：“创建一个继承自Contents的匿名类的对象”

#### （4）为什么需要内部类

- **每个内部类都可以独立地继承自一个实现。因此外部类是否已经继承了某个实现，对内部类并没有限制。**
  - 实际上是完善了“多重继承问题”的解决方案。

#### （5）回调

- 回调：给一个对象提供一段信息，支持其在之后的某个时间点调用回原始的对象中。
- 价值：灵活性，可以在运行时动态决定调用哪些方法，常用于GUI

#### （6）内部类不能像方法一样被重写

```java
class Egg {
  private Yolk y;
  protected class Yolk {
    public Yolk() {
      System.out.println("Egg.Yolk()");
    }
  }
  Egg() {
    System.out.println("New Egg()");
    y = new Yolk(); // 即使new BigEgg调用到此处，创建的依然是Egg.Yolk，而不是BigEgg.Yolk
  }
}

public class BigEgg extends Egg {
  public class Yolk {
    public Yolk() {
      System.out.println("BigEgg.Yolk()");
    }
  }
  public static void main(String[] args) {
    new BigEgg();
  }
}
/* Output:
New Egg()
Egg.Yolk()
*/

```

