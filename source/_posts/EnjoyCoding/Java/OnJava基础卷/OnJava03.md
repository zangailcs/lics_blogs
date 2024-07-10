---
title: OnJava03--final、多态
categories: 
- EnjoyCoding
- Java
- OnJava笔记
date: 2024-06-18 20:20:00
typora-root-url: ../ 
---

# OnJava03--final、多态

### 1、final

可以用在：**数据、方法和类**

#### 1.1 final数据

（1）常量很有用，因为：

- 可以是一个永远不会改变的编译时常量 -- 必须是基本类型
- 可以是在运行时初始化的值，且不希望它被更改

（2）对于编译时常量，计算可以在编译时进行，以节省运行时开销。

（3）final修饰对象引用时，作用是让引用恒定不变，一旦被初始化为一个对象，就不能更改为指向另一个对象（对象本身可以修改）。

（4）**对final执行赋值操作只能发生在以下地方**，以保证final字段在使用前总是被初始化：

- 字段定义处
- 构造器中
- 静态/非静态初始化代码块

（5）final参数：方法内部不能修改该参数引用指向的内容

#### 1.2 final方法

一般出于设计原因，防止继承类重写方法。

#### 1.3 final类

阻止一个类有任何子类。



### 2、多态

#### 2.1 方法调用绑定

绑定：将一个方法调用和一个方法体关联起来的动作

- 前期绑定：程序运行之前执行绑定 -- 编译器和链接器实现，C等面向过程语言中是前期绑定
- 后期绑定（也叫动态绑定或运行时绑定）：基于对象的类型，运行时才绑定

关于后期绑定：

- 编译器仍不知道对象类型，但方法调用机制能找到正确的方法体
- 需要将某种类型信息放在对象里
- Java中除了static或者final（private隐式为final），都是后期绑定
  - final：防止重写，有效地“关闭”了动态绑定

#### 2.2 可拓展性

设计良好的OOP程序中，方法尽量遵循：只与基类接口通信。

这样的程序易于拓展：可以通过继承公共基类来得到新的数据类型，以添加新功能；而操作基类接口的方法不需要任何修改。

#### 2.3 陷阱：“重写”private方法

```java
public class PrivateOverride {
  private void f() {
    System.out.println("private f()");
  }
  public static void main(String[] args) {
    PrivateOverride po = new Derived();
    po.f();
  }
}

class Derived extends PrivateOverride {
  // 此方法并不能重写PrivateOverride里的私有方法f，所以并不是多态场景，main中调用的还是原来的f方法
  public void f() { System.out.println("public f()"); }
}
/* Output:
private f()
*/
```

#### 2.4 陷阱：不能在构造函数中调用被子类重写的方法！

![不要在构造函数中调用被子类重写的方法](https://lics-blogs-1258546254.cos.ap-nanjing.myqcloud.com/images/OnJava/image-20240618213233413.png)

只有基类中的final或private方法可以在构造器中安全调用。

#### 2.5 协变返回类型：

子类中重写方法的返回值可以是基类方法返回值的子类型。（Java5开始支持）

#### 2.6 继承设计的通用准则：

**用继承表达行为上的差异，用字段表达状态上的变化**。（慢慢感悟）