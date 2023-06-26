

# 【概述】

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

# 【快速开始】

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

# 【IOC】

> **控制反转（Inversion of Control）**，控制反转的是对象的创建权
>
> **依赖注入（Dependency Injection）**，绑定对象与对象之间的依赖关系

## 简介

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

## 入门案例

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



## 基于XML配置

**StringBean 配置详解**

|            标签            |                             功能                             |
| :------------------------: | :----------------------------------------------------------: |
|  `<bean id="" class="">`   |                    Bean的id和全限定名配置                    |
|      `<bean name="">`      |  通过name设置Bean的别名<br />通过别名也能直接获取到Bean实例  |
|     `<bean scope="">`      | Bean的作用范围<br />BeanFactory作为容器时取值singleton和prototype |
|   `<bean lazy-init="">`    | Bean的实例化时机，是否延迟加载。<br />BeanFactory作为容器时无效 |
|   `<bean init-method=""`   |      Bean实例化后自动执行的初始化方法，method指定方法名      |
| `<bean destroy-method="">` |            Bean实例销毁前的方法，method指定方法名            |
| `<bean autowire="byType">` |   设置自动注入模式，常用的有按照类型byType，按照名字byName   |
|  `<bean factory-bean=""`   |                指定创建Bean的工厂Bean及其方法                |

### id | class

```xml
<bean id="" class=""/>
```

![image-20210729183500978](https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-master202306121347613.png)

### 别名

name属性定义bean的别名，定义后可以通过bean标签的id和name属性中任意一个值获取bean对象

![](https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-master202306121348386.png)

### 作用范围

![](https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-master202306121350219.png)

**Bean的范围配置：**

默认情况下，单纯的Spring环境Bean的作用范围有两个：

- **singleton：**

  单例，默认值。Spring容器创建的时候，就会进行Bean的实例化，并存储到容器内部的单例池中，每次getBean时都是从单例池中获取相同的Bean实例

- **prototype：**

  原型。Spring容器初始化时不会创建Bean实例，当调用getBean时，每次调用都会创建一个新的Bean实例

  

> * **为什么bean默认为单例?**
>   * bean为单例的意思是在Spring的IOC容器中只会有该类的一个对象
>   * bean对象只有一个就避免了对象的频繁创建与销毁，达到了bean对象的复用，性能高
> * **bean在容器中是单例的，会不会产生线程安全问题?**
>   * 如果对象是有状态对象，即该对象有成员变量可以用来存储数据的，因为所有请求线程共用一个bean对象，所以会存在线程安全问题。
>   * 如果对象是无状态对象，即该对象没有成员变量没有进行数据存储的，因方法中的局部变量在方法调用完成后会被销毁，所以不会存在线程安全问题。
> * **哪些bean对象适合交给容器进行管理?**
>   * 表现层对象
>   * 业务层对象
>   * 数据层对象
>   * 工具对象
> * **哪些bean对象不适合交给容器进行管理?**
>   * 封装实例的域对象，因为会引发线程安全问题，所以不适合
>

### 延迟加载

**Bean的延迟加载**

```xml
<bean 
      id="userDao" 
      class="com.chong.dao.impl.UserDaoImpl" 
      lazy-init="true"
 />
```

当lazy-init设置为true时为延迟加载（true），也就是当Spring容器创建的时候，不会立即创建Bean实例，等待用到时在创建Bean实例并存储到单例池中去，后续在使用该Bean直接从单例池获取即可，本质上该Bean还是单例的。

### 初始化与销毁方法

Bean在被实例化后，可以执行指定的初始化方法完成一些初始化的操作。Bean在销毁之前也可以执行指定的销毁方法完成一些操作。

**配置初始化与销毁方法：**

```xml
<bean 
      id="userDao" 
      class="com.chong.dao.impl.UserDaoImpl" 
      init-method="init" 
      destroy-method="destroy"
/>
```

```java
public class UserDaoImpl implements UserDao {
    public UserDaoImpl() { 
        System.out.println("UserDaoImpl创建了..."); 
    }
    public void init(){ 
        System.out.println("初始化方法..."); 
    }
    public void destroy(){ 
        System.out.println("销毁方法..."); 
    }
}
```

**扩展：可以通过实现InitializingBean接口，完成一些Bean的初始化操作：**

```java
public class UserDaoImpl implements UserDao, InitializingBean {
    //执行时机早于init-method配置的方法
    public void afterPropertiesSet() throws Exception {
        System.out.println("InitializingBean..."); 
    } 
}
```

### 实例化配置

Spring的实例化方法主要有如下两种

#### 构造方法实例化

- **构造方法实例化：**底层通过构造方法对Bean进行实例化

  - **无参构造：**spring默认采用无参构造方式创建bean

  - **有参构造：**有参构造在实例化Bean时，需要参数的注入，通过标签，嵌入在标签内部提供构造参数

    ```xml
    <bean id="userDao" class="com.chongming.dao.impl.UserDaoImpl">
        <constructor-arg name="pwd" value="123456"/>
    </bean>
    ```

#### 工厂方式实例化

**工厂方式实例化：**底层通过调用自定义的工厂方法对Bean进行实例化

- 静态工厂
- 实例工厂
- BeanFactory

##### 静态工厂实例化

静态工厂方法实例化Bean，其实就是定义一个工厂类，提供一个静态方法用于生产Bean实例，在将该工厂类及其静态方法配置给Spring即可

- 编写静态工厂：

  ```java
  // 工厂类
  public class UserDaoFactoryBean {
      // 非静态工厂方法
      public static UserDao getUserDao(String name){
          // 可以在此编写一些其他逻辑代码
          return new UserDaoImpl();
      }
  }
  ```

- 配置spring

  ```xml
  <bean id="userDao" 
        class="com.chongming.factory.UserDaoFactoryBean" 
        factory-method="getUserDao">
      <constructor-arg name="pwd" value="123456"/>
  </bean>
  ```

  - class:工厂类的类全名
  - factory-mehod:具体工厂类中创建对象的方法名
  - `<constructor-arg>`标签不仅仅是为构造方法传递参数，只要是为了实例化对象而传递的参数都可以通过其完成

- 通过IOC容器获取对象：

  ```java
  ApplicationContext applicationContext =
      new ClassPathxmlApplicationContext("applicationContext.xml");
  Object userDao = applicationContext.getBean("userDao");
  ```

##### 实例工厂实例化

实例工厂方法，也就是非静态工厂方法产生Bean实例。

与静态工厂方式比较，该方式需要先有工厂对象，再用工厂对象去调用非静态方法，所以在进行配置时，要先配置工厂Bean，在配置目标Bean

- 创建实例工厂

  ```java
  // 工厂类
  public class UserDaoFactoryBean2 {
      // 非静态工厂方法
      public UserDao getUserDao(String name){
          // 可以在此编写一些其他逻辑代码
          return new UserDaoImpl();
      }
  }
  ```

- 配置Bean

  ```xml
  <!-- 配置实例工厂Bean -->
  <bean id="userDaoFactoryBean2" class="com.chongming.factory.UserDaoFactoryBean2"/>
  <!-- 配置实例工厂Bean的哪个方法作为工厂方法 -->
  <bean id="userDao" factory-bean="userDaoFactoryBean2" factory-method="getUserDao">
      <constructor-arg name="pwd" value="123456"/>
  </bean>
  ```

- 从IOC容器中获取Bean

  ```java
  ApplicationContext applicationContext =
      new ClassPathxmlApplicationContext("applicationContext.xml");
  Object userDao = applicationContext.getBean("userDao");
  ```

##### FactoryBean延迟实例化

Spring提供了FactoryBean的接口规范， FactoryBean接口定义如下：

```java
// 重写后，在方法中进行对象的创建并返回
T getObject() throws Exception;
// 重写后，主要返回被创建类的class对象
Class<?> getObjectType();
// 默认单例模式
default boolean isSingleton() { return true; }
```

Spring通过FactoryBean简化实例工厂的配置方式

- 创建一个UserDaoFactoryBean的类，实现FactoryBean接口，重写接口的方法

  ```java
  public class UserDaoFactoryBean implements FactoryBean<UserDao> {
      // 代替原始实例工厂中创建对象的方法
      public UserDao getObject() throws Exception {
          return new UserDaoImpl();
      }
      // 返回所创建类的Class对象
      public Class<?> getObjectType() {
          return UserDao.class;
      }
  }
  ```

- 在Spring的配置文件中进行配置，注意class配置的是BeanFactory

  ```xml
  <bean id="userDao" class="com.chongming.factory.UserDaoFactoryBean"/>
  ```

- 从IOC容器中正常获取Bean

  ```java
  ApplicationContext applicationContext =
      new ClassPathxmlApplicationContext("applicationContext.xml");
  Object userDao = applicationContext.getBean("userDao");
  ```

这种方式在Spring去整合其他框架的时候会被用到。

> 通过断点观察发现Spring容器创建时，FactoryBean被实例化了，并存储到了单例池singletonObjects中。
>
> 但是 getObject() 方法尚未被执行，UserDaoImpl也没被实例化。当首次用到UserDaoImpl时，才调用getObject()。
>
> 此工厂方式产生的Bean实例不会存储到单例池singletonObjects中，会存储到factoryBeanObjectCache缓存池中，并且后期每次使用到userDao都从该缓存池中返回同一个userDao实例

### 依赖注入

Bean的依赖注入有两种方式：

|        注入方式        |                           配置方式                           |
| :--------------------: | :----------------------------------------------------------: |
| 通过Bean的set方法注入  | `<property name="userDao" ref="userDao"`/><br/>`<property name="userDao" value="haohao"/>` |
| 通过构造Bean的方法注入 | `<constructor-arg name="name" ref="userDao"/>`<br/>`<constructor-arg name="name" value="haohao"/>` |

其中，ref用于引用其他Bean的id。value用于注入普通属性值

**依赖注入的数据类型有如下三种：**

- **普通数据类型：**String、int、boolean等，通过value属性指定

- **引用数据类型：**UserDaoImpl、DataSource等，通过ref属性指定

  自动装配方式：

  如果被注入的属性类型是Bean引用的话，那么可以在标签中使用 autowire属性去配置自动注入方式，属性值有两个：

  - `byName`：通过属性名自动装配，即去匹配 setXxx 与 id="xxx"（name="xxx"）是否一致
  - `byType`：通过Bean的类型从容器中匹配，匹配出多个相同Bean类型时，报错

  ```xml
  <bean id="userService" class="com.service.impl.UserServiceImpl" autowire="byType">
  ```

- **集合数据类型：**List、Map、Properties等

  - **注入List集合**

    ```java
    public void setObjList(List<UserDao> objList){
        objList.forEach(obj->{
            System.out.println(obj);
        });
    }
    ```

    ```xml
    <property name="objList">
        <list>
            <bean class="com.dao.UserDaoImpl"/>
            <bean class="com.dao.UserDaoImpl"/>
            <bean class="com.dao.UserDaoImpl"/>
        </list>
    </property>
    ```

    也可以直接引用容器中存在的bean

    ```xml
    <!-- 容器中已存在的bean  -->
    <bean id="userDao0" class="com.dao.UserDaoImpl"/>
    <bean id="userDao1" class="com.dao.UserDaoImpl"/>
    <bean id="userDao2" class="com.dao.UserDaoImpl"/>
    
    <!--配置UserService-->
    <bean id="userService" class="com.service.UserServiceImpl">
        <property name="objList">
            <list>
                <ref bean="userDao0"></ref>
                <ref bean="userDao1"></ref>
                <ref bean="userDao2"></ref>
            </list>
        </property>
    </bean>
    ```

  - **注入Set集合**

    ```java
    // 注入泛型为字符串的Set集合
    public void setValueSet(Set<String> valueSet){
        valueSet.forEach(str->{
            System.out.println(str);
        });
    }
    // 注入泛型为对象的Set集合
    public void setObjSet(Set<UserDao> objSet){
        objSet.forEach(obj->{
            System.out.println(obj);
        });
    }
    ```

    ```xml
    <!-- 注入泛型为字符串的Set集合 -->
    <property name="valueSet">
        <set>
            <value>muzi</value>
            <value>muran</value>
        </set>
    </property>
    <!-- 注入泛型为对象的Set集合 -->
    <property name="objSet">
        <set>
            <ref bean="userDao"></ref>
            <ref bean="userDao2"></ref>
            <ref bean="userDao3"></ref>
        </set>
    </property>
    ```

  - 注入Map集合

    ```java
    // 注入值为字符串的Map集合
    public void setValueMap(Map<String,String> valueMap){
        valueMap.forEach((k,v)->{
            System.out.println(k+"=="+v);
        });
    }
    // 注入值为对象的Map集合
    public void setObjMap(Map<String,UserDao> objMap){
        objMap.forEach((k,v)->{
            System.out.println(k+"=="+v);
        });
    }
    ```

    ```xml
    <!--注入值为字符串的Map集合-->
    <property name="valueMap">
        <map>
            <entry key="aaa" value="AAA" />
            <entry key="bbb" value="BBB" />
            <entry key="ccc" value="CCC" />
        </map>
    </property>
    <!--注入值为对象的Map集合-->
    <property name="objMap">
        <map>
            <entry key="ud" value-ref="userDao"/>
            <entry key="ud2" value-ref="userDao2"/>
            <entry key="ud3" value-ref="userDao3"/>
        </map>
    </property>
    ```

  - **注入Properties键值对**

    ```java
    //注入Properties
    public void setProperties(Properties properties){
        properties.forEach((k,v)->{
            System.out.println(k+"=="+v);
        });
    }
    ```

    ```xml
    <property name="properties">
        <props>
            <prop key="xxx">XXX</prop>
            <prop key="yyy">YYY</prop>
        </props>
    </property>
    ```


### 其他配置标签

Spring的xml标签大体上分类两类，一种是默认标签，一种是自定义标签

- 默认标签：就是不用额外导入其他命名空间约束的标签，例如  标签
- 自定义标签：就是需要额外引入其他命名空间约束，并通过前缀引用的标签，例如`<context:property-placeholder/>`

Spring的默认标签用到的是Spring的默认命名空间

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans 
                           http://www.springframework.org/schema/beans/spring-beans.xsd">
</beans>
```

**该命名空间约束下的默认标签如下：**

|    标签    |                        作用                         |
| :--------: | :-------------------------------------------------: |
| `<beans>`  | 一般作为 xml 配置根标签，其他标签都是该标签的子标签 |
|  `<bean>`  |                   Bean的配置标签                    |
| `<import>` |                  外部资源导入标签                   |
| `<alias>`  |            指定Bean的别名标签，使用较少             |

#### beans

`<beans>`标签，除了经常用的做为根标签外，还可以嵌套在根标签内，使用profile属性切换开发环境。

```xml
<!-- 配置测试环境下，需要加载的Bean实例 -->
<beans profile="test"></beans>
<!-- 配置开发环境下，需要加载的Bean实例 -->
<beans profile="dev"></beans>
```

**激活方式：**

- 使用命令行动态参数，虚拟机参数位置加载`-Dspring.profiles.active=test`
- 使用代码的方式设置环境变量`System.setProperty("spring.profiles.active","test")`

#### import

`<import>`标签，用于导入其他配置文件。

项目变大后，就会导致一个配置文件内容过多，可以将一个配置文件根据业务某块进行拆分。拆分后，最终通过`<import>`标签导入到一个主配置文件中，项目加载主配置文件就连同导入的文件一并加载了

```xml
<!--导入用户模块配置文件-->
<import resource="classpath:UserModuleApplicationContext.xml"/>
<!--导入商品模块配置文件-->
<import resource="classpath:ProductModuleApplicationContext.xml"/>
```

#### 自定义标签

Spring的自定义标签需要引入外部的命名空间，并为外部的命名空间指定前缀，使用 `<前缀:标签>` 形式的标签，称 之为自定义标签，自定义标签的解析流程也是 Spring.xml扩展点方式之一

```xml
<!--默认标签-->
<bean id="userDao" class="com.dao.impl.UserDaoImpl"/>
<!--自定义标签-->
<context:property-placeholder/>
<mvc:annotation-driven/>
<dubbo:application name="application"/>
```

### Spring的get方法

|                方法定义                 |                         返回值和参数                         |
| :-------------------------------------: | :----------------------------------------------------------: |
|    Object getBean (String beanName)     | 根据beanName从容器中获取Bean实例，要求容器中Bean唯一，返回值为Object，需要强转 |
|         T getBean (Class type)          | 根据Class类型从容器中获取Bean实例，要求容器中Bean类型唯一，返回值为Class类型实例， 无需强转 |
| T getBean (String beanName，Class type) | 根据beanName从容器中获得Bean实例，返回值为Class类型实例，无需强转 |

```java
// 根据beanName获取容器中的Bean实例，需要手动强转
UserService userService = (UserService) applicationContext.getBean("userService");
// 根据Bean类型去容器中匹配对应的Bean实例，如存在多个匹配Bean则报错
UserService userService2 = applicationContext.getBean(UserService.class);
// 根据beanName获取容器中的Bean实例，指定Bean的Type类型
UserService userService3 = applicationContext.getBean("userService", 
                                                      UserService.class);
```

### 配置非自定义bean

Spring管理第三方jar包中的bean：

- **配置Druid数据源：**

  ```xml
  <!-- 配置Maven坐标 -->
  <!-- mysql驱动 -->
  <dependency>
      <groupId>mysql</groupId>
      <artifactId>mysql-connector-java</artifactId>
      <version>5.1.49</version>
  </dependency>
  <!-- druid数据源 -->
  <dependency>
      <groupId>com.alibaba</groupId>
      <artifactId>druid</artifactId>
      <version>1.1.23</version>
  </dependency>
  ```

  ```xml
  <!--配置 DruidDataSource数据源-->
  <bean class="com.alibaba.druid.pool.DruidDataSource">
      <!--配置必要属性-->
      <property name="driverClassName" value="com.mysql.jdbc.Driver"/>
      <property name="url" value="jdbc://localhost:3306/mybatis"/>
      <property name="username" value="root"/>
      <property name="password" value="root"/>
  </bean>
  ```

- **配置Connection**

  Connection的产生是通过DriverManager的静态方法getConnection获取的，所以要用静态工厂方式配置

  ```xml
  <bean class="java.lang.Class" factory-method="forName">
      <constructor-arg name="className" value="com.mysql.jdbc.Driver"/>
  </bean>
  
  <bean id="connection" class="java.sql.DriverManager" 
        factory-method="getConnection" 
        scope="prototype">
      <constructor-arg name="url" value="jdbc:mysql://mybatis"/>
      <constructor-arg name="user" value="root"/>
      <constructor-arg name="password" value="root"/>
  </bean>
  ```

- **配置日期对象**

  产生一个指定日期格式的对象的原始代码：

  ```java
  String currentTimeStr = "2023-08-27 07:20:00";
  SimpleDateFormat simpleDateFormat = 
      new SimpleDateFormat("yyyy-MM-dd HH:mm:ss");
  Date date = simpleDateFormat.parse(currentTimeStr);
  ```

  可以看成是实例工厂方式，使用Spring配置方式产生Date实例：

  ```xml
  <bean id="simpleDateFormat" class="java.text.SimpleDateFormat">
      <constructor-arg name="pattern" value="yyyy-MM-dd HH:mm:ss"/>
  </bean>
  
  <bean id="date" factory-bean="simpleDateFormat" factory-method="parse">
      <constructor-arg name="source" value="2023-08-27 07:20:00"/>
  </bean>
  ```

- **配置MyBatis的SqlSessionFactory**

  ```xml
  <!--mybatis框架-->
  <dependency>
      <groupId>org.mybatis</groupId>
      <artifactId>mybatis</artifactId>
      <version>3.5.5</version>
  </dependency>
  <!-- mysql驱动 -->
  <dependency>
      <groupId>mysql</groupId>
      <artifactId>mysql-connector-java</artifactId>
      <version>5.1.49</version>
  </dependency>
  ```

  原始配置方式：

  ```java
  // 加载mybatis核心配置文件，使用Spring静态工厂方式
  InputStream in = Resources.getResourceAsStream(“mybatis-conifg.xml”);
  // 创建SqlSessionFactoryBuilder对象，使用Spring无参构造方式
  SqlSessionFactoryBuilder builder = new SqlSessionFactoryBuilder();
  // 调用SqlSessionFactoryBuilder的build方法，使用Spring实例工厂方式
  SqlSessionFactory sqlSessionFactory = builder.build(in);
  ```

  spring配置方式：

  ```xml
  <!--静态工厂方式产生Bean实例-->
  <bean id="inputStream" class="org.apache.ibatis.io.Resources" factory-method="getResourceAsStream">
      <constructor-arg name="resource" value="mybatis-config.xml"/>
  </bean>
  <!--无参构造方式产生Bean实例-->
  <bean id="sqlSessionFactoryBuilder" class="org.apache.ibatis.session.SqlSessionFactoryBuilder"/>
  <!--实例工厂方式产生Bean实例-->
  <bean id="sqlSessionFactory" factory-bean="sqlSessionFactoryBuilder" factory-method="build">
      <constructor-arg name="inputStream" ref="inputStream"/>
  </bean>
  ```


## Bean实例化基本流程

> ![image-20230626140015553](https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-master202306261400627.png)

- 加载xml配置文件，解析获取配置中的每个`<bean>`的信息，封装成一个个的BeanDefinition对象

  ![image-20230626135231195](https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-master202306261352332.png)

- 将BeanDefinition存储在一个名为beanDefinitionMap的Map中;

  该Map由DefaultListableBeanFactory对象内部维护

  ```java
  public class DefaultListableBeanFactory extends ... implements ... {
      // 存储<bean>标签对应的BeanDefinition对象
      // key是Bean的beanName，value是Bean定义对象BeanDefinition
      private final Map<String, BeanDefinition> beanDefinitionMap = new ConcurrentHashMap<>(256);
  }
  ```

- ApplicationContext底层遍历beanDefinitionMap，基于反射构造方法，或工厂方法创建Bean实例对象;

  > 所以只要将BeanDefinition注册到beanDefinitionMap这个Map中，Spring就会进行对应的Bean的实例化操作

- 创建好的Bean对象，存储到Map集合singletonObjects中，该集合由DefaultSingletonBeanRegistry内部维护，此类为DefaultListableBeanFactory的上四级父类

  ```java
  public class DefaultSingletonBeanRegistry extends ... implements ... {
      //存储Bean实例的单例池
      //key是Bean的beanName，value是Bean的实例对象
      private final Map<String, Object> singletonObjects = new ConcurrentHashMap(256);
  }
  
  ```

- 当执行applicationContext.getBean(beanName)时，从singletonObjects去匹配Bean实例返回。

## Spring后处理器

Spring的后处理器是Spring对外开发的重要扩展点，允许使用者介入到Bean的整个实例化流程中来，以达到动态注册 BeanDefinition，动态修改BeanDefinition，以及动态修改Bean的作用。

**Spring主要有两种后处理器：**

- **BeanFactoryPostProcessor**

  Bean工厂后处理器，在BeanDefinitionMap填充完毕，Bean实例化之前执行；

- **BeanPostProcessor**

  Bean后处理器，一般在Bean实例化之后，填充到单例池singletonObjects之前执行

### 工厂后处理器

**Bean工厂后处理器 – BeanFactoryPostProcessor** 

BeanFactoryPostProcessor是一个接口规范，实现了该接口的类只要交由Spring容器管理的话，那么Spring就会回调该接口的方法，用于对BeanDefinition注册和修改的功能。

BeanFactoryPostProcessor 定义如下：

```java
public interface BeanFactoryPostProcessor {
    void postProcessBeanFactory(ConfigurableListableBeanFactory beanFactory);
}
```

**使用：**

- 编写BeanFactoyostProcessor

  ```java
  public class MyBeanFactoryPostProcessor implements BeanFactoryPostProcessor {
      @Override
      public void postProcessBeanFactory(ConfigurableListableBeanFactory beanFactory) 
          throws BeansException{
          System.out.println("beanDefinitionMap填充完毕后回调该方法");
      }
  }
  ```

- 配置BeanFactoryPostProcessor

  ```xml
  <bean class="com.chong.processor.MyBeanFactoryPostProcessor"/>
  ```

配置完成后，该重写方法中的代码会在bean实例化之前执行

**修改**

postProcessBeanFactory 参数本质就是 DefaultListableBeanFactory。

拿到BeanFactory的引用，自然就可以对beanDefinitionMap中的BeanDefinition进行操作了，例如对UserDaoImpl的BeanDefinition进行修改操作

```java
public class MyBeanFactoryPostProcessor implements BeanFactoryPostProcessor {
    public void postProcessBeanFactory(ConfigurableListableBeanFactory beanFactory) throws BeansException {
        //获得UserDao定义对象
        BeanDefinition userDaoBD = beanFactory.getBeanDefinition("userDao");
        //修改class
        userDaoBD.setBeanClassName("com.dao.impl.UserDaoImpl2"); 
        //userDaoBD.setInitMethodName(methodName); //修改初始化方法
        //userDaoBD.setLazyInit(true); //修改是否懒加载
        //.. 省略其他的设置方式 ...
    }
}
```

**注册**

注册自己的BeanDefintion到BeanFactory的BeanDefintionMap中，就可以在之后的流程中实例化这个bean

```java
public class MyBeanFactoryPostProcessor implements BeanFactoryPostProcessor {
    @Override
    public void postProcessBeanFactory(ConfigurableListableBeanFactory configurableListableBeanFactory) throws BeansException {
        //强转成子类DefaultListableBeanFactory
        if(configurableListableBeanFactory instanceof DefaultListableBeanFactory) {
            DefaultListableBeanFactory beanFactory = (DefaultListableBeanFactory) configurableListableBeanFactory;
            // 创建自己的BeanDefintion
            BeanDefinition beanDefinition = new RootBeanDefinition();
            beanDefinition.setBeanClassName("com.dao.UserDaoImpl2");
            // 注册自己的BeanDefintion到beanFactory中
            beanFactory.registerBeanDefinition("userDao2",beanDefinition);
        }
    }
}
```

Spring提供了一个BeanFactoryPostProcessor的子接口：BeanDefinitionRegistryPostProcessor，专门用于注册BeanDefinition操作

```java
public class MyBeanFactoryPostProcessor2 implements BeanDefinitionRegistryPostProcessor {
    @Override
    public void postProcessBeanFactory(ConfigurableListableBeanFactory 
                                       configurableListableBeanFactory) throws BeansException {}
    @Override
    public void postProcessBeanDefinitionRegistry(BeanDefinitionRegistry beanDefinitionRegistry) 
        throws BeansException {
        BeanDefinition beanDefinition = new RootBeanDefinition();
        beanDefinition.setBeanClassName("com.dao.UserDaoImpl2");
        beanDefinitionRegistry.registerBeanDefinition("userDao2",beanDefinition);
    }
}
```

### Bean后处理器

**Bean后处理器 – BeanPostProcessor** 

从Bean被实例化后，到最终缓存到名为singletonObjects单例池之前，中间会经过Bean的初始化过程，例如：属性的填充、初始方法init的执行等。

其中有一个对外进行扩展的点BeanPostProcessor，称为Bean后处理。跟Bean工厂后处理器相似，它也是一个接口，实现了该接口并被容器管理的BeanPostProcessor，会在流程节点上被Spring自动调用。

**BeanPostProcessor的接口定义如下：**

```java
public interface BeanPostProcessor {
    @Nullable
    //在属性注入完毕，init初始化方法执行之前被回调
    default Object postProcessBeforeInitialization(Object bean, String beanName) throws 
        BeansException {
        return bean;
    }
    @Nullable
    //在初始化方法执行之后，被添加到单例池singletonObjects之前被回调
    default Object postProcessAfterInitialization(Object bean, String beanName) throws 
        BeansException {
        return bean;
    }
}
```

**自定义MyBeanPostProcessor**

```java
public class MyBeanPostProcessor implements BeanPostProcessor {
    /** 
     * 参数：bean是当前被实例化的Bean，beanName是当前Bean实例在容器中的名称
	 * 返回值：当前Bean实例对象 
	 **/
    public Object postProcessBeforeInitialization(Object bean, String beanName) throws BeansException {
        System.out.println("BeanPostProcessor的before方法...");
        return bean;
    }
    /** 
     * 参数：bean是当前被实例化的Bean，beanName是当前Bean实例在容器中的名称
	 * 返回值：当前Bean实例对象 
	 **/
    public Object postProcessAfterInitialization(Object bean, String beanName) throws BeansException {
        System.out.println("BeanPostProcessor的after方法...");
        return bean;
    }
}
```

```xml
<bean class="com.processors.MyBeanPostProcessor"></bean>
```

## Bean生命周期

Spring Bean的生命周期，是从Bean实例化之后，即通过反射创建出对象之后，到Bean成为一个完整对象，最终存储到单例池中。

这个过程被称为Spring Bean的生命周期。Spring Bean的生命周期大体上分为三个阶段：

- **Bean的实例化阶段**

  Spring框架会取出BeanDefinition的信息进行判断当前Bean的范围是否是singleton的，是否不是延迟加载的，是否不是FactoryBean等，最终将一个普通的singleton的Bean通过反射进行实例化；

- **Bean的初始化阶段**

  Bean创建之后还仅仅是个"半成品"，还需要对Bean实例的属性进行填充、执行一些Aware接口方法、执行BeanPostProcessor方法、执行InitializingBean接口的初始化方法、执行自定义初始化init方法等。该阶段是Spring最具技术含量和复杂度的阶段，Aop增强功能、Spring的注解功能、Bean的循环引用问题都是在这个阶段体现的；

- **Bean的完成阶段**

  经过初始化阶段，Bean就成为了一个完整的Spring Bean，被存储到单例池singletonObjects中去了，即完成了Spring Bean的整个生命周期



**Bean的初始化阶段最复杂，也是重点研究对象：**

- Bean实例的属性填充
- Aware接口属性注入
- BeanPostProcessor的before()方法回调
- InitializingBean接口的初始化方法回调
- 自定义初始化方法init回调
- BeanPostProcessor的after()方法回调

### Bean实例属性填充

Spring在进行属性注入时，会分为如下几种情况：

- 注入普通属性，String、int或存储基本类型的集合时，直接通过set方法的反射设置进去； 

- 注入单向对象引用属性时，从容器中getBean获取后通过set方法反射设置进去；如果容器中没有，则先创建被注入对象Bean实例（完成整个生命周期）后，在进行注入操作；

-  注入双向对象引用属性时，比较复杂，涉及了循环引用（循环依赖）问题

**循环依赖问题：**

多个实体之间相互依赖并形成闭环

![image-20230626144150327](https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-master202306261441401.png)

Spring提供了 **三级缓存** 存储 **完整Bean实例** 和 **半成品Bean实例**，用以解决循环依赖问题

在DefaultListableBeanFactory的上四级父类DefaultSingletonBeanRegistry中提供如下三个Map：

```java
public class DefaultSingletonBeanRegistry ... {
    //1、最终存储单例Bean成品的容器，即实例化和初始化都完成的Bean，称之为"一级缓存"
    Map<String, Object> singletonObjects = new ConcurrentHashMap(256);
    //2、早期Bean单例池，缓存半成品对象，且当前对象已经被其他对象引用了，称之为"二级缓存"
    Map<String, Object> earlySingletonObjects = new ConcurrentHashMap(16);
    //3、单例Bean的工厂池，缓存半成品对象，对象未被引用，使用时再通过工厂创建Bean，称之为"三级缓存"
    Map<String, ObjectFactory<?>> singletonFactories = new HashMap(16);
}
```

**假设UserService和UserDao循环依赖，三级缓存解决过程：**

- UserService 实例化对象，但尚未初始化，将UserService存储到三级缓存；
- UserService 属性注入，需要UserDao，从缓存中获取，没有UserDao；
- UserDao实例化对象，但尚未初始化，将UserDao存储到到三级缓存；
- UserDao属性注入，需要UserService，从三级缓存获取UserService，UserService从三级缓存移入二级缓存；
- UserDao执行其他生命周期过程，最终成为一个完成Bean，存储到一级缓存，删除二三级缓存；
- UserService 注入UserDao；
- UserService执行其他生命周期过程，最终成为一个完成Bean，存储到一级缓存，删除二三级缓存。

### 常用Aware接口

Aware接口是一种框架辅助属性注入的一种思想，其他框架中也可以看到类似的接口。

框架具备高度封装性，使用者接触到的一般都是业务代码，一个底层功能API不能轻易的获取到，但是这不意味着永远用不到这些对象，

如果用到了，就可以使用框架提供的类似Aware的接口，让框架注入该对象。

|        Aware接口        |                           回调方法                           |                            作用                            |
| :---------------------: | :----------------------------------------------------------: | :--------------------------------------------------------: |
|   ServletContextAware   |          setServletContext(ServletContext context)           | Spring框架回调方法注入ServletContext对象， web环境下才生效 |
|    BeanFactoryAware     |             setBeanFactory(BeanFactory factory)              |           Spring框架回调方法注入beanFactory对象            |
|      BeanNameAware      |                 setBeanName(String beanName)                 |     Spring框架回调方法注入当前Bean在容器中 的beanName      |
| ApplicationContextAware | setApplicationContext(ApplicationContext applicationContext) |       Spring框架回调方法注入applicationContext 对象        |

## IOC整体流程总结

![image-20230626145758841](https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-master202306261457940.png)

## XML整合第三方框架

xml整合第三方框架有两种整合方案：

- 不需要自定义命名空间，不需要使用Spring的配置文件配置第三方框架本身内容。例如：MyBatis
- 需要引入第三方框架命名空间，需要使用Spring的配置文件配置第三方框架本身内容。例如：Dubbo

### 整合Myabtis

MyBatis提供了mybatis-spring.jar专门用于两大框架的整合。

- 导入Mybatis整合Spring相关坐标

  ```xml
  <!--mysql驱动-->
  <dependency>
      <groupId>mysql</groupId>
      <artifactId>mysql-connector-java</artifactId>
      <version>5.1.49</version>
  </dependency>
  <!--mybatis框架-->
  <dependency>
      <groupId>org.mybatis</groupId>
      <artifactId>mybatis</artifactId>
      <version>3.5.5</version>
  </dependency>
  <!--mybatis整合spring-->
  <dependency>
      <groupId>org.mybatis</groupId>
      <artifactId>mybatis-spring</artifactId>
      <version>2.0.5</version>
  </dependency>
  <!--数据源druid-->
  <dependency>
      <groupId>com.alibaba</groupId>
      <artifactId>druid</artifactId>
      <version>1.1.23</version>
  </dependency>
  ```

- 编写Mapper和Mapper.xml

  ```java
  public interface UserMapper {
      List<User> findAll();
  }
  ```

  ```xml
  <?xml version="1.0" encoding="UTF-8" ?>
  <!DOCTYPE mapper
  PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
  "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
  <mapper namespace="com.itheima.dao.UserMapper">
      <select id="findAll" resultType="com.pojo.User">
          select * from tb_user
      </select>
  </mapper>
  ```

- 配置SqlSessionFactoryBean和MapperScannerConfigurer

  ```xml
  <!--配置数据源-->
  <bean id="dataSource" class="com.alibaba.druid.pool.DruidDataSource">
      <property name="url" value="jdbc:mysql://localhost:3306/mybatis"></property>
      <property name="username" value="root"></property>
      <property name="password" value="root"></property>
  </bean>
  <!--配置SqlSessionFactoryBean-->
  <bean class="org.mybatis.spring.SqlSessionFactoryBean">
      <property name="dataSource" ref="dataSource"></property>
  </bean>
  <!--配置Mapper包扫描-->
  <bean class="org.mybatis.spring.mapper.MapperScannerConfigurer">
      <property name="basePackage" value="com.itheima.dao"></property>
  </bean>
  ```

- 编写测试代码

  ```java
  ClassPathxmlApplicationContext applicationContext =
      new ClassPathxmlApplicationContext("applicationContext.xml");
  UserMapper userMapper = applicationContext.getBean(UserMapper.class);
  List<User> all = userMapper.findAll();
  System.out.println(all);
  ```



**原理：**

整合包里提供了一个SqlSessionFactoryBean和一个扫描Mapper的配置对象，SqlSessionFactoryBean一旦被实例化，就开始扫描Mapper并通过动态代理产生Mapper的实现类存储到Spring容器中。

相关的有如下四个类：

- `SqlSessionFactoryBean`：需要进行配置，用于提供SqlSessionFactory；

  配置SqlSessionFactoryBean作用是向容器中提供SqlSessionFactory，SqlSessionFactoryBean实现了FactoryBean和InitializingBean两个接口，所以会自动执行getObject() 和afterPropertiesSet()方法

  ```java
  SqlSessionFactoryBean implements FactoryBean<SqlSessionFactory>, InitializingBean{
      public void afterPropertiesSet() throws Exception {
          //创建SqlSessionFactory对象
          this.sqlSessionFactory = this.buildSqlSessionFactory();
      }
      public SqlSessionFactory getObject() throws Exception {
          return this.sqlSessionFactory;
      }
  }
  ```

- `MapperScannerConfigurer`：需要进行配置，用于扫描指定mapper注册BeanDefinition；

  配置MapperScannerConfigurer作用是扫描Mapper，向容器中注册Mapper对应的MapperFactoryBean。

  MapperScannerConfigurer实现了BeanDefinitionRegistryPostProcessor和InitializingBean两个接口，会在postProcessBeanDefinitionRegistry方法中向容器中注册MapperFactoryBean

  ```java
  class MapperScannerConfigurer implements BeanDefinitionRegistryPostProcessor, InitializingBean {
      public void postProcessBeanDefinitionRegistry(BeanDefinitionRegistry registry) {
          ClassPathMapperScanner scanner = new ClassPathMapperScanner(registry);
          scanner.scan(StringUtils.tokenizeToStringArray(this.basePackage, ",; \t\n"));
      }
  }
  ```

- `MapperFactoryBean`：Mapper的FactoryBean，获得指定Mapper时调用getObject方法；

  ```java
  public class MapperFactoryBean<T> extends SqlSessionDaoSupport implements FactoryBean<T> {
      public MapperFactoryBean(Class<T> mapperInterface) {
          this.mapperInterface = mapperInterface;
      }
      public void setSqlSessionFactory(SqlSessionFactory sqlSessionFactory) {
          this.sqlSessionTemplate = this.createSqlSessionTemplate(sqlSessionFactory);
      }
      public T getObject() throws Exception {
          return this.getSqlSession().getMapper(this.mapperInterface);
      }
  }
  ```

- `ClassPathMapperScanner`：

  definition.setAutowireMode(2) 修改了自动注入状态，所以MapperFactoryBean中的setSqlSessionFactory会自动注入进去。

  ```java
  class ClassPathMapperScanner extends ClassPathBeanDefinitionScanner {
      public Set<BeanDefinitionHolder> doScan(String... basePackages) {
          Set<BeanDefinitionHolder> beanDefinitions = super.doScan(basePackages);
          if (beanDefinitions.isEmpty()) {
          } else {
              this.processBeanDefinitions(beanDefinitions);
          }
      }
      private void processBeanDefinitions(Set<BeanDefinitionHolder> beanDefinitions) {
          // 设置Mapper的beanClass是org.mybatis.spring.mapper.MapperFactoryBean
          definition.setBeanClass(this.mapperFactoryBeanClass);
          definition.setAutowireMode(2); // 设置MapperBeanFactory 进行自动注入
      }
  }
  
  class ClassPathBeanDefinitionScanner{
      public int scan(String... basePackages) {
          this.doScan(basePackages);
      }
      protected Set<BeanDefinitionHolder> doScan(String... basePackages) {
          // 将扫描到的类注册到beanDefinitionMap中，此时beanClass是当前类全限定名
          this.registerBeanDefinition(definitionHolder, this.registry);
          return beanDefinitions;
      }
  }
  ```

