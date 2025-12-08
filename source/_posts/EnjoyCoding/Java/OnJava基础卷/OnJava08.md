---
title: OnJava08--泛型
categories: 
- EnjoyCoding
- Java
- OnJava笔记
date: 2024-07-29 22:20:00
typora-root-url: ../ 
---

# OnJava08--泛型

### 1、泛型

#### 1.1 简单泛型

（1）最重要的初衷之一是创建**集合类**。

（2）相较于使用Object接收任意类型的对象（为了工具代码的复用性），更好的做法是指定一个类型占位符，并延迟决定具体类型的时间。

​	-- 需要在类名后的尖括号内放置一个类型参数，在使用该类时再替换为实际的类型。

```java
@Data
public class GenericHolder<T> {
    private T a;

    public GenericHolder() {
    }

    public static void main(String[] args) {
        GenericHolder<Automobile> h3 = new GenericHolder<>();
        h3.set(new Automobile()); // type checked
        Automobile a = h3.get(); // No cast needed
        //- h3.set("Not an Automobile"); // Error
        //- h3.set(1); // Error
    }
}
```

（3）Java泛型的核心理念：只需要告诉泛型所需的类型，剩下的全部细节都可以交给它。

#### 1.2 泛型接口

如：

```java
    Supplier<T>
```

#### 1.3 泛型方法

如：

```java
    public <T> void func(T x) {}
```

#### 1.4 类型擦除的奥秘

```java
Class c1 = new ArrayList<String>().getClass();
Class c2 = new ArrayList<Integer>().getClass();
System.out.println(c1 == c2); // true
// ArrayList<String>、ArrayList<Integer>应该是不同的类型，具有不同的行为（分别只能存放String和Integer），但是上面的结果返回true
```

- **泛型代码内部并不存在有关泛型参数类型的可用信息。**
  - Class.getTypeParameters() 只能获取参数占位符的标识符，并不能获取具体类型。
- Java泛型是通过**类型擦除**实现的，任何具体的类型信息都会被擦除，泛型内部唯一知道的就是在使用对象（Object）
  - 因此，List\<String\>和List\<Integer\>在运行时实际上是相同的类型。两者的类型都被“擦除”为原始类型：List

- 泛型类型参数会**被擦除为其第一个边界**
  - eg：边界\<T extends ObjA> , T会被擦除为ObjA
- 擦除存在的问题：泛型代码无法用于需要显式引用运行时类型的操作，因为关于参数的所有类型信息都丢失了，比如：
  - 类型转换、instanceof、new表达式

