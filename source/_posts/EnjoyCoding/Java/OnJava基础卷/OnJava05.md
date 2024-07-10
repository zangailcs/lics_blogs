---
title: OnJava05--集合、函数式编程、流
categories: 
- EnjoyCoding
- Java
- OnJava笔记
date: 2024-06-25 22:20:00
typora-root-url: ../ 
---

## OnJava05--集合、函数式编程、流

### 1、集合

Java集合类库是用来“持有”对象的，有两个基本接口：

- Collection：由单独元素组成的序列
- Map：一组键值对象对，用键来查找值

#### 小记

- 队列和栈的行为都是通过LinkedList提供的
- 栈：Java 1.0开始提供的Stack类设计很糟糕（继承了Vector ），推荐使用ArrayDeque代替：

```java
    Deque<String> stack = new ArrayDeque<>();
```

- Queue 队列：典型的先进先出集合，放入顺序和取出顺序一样。

  - LinkedList实现了Queue 接口，所以可以将其作为Queue的一种实现来使用：

    ```java
        Queue<Integer> queue = new LinkedList<>();
    ```

  - PriorityQueue：优先级队列，默认的排序方法是对象在队列中的**自然排序**，因此默认是小根堆



### 2、函数式编程

**面向对象编程抽象数据，函数式编程抽象行为。**

#### 2.1 lambda表达式

- lambda表达式产生的是函数，而不是类

- 基本语法

  1. 参数；

  2. 后面跟->，可以理解为“产生”

  3. ->后面的都是方法体

#### 2.2 方法引用

#### 2.3 函数式接口

- Java8引入了包含一组接口的java.util.function，其中的每个接口都只包含一个抽象方法，叫做**函数式方法**。

- 注解@FunctionalInterface：强制实施

  - 如果使用了该注解的接口中方法多于一个，编译会报错“Multiple non-overriding abstract methods found in interface xxx“
  - 使用了该注解的接口也叫**单一抽象方法**类型

- 函数式接口的基本类型变种 存在的原因：

  - 防止在传递参数和返回结果时涉及自动装箱和自动拆箱 -- 为了性能
  - eg：

  ```java
      Function<Integer, Double> fid = i -> (double)i;  // 使用基础的Function，必须进行强转
      IntToDoubleFunction fid2 = i -> i; // IntToDoubleFunction不需要强转
  ```

#### 2.4 高阶函数

**是一个能接受函数作为参数或返回值的函数。**

#### 2.5 闭包

```java
// 代码段1
// {WillNotCompile}
import java.util.function.*;

public class Closure1 {
  IntSupplier makeFun(int x) {
    int i = 0;
    // Neither x++ nor i++ will work:
    return () -> x++ + i++;
  }
}
```

上述代码段1编译会失败，提示：“Variable used in lambda expression should be final or effectively final”

而下面的代码段2可以编译：

```java
// 代码段2
import java.util.function.*;

public class Closure2 {
  IntSupplier makeFun(int x) {
    int i = 0;
    return () -> x + i;
  }
}
```

- 代码段2中，x和i并不是final的变量，却可以编译成功。**Effectively final（“实际上的最终变量”）**意义就出现在这里，这个术语是为Java 8创建的，意思是虽然未显式地将一个变量声明为最终变量，但仍然可以用最终变量的方式对待它。如果一个局部变量的初始值从不改变，就是Effectively final。
  - 所谓Effectively final，意味着可以在变量声明前加上final关键字，而不用修改其余代码。
  - 对于对象的引用，只要不重新指向其他对象就仍可以是Effectively final，可以修改对象本身。

#### 	2.6 函数组合

将多个函数结合使用，以创建新的函数。

- func.andThen(after)：先执行func，再执行after
- func.compose(before)：先执行before，再执行func
- pred.and(other)：短路与，pred && other
- pred.or(other)：短路或，pred || other
- pred.negate()：pred取反



### 3、流

#### 3.1 流的重要特性

- **内部迭代**
  - 外部迭代：显式编写（for、while等）
  - **内部迭代**：流编程的一个核心特性，优势：
    - 可读性更好
    - 更易利用多处理器：并行化机制会自动控制
- **惰性求值**：流只在绝对必要时才被求值，可以想象成“延迟列表”
  - 因此，流可以表示非常大甚至无限大的序列，而不用考虑内存问题

#### 3.2 流的创建

（1）Stream.of(T... values)：将一组条目变成一个流

（2）每个Collection都有stream()方法，生成一个流

（3）随机数流：

- Random类生成的是基本类型，boxed()流操作会自动装箱
- ints、longs、doubles都有多套方法签名（无参、限制上下限、限制流大小、同时限制流大小和边界），见代码示例

```java
public class RandomGenerators {
  public static <T> void show(Stream<T> stream) {
    stream
      .limit(4)
      .forEach(System.out::println);
    System.out.println("++++++++");
  }
  public static void main(String[] args) {
    Random rand = new Random(47);
    show(rand.ints().boxed());
    show(rand.longs().boxed());
    show(rand.doubles().boxed());
    // Control the lower and upper bounds:
    show(rand.ints(10, 20).boxed());
    show(rand.longs(50, 100).boxed());
    show(rand.doubles(20, 30).boxed());
    // Control the stream size:
    show(rand.ints(2).boxed());
    show(rand.longs(2).boxed());
    show(rand.doubles(2).boxed());
    // Control the stream size and bounds:
    show(rand.ints(3, 3, 9).boxed());
    show(rand.longs(3, 12, 22).boxed());
    show(rand.doubles(3, 11.5, 12.3).boxed());
  }
}
```

（4）Stream.generate(Supplier<T> s)：接受任何的Supplier<T> s，生成一个由T的对象组成的流

（5）Stream.iterate(final T seed, final UnaryOperator<T> f)：从seed开始，将其传给f，f的结果作为下一次调用f的入参。

（6）Stream.builder，流生成器，代码示例：一旦build()就不能再往里add()

```java
    Stream.Builder<String> builder = Stream.builder();
    builder.add("aaa");
    builder.add("bbb");
    // ... 其他方法，往builder里add
    builder.build().forEach(System.out::println);
    // builder.add("ccc");  报错：java.lang.IllegalStateException at java.util.stream.Streams$StreamBuilderImpl.accept
```

（7）Arrays.stream(T[] array) 将数组转为流

```java
    stream(T[] array)
    stream(T[] array, int startInclusive, int endExclusive) // 选取数组下标在[startInclusive,endExclusive)左开右闭区间内的元素生成流
```

（8）正则表达式，splitAsStream：

```java
    String charSeq = "Not much,of.a cheese? shop really, is it?";
    Pattern.compile("[ .,?]+").splitAsStream(charSeq).map(s -> s + " ").forEach(System.out::print);       
    // 输出：Not much of a cheese shop really is it 
```

### 3.3 中间操作

中间操作从一个流中接收对象，并将对象作为另一个流送出后端，连接到其他操作。

（1）peek()：跟踪与调试，接收遵循Consumer函数式接口的函数，没有返回值，所以只能“看看”对象

（2）sorted()：排序，可以按需指定比较器

```java
   xxx.stream()
    .sorted(Comparator.reverseOrder()) // 逆向自然顺序
    .forEach(System.out::print);
```

（3）移除元素：

- distinct()：移除重复元素
- filter(Predicate pred)：保留pred为true的元素

（4）映射

- map(Function)
- mapToInt(ToIntFunction)
- mapToLong(ToLongFunction)
- mapToDouble(ToDoubleFunction)

（5）flatMap()：扁平化，将元素流的流里的每个元素流展开为元素（扁平化），传出元素流

### 3.4 Optional类型

#### 3.4.1 基本概念

（1）既可以作为流元素来占位，又可以保障不存在要找的元素时不会抛出异常。

（2）以下流操作会返回Optional对象，因为无法确保它们的结果一定存在：

- findFirst
- findAny
- max、min
- reduce
- average

当流为空时，上述操作的结果均不存在，会返回Optional.empty。

（3）PS：创建空流的语法：

```java
    Stream.<String>empty();  // 如果没有上下文，且不加<String>，Java无法推断类型
    Stream<String> empty = Stream.empty();   // 有上下文，可以省略钻石符号
```

（4）Optional的两个基本动作：

- isPresent：判断里面是否有东西
- get：如果有东西，获取它

#### 3.4.2 便捷函数

简化了“先检查再处理所包含的对象”的过程

- ifPresent：如果值存在，就用这个值调用consumer，否则什么都不做

  ```java
  void ifPresent(Consumer<? super T> consumer)
  ```

- orElse：如果对象存在，则返回这个对象，否则返回other

  ```java
  T orElse(T other)
  ```

- orElseGet：如果对象存在，则返回这个对象，否则返回other函数创建的对象

  ```java
  T orElseGet(Supplier<? extends T> other)
  ```

- orElseThrow：否则抛出一个exceptionSupplier创建的异常

  ```java
  <X extends Throwable> T orElseThrow(Supplier<? extends X> exceptionSupplier) throws X
  ```

#### 3.4.3 创建Optional

Optional类中有下面三个静态方法，来创建Optional：

- empty()：空的Optional
- of(value)：已知value非空
- ofNullable（value）：不确定value是不是null
  - value是null时，自动返回Optional.empty
  - 否则将value包在Optional中

#### 3.4.4 Optional上的filter

- 普通的流filter：如果pred返回false，会从流中删除元素
- Optional.filter：如果pred返回false，不会删除元素，而是转化为empty

#### 3.4.5 由Optional组成的流

需要针对不同应用采取不同的方法，见代码示例：

```java
Signal.stream()
  .limit(10)
  .filter(Optional::isPresent) // 此处只保留了非empty的Optional
  .map(Optional::get) // 再map调用get来获取包在其中的对象
  .forEach(System.out::println);
```

### 3.5 终结操作

接受一个流，并生成一个最终结果。

（1）将流转换为数组：

- toArray()

（2）在每个元素上应用某个终结操作：

- forEach：

```java
  void forEach(Consumer<? super T> action); // parallel()时，不确保操作元素的顺序，可以最大程度地利用并行流提高效率
```

```java
  void forEachOrdered(Consumer<? super T> action); // parallel()时，也确保顺序是原始流的顺序
```

（3）收集操作：

- collect(Collector)：若java.util.stream.Collectors里没有想要的集合类型，可以使用Collectors.toCollection，并将任何类型的Collection的构造方法引用传递给它：

```java
        Set<String> words2 =
                Stream.of("Arrays", "Collectors", "Exception", "is", "Output")
                        .filter(s -> s.length() > 2)
                        .limit(100)
                        .collect(Collectors.toCollection(TreeSet::new));
        System.out.println(words2);
```

（4）组合所有的流元素：
- reduce：
```
  Optional<T> reduce(BinaryOperator<T> accumulator)

  T reduce(T identity, BinaryOperator<T> accumulator);
```

区别在于下面的方法会将identity作为初值，即使流是空的，也能返回identity。

（5）匹配，下面的api都是短路计算，即遇到一个不符合的元素时，立即返回false

- allMatch：
```java
  boolean allMatch(Predicate<? super T> predicate);  // 流中所有元素，predicate都为true
```

- anyMatch：
```java
  boolean anyMatch(Predicate<? super T> predicate);  // 流中存在任一元素，predicate为true
```

- noneMatch：
```java
  boolean noneMatch(Predicate<? super T> predicate); // 流中所有元素，predicate都为false
```

（6）选择一个元素

- findFirst：
```java
  Optional<T> findFirst();
```

- findAny：
```java
  Optional<T> findAny();
```

如果需要选择流的最后一个元素，需要使用reduce：

```java
Optional<String> lastobj =
      Stream.of("one", "two", "three")
        .reduce((n1, n2) -> n2);
```

（7）获得流的相关信息

- count：
```java
  long count();  // 流中元素个数
```

- max：
```java
  Optional<T> max(Comparator<? super T> comparator);  // 最大元素
```

- min：
```java
  Optional<T> min(Comparator<? super T> comparator);  // 最小元素
```

数值类型的流还可以计算均值、求和等：

```java
    private static int[] rints =
            new Random(47).ints(0, 1000).limit(100).toArray();

    public static IntStream rands() {
        return Arrays.stream(rints);
    }

    public static void main(String[] args) {
        System.out.println(rands().average().getAsDouble());
        System.out.println(rands().max().getAsInt());
        System.out.println(rands().min().getAsInt());
        System.out.println(rands().sum());
        System.out.println(rands().summaryStatistics());
    }

/** 输出：
 * 507.94
 * 998
 * 8
 * 50794
 * IntSummaryStatistics{count=100, sum=50794, min=8, average=507.940000, max=998}
 */
```

