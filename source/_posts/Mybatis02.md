---
title: Mybatis笔记02
categories: Mybatis
typora-root-url: ../
---

### Mybatis笔记02

##### 1. CRUD

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
