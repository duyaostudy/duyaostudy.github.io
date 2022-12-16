---
title: Sping5
tags: java
categories: 后端
---

> 文章测试，布局问题待解决

## 1.1 概念概述

> 1. 是一个轻量级开源JavaEE框架
> 2. Spring可以解决企业应用开发的复杂性
> 3. Spring两核心： IOC和Aop
>    1. IOC：控制反转，把创建对象的过程交给Spring管理
>    2. Aop：面向切面，不修改源代码的情况下对功能进行增强
> 4. Spring特点
>    1. 方便解耦，简化开发
>    2. Aop编程支持
>    3. 方便程序测试
>    4. 方便集成各种优秀框架
>    5. 降低JavaEE的使用难度
>    6. 方便进行事务的操作
>    7. 降低API开发难度
>    8. Java源码的经典示范

## IOC容器

### 1 IOC

#### 1. 什么是IOC：

> 1. 容器
> 2. 控制反转：将对象创建和调用都交给spring进行管理
> 3. 目的：降低耦合度

#### 2. 底层原理

> 1. xml解析、工厂模式、反射
>
>    1. 工厂模式：
>
>       1. ```java
>          class Userservice{
>              execute(){
>                  UserDao dao = UserFactory.getDao();
>                  dao.add();
>              }
>          }
>
>          class UserDao{
>              add(){
>                  ....
>              }
>          }
>
>          class UserFactory{
>              public static UserDao getDao(){
>                  return new UserDao();
>              }
>          }
>          ```
>       2. 目的：让耦合度降低到最低限度

#### 3. IOC过程：

> 1. xml配置文件，配置创建的对象
>
>    1. ```xml
>       <bean id="dao" class="com.dy.UserDao"></bean>
>       ```
> 2. 有service类和dao类，创建工厂类
>
>    1. ```java
>       class UserFactory{
>           public static UserDao getDao(){
>               String classValue = class属性值;//1. xml解析
>               Class clazz = Class.forName(classValue);// 2. 通过反射创建对象
>               return clazz.newInstance();
>
>           }
>       ```

#### 4. IOC接口

> 1. IOC思想基于IOC容器完成，IOC容器底层就是对象工厂
> 2. Spring提高IOC容器实现的两种方式（两种接口）：
>    1. BeanFactory：IOC容器基本实现，spring内部使用接口，一般不提供给开发人员
>       1. 加载配置文件时不会创建对象，获取对象时才去创建对象
>    2. ApplicationContext：BeanFactory接口的子接口，提高更多更强大的功能，一般使用与开发人员
>       1. 加载配置文件时就会把配置文件创建了
>    3. ApplicationContext实现类：
>       1. ![image-20220326112518394](C:\Users\86152\AppData\Roaming\Typora\typora-user-images\image-20220326112518394.png)
>          1. Class类是相当于xml读取相对路径
>          2. File类相当于xml读取绝对路径

### 2 IOC操作Bean管理

#### Bean管理

> 1. Spring创建对象
> 2. Spring注入属性
> 3. Bean管理操作有两种方式
>    1. 基于xml配置文件方式实现
>    2. 基于注解方式实现

#### IOC管理方式实现

##### 1. 基于xml方式实现

> 1. id属性：唯一标识，
> 2. class属性：类全路径（包类路径）
> 3. name属性：和id属性类似，但不能使用特殊字符（现不常用）
> 4. 创建对象时默认执行的无参数构造方法完成创建**
>
> ```java
> <bean id="user" class="com.dy.spring.study.User"></bean>
> ```

##### 2. 基于xml方式注入属性

> 1. DI：依赖注入，注入属性
> 2. 先创建对象，再注入属性

##### 3.set方法注入

> 1. 创建Book对象，用set方法进行属性注入
> 2. name属性名称 value属性值
> 3. ```Java
>
>    ```
>
> <bean id="book" class="com.dy.spring.study.Book">
>    //使用property完成属性注入  
>        <property name="bname" value="hello world"></property>
>        <property name="bauthor" value="dy"></property>
>    </bean>
>    ```
>
> 6. 默认找Book方法里面的set方法，set方法不一致就会报错

##### 4. 有参构造注入

> 1. 创建类、定义属性、定义属性的构造方法
> 2. 配置spring文件
> 3. ```java
>    <!--有参数构造注入属性-->
>     <!--constructor_org进行参数注入-->
>           <bean id="orders" class="com.dy.spring.study.Orders">
>               //属性名称注入属性
>                   <constructor-arg name="address" value="中国"></constructor-arg>
>               //数组下标位置注入属性
>                   <constructor-arg index="0" value="1002"></constructor-arg>
>           </bean>
>    ```
>
> ```
>
>
> 8. p名称空间注入（了解）
>
> ```

##### 5. 简化基于xml配置的方式

> 1. 添加p名称空间再xml文件当中
> 2. ```java
>    <beans xmlns="http://www.springframework.org/schema/beans"
>           xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
>           xmlns:p="http://www.springframework.org/schema/p"
>           xsi:schemaLocation="http://www.springframework.org/schema/beans
>            https://www.springframework.org/schema/beans/spring-beans.xsd">
>    ```
> 3. 进行属性注入
> 4. ```java
>     <bean id="book" class="com.dy.spring.study.Book" p:bname="阿三" p:bauthor="无名狮子"</bean>
>    ```
> 5. 底层基于set方法注入属性
> 6. DI是IOC的一种具体实现

#### xml注入其他类型属性：

> 1. 字面量
> 2. 空值
>
> ```java
>    <property name="bname">
>       <null></null>
>    </property>
>  //属性值赋值为空值  
> ```
>
> 1. 属性值包含特殊值
> 2. 把特殊符合进行转义
> 3. <> 特殊字符
>
> ```java
>    < >
> ```
>
> 3. 把特殊符号内容写道CDATA
> 4. 
>
> ```xml
> <property name="bauthor">
>            <value><![CDATA[<<南京>>]]></value>
>  </property>
> ```
>
> 1. <![CDATA[<<南京>>]]>

##### 注入属性-外部bean

> 1. 先创建dao,service类里导入dao类，在bean里注入外部类
> 2. ```xml
>    <bean id="userService" class="com.dy.spring.service.UserService">
>         <!-- 
>             name中添加的是属性名称
>             ref:创建userDao对象bean标签的id值
>         -->
>         //接口类
>    	<property name="userdao" ref="userDaoImp"></property>
>    </bean>
>
>                                                    //实现类
>     <bean id="userDaoImp" class="com.dy.spring.dao.UserDaoImp"></bean>
>    ```

##### 注入属性-内部bean和级联赋值

> 1. 一对多关系
> 2. 内部bean
> 3. bean文件配置
> 4. ```xml
>    <!--    内部bean-->
>    <bean id="emp" class="com.dy.spring.bean.Emp">
>
>       <!--    设置两个普通属性-->
>       <property name="ename" value="张三"></property>
>       <property name="genderl" value="男"></property>
>       <property name="dep">
>           <!--    在属性中引入需要的bean-->
>           <bean id="dep" class="com.dy.spring.bean.Dept">
>           	<property name="dname" value="财务"></property>
>            </bean>
>       </property>
>    </bean>
>    ```
> 5. emp类属性
> 6. ```
>    private String ename;
>    private String genderl;
>    //  员工属于某一个部门
>    private Dept dep;
>    ```
> 7. 级联赋值
>
>    1. ```xml
>       <bean id="emp" class="com.dy.spring.service.Emp">
>       	<property name="dep" ref="dep"></property>
>       </bean>
>       <bean id="dep" class="com.dy.spring.dao.Dept">
>       	<property name="dname" value="财务"></property>
>       </bean>
>       ```

##### 注入集合属性

> 1. 注入数组
> 2. 注入List集合类型属性
> 3. 注入Map集合类型属性
> 4. ```xml
>    <!--    集合注入-->
>            <bean name="stu" class="com.dy.spring.collectiontype.Student">
>    <!--    数组属性注入-->
>                <property name="course">
>                    <array>
>                        <value>java课程</value>
>                        <value>数据库课程</value>
>                    </array>
>                </property>
>    <!--    list属性注入-->
>                <property name="name">
>                    <list>
>                        <value>张三</value>
>                        <value>王二</value>
>                    </list>
>                </property>
>    <!--    Map属性注入-->
>                <property name="maps">
>                    <map>
>                        <entry key="JAVA" value="java"></entry>
>                        <entry key="PHP" value="php"></entry>
>                    </map>
>                </property>
>    <!--    set属性注入-->
>                <property name="sets">
>                    <set>
>                        <value>1</value>
>                        <value>2</value>
>                    </set>
>                </property>
>            </bean>
>    ```
> 5. 在集合里面设置对象类型值
> 6. 与级联赋值同理
>
> ```xml
>  <property name="list">
>  	<list>
>       	<ref bean="course1"></ref>
>        <ref bean="course2"></ref>
>       </list>
>   </property>
> ```
>
> 2. 
> 3. 把集合中属性提取出来作为公共部分
> 4. 在spring配置空间引入util
>
> ```xml
> xsi:schemaLocation="http://www.springframework.org/schema/beans https://www.springframework.org/schema/beans/spring-beans.xsd
> http://www.springframework.org/schema/util 
> https://www.springframework.org/schema/util/spring-beans.xsd"
> ```
>
> 2. 地址配置
> 3. 提取list集合类型属性注入
>
> ```xml
>   <!--    提取list集合类型属性注入-->
> <util:list id="bookList">
>     <value>java基础</value>
>     <value>javaspring</value>
>     <value>springboot</value>
> </util:list>
>
> <bean id="book" class="com.dy.spring.study.Book">
>     <property name="bname" ref="bookList"></property>
>     <property name="bauthor" value="中国"></property>
> </bean>
> ```

#### FactoryBean

> 1. 普通beans:定义类型就是返回类型
> 2. 工厂beans:在配置文件定义benas类型可以和返回类型不一样
>
>    1. 方法接口为 FactoryBean，修改getObject方法作为返回值类型
>    2. ```Java
>       public class MyBean implements FactoryBean<Course> {
>           @Override
>           public Course getObject() throws Exception {
>               Course course = new Course();
>               course.setName("javaee基础");
>               return course;
>           }
>       ```
>    3. xml文件引用
>    4. ```xml
>       <bean id="mybean" class="com.dy.spring.bean.MyBean"></bean>
>       ```
>    5. 方法实现
>    6. ```java
>       public void MyBean(){
>
>           ApplicationContext context = new ClassPathXmlApplicationContext("beans.xml");
>           Course course = context.getBean("mybean", Course.class);
>           System.out.println(course);
>       }
>       ```

#### bean作用域

> 1. spring默认单实例对象，每次加载后只创建一个对象
> 2. 多实例对象：
>
>    ```xml
>    <bean id="book" class="com.dy.spring.study.Book" scope="singleton"></bean>
>    ```
>
>    1. bean里的scope属性里的值
>       1. singleton(默认)：单实例
>          1. 创建spring配置文件时候就会创建一个单实例对象
>       2. protorype：创建多实例
>          1. 在调用getBean方法后创建实例对象
>       3. request：创建对象时，将对象放入到request中
>       4. session：创建对象时，将对象放入到session中

#### 生命周期

> 对象创建到销毁的过程
>
> 1. bean的生命周期：
>
>    1. 通过构造器创建bean实例（通过无参数构创建）
>    2. 对bean属性设置和对其他bean应用（调用set方法）
>       1. 把bean实例传递给后置处理器的方法（后置处理器）
>    3. 调用bean初始化方法        （需要配置）init-method方法
>       1. 把bean实例传递给后置处理器的方法（后置处理器）
>    4. bean可以使用（对象已获取）
>    5. 容器关闭的时候，调用bean的销毁方法（需要进行配置销毁方法）
> 2. 创建——注入——初始化——关闭——销毁
> 3. 例子
>
>    1. init-method:初始化方法	destroy-method=销毁方法
>    2. ```xml
>       <bean id="book" class="com.dy.spring.study.Book" init-method="show" destroy-method="destroyMethod">
>           <property name="bname" ref="bookList"></property>
>           <property name="bauthor" value="中国"></property>
>       </bean>
>       ```
> 4. bean后置处理器，bean生命周期七步：

#### xml自动装配

> 根据指定装配规则（属性名称或者属性类型），spring自动将匹配的属性进行注入
>
> 1. 根据属性类型自动注入：autowire
>    1. byName：根据属性名称注入，注入值bean和id值和类属性名称的一样
>    2. btType：根据属性类型注入

#### 外部属性文件

> 1. 直接配置数据信息
>
>    1. 配置德鲁伊连接池
>    2. 引入德鲁伊jar包
>
>       1. 配置数据库连接：
>       2. ```xml
>          <bean id="durid" class="com.alibaba.druid.pool.DruidDataSource">
>              <property name="driverClassName" value="com.mysql.jdbc.Driver"></property>
>              <property name="url" value="jdbc:mysql//localhost:3306/userdb"></property>
>              <property name="username" value="root"></property>
>              <property name="password" value="root"></property>
>          </bean>
>          ```
>       3. 加载配置文件的方式
>
>          1. 添加jdbc.properties文件
>
>             1. ```properties
>                prpo.driverClass=com.mysql.jdbc.Driver
>                prpo.url=jdbc:mysql//localhost:3306/userdb
>                prpo.userName=root
>                prpo.password=root
>                ```
>          2. bean导入属性文件，配置数据库连接
>
>             1. ```xml
>                <!--在spring引入外部属性文件-->
>                    <context:property-placeholder location="classpath:jdbc.properties"/>
>
>                    <!--连接池-->
>                    <bean id="durid" class="com.alibaba.druid.pool.DruidDataSource">
>                        <property name="driverClassName" value="${prpo.driverClass}"></property>
>                        <property name="url" value="${prpo.url}"></property>
>                        <property name="username" value="${prpo.userName}"></property>
>                        <property name="password" value="${prpo.password}"></property>
>                    </bean>
>                ```

#### 基于注解方式注入属性

> 1. 什么是注解：
>
>    1. 注解是代码的特殊标记，格式：@注解名称(属性名称=属性值属性名称=属性值)
>    2. 注解作用于方法、类、属性上面
> 2. 目的：简化xml配置
> 3. Bean管理
>
>    1. @Component：普通注解、都可以创建对象
>    2. @Service：业务逻辑层
>    3. @Controller：web层、控制层
>    4. @Repository：dao层
>    5. 四个注解功能一样，都可以用来出创建beans对象
> 4. 基于注解方式实现对象创建
>
>    1. 引入aop依赖
>    2. 开启组件扫描
>
>       1. ```xml
>          <!--地址修改-->
>          http://www.springframework.org/schema/context   https://www.springframework.org/schema/context/spring-context.xsd"
>          <!--    开启组件扫描-->
>          <context:component-scan base-package="com.dy.spring2"></context:component-scan>
>          ```
>    3. 创建类为类添加注解
>
>       1. ```java
>          @Service(value = "UserService")
>          public class UserService {
>
>              public void add(){
>                  System.out.println("Service add...");
>              }
>
>          }
>          ```
>
>          1. 注解中vlaue的值可以不写，默认为类的名称，首字母为小写
>    4. 组件扫描细节配置
>
>       1. user-default-filters：包含的规则（过滤）,默认是true，为全部都扫描
>       2. 意义：只扫描spring2里的Controller注解，在expression里定义
>       3. ```xml
>          <!--    示例1-->
>          <context:component-scan base-package="com.dy.spring2" use-default-filters="false">
>              <context:include-filter type="annotation" expression="org.springframework.stereotype.Controller"/>
>          </context:component-scan>
>          ```
>       4. context:exclude表示为不扫描哪一个标签
>       5. ```xml
>          <context:component-scan base-package="com.dy.spring2">
>              <context:exclude-filter type="annotation" expression="org.springframework.stereotype.Controller"/>
>          </context:component-scan>
>          ```
>    5. 基于注解实现属性注入
>
>       1. @Autowired：根据属性类型进行注入
>
>          1. ```java
>              //    属性上添加属性注解
>             //    不需要添加set方法
>             @Autowired  //根据类型进行注入
>             private UserDao ud;
>             ```
>       2. @Quailfier：根据属性名称进行注入
>
>          1. 和@Quaifier一起使用
>          2. 当一个接口有多个对象时，通过属性类型注入就会失败，需要添加Quaillfier(value="属性名")进行区分
>          3. ```java
>              @Autowired  //根据类型进行注入
>             @Qualifier(value = "userDaoImp1")
>             private UserDao ud;
>             ```
>       3. @Resource：根据类型或者名称注入
>
>          1. 可以根据类型和名称注入
>          2. ```java
>              @Resource(name = "userDaoImp1")
>             private UserDao ud1;
>             ```
>       4. @Value：注入普通类型属性
>
>          1. 为属性注入值
>          2. ```java
>              @Value(value = "启动")
>             private String stat;
>             ```

#### 完全注解开发

> 1. 创建配置类
>
>    1. ```java
>       @Configuration //作为配置类
>       @ComponentScan(basePackages = {"com.dy.spring2"})//开启注解扫描
>       ```
>    2. 等价于
>    3. ```xml
>       <context:component-scan base-package="com.dy.spring2"></context:component-scan>
>       ```
> 2. 创建测试类
>
>    1. 将引入配置文件布置换为引入配置类
>    2. ```
>       ApplicationContext context = new AnnotationConfigApplicationContext(SpringConfig.class);
>       ```

## AOP

> 1. 面向切面编程（方面），利用AOP对**业务逻辑的各个部分进行隔离**，从而使得业务逻辑各部分耦合度降低，提高程序的可读性和开发效率
> 2. 通俗概念：不通过修改源代码添加新的功能

### 1. 底层原理

> 1. AOP底层：动态代理
>    1. 有接口，使用JDK动态代理
>       1. 创建接口实现类代理对象，增强类的方法
>    2. 无接口，使用CGLIB动态代理
>       1. 创建子类代理对象，进行方法增强

#### jdk动态代理

> ```java
>     java.lang.reflect.Proxy //Proxy类下的
>     newProxyInstance(ClassLoader loader, 类<?>[] interfaces, InvocationHandler h) //方法
> ```
>
> 1. 调用newProxyInstance方法进行实现
> 2. static Object（静态方法）
>    1. newProxyInstance(ClassLoader loader, 类<?>[] interfaces, InvocationHandler h)	返回指定接口的代理类的实例，该接口方法调用分派给指定的调用处理程序
>    2. ClassLoader：类加载器
>    3. Interfaces：增强方法所在的类，这个类实现多个接口，支持多个接口
>    4. InvocationHandler： 实现这个接口InvocationHandler，创建代理对象，写增强方法

##### 代码实现jdk动态代理

> 1. 创建接口和接口类
> 2. 调用JDKProxy实现动态代理
> 3. 动态代理方法
> 4. ```java
>    public static void main(String[] args) {
>        UserDao ud = new UserDaoImp();
>        MyHandlerProxy invocation = new MyHandlerProxy(ud);
>        UserDao proxy =  (UserDao)Proxy.newProxyInstance(ud.getClass().getClassLoader(),
>                ud.getClass().getInterfaces(),
>                invocation);
>        System.out.println(ud.add(1,2));
>        System.out.println(proxy.add(1,2));
>    }
>    ```

### 2. AOP常用术语

> 1. 连接点
>    1. 类中方法可以被增强的方法称为连接点
> 2. 切入点
>    1. 实际被增强的方法称为切入点
> 3. 通知（增强）
>    1. 实际增强的逻辑部分称为通知（例如在登录中加入权限判断的功能）
>    2. 通知类型
>       1. 前置通知
>       2. 后置通知
>       3. 环绕通知
>          1. 前执行或者后执行
>       4. 异常通知
>       5. 最终通知：（finally）最后都要执行
> 4. 切面
>    1. 把通知运用到切入点的过程

### 3. AOP操作

#### 1. 准备

> 1. spring框架一般基于AspectJ实现AOP操作
>
>    1. AspectJ是一种独立的AOP框架
> 2. 基于AspectJ实现AOP操作
>
>    1. 基于配置文件操作
>    2. 基于注解方式实现操作
> 3. 引入依赖
>
>    1. maven导入的依赖包
>    2. ```xml
>       <dependency>
>           <groupId>org.springframework</groupId>
>           <artifactId>spring-aspects</artifactId>
>           <version>5.2.6.RELEASE</version>
>       </dependency>
>
>       <dependency>
>           <groupId>org.aspectj</groupId>
>           <artifactId>aspectjweaver</artifactId>
>           <version>1.9.6</version>
>           <scope>runtime</scope>
>       </dependency>
>       <dependency>
>           <groupId>aopalliance</groupId>
>           <artifactId>aopalliance</artifactId>
>           <version>1.0</version>
>       </dependency>
>       <dependency>
>           <groupId>net.sourceforge.cglib</groupId>
>           <artifactId>com.springsource.net.sf.cglib</artifactId>
>           <version>2.2.0</version>
>       </dependency>
>       ```
> 4. 切入点表达方式
>
>    1. 作用：知道对那个类里面的那个方法进行增强
>    2. 语法结构：
>       1. execution([ 权限修饰符 ] [返回类型] [类全路径] [方法名称] (参数列表))
>    3. 例子：
>       1. execution(*.com.dy.BookDao.add(..))
>          1. 增强add方法
>       2. execution(*.com.dy.BookDao.\*(..))
>          1. 增强BookDao下的所有方法
>       3. execution(*.com.dy.\*.\*(..))
>          1. 增强dy包下的所有方法

#### 2.基于注解实现操作：

> 1. 创建类
> 2. 创建增强类，在增强类里创建方法
> 3. 修改配置文件
>
>    1. 开启aspectj生产代理对象
>    2. ```xml
>       <aop:aspectj-autoproxy></aop:aspectj-autoproxy>
>       ```
> 4. 创建目标类和增强类
> 5. 将目标类的增强类注解实例
> 6. 在增强类添加注解@Aspect（代表生产代理对象）
>
>    1. 增强类注释
>    2. ```java
>       @Component
>       @Aspect // 生产代理对象，切面
>       public class UserProxy {}
>       ```
> 7. 使用execution方法增加通知
>
>    1. ProceedingJoinPoint processJoinPoint为环绕通知添加方法执行的次序
>    2. 执行：processJoinPoint.proceed();
>    3. ```java
>       //    前置通知
>           @Before(value = "execution(* com.dy.aop.aopanno.service.imp.UserServiceImp.add(..))")
>           public void before(){
>               System.out.println("前置增加");
>           }
>       //  最终通知，无论是否有异常都要执行
>           @After(value = "execution(* com.dy.aop.aopanno.service.imp.UserServiceImp.add(..))")
>           public void after(){
>               System.out.println("after");
>           }
>       // 后置通知，返回值后执行，报错不执行
>           @AfterReturning(value"execution(*com.dy.aop.aopanno.service.imp.UserServiceImp.add(..))")
>           public void afterReturning(){
>               System.out.println("after Return");
>           }
>       // 异常通知
>           @AfterThrowing(value = "execution(* com.dy.aop.aopanno.service.imp.UserServiceImp.add(..))")
>           public void afterThrowing(){
>               System.out.println("afterThrowing");
>           }
>       //环绕通知 环绕之后属于后置通知
>           @Around(value = "execution(* com.dy.aop.aopanno.service.imp.UserServiceImp.add(..))")
>           public void around(ProceedingJoinPoint processJoinPoint) throws Throwable {
>               System.out.println("环绕前");
>               processJoinPoint.proceed();
>               System.out.println("环绕后");
>       ```
> 8. 重用切入点
>
>    1. 对重复的切入点进行抽取
>    2. 注解类@Pointcut(value = "切入点")
>    3. ```Java
>         @Pointcut(value = "execution(* com.dy.aop.aopanno.service.imp.UserServiceImp.add(..))")
>          public void pointcut(){
>
>        //    前置通知
>             @Before(value = "pointcut()")
>          public void before(){
>                 System.out.println("前置增加");
>                 System.out.println("公共抽取类");
>             }
>         }
>
>
>
>       ```
> 9. 对一个类里多个方法进行增强
>
>    1. 注解@Order(1)进行优先级设置，数字越低优先级越高
>    2. ```java
>       @Component
>       @Aspect
>       @Order(1)
>       ```

#### 3. AspectJ配置文件

> 1. 创建两个类，增强类和目标类
> 2. 在spring配置中 创建两个对象
> 3. 在spring配置中加入切入点
>
>    1. xml切入点于切面配置
>    2. ```xml
>       <!--aop配置增强-->
>           <aop:config>
>       <!--        切入点-->
>               <aop:pointcut id="p" expression="execution(* com.dy.aop.xmlclass.dao.User.add(..))"/>
>       <!--        配置切入面-->
>               <aop:aspect ref="prouser">//增强类
>                   <aop:before method="before" pointcut-ref="p"></aop:before>
>               </aop:aspect>
>           </aop:config>
>       ```

#### 4. 完全注解开发

> 配置类：
>
> 1. ```java
>    @Configuration //作为配置类
>    @ComponentScan(basePackages = {"com.dy.aop.aopanno"})//开启注解配置
>    @EnableAspectJAutoProxy(proxyTargetClass = true)//生产代理对象
>    public class AopConfig {
>
>    }
>    ```

## JdbcTemplate

### 1. 开始准备

> 1. 导入依赖
> 2. ```xml
>    <dependency>
>            <groupId>org.springframework</groupId>
>            <artifactId>spring-jdbc</artifactId>
>        </dependency>
>
>        <dependency>
>            <groupId>org.springframework</groupId>
>            <artifactId>spring-tx</artifactId>
>        </dependency>
>
>        <dependency>
>            <groupId>org.springframework</groupId>
>            <artifactId>spring-orm</artifactId>
>        </dependency>
>
>        <dependency>
>            <groupId>com.alibaba</groupId>
>            <artifactId>druid</artifactId>
>        </dependency>
>        <dependency>
>            <groupId>mysql</groupId>
>            <artifactId>mysql-connector-java</artifactId>
>        </dependency>
>    </dependencies>
>    ```
> 3. 在spring配置文件配置数据库连接池》》》ioc容器引入外部文件有
> 4. 配置JdbcTemplate对象，注入DataSource
>
>    1. xml文件注入
>    2. ```xml
>       <!--   引入外部属性文件-->
>           <context:property-placeholder location="classpath:jdbc.properties"/>
>
>           <!--连接池-->
>           <bean id="dataSource" class="com.alibaba.druid.pool.DruidDataSource">
>               <property name="driverClassName" value="${prpo.driverClass}"></property>
>               <property name="url" value="${prpo.url}"></property>
>               <property name="username" value="${prpo.userName}"></property>
>               <property name="password" value="${prpo.password}"></property>
>           </bean>
>       <!--创建连接-->
>           <bean id="myjdbc" class="org.springframework.jdbc.core.JdbcTemplate">
>       <!--        注入数据库信息-->
>               <property name="dataSource" ref="dataSource"></property>
>           </bean>
>       <!--开启注解扫描-->
>           <context:component-scan base-package="com.dy.spring.demo1"></context:component-scan>
>       ```
> 5. 利用注解创建service和dao对象

### 数据库基本操作

#### 1. 增加操作

> 1. 对应数据库创建实体类>>>>Po类
> 2. 创建连接属性并进行注解实例化
>
>    1. xml文件下进行实例化
>    2. ```java
>       @Autowired
>       private JdbcTemplate myjdbc;
>       ```
> 3. 在dao进行数据库添加操作
>
>    1. 相当于把jdbc功能封装了
>    2. ```java
>        @Autowired  //注解引入myjdbc类
>       private JdbcTemplate myjdbc;
>
>       public boolean add(User user) {
>       //      创建sql语句
>               String sql = "insert into user values(?,?,?)";
>       //
>               int i = myjdbc.update(sql,user.getName(),user.getAge(),user.getGender());
>       }
>       ```

#### 2. 删除操作

> ```java
> @Override
> public boolean delet(int id) {
>     String sql = "delete from user where id=?";
>     int flag = myjdbc.update(sql,id);
> }
> ```

#### 3. 修改操作

> ```java
> public boolean updata(User user, int id) {
>     String sql = "update user set name=?,age=?,gender=? where id=?";
>     Object[] obj = {user.getName(), user.getAge(), user.getGender(), 1};
>     int flag = myjdbc.update(sql,obj);
> }
> ```

#### 4.查询操作

> 1. 查询返回记录数：
>
>    1. queryForObject(sql, Object.class):
>
>       1. 第一个是数据库语句
>       2. 第二个是返回参数类型
>    2. ```java
>       String sql = "select count(*) from user";
>       int count = myjdbc.queryForObject(sql,Integer.class);
>       return count;
>       ```
> 2. 查询返回某个对象
>
>    1. queryForObject(String sql, RowMapper `<T>` rowMapper, Object... args):
>
>       1. 数据库语句
>       2. 返回不同数据类型，利用接口实现类完成数据封装
>       3. sql语句值
>    2. ```java
>       String sql = "select * from user where id=?";
>       User user = myjdbc.queryForObject(sql,new BeanPropertyRowMapper<User>(User.class),id);
>       return user;
>       ```
> 3. 查询返回集合
>
>    1. 返回集合调用query方法完成
>    2. ```Java
>       String sql = "select * from user where name=?";
>       List<User> userList = myjdbc.query(sql,new BeanPropertyRowMapper<User>(User.class),name);
>       return userList;
>       ```

#### 5. 批量操作

> 操作表里的多条数据
>
> 1. 批量添加
>
>    1. batchUpdata函数，底层会给你遍历取出逐个加入
>    2. ```java
>        public boolean addList(List<Object[]> batchArgs) {
>               String sql = "insert into user (name, gender,age) values(?,?,?)";
>               int[] ins = myjdbc.batchUpdate(sql,batchArgs);
>               return true;
>           }
>       ```
>    3. 列表赋值
>
>       1. Object[] 数组里的值与sql语句里缺少值的数量要一致
>    4. ```java
>       List<Object[]> addList = new ArrayList<>();
>       Object[] o1 = {"李四", "女", "24"};
>       Object[] o2 = {"李四", "男", "34"};
>       Object[] o3 = {"李四", "女", "20"};
>       addList.add(o1);
>       addList.add(o2);
>       addList.add(o3);
>       usr.addList(addList);
>       ```
> 2. 批量修改
>
>    1. ```java
>       public boolean updataList(List<Object[]> batchArgs) {
>           String sql = "update user set name=?,gender=?,age=? where id=?";
>           int[] ins = myjdbc.batchUpdate(sql,batchArgs);
>
>           return false;
>       }
>       ```
> 3. 批量删除
>
>    1. ```java
>       public void deleteList(List<Object[]> batchArgs) {
>           String sql = "delete from user where id=?";
>           int[] ins = myjdbc.batchUpdate(sql, batchArgs);
>       }
>       ```

## 事务操作：

### 1. 概述

> 1. 事务管理添加到javaee三层结构里的Service层里
> 2. 事务管理操作：
>    1. 编程式事务管理
>    2. 声明式事务管理
> 3. spring一般使用声明式事务管理
>    1. 基于注解方式
>    2. 基于xml配置文件方式
> 4. 底层使用AOP 原理
> 5. spring事务管理API
>    1. 提供一个接口代表事务管理器，针对不同框架提供不同实现类
>    2. 调用PlatformTransactionManager接口下的方法进行实现
> 6. 当service业务处理发生异常时，事务会进行回滚，不进行操作，不发生异常就操作完

### 2. 基于注解方式配置进行事务管理

> 1. 在xml文件里声明TransationManager，事务管理器
> 2. ```xml
>    <bean id="TransactionManager" class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
>        <property name="dataSource" ref="dataSource"></property><!--注入属性连接池-->
>    </bean>
>    ```
> 3. 引入名称空间tx
> 4. ```xml
>    xmlns:tx="http://www.springframework.org/schema/tx"
>    ```
> 5. 开启事务注解
>
>    1. 将刚才创建的事务管理器放入transaction-manager里
>    2. ```xml
>       <tx:annotation-driven transaction-manager="TransactionManager"></tx:annotation-driven>
>       ```
> 6. 在service类里添加事务注解：
>
>    1. 注解可添加到类或者方式上
>    2. ```java
>       @Transactional
>       ```
>
>       1. 添加到类上>>>>代表类中方法都添加事务
>       2. 方法上只代表方法上添加事务

#### 1. 在事务管理中配置参数

> 在Transactional中配置属性参数

##### propagation()

> 1. 事务传播行为：在不同事务之间进行操作，进行互相调用的行为
> 2. 事务方法：对数据库表数据进行变化的操作
> 3. REQUIRED
>
>    1. 当前方法有事务，调用方法就在这个事务里运行，没有事务则调用方法新创建一个事务
> 4. REQUIRED_NEW
>
>    1. 无论当前方法有没有事务，调用方法都创建新的事务
> 5. ```JAVA
>    @Service
>    @Transactional(propagation = Propagation.REQUIRED)
>    public class UserService {}
>    ```

##### **isolation()**

> 1. 事务隔离级别
> 2. 特性：隔离性、、、多事务操作不会产生影响
>
>    1. 不考虑隔离性产生的问题：脏读、不可重复读、虚（幻）读
>
>       1. 脏读：一个**未提交**事务读取另一个**未提交**事务的数据
>       2. 不可重复读：一个**未提交**的事务读取到另一个**提交**事务修改、
>       3. 虚读：一个**未提交**的事务读取到另一个**提交**事务添加数据
>    2. 设置隔离级别解决只读问题
>
>       1. | 参数                                         | 脏读 | 不可重复读 | 幻读 |
>          | :------------------------------------------- | ---- | ---------- | :--- |
>          | READ_COMMITTED（读未提交）                   | 有   | 有         | 有   |
>          | READ_UNCOMMITTED（读已提交）                 | 无   | 有         | 有   |
>          | REPEATABLE_READ（可重复读）（mysql默认设置） | 无   | 无         | 有   |
>          | SERIALIZABLE（串行化）                       | 无   | 无         | 无   |

##### timeout()

> 1. 在一定时间内将那些提交，未提交进行事务回滚
> 2. 默认时间-1:表示未不超时
> 3. 设置时间以秒单位进行计算

##### readOn()

> 1. 默认值为false,可以进行增删改查操作
> 2. true:只能进行查找操作

##### roolbackFor()：回滚

> 1. 设置查询那些异常进行事务回滚

##### noRollbackFor()：不回滚

> 1. 设置那些异常不进行事务回滚

### 3. 基于xml方式声明事务管理（待定）

> 1. 配置事务管理器
> 2. 配置通知
> 3. 配置切入点和切面

### 4. 完全注解开发(完全注解声明式事务管理)

> 1. 创建一个配置类
> 2. 
> 3. ```java
>    @Configuration
>    @ComponentScan(basePackages = "com.dy.spring.demo2")
>    //开启事务注解扫描
>    @EnableTransactionManagement
>    public class TransactionConfig {
>
>    //    创建一个数据库连接池 与配置文件中功能相同
>        @Bean
>        public DruidDataSource getDruidDataSource(){
>            DruidDataSource dataSource = new DruidDataSource();
>            dataSource.setDriverClassName("com.mysql.jdbc.Driver");
>            dataSource.setUrl("jdbc:mysql://localhost:3306/userdb");
>            dataSource.setUsername("root");
>            dataSource.setPassword("root");
>            return dataSource;
>        }
>    //    创建数据库配置
>        @Bean
>        public JdbcTemplate getJdbcTemplate(DataSource dataSource){
>    //        在IOC容器中找到同类型的进行注入
>            JdbcTemplate myjdbc = new JdbcTemplate();
>            myjdbc.setDataSource(dataSource);
>            return myjdbc;
>        }
>    //    创建事务管理器
>        @Bean
>        public DataSourceTransactionManager dataSourceTransactionManager(DataSource dataSource){
>            DataSourceTransactionManager dataSourceTransactionManager = new DataSourceTransactionManager();
>            dataSourceTransactionManager.setDataSource(dataSource);
>            return dataSourceTransactionManager;
>        }
>
>    }
>    ```

## spring5新功能

### 整合日志框架

> spring5新功能
>
> 1. 整个spring5框架基于java8，运行时兼容jkd9,许多不建议使用的类和方法在代码库中删除
> 2. spring5自带通用的日志封装
> 3. Log4j2
> 4. Spring框架整合Log4j2框架
>
>    1. 引入相关jar包或者依赖
>
>       1. maven导入相关的包
>       2. ```xml
>          <dependency>
>              <groupId>org.apache.logging.log4j</groupId>
>              <artifactId>log4j-api</artifactId>
>              <version>2.11.2</version>
>          </dependency>
>          <dependency>
>              <groupId>org.apache.logging.log4j</groupId>
>              <artifactId>log4j-core</artifactId>
>              <version>2.11.2</version>
>          </dependency>
>          <dependency>
>              <groupId>org.slf4j</groupId>
>              <artifactId>slf4j-api</artifactId>
>              <version>1.7.30</version>
>          </dependency>
>          <dependency>
>              <groupId>org.apache.logging.log4j</groupId>
>              <artifactId>log4j-slf4j-impl</artifactId>
>              <version>2.11.2</version>
>          </dependency>
>          ```
>    2. 创建log4j2.xml配置文件
>
>       1. 日志结构
>       2. ```xml
>          <?xml version="1.0" encoding="UTF-8" ?>
>          <configuration status="INFO">
>          <!-- 先定义所有的appender-->
>              <appenders>
>          <!-- 输出日志信息到控制台-->
>                  <console name="Console" target="SYSTEM_OUT">
>          <!--  控制日志输出的格式-->
>                      <PatternLayout pattern="%d{yyyy-MM-dd HH:mm:ss.SSS} [%t] %-5level %logger{36} - %msg%n"/>
>                  </console>
>              </appenders>
>          <!--定义loggers,引入appender，定义loggers并引入appender才会生效-->
>          <!--    root:用于知道项目的跟目录，如果没有单独指定Logger,则会使用root作为默认日志输出-->
>              <loggers>
>                  <root level="info">
>                      <appender-ref ref="Console"/>
>                  </root>
>              </loggers>
>          </configuration>
>          ```

### Nullable注解和函数式注册对象

> 1. @Nullable注解可以使用在方法、属性、参数。表示方法返回值、属性、参数可以为空
> 2. 核心容器支持函数式风格：
>
>    1. 可以基于这个创建对象，name可以为空
>    2. ```java
>       public void testGenericApplication(){
>           GenericApplicationContext context = new GenericApplicationContext();
>           context.refresh();
>           context.registerBean("userUservice"(@Nullable),UserService.class() -> new UserService());
>           UserService us = (UserService) context.getBean("com.dy.spring.demo2.service.UserService");
>       }
>       ```

### junit测试框架

> 1. 引入相关的测试依赖
>
>    1. ```xml
>       <dependency>
>           <groupId>org.springframework</groupId>
>           <artifactId>spring-test</artifactId>
>       </dependency>
>       ```
> 2. 创建测试类
>
>    1. ```Java
>       ApplicationContext context = new ClassPathXmlApplicationContext("beans2.xml");
>       UserService us = context.getBean("userService", UserService.class);
>       ```
>    2. 两个效果等同
>    3. ```java
>       //指定相关的j4版本
>       @RunWith(SpringJUnit4ClassRunner.class)
>        @ContextConfiguration("classpath:beans2.xml")
>       ```
>    4. 使用注解实例化对象

## webFlux

### 概述

> 1. 一种一步非阻塞的框架、在Servlet3.1以后才进行支持，核心时基于Reactor相关的API 实现
> 2. 异步与同步：针对调用者来说
>    1. 同步：调用者发送请求后接受者回应之后，调用者才可以做其他处理
>    2. 异步：调用者发送请求后接受者不需要回，调用者也可以做其他事务处理
> 3. 阻塞与非阻塞：针对被调用者
>    1. 阻塞：接收者接受请求后，完成请求之后才给出反馈
>    2. 非阻塞：接收者接受请求后，没有完成请求就先给出反馈
> 4. 响应式编程
