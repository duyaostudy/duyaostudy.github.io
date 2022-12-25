---
title: Mybatis
date: 2022-12-25 22:31:38
tags: [mybatis,框架]
categories: [后端,mybatis]
toc: true
---

# MyBatis

## 1. 搭建Mybaits

### 1. maven包配置文件

```xml
<dependency>
    <groupId>org.mybatis</groupId>
    <artifactId>mybatis</artifactId>
    <version>3.5.7</version>
</dependency>


<dependency>
 <groupId>junit</groupId>
 <artifactId>junit</artifactId>
 <version>4.12</version>
 <scope>test</scope>
</dependency>

<dependency>
 <groupId>mysql</groupId>
 <artifactId>mysql-connector-java</artifactId>
</dependency>
```

<!--more-->

### 2. mybaits配置文件

1. xml配置文件

```xml
   <?xml version="1.0" encoding="UTF-8" ?>
   <!DOCTYPE configuration
           PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
           "http://mybatis.org/dtd/mybatis-3-config.dtd">
   <configuration>
   <!--    配置连接数据库环境-->
       <environments default="development">
           <environment id="development">
   <!--    事务管理器-->
               <transactionManager type="JDBC"/>
   <!--    数据库连接池-->
               <dataSource type="POOLED">
                   <property name="driver" value="com.mysql.jdbc.Driver"/>
                   <property name="url" value="jdbc:mysql://localhost:3306/mybatis"/>
                   <property name="username" value="root"/>
                   <property name="password" value="root"/>
               </dataSource>
           </environment>
       </environments>
   <!--    引入映射文件-->
       <mappers>
   <!--外部映射文件-->   
           <mapper resource="org/mybatis/example/BlogMapper.xml"/>
       </mappers>
   </configuration>
```

### 3. 创建mapper接口

> myBatis中的mapper接口相当于dao，区别：mapper仅仅是接口，不需要提供实现类

### 4. 创建myBatis映射文件

1. 相关概念：ORM 对象关系映射

   1. 对象：java实体类对象
   2. 关系：关系型数据库
   3. 映射：二者之间的对应关系

2. | java概念 | 数据库概念 |
   | -------- | ---------- |
   | 类       | 表         |
   | 属性     | 字段\列    |
   | 对象     | 记录\行    |


    1. 映射文件命名规则
       
       1. 表对应的实体类的类名+Mapper.xml
       2. 一个映射文件对应一个实体类、一张表的操作
       3. MyBatis用于编写SQL、访问以及操作表中的数据
       
    2. 映射文件
       
      ```xml
          <?xml version="1.0" encoding="UTF-8" ?>
          <!DOCTYPE mapper
                  PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
                  "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
          <!--namespace是接口-->
          <mapper namespace="org.mybatis.example.BlogMapper">
          
          </mapper>
          ```
       
    3. MyBatis面向接口编程的两个一致
       
       1. 映射文件要与mapper接口全类名一致
       2. 映射文件sql的id要与mapper接口中的方法名一致
       3. 表--实体类--mapper接口--映射文件

### 4.增删改查

#### 添加

1. mapper接口类

```java
public interface UserMapper {
  
    int insertUser(User user);
}
```

2. xml映射文件

```xml
<insert id="insertUser">
    insert into t_user values (null,'root', 'root', 23, '男', '123456@qq.com')
</insert>
```

3. 启动类

```java
//加载核心配置文件
InputStream is = Resources.getResourceAsStream("mybatis-config.xml");
//构建sqlSessionFactoryBuilder
SqlSessionFactoryBuilder sqlSessionFactoryBuilder = new SqlSessionFactoryBuilder();
//获取sqlsession工厂对象
SqlSessionFactory sqlSessionFactory = sqlSessionFactoryBuilder.build(is);
//获取数据库的会话对象,相当于javaweb session对象
SqlSession sqlSession = sqlSessionFactory.openSession();
//获取UserMapper对象	底层是代理模式
UserMapper mapper = sqlSession.getMapper(UserMapper.class);
int i = mapper.insertUser();
//提交事务
sqlSession.commit();
```

优化：

1. sqlSessionFactory.openSession(true);
2. 默认提交事务

只需要修改maopper.xml文件即可

#### 修改

```xml
<update id="update">
    update t_user set username='阿三' where id = 14
</update>
```

#### 删除

```xml
<delete id="delet">
    delete from t_user where id = 16
</delete>
```

#### 查询

根据Id查询返回一个对象值

属性要添加一个返回值

1. resultType:设置结果返回默认映射关系，值为对象全类名
2. resultMap:设置自定义映射关系

```xml
<select id="queryId" resultType="com.dy.spring.Po.User">
    select * from t_user where id = 14
</select>
```

## 2. Mybatis核心配置文件说明（了解）

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>
<!--    执行log4j日志-->
    <settings>
        <setting name="logImpl" value="LOG4J"/>
    </settings>
    <!--    environments配置连接数据库环境-->
    <!--    default：表示默认使用环境id
            id:表示连接数据环境唯一标识（不能重复）
    -->
    <environments default="development">
        <environment id="development">
<!--    设置事务管理方式
        type值：
            JDBC:表是当前坏境中，执行SQL中使用的是原生事务管理方法，事务提交
            MANAGED:将mybatis交由spring管理
-->
            <transactionManager type="JDBC"/>
<!--    数据库连接池
        type:设置数据源类型
            POLLED:使用数据库连接池缓存数据库连接
            UNPOLLED:表示不使用数据库连接池
            JNDI:表示使用上下文中的数据源
-->
            <dataSource type="POOLED">
<!-- 设置连接数据库的驱动-->
                <property name="driver" value="com.mysql.cj.jdbc.Driver"/>
                <property name="url" value="jdbc:mysql://localhost:3306/mybatis"/>
                <property name="username" value="root"/>
                <property name="password" value="root"/>
            </dataSource>
        </environment>
    </environments>
    <!--    引入映射文件-->
    <mappers>
        <mapper resource="mapper/UserMapper.xml"/>
    </mappers>
</configuration>
```

### 1. 引入数据库驱动属性文件

jdbc.properties

```properties
prpo.driverClass=com.mysql.jdbc.Driver
prpo.url=jdbc:mysql://localhost:3306/mybatis
prpo.username=root
prpo.password=root
```

引入方式

```xml
<properties resource="jdbc.properties"/>
```

### 2. 设置类型别名

```xml
<typeAliases>
    <typeAlias type="com.dy.spring.Po.User" alias="User"></typeAlias>
</typeAliases>
```

> 类型别名不区分大小写
>
> 不设置alias，别名默认是类名且不区分大小写

```xml
<package name="com.dy.spring.Po"/>
```

> 以包为单位，将包下所有的类型设置为默认的类型别名，不区分大小写
>
> 要求：
>
> 1. mapper接口所在包名要与映射文件路径一致
> 2. mapper接口名要与映射文件名一致

## 3. Mybatis获取参数的两种方式

${}与#{}

${}的本质是字符串拼接

> 字符串拼接时要加入''号

#{}的本质是占位符赋值

### 1. 单个字面量赋值

```xml
<!--    User queryId(int id)-->
<select id="queryId" resultType="user">
    select * from t_user where id=#{id};
</select>
```

> 单个变量时与名称无关，与参数传入顺序有关

### 2. 多个字面赋值

```xml
<!--    User checkLogin(String name, String password);-->
<select id="checkLogin" resultType="user">
    select * from t_user where username=#{arg0} and password=#{arg1};
</select>
```

Mybati会将多个参数放入一个map集合中，以arg0...或者param1...为键，传入参数为值

只需通过#{键}或者${键}方式来进行访问

### 3. map集合赋值

```xml
<!--    User checkLoginMap(Map<String, Object> map); -->
    <select id="checkLoginMap" resultType="user">
        select * from t_user where username=#{username} and password=#{password};
    </select>
```

> 也可以自己创建map传入参数

```java
Map<String,Object> map = new HashMap<>();
map.put("username","李四");
map.put("password","root");
```

> map集合

### 4. 实体类型参数

```xml
<!--    int insertUser(User user); -->
<insert id="insertUser">
    insert into t_user values(null,#{username},#{password},#{age},#{sex},#{email});
</insert>
```

> #{}里属性是根据类里的get和set方法去调用，name为去掉get与set首字母小写后的情况

### 5.命名参数

在方法参数前使用@Param注解

```xml
<!-- User checkLoginByParam(@Param("username") String username, @Param("password") String password);-->
<select id="checkLoginByParam" resultType="user">
    select * from t_user where username=#{username} and password=#{password};
</select>
```

> @Param相当于将注解的值作为map集合里的键值

## 4. 查询功能

> 查询时，返回的结果为多个时，接口不能使用对象为返回值

1. 查询返回一条数据时
   1. List接受
   2. 实体类对象接受
   3. map接受
2. 查询返回多条数据时
   1. 通过实体类型的List类型接受
   2. 通过map类型的List集合接受
   3. 在接口方法上添加@MapKey("")注解，将每条数据转换为map集合作为值，注解的属性为键

### 1. 返回值为基本类型时

```xml
<!--  Integer selectCount() -->
<select id="selectCount" resultType="Integer">
    select count(*) from t_user;
</select>
```

1. Mybatis中设置了默认别名

> java.lang.Integer --> int,Integer
>
> int --> _int, _integer（int设置了两个默认别名）

1. 大部分为基础数据类型

### 2. map返回多个查询数值

映射文件设置

```xml
<!--  Map<String, Object> selectAll();-->
<select id="selectAll" resultType="map">
    select * from t_user;
</select>
```

接口设置

```java
@MapKey("id")
Map<String, Object> selectAll();
```

返回多个查询结果和map

## 5. 特殊SQL 的执行

### 1. 模糊查询

### 2. 批量删除

```xml
<!--int deleteUsers(@Param("ids") String ids);-->
<delete id="deleteUsers">
    delete from t_user where id in(${ids});
</delete>
```

只能通过${}获取参数，#{}会自带''符号

### 3. 动态设置表名

根据表名返回数据

```xml
<!-- List<User> getUserByTableName(@Param("tableName") String tableName); -->
    <select id="getUserByTableName" resultType="User">
        select * from ${tableName};
    </select>
```

### 4.设置自动递增

1.keyProperty中对应的值是实体类的属性，而不是数据库的字段。
2.添加该属性之后并非改变**insert**方法的返回值，也就是说，该方法还是返回新增的结果。而如果需要获取新增行的主键ID，直接使用传入的实体对象的主键对应属性的值。

```xml
<insert id="insertUser" useGenertedKeys="true" keyProperty="id">
    insert into t_user values (null,'root', 'root', 23, '男', '123456@qq.com')
</insert>
```

将id赋值在传入过来的对象里

> useGenertedKeys="true"
>
> 设置当前标签sql使用自增id
>
> keyProperty="id"
>
> 键自增主键的值赋值给传输到映射文件中的值

## 6. Mybatis框架

### 1. 解决字段名和属性不一致的情况

SQL中的解决办法：

```mysql
select user_name name, id from t_user where id=?
```

> 通过空格为属性名起别名

Mybatis全局配置文件解决:

```xml
<!-- 将mysql属性中的_自动映射为驼峰-->
<settings>
	<setting name="mapUnderscoreToCamelCaseEnables" value="true"/>
</settings>
```

> 例子：emp_name 映射为 emoName
>
> 字段名要与属性映射一致

### 2. 自定义映射关系

```xml
<!--自定义映射关系-->
    <resultMap id="userResultMap" type="User">
        <id property="id" column="id"></id>
        <result property="username" column="user_name"></result>
        <result property="password" column="password"></result>
        <result property="age" column="age"></result>
        <result property="sex" column="sex"></result>

    </resultMap>
```

夫标签：

    id：设置映射名
    
    type：设置映射关系中的实体类型

子标签：

    id：主键映射关系
    
    result：属性映射关系
    
    property: 属性名，必须时type属性所设置的实体类型中的属性名
    
    column：数据库字段名，必须时sql语句查询出的字段名

<--to 动态sql语句-->
