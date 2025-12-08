---
title: OnJava06--异常、文件
categories: 
- EnjoyCoding
- Java
- OnJava笔记
date: 2024-07-08 22:20:00
typora-root-url: ../ 
---

# OnJava06--异常、文件

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

#### （6）try-with-resources语句

以下场景：

- 需要清理
- 需要在特定的时刻清理--当走出某个作用域的时候

```java
        try (InputStream in = Files.newInputStream(new File("filePath").toPath())) {
            int contents = in.read();
            // Process contents
        } catch (IOException e) {
            // Handle errors
        }
```

上面的代码示例中：

- try后的()内的内容即是**资源说明头**。 

- in在try块的其他部分（try内、catch子句内）都是可用的

- 最重要的是：**不管如何退出try，都会执行类似下面的finally子句等同的操作，来关闭in**：

  ```java
      finally {
          if (in != null) {
              try {
                  in.close();
              } catch (IOException e) {
                  // 处理close()错误
              }
          }
      }
  ```

- 工作原理：资源说明头中创建的对象必须实现java.lang.AutoCloseable接口，接口中只有一个方法close()
- 资源说明头中可以包含多个定义，用分号隔开
- try-with-resources的try块可以独立存在，没有catch或finally，此时异常会被传递出去。

#### （7）异常使用指南

下面是使用异常的一些指导原则：

1. 尽可能使用 try-with-resources.
2. 要在恰当的层次处理问题。( 除非知道怎么处理，否则不要捕捉异常。)
3. 可以使用异常来修复问题，并重新调用引发异常的方法。
4. 可以选择做好补救措施然后继续，不再重新尝试引发异常的方法。
5. 可以借助异常处理的过程计算出某个结果，以代替该方法应该生成的值。
6. 可以在当前上下文中把能做的事情都做了，然后将**相同**的异常重新抛出，使其进入更上层的上下文中。
7. 可以在当前上下文中把能做的事情都做了，然后重新抛出一个**不同**的异常，使其进人更上层的上下文中。
8. 使用异常来终止程序。
9. 使用异常来简化问题。( 如果你的异常模式使问题更复杂了，用起来会非常麻烦。)
10. 使用异常让我们的库和程序更安全。( 这既是为调试所做的短期投资，也是为程序的稳健性所做的长期投资。)



### 2、文件

#### 2.1 文件或目录的路径--Path

（1）Files工具类中包含一整套用于检查Path的各种信息的方法。

```java
import java.nio.file.*;
import java.io.IOException;

public class PathAnalysis {
  static void say(String id, Object result) {
    System.out.print(id + ": ");
    System.out.println(result);
  }
  public static void
  main(String[] args) throws IOException {
    System.out.println(System.getProperty("os.name"));
    Path p =
      Paths.get("PathAnalysis.java").toAbsolutePath();
    say("Exists", Files.exists(p));
    say("Directory", Files.isDirectory(p));
    say("Executable", Files.isExecutable(p));
    say("Readable", Files.isReadable(p));
    say("RegularFile", Files.isRegularFile(p));
    say("Writable", Files.isWritable(p));
    say("notExists", Files.notExists(p));
    say("Hidden", Files.isHidden(p));
    say("size", Files.size(p));
    say("FileStore", Files.getFileStore(p));
    say("LastModified: ", Files.getLastModifiedTime(p));
    say("Owner", Files.getOwner(p));
    say("ContentType", Files.probeContentType(p));
    say("SymbolicLink", Files.isSymbolicLink(p));
    if(Files.isSymbolicLink(p))
      say("SymbolicLink", Files.readSymbolicLink(p));
    if(FileSystems.getDefault()
       .supportedFileAttributeViews().contains("posix"))
      say("PosixFilePermissions",
        Files.getPosixFilePermissions(p));
  }
}
/* Output:
Windows 10
Exists: true
Directory: false
Executable: true
Readable: true
RegularFile: true
Writable: true
notExists: false
Hidden: false
size: 1617
FileStore: (C:)
LastModified: : 2021-11-08T00:34:52.693768Z
Owner: GROOT\Bruce (User)
ContentType: text/plain
SymbolicLink: false
*/

```

（2）递归删除：

```java
import java.nio.file.*;
import java.nio.file.attribute.BasicFileAttributes;
import java.io.IOException;

public class RmDir {
    public static void rmdir(Path dir)
            throws IOException {
        Files.walkFileTree(dir,
                new SimpleFileVisitor<Path>() {
                    @Override
                    public FileVisitResult
                    visitFile(Path file, BasicFileAttributes attrs)
                            throws IOException {
                        Files.delete(file);
                        return FileVisitResult.CONTINUE;
                    }

                    @Override
                    public FileVisitResult
                    postVisitDirectory(Path dir, IOException exc)
                            throws IOException {
                        Files.delete(dir);
                        return FileVisitResult.CONTINUE;
                    }
                });
    }
}

```

SimpleFileVisitor使用了访问者设计模式，包含以下方法：

- preVisitDirectory(T dir, BasicFileAttributes attrs) :先在当前目录上运行，然后进入这个目录下的文件和目录
- visitFile(T file, BasicFileAttributes attrs)：在这个目录下的每个文件上运行
- visitFileFailed(T file, IOException exc)：文件无法访问时调用
- postVisitDirectory(T dir, IOException exc)：先进入当前目录下的文件和目录（包括所有的子目录），最后在当前目录上运行

（3）监听Path：WatchService，对某个目录的变化做出反应

```java
    Path test = Paths.get("test");
    WatchService watcher =
      FileSystems.getDefault().newWatchService();
    test.register(watcher, ENTRY_DELETE);

    WatchKey key = watcher.take();
    for(WatchEvent evt : key.pollEvents()) {
      System.out.println(
        "evt.context(): " + evt.context() +
        "\nevt.count(): " + evt.count() +
        "\nevt.kind(): " + evt.kind());
      System.exit(0);
    }
```

- 事件类型包括：ENTRY_CREATE、ENTRY_DELETE、ENTRY_MODIFY，分别代表文件的创建删除和修改。
- 注意：在目录上注册WatchService后，只监听这个目录，不包含子目录下的文件。

#### 2.2 读写文件

- Files.readAllLines：一次性读入整个文件，生成List\<String\>
- Files.lines：将文件变为行组成的Stream -- 可以处理大文件或读取文件的一部分。
