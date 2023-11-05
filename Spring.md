# Spring概述

1. **Spring**是轻量级的开源的 JavaEE 框架
2. **Spring**可以解决企业应用开发的复杂性
3. **Spring**有两个核心部分：IOC和AOC
    - **IOC：**控制反转，把创建对象过程交给Spring进行管理
    - **AOP：**面向切面，不修改源代码进行功能增强
4. **Spring特点：**
    - 方便解耦，简化开发
    - AOP编程支持
    - 方便程序的测试
    - 方便和其他框架进行整合
    - 方便进行事务操作
    - 降低API开发难度

# Maven依赖

[Spring Framework](https://spring.io/projects/spring-framework#learn)

- GA：稳定版本

## IOC依赖

**Spring Beans**

```xml
<!-- https://mvnrepository.com/artifact/org.springframework/spring-beans -->
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-beans</artifactId>
    <version>5.3.9</version>
</dependency>
```

**Spring Context**

```xml
<!-- https://mvnrepository.com/artifact/org.springframework/spring-context -->
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-context</artifactId>
    <version>5.3.9</version>
</dependency>
```

**Spring Core**

```xml
<!-- https://mvnrepository.com/artifact/org.springframework/spring-core -->
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-core</artifactId>
    <version>5.3.9</version>
</dependency>
```

**Spring Expression Language**

```xml
<!-- https://mvnrepository.com/artifact/org.springframework/spring-expression -->
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-expression</artifactId>
    <version>5.3.9</version>
</dependency>
```

**Apache Commons Loggin**

```xml
<!-- https://mvnrepository.com/artifact/commons-logging/commons-logging -->
<dependency>
    <groupId>commons-logging</groupId>
    <artifactId>commons-logging</artifactId>
    <version>1.2</version>
</dependency>
```

## AOP依赖

**spring-aop**

```xml
<!-- https://mvnrepository.com/artifact/org.springframework/spring-aop -->
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-aop</artifactId>
    <version>5.3.6</version>
</dependency>
```

**spring-aspects**

```xml
<!-- https://mvnrepository.com/artifact/org.springframework/spring-aspects -->
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-aspects</artifactId>
    <version>5.3.6</version>
</dependency>
```

**com.springsource.net.sf.cglib**

```xml
<!-- https://mvnrepository.com/artifact/net.sourceforge.cglib/com.springsource.net.sf.cglib -->
<dependency>
    <groupId>net.sourceforge.cglib</groupId>
    <artifactId>com.springsource.net.sf.cglib</artifactId>
    <version>2.2.0</version>
</dependency>
```

**com.springsource.org.aopalliance**

```xml
<!-- https://mvnrepository.com/artifact/org.aopalliance/com.springsource.org.aopalliance -->
<dependency>
    <groupId>org.aopalliance</groupId>
    <artifactId>com.springsource.org.aopalliance</artifactId>
    <version>1.0.0</version>
</dependency>
```

**com.springsource.org.aspectj.weaver**

```xml
<!-- https://mvnrepository.com/artifact/org.aspectj/aspectjweaver -->
<dependency>
    <groupId>org.aspectj</groupId>
    <artifactId>aspectjweaver</artifactId>
    <version>1.9.6</version>
</dependency>
```

## JDBC模板依赖

**druid**

```xml
<dependency>
     <groupId>com.alibaba</groupId>
     <artifactId>druid</artifactId>
     <version>1.2.8</version>
</dependency>
```

**mysql-connector**

```xml
<dependency>
	<groupId>mysql</groupId>
	<artifactId>mysql-connector-java</artifactId>
	<scope>runtime</scope>
</dependency>
```

**spring-jdbc**

 这个jar 文件包含对Spring 对JDBC 数据访问进行封装的所有类。 ​ 外部依赖spring-beans，spring-dao。

```xml
<dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-jdbc</artifactId>
        <version>5.3.6</version>
</dependency>
```

**spring-tx**

 以前是在这里org.springframework.transaction

 为JDBC、Hibernate、JDO、JPA、Beans等提供的一致的声明式和编程式事务管理支持。

```xml
<dependency>
	<groupId>org.springframework</groupId>
	<artifactId>spring-tx</artifactId>
	<version>5.3.6</version>
</dependency>
```

**spring-orm**

 包含Spring对DAO特性集进行了扩展，使其支持iBATIS、JDO、OJB、TopLink，
因为Hibernate已经独立成包了，现在不包含在这个包里了。这个jar文件里大部分的类都要依赖spring-dao.jar里的类，用这个包时你需要同时包含spring-dao.jar包。

```xml
<dependency>
	<groupId>org.springframework</groupId>
	<artifactId>spring-orm</artifactId>
	<version>5.3.6</version>
</dependency>
```

# 快速开始

- 创建SpringWeb项目
- 创建User实体类

```java
public class User {
    public void add() {
        System.out.println("add...");
    }
}
```

- 创建配置文件

  xml文件：bean1.xml，放在resources目录下

  bean标签

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
 
    <!--bean标签标示配置bean
    	id属性标示给bean起名字
    	class属性表示给bean定义类型
	-->
	<bean id="bookDao" class="com.itheima.dao.impl.BookDaoImpl"/>
    <bean id="bookService" class="com.itheima.service.impl.BookServiceImpl"/>

</beans>
```

- 编写测试代码

```java
public class TestSpring5 {
    @Test
    public void testAdd() {
        // 1.加载spring配置文件                                           // 配置文件名
        ApplicationContext context = new ClassPathXmlApplicationContext("bean1.xml");
        // 2.获取配置文件创建的对象，name是bean标签中起的别名(id)
        User user = context.getBean("user", User.class);
        // 3.测试
        System.out.println(user);
        user.add();
    }
}
```

# IOC

## IOC概念

**控制反转**（Inversion of Control，缩写为IOC），是面向对象编程中的一种设计原则，**把对象的创建和对象之间的调用过程，交给Spring进行管理**，目的是为了降低耦合度。

## IOC底层原理

- xml解析
- 工厂模式
- 反射

### IOC过程

service调用dao:

#### **传统模式**

```java
class UserService {
    execute() {
		UserDao userDao = new UserDao();
		userDao.add();
    }
}
```

这样做的缺点是，耦合度太高

#### **工厂模式**

使用一个工厂类创建UserDao对象

```java
class UserFactory {
    public static UserDao getDao() {
        return new UserDao();
    }
}
```

Service调用

```java
class UserService {
    execute() {
		UserDao userDao = UserFactory.getDao();
		userDao.add();
    }
}
```

这样子降低了service和dao间的耦合度，因为相对于UserService来说UserDao的创建相当于一个黑盒，对UserDao创建过程代码的任何修改，都不会影响到sevice里dao对象的创建

(service里可能会频繁创建对象，因为创建对象的代码位于Factory类里，直接修改Factory类就行，从而避免了牵一发动全身的窘况)。

#### IOC模式

- **第一步：xml配置文件， 配置待创建的对象(类的路径)**

  ```xml
  <bean id="dao" class="com.dm.UserDao"></bean>
  ```

- **第二步：有service类和dao类，创建工厂类**

  ```java
  class UserFactory {
      public static UserDao getDao() {
          //1.xml解析得到类的全路径
          String classValue = class中的属性值("com.dm.UserDao");
          //2.通过反射，根据类的全路径创建对象
          Class clazz = Class.forName(classValue);
          return (UserDao)clazz.newInstance();
      }
  }
  ```

IOC模式进一步降低了耦合度，比如当类的路径发生改变时，仅仅只需要变动xml配置文件中的配置

### IOC接口

IOC接口（BeanFactory）

**1.IOC思想基于IOC容器完成，IOC容器底层就是对象工厂。**

**2.Spring提供了IOC容器实现的两种方式：**

- **BeanFactory:** IOC容器基本实现，是Spring内部的使用接口，不提供开发人员进行使用。（加载配置文件时不会创建对象，在获取/使用对象的时候才会创建对象）
- **ApplicationContext:** BeanFactory接口的子接口，提供更多更强大的功能，一般由开发人员进行使用。(加载配置文件对象时创建对象)
- 一般都用ApplicationContext方法，将耗时耗资源的操作在项目启动(服务器启动)的时候进行处理

**3.ApplicationContext接口有实现类：**

（IDEA ctrl+h查看继承树）

![](https://gitee.com/dong-dameng/images/raw/master/img/20211004072743.png)

- FileSystemXmlApplicationContext：需要盘符路径
- ClassPathXmlApplicationContext：需要类路径

## xml管理

### 创建对象

（1）在Spring配置文件中，使用bean标签，标签里面添加对应属性，就可以实现对象创建。

```xml
<bean id="book" class="com.dm.Book"></bean>
```

```java
ApplicationContext context = new ClassPathXmlApplicationContext("bean.xml");
Book book = context.getBean("book",Book.class);  
```

（2）bean标签中常见属性

- id：获取对象的唯一标识（不是对象名，相当于别名）
- class：类全路径（包类路径）
- name: 基本不使用；作用与id相同，区别在于id属性中不能添加特殊符号，而name可以

（3）默认执行无参数构造方法

### 注入属性

#### 基本注入

 DI(dependency injection)：依赖注入，就是注入属性

- **第一种：使用set方法进行注入**

  需要在实体类中写好set方法

```xml
<!--创建对象-->
<bean id="book" class="com.dm.Book">
	<!--使用property完成属性注入
		name:属性名称
		vlaue:属性值
	-->
	<property name="bookName" vlaue="JOJO的奇妙冒险"></property>
    <property name="author" vlaue="荒木吕飞彦"></property>
</bean>
```

- **第二种注入方式：通过有参构造注入**

  需要在实体类中写好有参构造器

```xml
<!--创建对象-->
<bean id="book" class="com.dm.Book">
	<!--使用construcior-arg完成属性注入
		name：属性名
		value：属性值
		index: 索引值，根据有参构造器中第几个参数进行注入，下标从0开始
	-->
    <constructor-arg name="bookName" vlaue="JOJO的奇妙冒险"></constructor-arg>
    <constructor-arg name="author" vlaue="荒木吕飞彦"></constructor-arg>
    <!--
		<constructor-arg name="1" vlaue="荒木吕飞彦"></constructor-arg>
	-->
</bean>
```

- **第三种注入方式：P名称空间注入(了解)：**

使用P名称空间注入，可以简化基于xml配置方式

（1）第一步：在配置文件中添加p名称空间

![](https://gitee.com/dong-dameng/images/raw/master/images/20210423202716.jpg)

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:p="http://www.springframework.org/schema/p"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
</beans>
```

（2）第二步：进行属性注入，在bean标签里面进行操作

​        **底层使用的是set方法注入**

```xml
<bean id="book" class="com.dm.Book" p:name="JOJO的奇妙冒险" p:author="荒木吕飞彦"></bean>
```

#### 其他类型

**字面量**

> 在计算机科学中，字面量（literal）是用于表达源代码中一个固定值的表示法（notation）。

```java
int b = 10; //b为常量，10为字面量
```

###### null值

```xml
<property name="author">
	<null/>
</property>
```

###### 包含特殊符号

```xml
<!--例如荒木<>飞彦>
<!--方法一：把<>进行转义-->
<property name="author" vlaue="荒木&lt&gt飞彦"></property>
<!--方法二：把带特殊符号内容写到CDATA-->
<property name="author">
    <value>
        <![CDATA[<>]]> 
	</value>
</property>
```

#### 外部bean

- 在UserService里创建UserDao类型属性，生成set方法

  ```java
  private UserDao userDao;
  
  public void setUserDao(UserDao userDao) {
  	this.userDao = userDao;
  }
  ```

- 配置文件

  ```xml
  <bean id="userService" class="com.dm.service.UserService">
  	<!--注入userDao对象
  	name属性值：类里面属性名称
  	ref属性：创建userDao对象bean标签id值-->
  	<property name="userDao" ref="userDao"></property>
  </bean>
  
  <bean id="userDao" class="com.dm.dao.UserDao"></bean>
  ```

- 调用UserService

  ```java
  @Test
  public void testAdd(){
  	//1.加载spring配置文件
  	ApplicationContext context = new ClassPathXmlApplicationContext("bean1.xml");
  	//2.获取配置创建的对象
  	UserService userService = context.getBean("userService", UserService.class);
  	userService.add();
  }
  ```

#### 内部bean

```java
//部门类
public class Dept {
	private String dname;
    ......
}

// 员工类
public class Emp {
    private String name;
    //员工用对象类型的属性来表示其所属的部门
    private Dept dept;
    ......
}
```

```xml
<bean id="emp" class="com.dm.Emp">
	<!--设置普通属性-->
    <property name="name" vlaue="lucky"></property>
    <!--设置对象类型属性，在属性中创建对象并赋值-->
    <property name="dept">
    	<bean name="dept" class="com.dm.Dept">
        	<property name="dname" vlaue="安保部">
        </bean>
    </property>
</bean>
```

#### 级联赋值

```xml
<!--级联赋值-->
<bean id="emp" class="com.dm.Emp">
    <!--这里注入的dept已经被赋好值了-->
	<property name="dept" ref="dept"></property>
</bean>
<!--在dept中赋值-->
<bean id="dept" class="com.dm.Dept">
	<property name="dname" value="安保部"></property>
</bean>
```

```xml
<!--级联赋值-->
<bean id="emp" class="com.dm.Emp">
	<property name="dept" ref="dept"></property>
    <!--需要在Dept中生成get方法-->
    <property name="dept.dname" value="技术部"></property>
</bean>

<bean id="dept" class="com.dm.Dept"></bean>
```

#### 数组和集合类型属性

```java
public class Stu {
    //1.数组类型属性
    private String[] courses;
    //2.list集合类型属性
    private List<String> list;
    //3.map集合类型属性
    private Map<String,String> maps;
    //4.set集合类型属性
    private Set<String> sets;
    ......
}
```

```xml
<bean id="stu" class="com.dm.Stu">
    <!--1.数组类型属性注入-->
	<property name="courses">
    	<array>
        	<value>数学分析</value>
            <value>高等代数</value>
        </array>
    </property>
    
    <!--2.list集合类型属性注入-->
    <property name="list">
    	<list>
        	<value>张三</value>
            <value>李四</value>
        </list>
    </property>
    
    <!--3.map集合类型属性注入-->
    <property name="maps">
    	<map>
        	<entry key="JAVA" value="java"></entry>
            <entry key="UNITY" vlaue="untiy"></entry>
        </map>
    </property>
    
    <!--set集合类型属性注入-->
    <property name="SET">
    	<set>
        	<value>MySQL</value>
            <value>Redis</value>
        </set>
    </property>
</bean>
```

**集合注入对象：**

```xml
<!--1.集合注入对象-->
<bean id="stu" class="com.dm.Stu">
	<property name="course">
    	<list>
        	<ref bean="course1"></ref>
            <ref bean="course2"></ref>
        </list>
    </property>
</bean>
<!--创建多个course对象-->
<bean id="course1" class="con.dm.course">
	<property name="cname" value="Spring框架"></property>
</bean>
<bean id="course2" class="con.dm.course">
	<property name="cname" value="MyBatis框架"></property>
</bean>
```

**提取list集合类型属性注入**:

**(1).在Spring配置文件中引入空间util**

![](https://gitee.com/dong-dameng/images/raw/master/images/20210428142541.jpg)

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:p="http://www.springframework.org/schema/p"
       <!--类似p空间，添加一个util空间-->
       xmlns:util="http://www.springframework.org/schema/util"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd
						<!--类似一行，beans改成util-->
                           http://www.springframework.org/schema/util http://www.springframework.org/schema/util/spring-beans.xsd">
</beans>

```

**(2).使用util标签提取集合**

```xml
<!--id相当于给公共方法命名-->
<util:list id="bookList">
	<value>斗破苍穹</value>
    <value>武动乾坤</value>
    <value>大主宰</value>
    <value>元尊</value>
    <value>万相之王</value>
</util:list>

<util:list id="jojoList">
	<ref bean="乔纳森乔斯达"></ref>
    <ref bean="乔瑟夫乔斯达"></ref>
</util:list>
```

**(3).使用**

```xml
<bean id="book" class="com.dm.Book">
	<property name="list" ref="bookList"></property>
</bean>
```

## IOC操作

### Bean管理

**Bean管理指的是两个操作：**

​    (1) Spring创建对象（管理对象生命周期）

​    (2) Spring注入属性

**Bean管理有两种方式：**

​    (1) 基于xml配置文件实现

​    (2) 基于注解方式实现

### 工厂模式

#### Bean种类

- Spring有两种类型bean，一种是普通bean(人为创建的)，另一种是工厂bean(FactoryBean)
- **普通bean：**
    - 在xml中定义了bean类型就是返回的类型
- **工厂bean:**
    - 在配置文件定义bean类型可以 和返回类型不同

#### 工厂bean使用

**第一步：创建类，让这个类作为工厂bean，实现接口FactoryBean**

**第二步：实现接口里面的方法，在实现的方法中定义返回的bean类型**

```java
//FactoryBean<...>设置返回的对象
public class MyBean implements FactoryBean<Course> {
    
    //定义返回的bean对象
    @Override
    public Course getObject() throws Exception {
        Course course = new Course();
        course.setCname("abc");
        return course;
    }

    
    @Override
    public Class<?> getObjectType() {
        return null;
    }

    @Override
    public boolean isSingleton() {
        return false;
    }
}
```

```xml
<bean id="book" class="com.dm.MyBean"></bean>
```

```java
//返回course类型，用course接收
ApplicationContext context = new ClassPathXmlApplicationContext("bean.xml");
Course course = context.getBean("myBean", Course.class);
```

### bean的作用域

- 在Spring里面，可以设置创建bean实例是单实例还是多实例
- 在Spring里面，默认情况下，bean是单实例对象

**设置单/多实例：**

- 在Spring配置文件bean标签里有一个属性(scope)用于设置单/多实例
    - 单例：仅创建一个实例
    - 多例：每次都创建一个实例

```xml
<bean id="book" class="com.dm.entity.Book" scope="prototype"></bean>
```

- scope属性值
    - singleton(默认值)：表示单实例对象
        - 在加载配置文件时(即`new ClassPathXmlApplicationContext("bean.xml")`)创建对象
    - prototype：表示多实例对象
        - 在调用`context.getBean()`的时候创建对象
    - request：把创建的对象放入请求request中
    - session：把创建的对象放入session中

![image-20211009195518043](https://gitee.com/dong-dameng/images/raw/master/img/20211009195525.png)

- 区别：
    - singtelon表示单实例，prototype表示多实例
    - 设置scope值为singleton的时候，加载spring配置文件就会创建单实例对象
    - 设置scope值为prototype，不会在加载配置文件时创建对象，而是在调用getBean方法时创建多实例对象

### bean的生命周期

- **生命周期：**从对象创建到对象销毁的过程

- **bean的生命周期：**

    - **第一步：** 通过构造器创建bean实例（无参数构造）

    - **第二步：** 为bean的属性设置值和对其他bean的引用（调用set方法）

        - **后置处理一：** 把bean的实例传递给bean后置处理器的方法

      ```java
      public Object postProcessBeforeInitialization();
      ```

    - **第三步：** 调用bean的初始化方法（需要进行配置初始化的方法）

        - **后置处理二：** 把bean的实例传递给bean后置处理器的方法

      ```java
      public Object postProcessAfterInitialization();
      ```

    - **第四步：**bean可以使用（对象获取到了）

    - **第五步：**当容器关闭的时候，调用bean的销毁方法（需要进行配置销毁的方法）

- **添加后置处理器：**

    - 创建类，实现BeanPostProcessor接口，创建后置处理器
    - 实现方法

  ```java
  public class MyBeanPost implements BeanPostProcessor {
      
      @Override
      public Object postProcessBeforeInitialization(Object bean, String beanName) throws BeansException {
          System.out.println("在初始化之前执行的方法");
          return bean;
      }
  
      @Override
      public Object postProcessAfterInitialization(Object bean, String beanName) throws BeansException {
          System.out.println("在初始化之后执行的方法");
          return bean;
      }
  }
  ```

- **演示：**

  ```java
  public class Orders {
  
      //无参构造
      public Orders() {
          System.out.println("第一步 执行无参构造创建bean实例");
      }
  
      private String oname;
      public void setOname(String oname){
          this.oname = oname;
          System.out.println("第二步 调用set方法设置属性值");
      }
  
      //创建执行的初始化的方法
      public void initMethod(){
          System.out.println("第三步 执行初始化的方法");
      }
  
      //写在创建对象那个的class里
      @Test
      public void testBean3(){
          ApplicationContext context = new ClassPathXmlApplicationContext("bean.xml");
          Orders orders = context.getBean("Orders", Orders.class);
          System.out.println(orders);
          System.out.println("第四步 获取创建bean实例对象");
          //手动让bean实例销毁，会调用destoryMethod
          ((ClassPathXmlApplicationContext)context).close();
      }
  
      //创建执行的销毁的方法
      public void destoryMethod(){
          System.out.println("第五步 执行销毁的方法");
      }
  }
  ```

  ```xml
  <bean id="orders" class="com.dm.Orders" init-method="initMethod" destory-Method="destoryMethod">
  	<property name="oname" value=""></property>
  </bean>
  
  <!--配置后置处理器,会给当前配置文件中的所有实例都添加上后置处理器-->
  <bean id="myBeanPost" class="com.dm.MyBeanPost"></bean>
  ```

### 自动装配

自动装配：根据指定的装配规则(属性名称/属性类型)，Spring自动将匹配的属性值进行注入

```xml
<!--实现自动装配
    bean标签属性autowire，配置自动装配
    autowire属性常用两个值：
        byName根据属性名称注入，注入值bean的id值需要与类属性名称给你一样
        byType根据属性类型注入，但当容器中存在多个相同类型的bean时会报错
-->
<bean id="emp" class="com.dm.autowire.Emp" autowire="byName"></bean>
<bean id="emp" class="com.dm.autowire.Emp" autowire="byType"></bean>

<bean id="dept" class="com.dm.autowire.Dept"></bean>
```

一般用注解，基于xml方式比较少

### 引入外部属性文件

1. 直接配置数据库

    - 引入druid依赖

      ```xml
      <dependency>
          <groupId>com.alibaba</groupId>
          <artifactId>druid</artifactId>
          <version>1.0.9</version>
      </dependency>
      ```

    - 配置druid连接池

      ```xml
       <!--直接配置连接池-->
       <bean id="dataSource" class="com.alibaba.druid.pool.DruidDataSource">
           <property name="driverClassName" value="com.mysql.cj.jdbc.Driver"></property>
           <property name="url" value="jdbc:mysql://localhost:3306/数据库名"></property>
           <property name="username" value="root"></property>
           <property name="password" value="123456"></property>
       </bean>
      ```

2. 引入外部文件配置数据库

    - 创建外部属性文件(properties格式)，写数据库信息

      ```properties
      prop.driverClass=com.alibaba.druid.pool.DruidDataSource
      prop.url=jdbc:mysql://localhost:3306/数据库名
      prop.username=root
      prop.password=123456
      ```

    - 把外部properties属性文件引入spring配置文件中

        - 引入context名称空间

      ```xml
      <!--名称空间-->
      xmlns:context="http://www.springframework.org/schema/context"
      xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd
                          <!--名称空间-->
                          http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-beans.xsd">
      ```

        - 配置连接池

      ```xml
      <!--引入外部属性文件-->
      <context:property-placeholder location="classpath:jdbc.properties"/>
      <!--配置连接池-->
      <bean id="dataSource" class="com.alibaba.druid.pool.DruidDataSource">
          <property name="driverClassName" value="${prop.driverClass}"></property>
          <property name="url" value="${prop.url}"></property>
          <property name="username" value="${prop.username}"></property>
          <property name="password" value="${prop.password}"></property>
      </bean>
      ```

## 注解管理

**注解：**

- 注解是代码特殊标记，格式：@注解名称（属性名称=属性值，属性名称=属性值..）
- 注解作用在类上面，方法上面，属性上面
- 使用注解的目的：简化xml配置

**Spring针对Bean管理中创建对象提供注解：**

- **@Component**：标注一个普通的spring Bean类
- **@Service**：标注一个业务逻辑组件类
- **@Controller**：标注一个控制器组件类
- **@Repository**：标注一个DAO组件类

**上面四个注解功能是一样的，都可以用来创建bean实例**

### 创建对象

- **第一步：引入依赖**

- **第二步：开启组件扫描**

```xml
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:p="http://www.springframework.org/schema/p"
       <!--引入context空间-->
       xmlns:context="http://www.springframework.org/schema/context"
       xmlns:util="http://www.springframework.org/schema/util"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd
                           http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context.xsd">

        <!--开启组件扫描
        多个包：
        1.如果扫描多个包，多个包使用逗号隔开
        2.如果都在一个目录下，就扫描你包的上层目录
        -->
        <context:component-scan base-package="com.dm.TestBean"></context:component-scan>

 		<!--use-default-filter="false"表示现在不使用默认filter，自己配置filter
            context:include-filter, 设置扫描哪些内容
			context:exclude-filter, 设置不扫描哪些内容
        -->
        <context:component-scan base-package="com.dm" use-default-filters="false">        
            <!--只扫描用Controller配置的类-->
        	<context:include-filter type="annotation" expression="org.springframework.stereotype.Controller"/>
        </context:component-scan>
    
        <context:component-scan base-package="com.dm">
            <!--不扫描用Controller配置的类-->
            <context:exclude-filter type="annotation" expression="org.springframework.stereotype.Controller"/>
        </context:component-scan>

</beans>
```

- **第三步：创建类，在类上面添加创建对象注解**

```java
// 在注解里面value属性值可以省略不写
// 默认值是类名称，首字母小写
@Component(value = "userService") //类似于<bean id="" class="">
public class UserService2 {
    public void add() {
        System.out.println("service add......");
    }
}
```

获取对象：

```java
ApplicationContext context = new ClassPathXmlApplicationContext("bean.xml");
UserService userService = context.getBean("userService",UserService.class);  
```

### 注入属性

**Spring提供的注解：**

- **@AutoWired**：根据属性类型进行注入
- **@Qualifier**：根据属性的名称进行注入, 和@AutoWired一起使用
- **@Resource**：可以根据类型注入，可以根据名称注入，高版本JDK移除了
- **@Value**：注入普通类型属性

**第一步：把service和dao对象创建——在service和dao类添加创建对象注解**

**第二步：在service注入dao对象——在service类添加dao类型属性，在属性上面使用注解**

```java
public class UserService{
    // 定义dao类型属性
    // 不需要添加set方法，已经封装好了
    // 添加注入属性注解
    
    // @AutoWired,根据类型注入
    @AutoWired
    private UserDao userDao;
    
    // @Qualifier,根据名称注入，可以指定那个实现类
    @AutoWired
    @Qualifier(value="userDao")
    private UserDao userDao;
    
    // @Resource,根据类型注入
    @Resource
    private UserDao userDao;
    // @Resource,根据名称注入
    @Resource(name="userDao")
    private UserDao userDao;
    
    //@Value，注入普通类型属性
    @Value(value = "abc")
    private String name;
}
```

## 完全注解开发

**一般用SpringBoot进行**

创建配置类，就不需要创建xml配置文件了

(1) 创建包config，创建配置类SpringConfig

```java
@Configuration // 作为配置类，替代xml配置文件
@ComponentScan(basePackages = {"com.dm"})// 组件扫描
public class SpringConfig{
}
```

(2) 编写测试类

```java
@Test
public void test(){
    // 加载配置文件
    ApplicationContext context = new AnnotationConfigApplicationContext(SpringConfig.class);
	UserService userService = context.getBean("userService", UserService.class);
    userService.add();
}
```

## xml与注解小结

- **xml与注解：**
    - xml更加万能，适用于任何场合！维护简单方便
    - 注解不是自己的类是用不了，维护相对复杂！
- **xml与注解的最佳实践：**
    - xml用来管理bean
    - 注解只负责完成属性的注入
    - 在使用过程中，要注意一个问题：必须要让注解生效，即开启对注解的支持

# AOP

## AOP概念

AOP（面向切面编程），利用AOP可以对业务逻辑的各个部分进行隔离，从而使得业务逻辑各部分之间的耦合度降低，提高程序的可重用性，同时提高了开发的效率。

**不通过修改源代码的方式，在主干功能里添加新的功能**

### 主要功能

日志记录，性能统计，安全控制，事务处理，异常处理等等。

### 主要意图

将日志记录，性能统计，安全控制，事务处理，异常处理等代码从业务逻辑代码中划分出来，通过对这些行为的分离，我们希望可以将它们独立到非指导业务逻辑的方法中，进而改变这些行为的时候不影响业务逻辑的代码。

### 应用举例

在登录过程中，希望加入一个权限判断的功能

- 原始方式：修改源代码

  ```java
  if 管理员
  else if 普通用户
  ```

- AOP模式：添加一个权限判断模块，不需要修改源代码

## AOP底层原理

AOP底层使用动态代理:

- 有接口的情况，使用JDK动态代理
    - 创建接口实现类代理对象（UserDaoImpl），增强类的方法
- 没有接口的情况，使用CGLIB动态代理
    - 创建子类的代理对象(UserDao)，增强类的方法

### JDK动态代理

​    **1.使用JDK动态代理，使用Proxy类里面的方法创建代理对象**

 调用`newPorxyInstance`方法

![](https://gitee.com/dong-dameng/images/raw/master/images/20210510164754.jpg)

 方法中有三个参数

- 第一个参数`ClassLoader`：类加载器
- 第二个参数`类<?>[]`增强方法所在的类，这个类实现的**接口**。它支持多个接口
- 第三个参数`InvocationHandler`：实现接口`InvocationHandler`，创建代理对象，写增强的方法

​    **2.编写JDK动态代理代码：**

 （1）创建接口，定义方法

```java
public interface UserDao {
    public int add(int a,int b);
    public String update(String id);
}
```

 （2）创建接口的实现类，实现方法

```java
public class UserDaoImpl implements UserDao{
    @Override
    public int add(int a, int b) {
        return a+b;
    }
    @Override
    public String update(String id) {
        return id;
    }
}
```

 （3）使用Porxy类创建接口代理对象

```java
public class JDKProxy {
    public static void main(String[] args) {
        //创建接口实现类代理对象(lang包)
        Class[] interfaces = {UserDao.class};
        UserDaoImpl userDaoImpl = new UserDaoImpl();
        UserDao userDao =(UserDao)Proxy.newProxyInstance(JDKProxy.class.getClassLoader(), interfaces, new UserDaoProxy(userDaoImpl));
        int add = userDao.add(1,2);
        System.out.println("result=" + add);
    }
}

//创建代理对象代码
class UserDaoProxy implements InvocationHandler {
    //把创建的代理对象的原型传递过来
    //有参构造传递
    private Object object;

    public UserDaoProxy(Object objject) {
        this.object = objject;
    }

    /*在方法中编写增强的逻辑
     * proxy  代理的对象
     * method 需要增强的方法
     * args   参数
     */
    @Override
    public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {
        //add方法之前做的处理
        System.out.println("方法之前执行..." + method.getName() + ":传递的参数:" + Arrays.toString(args));
        //被增强的方法执行
        Object result = method.invoke(object, args);
        //add方法之后做的处理
        System.out.println("方法之后执行..." + object);
        return result;
    }
}
```

 （4）结果：

![](https://gitee.com/dong-dameng/images/raw/master/images/20210510172252.jpg)

## AOP操作术语

- **连接点：**类里面可以被增强的方法，称为连接点

- **切入点：**实际被真正增强的方法，称为切入点

- **通知（增强）：**

  （1）实际增强的逻辑部分称为通知（增强）

  （2）通知有多种类型：

    - 前置通知：方法之前执行
    - 后置通知：方法之后执行
    - 环绕通知：方法前后都执行
    - 异常通知：出现异常时执行
    - 最终通知：无论如何都会在最后执行（类似于try...catch中的finally）

- **切面：**切面是一个动作，把通知应用到切入点的过程，称为切面

## AOP准备工作

**1.在Spring框架中，一般基于AspectJ实现AOP操作**

 （1）什么是AspectJ

 AspectJ不是Spring组成部分，独立于AOP框架，一般把AspectJ和Spring框架一起使用，进行AOP操作

**2.基于AspectJ实现AOP操作**

 （1）基于xml配置文件实现

 （2）基于注解方式实现（推荐）

**3.在项目工程里面引入AOP相关依赖**

**4.切入点表达式**

（1）切入点表达式的作用：知道对哪个类里面的哪个方法进行增强

（2）语法结构：

- `execution([权限修饰符(可省略)][返回值类型][类全路径].[方法名称]([参数列表]))`

    - 例1：对com.dm.dao.BookDao类里面的add进行增强

      权限修饰符:`省略`

      返回值类型: `*`

      类全路径.方法名称([参数列表]):`com.dm.dao.BookDao.add(...)`

      `execution(* com.dm.dao.BookDao.add(...))`

    - 例2：对com.dm.dao.BookDao类里面所有的方法进行增强

      ``execution(* com.dm.dao.BookDao.*(...))``

    - 例3：对com.dm.dao包里面的所有类，类里面的所有方法进行增强

      `execution(* com.dm.dao.*.*(...))`

## Aspectj 注解操作

### 1、创建一个类，在类里面定义方法

```java
public class User{
    public void add(){
        System.out.printlin("add...")
    }
}
```

### 2、创建增强类（编写增强逻辑）

在增强的类里面，创建方法，让不同的方法代表不同的通知类型

```java
//增强的类
public class UserProxy{
    //前置通知
    public void before(){
        System.out.println("before...")
    }    
}
```

### 3、进行通知的配置

#### （1）在spring配置文件中，开启注解扫描

 举例：

```xml
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       <!--引入空间-->
       xmlns:context="http://www.springframework.org/schema/context"
       xmlns:util="http://www.springframework.org/schema/util"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd
                           http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context.xsd">

        <!--开启组件扫描
        多个包：
        1.如果扫描多个包，多个包使用逗号隔开
        2.如果都在一个目录下，就扫描你包的上层目录
        -->
        <context:component-scan base-package="com.dm.TestBean"></context:component-scan>

 		<!--use-default-filter="false"表示现在不使用默认filter，自己配置filter
            context:include-filter, 设置扫描哪些内容
			context:exclude-filter, 设置不扫描哪些内容
        -->
        <context:component-scan base-package="com.dm" use-default-filters="false">
            <!--只扫描用Controller配置的类-->
        	<context:include-filter type="annotation" expression="org.springframework.stereotype.Controller"/>
        </context:component-scan>
    
        <context:component-scan base-package="com.dm">
            <!--不扫描用Controller配置的类-->
            <context:exclude-filter type="annotation" expression="org.springframework.stereotype.Controller"/>
        </context:component-scan>
</beans>
```

#### （2）使用注解创建User和UserProxy对象

```java
//被增强的类
@Component
public class User{...}
//增强的类
@Component
public class UserProxy{...}
```

#### （3）在增强的类上面添加注解@Aspect

```java
//增强的类
@Component
@Aspect
public class UserProxy{...}
```

#### （4）在spring配置文件中开启生成代理对象

```xml
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:aop="http://www.springframework.org/schema/aop"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd
       http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context.xsd
       http://www.springframework.org/schema/aop http://www.springframework.org/schema/aop/spring-aop.xsd">
	<!--开启注解扫描-->
	<context:component-scan base-package="com.dm.spring"></context:component-scan>
	<!--开启Aspect生成代理对象-->
	<aop:aspectj-autoproxy/>
</beans>
```

### 4.配置不同类型的通知

（1）在增强类的里面，在作为通知方法上面添加通知类型注解，使用切入点表达式配置。

```java
@Component
@Aspect
//增强的类
public class UserProxy{
    // 前置通知
    // aspectj包下的注解
    // @Before注解表示作为前置通知
    @Before(value="execution(* com.dm.spring.aop.User.add(..))")
    public void before(){
        System.out.println("before...")
    }    
}
```

### 5.测试

正确输出：

```
before...
add...
```

### 6.其他通知类型

```java
// @After 最终通知
@After(value="execution(* com.dm.spring.aop.User.add(..))")

// @AfterReturning 后置通知
@AfterReturning(value="execution(* com.dm.spring.aop.User.add(..))")

// @AfterThrowing 异常通知
@AfterThrowing(value="execution(* com.dm.spring.aop.User.add(..))")

// @Around 环绕通知(在方法前后都执行)
@Around(value="execution(* com.dm.spring.aop.User.add(..))")

public void around(ProceedingJoinPoint proceedingJoinPoint) throws Throwable{
    System.out.println("环绕之前...")
    //被增强的方法
    proceedingJoinPoint.proceed();
    System.out.println("环绕之后...")
}
```

- After表示在方法执行之后执行
- AfterReturning表示在方法返回值之后执行
- 环绕前-前置-AfterReturning-后置-环绕后

### 7、公共切入点抽取

```java
// 抽取公共切入点
@Pointcut(value="execution(* com.dm.spring.aop.User.add(..))")
public void pointdemo(){
    
}
// 直接传入公共的增强方法名
@Before(value="pointdemo()")
...
```

### 8、设置增强类的优先级

（1）在增强类的上面添加注解@Order(数字类型值)，数字类型的值越小，优先级越高。

```java
//PersonProxy先执行

@component
@Aspect
@Order(1)
public class PersonProxy{...}

@component
@Aspect
@Order(3)
public class UserProxy{...}
```

## Aspectj xml操作

实际中一般用注解模式

### 1.创建增强类和被增强类

```java
// 被增强类
public class Book {
    public void buy() {
        System.out.println("buy");
    }
}
// 增强类
public class BookProxy {
    public void before() {
        System.out.println("before...");
    }
}
```

### 2.创建两个类对象

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
    
    <!--创建对象-->
    <bean id="book" class="com.dm.aop_xml.Book"></bean>
    <bean id="bookProxy" class="com.dm.aop_xml.BookProxy"></bean>

</beans>
```

### 3.在Spring配置文件中配置切入点

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:aop="http://www.springframework.org/schema/aop"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd
       http://www.springframework.org/schema/aop http://www.springframework.org/schema/aop/spring-aop.xsd">

    <!--创建对象-->
    <bean id="book" class="com.dm.aop_xml.Book"></bean>
    <bean id="bookProxy" class="com.dm.aop_xml.BookProxy"></bean>

    <!--配置AOP增强-->
    <aop:config>
        <!--切入点
            id:切入点名字,随便起
            expression:切入点表达式
        -->
        <aop:pointcut id="p" expression="execution(* com.dm.aop_xml.Book.buy())"/>
        <!--配置切面
            ref:增强类
        -->
        <aop:aspect ref="bookProxy">
            <!--增强作用在具体的方法
                aop:通知类型
                method:增强的方法名
                pointcut-ref:切入点名称
            -->
            <aop:before method="before" pointcut-ref="p"></aop:before>
        </aop:aspect>
    </aop:config>
    
</beans>
```

### 4.测试

```shell
before...
buy
```

## 完全注解开发

创建配置类，替代xml配置文件

```java
@Configuration
@ComponentScan(basePackages = {"com.dm.aop_xml"})
@EnableAspectJAutoProxy(proxyTargetClass = true) // 默认为false
public class ConfigAop {
}
```

# JDBCTemplate

## 概念

**JDBC：**Java DataBase Connect

**JdbcTemplate：**Spring框架对JDBC进行封装，使用JDBCTemplate方便实现对数据库的操作

## 快速开始

### 引入依赖

```xml
<!--mysql-->
<dependency>
    <groupId>mysql</groupId>
    <artifactId>mysql-connector-java</artifactId>
    <scope>runtime</scope>
</dependency>
<!--spring-jdbc-->
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-jdbc</artifactId>
    <version>5.3.6</version>
</dependency>
<!--spring-tx-->
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-tx</artifactId>
    <version>5.3.6</version>
</dependency>
<!--spring-orm-->
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-orm</artifactId>
    <version>5.3.6</version>
</dependency>
<!--druid-->
<dependency>
    <groupId>com.alibaba</groupId>
    <artifactId>druid</artifactId>
    <version>1.0.9</version>
</dependency>
```

### 配置连接池

```xml
<!--直接配置连接池-->
<bean id="dataSource" class="com.alibaba.druid.pool.DruidDataSource" destroy-method="close">
    <property name="driverClassName" value="com.mysql.cj.jdbc.Driver"></property>
    <property name="url" value="jdbc:mysql://localhost:3306/数据库名?useUnicode=true&amp;characterEncoding=UTF-8&amp;serverTimezone=Asia/Shanghai&amp;useSSL=true"></property>
    <property name="username" value="root"></property>
    <property name="password" value="123456"></property>
</bean>
```

### 配置JDBC模板

```xml
<!--JdbcTemplate对象-->
<bean id="jdbcTemplate" class="org.springframework.jdbc.core.JdbcTemplate">
    <!--注入dataSource-->
    <property name="dataSource" ref="dataSource"></property>
</bean>
```

### 创建相关类

创建service类和dao类，在dao类中注入jdbcTemplate对象

- 开启组件扫描

```xml
<!--开启组件扫描-->
<context:component-scan base-package="com.dm"></context:component-scan>
```

- 创建BookDao，注入JdbcTemplate

```java
// 接口
public interface BookDao {
}

@Repository
public class BookDaoImpl implements BookDao{
    // 注入JdbcTemplate
    @Autowired
    private JdbcTemplate jdbcTemplate;
}
```

- 创建BookService，注入BookDao

```java
@Service
public class BookService {
    // 注入Dao
    @Autowired
    private BookDao bookDao;
}
```

### 编写实体类

entity层

```java
@Data
@NoArgsConstructor
public class Book {
    private String userId;
    private String username;
    private String ustatus;
}
```

### 编写service和dao

- 在dao进行数据库添加操作
- 调用JdbcTemplate 对象里面的update方法
    - 第一个参数：sql语句
    - 第二个参数：可变参数，设置sql语句中的值

```java
// dao类，先写接口，再实现接口
@Override
public void add(Book book) {
    // 1.创建sql语句
    String sql = "insert into t_book values(?,?,?)";
    Object[] args = {book.getUserId(), book.getUsername(), book.getUstatus()};
    // 2.调用方法实现
    int update = jdbcTemplate.update(sql, args);
    System.out.println(update);
}
```

![image-20211018211338544](../../../AppData/Roaming/Typora/typora-user-images/image-20211018211338544.png)

- 编写service

```java
// 添加的方法
public void addBook(Book book) {
    bookDao.add(book);
}
```

- 测试

```java
@Test
public void testJT() {
    ClassPathXmlApplicationContext context = new ClassPathXmlApplicationContext("bean1.xml");
    BookService bookService = context.getBean("bookService", BookService.class);
    Book book = new Book();
    book.setUserId("1");
    book.setUsername("Java");
    book.setUstatus("a");
    bookService.addBook(book);
}
```

- 结果

```shell
10月 18, 2021 9:40:03 下午 com.alibaba.druid.support.logging.JakartaCommonsLoggingImpl info
信息: {dataSource-1} inited # 1表示影响的行数
1 # 返回值，表示操作成功
```

## 增添操作

```java
@Override
public void add(Book book) {
    // 1.创建sql语句
    String sql = "insert into t_book values(?,?,?)";
    Object[] args = {book.getUserId(), book.getUsername(), book.getUstatus()};
    // 2.调用方法实现
    int update = jdbcTemplate.update(sql, args);
    System.out.println(update);
}
```

测试：

```java
@Test
public void testJT1() {
    ClassPathXmlApplicationContext context = new ClassPathXmlApplicationContext("bean1.xml");
    BookService bookService = context.getBean("bookService", BookService.class);
    Book book = new Book();
    book.setUserId("1");
    book.setUsername("Java");
    book.setUstatus("a");
    // 增添
    bookService.addBook(book);
}
```

## 修改操作

```java
@Override
public void update(Book book) {
    // 1.创建sql语句
    String sql = "update t_book set username=?,ustatus=? where user_id=?";
    Object[] args = {book.getUsername(), book.getUstatus(), book.getUserId()};
    // 2.调用方法实现
    int update = jdbcTemplate.update(sql, args);
    System.out.println(update);
}
```

测试：

```java
@Test
public void testJT2() {
    ClassPathXmlApplicationContext context = new ClassPathXmlApplicationContext("bean1.xml");
    BookService bookService = context.getBean("bookService", BookService.class);
    Book book = new Book();
    book.setUserId("1");
    book.setUsername("JoJo");
    book.setUstatus("VBVB");
    // 修改
    bookService.updateBook(book);
}
```

## 删除操作

```java
@Override
public void delete(String id) {
    // 1.创建sql语句
    String sql = "delete from t_book where user_id=?";
    // 2.调用方法实现
    int update = jdbcTemplate.update(sql, id);
    System.out.println(update);
}
```

测试：

```java
 @Test
 public void testJT3() {
     ClassPathXmlApplicationContext context = new ClassPathXmlApplicationContext("bean1.xml");
     BookService bookService = context.getBean("bookService", BookService.class);
     // 删除
     bookService.deleteBook("1");
 }
```

## 查询操作

### 查询并返回某个值

### 查询并返回对象

### 查询返回集合





