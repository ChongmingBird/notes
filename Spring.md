# 【Spring】

## 概述

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

## 快速开始

- 建SpringWeb项目
- 创建User实体类

```java
public class User {
    public void add() {
        System.out.println("add...");
    }
}
```

- 创建配置文件

  xml文件：applicationContext.xml，放在resources目录下


```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
    <!--配置User对象创建-->
    <bean id="User" class="com.dm.entity.User"></bean>
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

## IOC

> **控制反转（Inversion of Control）**，控制反转的是对象的创建权
>
> **依赖注入（Dependency Injection）**，绑定对象与对象之间的依赖关系

### 简介

项目中new多个对象，复杂度和维护难度很高。

**控制反转（IOC）思想：**使用对象时，由主动new产生对象转换为由外部提供对象，此过程中**对象创建控制权由程序转移到外部**。

**Spring和IOC的关系：**

- Spring技术对IOC思想进行了实现
- Spring提供了一个容器，称为**IOC容器**，用来充当IOC思想中的"外部"，IOC思想中的外部指的就是Spring的IOC容器

**IOC容器的作用于内部存放的是什么：**

* IOC容器负责对象的创建、初始化等一系列工作，其中包含了数据层和业务层的类对象
* 被创建或被管理的对象在IOC容器中统称为bean
* IOC容器中放的就是一个个的bean对象

当IOC容器中创建好service和dao对象后，程序不能正确执行。因为service运行需要依赖dao对象。IOC容器中虽然有service和dao对象，但是service对象和dao对象没有任何关系。需要把dao对象交给service,也就是说要绑定service和dao对象之间的关系。

**依赖注入（Dependency Injection）：**在容器中建立bean与bean之间的依赖关系的整个过程，称为依赖注入。

IOC和DI的最终目标就是：充分解耦

- 使用IOC容器管理bean
- 在IOC容器内将所有依赖关系的bean进行关系绑定（DI）
- 最终结果为：使用对象时不仅可以直接从IOC容器中获取，并且获取的bean已经绑定了所有的依赖关系

### 入门案例

- 添加jar包

  ```xml
  <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-context</artifactId>
      <version>5.2.10.RELEASE</version>
  </dependency>
  ```

- 创建dao和service类，为属性提供setter方法

- 在配置文件中添加依赖注入的配置

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
  
      <bean id="bookService" class="com.itheima.service.impl.BookServiceImpl">
          <!--配置server与dao的关系-->
          <!--property标签表示配置当前bean的属性
          		name属性表示bean中的属性
          		ref属性表示对应id的bean
  		-->
          <property name="bookDao" ref="bookDao"/>
      </bean>
      
  </beans>
  ```

  **注意配置中两个bookDao的含义不同：**

  * name="bookDao"中`bookDao`的作用是让Spring的IOC容器在获取到名称后，找对应的`setBookDao()`方法进行对象注入
  * ref="bookDao"中`bookDao`的作用是让Spring能在IOC容器中找到id为`bookDao`的Bean对象给`bookService`进行注入

  ![1629736314989](https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-master202306121343996.png)

- 获取IOC容器

  从容器中获取对象进行方法调用

  ```java
  public class App {
      public static void main(String[] args) {
          //获取IOC容器
  		ApplicationContext ctx = new ClassPathXmlApplicationContext("applicationContext.xml"); 
          BookService bookService = (BookService) ctx.getBean("bookService");
          // save方法底层调用了bookDao的save()，依赖注入交给Spring处理
          bookService.save();
      }
  }
  ```

### bean基础配置

#### id与class

```xml
<bean id="" class=""/>
```

![image-20210729183500978](https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-master202306121347613.png)

#### name

name属性定义bean的别名，定义后可以通过bean标签的id和name属性中任意一个值获取bean对象

![](https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-master202306121348386.png)

#### 作用范围与scope配置

![](https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-master202306121350219.png)

**Spring中bean默认为单例：**

* **为什么bean默认为单例?**
  * bean为单例的意思是在Spring的IOC容器中只会有该类的一个对象
  * bean对象只有一个就避免了对象的频繁创建与销毁，达到了bean对象的复用，性能高
* **bean在容器中是单例的，会不会产生线程安全问题?**
  * 如果对象是有状态对象，即该对象有成员变量可以用来存储数据的，
  * 因为所有请求线程共用一个bean对象，所以会存在线程安全问题。
  * 如果对象是无状态对象，即该对象没有成员变量没有进行数据存储的，
  * 因方法中的局部变量在方法调用完成后会被销毁，所以不会存在线程安全问题。
* **哪些bean对象适合交给容器进行管理?**
  * 表现层对象
  * 业务层对象
  * 数据层对象
  * 工具对象
* **哪些bean对象不适合交给容器进行管理?**
  * 封装实例的域对象，因为会引发线程安全问题，所以不适合

### bean的实例化

实例化bean的三种方式，

- **构造方法**
- **静态工厂**
- **实例工厂**

#### 构造方法

Spring默认使用类的无参构造方法创建bean

#### 静态工厂

- 编写静态工厂：

  创建一个工厂类OrderDaoFactory并提供一个静态方法

  ```java
  //静态工厂创建对象
  public class OrderDaoFactory {
      public static OrderDao getOrderDao(){
          return new OrderDaoImpl();
      }
  }
  ```

- 静态工厂实例化：

  在spring的配置文件中添加以下内容:

  ```xml
  <bean id="orderDao" class="com.itheima.factory.OrderDaoFactory" factory-method="getOrderDao"/>
  ```

  - class:工厂类的类全名
  - factory-mehod:具体工厂类中创建对象的方法名

- 通过IOC容器获取对象：

  ```java
  public class AppForInstanceOrder {
      public static void main(String[] args) {
          //通过静态工厂创建对象
          OrderDao orderDao = OrderDaoFactory.getOrderDao();
          orderDao.save();
      }
  }
  ```

#### 实例工厂

- 创建一个工厂类OrderDaoFactory并提供一个普通方法。

  注意此处和静态工厂的工厂类不一样，不是静态方法

  ```java
  public class UserDaoFactory {
      public UserDao getUserDao(){
          return new UserDaoImpl();
      }
  }
  ```

- 在spring的配置文件中添加以下内容:

  ```xml
  <bean id="userFactory" class="com.itheima.factory.UserDaoFactory"/>
  <bean id="userDao" factory-method="getUserDao" factory-bean="userFactory"/>
  ```

  **运行顺序：**

  - 创建实例化工厂对象,对应的是第一行配置
  - 调用对象中的方法来创建bean，对应的是第二行配置
    - factory-bean:工厂的实例对象
    - factory-method:工厂对象中的具体创建对象的方法名

- 从IOC容器中获取bean

  ```java
  public class AppForInstanceUser {
      public static void main(String[] args) {
          ApplicationContext ctx = new 
              ClassPathXmlApplicationContext("applicationContext.xml");
          UserDao userDao = (UserDao) ctx.getBean("userDao");
          userDao.save();
      }
  }
  ```

#### FactoryBean

Spring通过FactoryBean简化实例工厂的配置方式

- 创建一个UserDaoFactoryBean的类，实现FactoryBean接口，重写接口的方法

  ```java
  public class UserDaoFactoryBean implements FactoryBean<UserDao> {
      //代替原始实例工厂中创建对象的方法
      public UserDao getObject() throws Exception {
          return new UserDaoImpl();
      }
      //返回所创建类的Class对象
      public Class<?> getObjectType() {
          return UserDao.class;
      }
  }
  ```

- 在Spring的配置文件中进行配置

  ```xml
  <bean id="userDao" class="com.itheima.factory.UserDaoFactoryBean"/>
  ```

- AppForInstanceUser运行类不用做任何修改，直接运行


这种方式在Spring去整合其他框架的时候会被用到。

FactoryBean接口有三个方法：

```java
// 重写后，在方法中进行对象的创建并返回
T getObject() throws Exception;
// 重写后，主要返回被创建类的class对象
Class<?> getObjectType();
// 默认单例模式
default boolean isSingleton() {
		return true;
}
```

### DI依赖注入

# 【Maven依赖】

## 

