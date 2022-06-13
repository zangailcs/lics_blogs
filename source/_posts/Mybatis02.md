---
title: Mybatis笔记02
categories: Mybatis
typora-root-url: ../
---

### Mybatis笔记02

#### 1. CRUD

（1）**namespace**: namespace中的包名要和Dao/mapper接口的包名一致！

（2）**select**: 选择/查询语句；

- id：就是对应的namespace中的方法名；
- resultType: Sql语句执行的返回值
- parameterType：参数类型

   **步骤**：


1. 编写接口

   ```java
   // 根据id查询用户
   User getUserById(int id);
   ```

2. 编写对应的mapper中的sql语句

   ```xml
   <select id="getUserById" parameterType="int" resultType="com.lics.pojo.User">
       select * from mybatis.user where id = #{id}
   </select>
   ```

3. 测试

   ```java
   @Test
   public void test02() {
       // 获取sqlSession对象
       SqlSession sqlSession = MybatisUtils.getSqlSession();
   
       // 执行SQL
       UserMapper mapper = sqlSession.getMapper(UserMapper.class);
       User user = mapper.getUserById(1);
       System.out.println(user);
       sqlSession.close();
   }
   ```

​    （3）增

      ```xml
      <insert id="addUser" parameterType="com.lics.pojo.User">
          insert into mybatis.user (id, name, pwd) values
          (#{id}, #{name}, #{pwd})
      </insert>
      ```

​    （4）删

```xml
<delete id="deleteUser" parameterType="int">
    delete from mybatis.user
    where id=#{id};
</delete>
```

​    （5）改

```xml
<update id="updateUser" parameterType="com.lics.pojo.User">
    update mybatis.user
    set name = #{name}, pwd=#{pwd}
    where id=#{id};
</update>
```



**<font color="red">注意： 增删改需要提交事务！ </font>**

#### 2. 核心配置

MyBatis 的配置文件包含了会深深影响 MyBatis 行为的设置和属性信息。 配置文档的顶层结构如下：

```xml
configuration（配置）
    properties（属性）
    settings（设置）
    typeAliases（类型别名）
    typeHandlers（类型处理器）
    objectFactory（对象工厂）
    plugins（插件）
    environments（环境配置）
    environment（环境变量）
    transactionManager（事务管理器）
    dataSource（数据源）
    databaseIdProvider（数据库厂商标识）
    mappers（映射器）
```

##### 2.1 环境配置（environments）

Mybatis可以配置成适应多种环境

**不过要记住：尽管可以配置多个环境，但每个 SqlSessionFactory 实例只能选择一种环境。**

为了指定创建哪种环境，只要将它作为可选的参数传递给 SqlSessionFactoryBuilder 即可。可以接受环境配置的两个方法签名是：

```java
SqlSessionFactory factory = new SqlSessionFactoryBuilder().build(reader, environment);
SqlSessionFactory factory = new SqlSessionFactoryBuilder().build(reader, environment, properties);
```

如果忽略了环境参数，那么将会加载默认环境，如下所示：

```java
SqlSessionFactory factory = new SqlSessionFactoryBuilder().build(reader);
SqlSessionFactory factory = new SqlSessionFactoryBuilder().build(reader, properties);
```

Mybatis默认的事务管理器：JDBC， 连接池：POOLED

##### 2.2 属性（properties）

这些属性可以在外部进行配置，并可以进行动态替换。既可以在典型的 Java 属性文件中配置这些属性，也可以在 properties 元素的子元素中设置【db.properties】。

编写一个配置文件db.properties

```properties
driver=com.mysql.jdbc.Driver
url=jdbc:mysql://localhost:3306/mybatis?useSSL=false&useUnicode=true&characterEncoding=UTF-8
username=root
password=***
```

也可以在核心配置文件中引入

```xml
<!-- 引入外部配置文件-->
<properties resource="db.properties">
    <property name="username" value="root"/>
    <property name="password" value="111"/>
</properties>
```

- 可以直接引入外部文件

- 可以在<properties> </properties>中增加一些属性配置

- 如果上面两处有同一个字段，优先使用外部文件中的！

     详细说明如下：

     如果一个属性在不只一个地方进行了配置，那么，MyBatis 将按照下面的顺序来加载：

  - 首先读取在 properties 元素体内指定的属性。
  - 然后根据 properties 元素中的 resource 属性读取类路径下属性文件，或根据 url 属性指定的路径读取属性文件，并**覆盖**之前读取过的同名属性。
  - 最后读取作为方法参数传递的属性，并**覆盖**之前读取过的同名属性。

   因此，通过方法参数传递的属性具有最高优先级，resource/url 属性中指定的配置文件次之，最低优先级的则是 properties 元素中指定的属性。

##### 2.3 类型别名（typeAliases）

类型别名可为 Java 类型设置一个缩写名字，意在降低冗余的全限定类名书写。

```xml
<typeAliases>
    <typeAlias type="com.lics.pojo.User" alias="User"/>
</typeAliases>
```

也可以指定一个包名，mybatis会在包名下搜索需要的Java Bean，比如：

扫描实体类的包，它的默认别名就是这个类的 类名，首字母小写。

```xml
<typeAliases>
    <package name="com.lics.pojo"/>
</typeAliases>
```



























