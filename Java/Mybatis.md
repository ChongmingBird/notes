# 【Mybatis】

## 简介

> MyBatis 是一款优秀的持久层框架，用于简化 JDBC 开发
>
> MyBatis本是Apache的一个开源项目iBatis, 2010年这个项目由apache software foundation 迁移到了google code，并且改名为MyBatis 。2013年11月迁移到Github
>
> 官网：https://mybatis.org/mybatis-3/zh/index.html 
>
> IDEA插件推荐：MybatisX

**持久层：**

- 负责将数据到保存到数据库的那一层代码。

  开发时会将操作数据库的Java代码作为持久层，而Mybatis就是对JDBC代码进行了封装。

- JavaEE三层架构：表现层、业务层、持久层

**框架：**

* 框架就是一个半成品软件，是一套可重用的、通用的、软件基础代码模型
* 在框架的基础之上构建软件编写更加高效、规范、通用、可扩展



**JDBC缺点：**

* 硬编码

  * 注册驱动、获取连接

    JDBC连接数据库需要四个基本信息，也就是代码中要有4个字符串，以后如果要将MySQL数据库换成其他的关系型数据库的话，这四个地方都需要修改，如果放在此处就意味着要修改源代码。

  * SQL语句

    如果表结构发生变化，SQL语句就要进行更改。这也不方便后期的维护。

* 操作繁琐

  * 手动设置参数
  * 手动封装结果集


**Mybatis优化：**

* 硬编码可以配置到配置文件
* 操作繁琐的地方mybatis都自动完成

## 快速入门

- **创建user表，添加数据：**

```sql
create database mybatis;
use mybatis;

drop table if exists tb_user;

create table tb_user(
	id int primary key auto_increment,
	username varchar(20),
	password varchar(20),
	gender char(1),
	addr varchar(30)
);

INSERT INTO tb_user VALUES (1, 'zhangsan', '123', '男', '北京');
INSERT INTO tb_user VALUES (2, '李四', '234', '女', '天津');
INSERT INTO tb_user VALUES (3, '王五', '11', '男', '西安');
```

- **创建模块，引入依赖：**

```xml
<dependencies>
    <!--mybatis 依赖-->
    <dependency>
        <groupId>org.mybatis</groupId>
        <artifactId>mybatis</artifactId>
        <version>3.5.5</version>
    </dependency>
    <!--mysql 驱动-->
    <dependency>
        <groupId>mysql</groupId>
        <artifactId>mysql-connector-java</artifactId>
        <version>8.0.25</version>
    </dependency>
    <!--junit 单元测试-->
    <dependency>
        <groupId>junit</groupId>
        <artifactId>junit</artifactId>
        <version>4.13</version>
        <scope>test</scope>
    </dependency>
    <!-- 添加slf4j日志api -->
    <dependency>
        <groupId>org.slf4j</groupId>
        <artifactId>slf4j-api</artifactId>
        <version>1.7.20</version>
    </dependency>
    <!-- 添加logback-classic依赖 -->
    <dependency>
        <groupId>ch.qos.logback</groupId>
        <artifactId>logback-classic</artifactId>
        <version>1.2.3</version>
    </dependency>
    <!-- 添加logback-core依赖 -->
    <dependency>
        <groupId>ch.qos.logback</groupId>
        <artifactId>logback-core</artifactId>
        <version>1.2.3</version>
    </dependency>
</dependencies>
```

注意：需要在项目的resources目录下创建logback的配置文件

- **编写 MyBatis 核心配置文件 --> 替换连接信息 解决硬编码问题**

  在模块下的resources目录下创建mybatis的配置文件 `mybatis-config.xml`，内容如下：

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>
    <environments default="development">
        <environment id="development">
            <transactionManager type="JDBC"/>
            <dataSource type="POOLED">
                <property name="driver" value="com.mysql.cj.jdbc.Driver"/>
                <!-- xml中&要用转义字符&amp;表示 -->
                <property name="url" value="jdbc:mysql://localhost:3306/mybatis?serverTimezone=UTC&amp;useSSL=false"/>
                <property name="username" value="root"/>
                <property name="password" value="123456"/>
            </dataSource>
        </environment>
    </environments>
    <!-- 加载SQL映射文件 -->
    <mappers>
        <mapper resource="org/mybatis/example/BlogMapper.xml"/>
    </mappers>
</configuration>
```

- **编写 SQL 映射文件 --> 统一管理sql语句，解决硬编码问题**

  在模块的 `resources` 目录下创建映射配置文件 `UserMapper.xml`，内容如下：

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN" "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<!-- namespace:命名空间，设置成Mapper接口的路径-->
<mapper namespace="test">
    <!--
        id:SQL语句唯一标识
        resultType:结果集的数据类型
    -->
    <select id="selectAll" resultType="com.meng.pojo.User">
        <!-- 编写SQL语句 -->
        SELECT * FROM tb_user
    </select>
</mapper>
```

- **编码：**

  - 在 `com.meng.pojo` 包下创建 User类：

  ```java
  public class User {
      private Integer id;
      private String username;
      private String password;
      private String gender;
      private String addr;
      
      //省略了 setter 和 getter
  }
  ```

  - 在 `com.meng` 包下编写 MybatisDemo 测试类

  ```java
  // 1.加载mybatis核心配置文件，获取SqlSessionFactory
  String resource = "mybatis-config.xml";
  InputStream inputStream = Resources.getResourceAsStream(resource);
  SqlSessionFactory sqlSessionFactory = new SqlSessionFactoryBuilder().build(inputStream);
      
  // 2.获取对应的SqlSession对象，用它来执行sql
  SqlSession session = sqlSessionFactory.openSession();
      
  // 3.执行sql，获得结果集，参数为Mapper的 [名称空间.sql唯一标识]
  List<User> users = session.selectList("test.selectAll");
      
  Iterator<User> iterator = users.iterator();
  while (iterator.hasNext()) {
      System.out.println(iterator.next());
  }
      
  // 4.释放资源
  session.close();
  ```

## 代理开发

### 简介

> Mapper代理方式的目的：
>
> * 解决原生方式中的硬编码
> * 简化后期执行SQL
>
> Mapper又名映射文件
>
> Mybatis 官网也是推荐使用 Mapper 代理的方式

![image-20220427203601609](https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-master/image-20220427203601609.png)

### 步骤

使用Mapper代理方式，必须满足以下要求：

* 定义与SQL映射文件同名的Mapper接口，并且将Mapper接口和SQL映射文件放置在同一目录下。如下图：

![image-20220427192932456](https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-master/image-20220427192932456.png)

- 设置SQL映射文件的`namespace`属性为Mapper接口全限定名：

```xml
<mapper namespace="com.meng.mapper.UserMapper">
```

- 在 Mapper 接口中定义方法，方法名就是SQL映射文件中sql语句的id，并保持参数类型和返回值类型一致：

```java
public interface UserMapper {
    List<User> selectAll();
}
```

- 调用sql：

```java
// 省略获得session的步骤

// 获取UserMapper接口的代理对象
UserMapper userMapper = session.getMapper(UserMapper.class);
                                          
// 4.执行sql
List<User> users = userMapper.selectAll();
Iterator<User> iterator = users.iterator();
while (iterator.hasNext()) {
    System.out.println(iterator.next());
}
// 4.释放资源
session.close();
```

> **注意：**
>
> 如果Mapper接口名称和SQL映射文件名称相同，并在同一目录下，则可以使用 **包扫描** 的方式简化SQL映射文件的加载。也就是将核心配置文件的加载映射配置文件的配置修改为
>
> ```xml
> <mappers>
>     <!--加载sql映射文件-->
>     <!--使用包扫描，会扫描包下的所有mapper接口和对应的xml-->
>     <package name="com.meng.mapper"/>
> </mappers>
> ```

## 增删改查

> 标签名：
>
> - 增: `<insert>`
> - 删: `<delete>`
> - 改: `<update>`
> - 查: `<select>`
>
> `parameterType`属性：指定传参的数据类型，sql语句中通过数据类型匹配，如果要按照参数名匹配，可以在mapper接口参数前添加注解`@Param("参数名")`
>
> `resultType`属性：指定结果集的数据类型
>
> 如果参数是实体，使用`#{实体属性名}`方式引用实体中的属性值；如果只传一个参数，就写`#{参数名}`
>
> 如果使用sqlsession开发，mybatis事务默认不提交，需要通过`sqlSession.commit()`手动提交

### Insert

- 映射文件：

```xml
<!-- 插入操作 parameterMap:参数类型-->
<insert id="insert" parameterType="com.meng.pojo.User">
    <!-- 这里的参数名要和实体类的成员变量名一一对应 -->
    INSERT INTO tb_user VALUES(#{id}, #{username}, #{password}, #{gender}, #{addr})
</insert>
```

- Mapper接口：

```java
int insert(User user);
```

- 测试：


```java
User user = new User(5, "123", "456", "男", "浙江");
int i = userMapper.insert(user);
System.out.println(i);
```

### Delete

- 映射文件：

```xml
<!-- 删除操作 -->
<!-- Mybatis自动帮java.lang.Integer配置好了别名 -->
<delete id="delete" parameterType="Integer">
    DELETE FROM tb_user WHERE id=#{id}
</delete>
```

- Mapper接口：

```java
int delete(Integer id);
```

- 测试：


```java
int i = userMapper.delete(6);
System.out.println(i);
```

### Update

- 映射文件：

```xml
<update id="update" parameterType="com.meng.pojo.User">
    UPDATE tb_user SET username=#{id}, password=#{password}, gender=#{gender}, addr=#{addr} WHERE id=#{id}
</update>
```

- Mapper接口：

```java
int update(User user);
```

- 测试：

```java
User user = new User(1,"lisi","778899","女","云南");
int i = userMapper.update(user);
System.out.println(i);
```

### Select

- 映射文件：

```xml
<!-- 查询操作 resultType:结果集类型-->
<select id="select" resultType="com.meng.pojo.User">
    SELECT * FROM tb_user;
</select>
```

- Mapper接口：

```java
List<User> select();
```

- 测试：

```java
List<User> users = userMapper.select();
System.out.println(users);
```

## 配置文件

> MyBatis 的配置文件包含了会深深影响 MyBatis 行为的设置和属性信息。 配置文档的顶层结构如下：
>
> - configuration（配置）
>   - properties（属性)
>   - settings（设置）
>   - typeAliases（类型别名）
>   - typeHandlers（类型处理器）
>   - objectFactory（对象工厂）
>   - plugins（插件）
>   - environments（环境配置）
>     - environment（环境变量）
>       - transactionManager（事务管理器）
>       - dataSource（数据源）
>   - databaseIdProvider（数据库厂商标识）
>   - mappers（映射器）
>
> **注意：** 配置文件中标签循序也要按照层次结构书写

### 属性

> **标签名：** `<properties>`
>
> 实际开发中，习惯将数据源的配置信息单独抽取出来

![image-20220428170020807](https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-master/image-20220428170020807.png)

- **加载外部properties：**

```xml
<!-- 通过properties标签加载外部properties文件 -->
<properties resource="jdbc.properties"></properties>
<environments default="development">
    <environment id="development">
        <transactionManager type="JDBC"/>
        <dataSource type="POOLED">
            <!-- ${属性名}进行动态配置 -->
            <property name="driver" value="${jdbc.driver}"/>
            <property name="url" value="${jdbc.url}"/>
            <property name="username" value="${jdbc.username}"/>
            <property name="password" value="${jdbc.password}"/>
        </dataSource>
    </environment>
</environments>
```

​	外部properties文件:

```properties
jdbc.driver=com.mysql.cj.jdbc.Driver
jdbc.url=jdbc:mysql://localhost:3306/mybatis?serverTimezone=UTC&useSSL=false
jdbc.username=root
jdbc.password=123456
```

- **内部抽取出信息:**

```xml
<!-- 通过properties标签抽取信息到其子标签 -->
<properties>
    <property name="driver" value="com.mysql.cj.jdbc.Driver"/>
    <property name="url" value="jdbc:mysql://localhost:3306/mybatis?serverTimezone=UTC&amp;useSSL=false"/>
    <property name="username" value="root"/>
    <property name="password" value="123456"/>
</properties>

<environments default="development">
    <environment id="development">
        <transactionManager type="JDBC"/>
        <dataSource type="POOLED">
            <!-- ${属性名}进行动态配置 -->
            <property name="driver" value="${driver}"/>
            <property name="url" value="${url}"/>
            <property name="username" value="${username}"/>
            <property name="password" value="${password}"/>
        </dataSource>
    </environment>
</environments>
```



### 类型别名

> 标签名：`<typeAliases>`
>
> 在映射文件中的参数类型和结果集类型属性需要配置数据封装的类型（类的全限定名）。而每次这样写是特别麻烦的，Mybatis 提供了`类型别名`(typeAliases)可以简化这部分的书写。
>
> `resultType="别名"`
>
> `parameterType="别名"`

在核心配置文件中配置类型别名，也就意味着给`pojo`包下所有的类起了别名（别名就是类名），不区分大小写，内容如下：

```xml
<typeAliases>
    <!--name属性的值是实体类所在包-->
    <package name="com.meng.pojo"/> 
</typeAliases>

<!-- 写在<environments>标签上方 -->
```

这样，就可以简化配置文件中`resultType`属性值的编写：

```xml
<mapper namespace="com.ment.mapper.UserMapper">
    <!-- resultType不用写包名，不区分大小写 -->
    <select id="selectAll" resultType="user">
        select * from tb_user;
    </select>
</mapper>
```

为常见的Java类型内建的类型别名，它们都是不区分大小写的，注意，为了应对原始类型的命名重复，采取了特殊的命名风格(截取部分)：

|    别名     | 映射的类型 |
| :---------: | :--------: |
|   string    |   String   |
|    long     |    Long    |
| int/integer |  Integer   |
|   double    |   Double   |
|    float    |   Float    |
|   boolean   |  Boolean   |
|    date     |    Date    |
|   object    |   Object   |
|     map     |    Map     |
|   hashmap   |  HashMap   |
|    list     |    List    |
|  arraylist  | ArrayList  |
| collection  | Collection |
|  iterator   |  Iterator  |

### 插件

> **标签：** `<plugins>`
>
> Mybatis可以使用第三方插件来对功能进行扩展

[分页插件案例](###分页插件)

### 类型处理器

> **标签：** `<typeHandlers>`
>
> 无论Mybatis在预处理语句(PreparedStatement)中设置一个参数时，还是从结果集中取出一个值时，都会用类型处理器将获取的值以合适的方法转换成Java类型
> 
> **截取部分类型处理器：**
> 
> |      类型处理器      |            Java类型            |                           JDBC类型                           |
> | :------------------: | :----------------------------: | :----------------------------------------------------------: |
> | `BooleanTypeHandler` | `java.lang.Boolean`, `boolean` |                    数据库兼容的 `BOOLEAN`                    |
> |  `ByteTypeHandler`   |    `java.lang.Byte`, `byte`    |               数据库兼容的 `NUMERIC` 或 `BYTE`               |
> |  `ShortTypeHandler`  |   `java.lang.Short`, `short`   |             数据库兼容的 `NUMERIC` 或 `SMALLINT`             |
> | `IntegerTypeHandler` |   `java.lang.Integer`, `int`   | 数据库兼容的 `NUMERIC` 或 `INTEGER`
可以通过重写类型处理器或创建自己的类型处理器来处理不支持的或非标准的类型。 |
>实现方法为：
> 
>- 实现一个TypeHandler接口
> - 继承一个BaseTypeHandler
> 
>然后选择性地将它映射到一个JDBC类型

**自定义类型转换器：**

> 例如：一个Java中的Date数据类型，想将之存到数据库的时候存成一个1970年至今的毫秒数，取出来成Java的Date，即Java的Date与数据库的varchar毫秒值之间的转换

- 定义转换类继承类`BaseTypeHandler<T>`:
- 覆盖4个未实现的方法：

```java
public class DateTypeHandler extends BaseTypeHandler<Date> {

    /**
     * 将Java类型转换成数据库类型
     * @param ps
     * @param i 表中需要转换数据的列的位置
     * @param date Java类型
     * @param jdbcType
     * @throws SQLException
     */
    @Override
    public void setNonNullParameter(PreparedStatement ps, int i, Date date, JdbcType jdbcType) throws SQLException {
        long time = date.getTime();
        ps.setLong(i, time);
    }

    /**
     * 将数据库中类型转换成Java类型
     * @param rs 结果集
     * @param s 结果集中需要转换数据的列的列名
     * @return
     * @throws SQLException
     */
    @Override
    public Date getNullableResult(ResultSet rs, String s) throws SQLException {
        // 将结果集中的数据(long)转换成Java的类型(Date)，返回
        long along = rs.getLong(s);
        Date date = new Date(along);
        return date;
    }
    // 将数据库中类型转换成Java类型
    @Override
    public Date getNullableResult(ResultSet rs, int i) throws SQLException {
        long along = rs.getLong(i);
        Date date = new Date(along);
        return date;
    }
    // 将数据库中类型转换成Java类型
    @Override
    public Date getNullableResult(CallableStatement cs, int i) throws SQLException {
        long aLong = cs.getLong(i);
        Date date = new Date(aLong);
        return date;
    }
}
```

- 在MyBatis核心配置文件中进行注册：

```xml
<!-- 注册类型处理器 -->
<typeHandlers>
    <typeHandler handler="com.meng.handler.DateTypeHandler"></typeHandler>
</typeHandlers>
```

### 环境配置

> **标签：** `<environments>`
>
> MyBatis 可以配置成适应多种环境，这种机制有助于将 SQL 映射应用于多种数据库之中。
>
> **不过尽管可以配置多个环境，但每个 SqlSessionFactory 实例只能选择一种环境。**

![image-20220428164816251](https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-master/image-20220428164816251.png)

`<environments>`标签下，可以配置多个`<environment>`(数据库)，通过`<environments default=?>`属性来切换，指向对应的`<environment id=?>`

```xml
<!-- environment：配置数据库链接环境信息 -->
<environments default="dl1">
    <environment id="dl1"> ... </environment>
    <environment id="dl2"> ... </environment>
</environments>
```

### 映射器

> **标签名：** `<mappers>`
>
> 映射器配置会告诉 MyBatis 去哪里找映射文件

加载方式有如下几种：

- 使用相对于类路径的资源引用

```xml
<!-- 使用相对于类路径的资源引用 -->
<mappers>
  <mapper resource="org/mybatis/builder/AuthorMapper.xml"/> 
</mappers>
```

- 使用完全限定资源定位符(URL)

```xml
<!-- 使用完全限定资源定位符（URL） -->
<mappers>
  <mapper url="file:///var/mappers/AuthorMapper.xml"/>
</mappers>
```

- 使用映射器接口实现类的完全限定名

```xml
<!-- 使用映射器接口实现类的完全限定类名 -->
<mappers>
  <mapper class="org.mybatis.builder.AuthorMapper"/>
</mappers>
```

- 将包内的映射器接口实现全部注册为映射器(包扫描)

​	   要求Mapper接口名称和SQL映射文件名称相同，并在同一目录下

```xml
<!-- 将包内的映射器接口实现全部注册为映射器 -->
<mappers>
  <package name="org.mybatis.builder"/>
</mappers>
```

## 动态SQL

> [mybatis – MyBatis 3 | 动态 SQL](https://mybatis.org/mybatis-3/zh/dynamic-sql.html)

**动态SQL：** sql语句会随着用户的输入或外部条件的变化而变化

Mybatis对动态SQL有很强大的支撑，它提供了很多的标签：

- `<if>`
- `<choose(when, otherwise)>`
- `<trim(where,set)>`
- `<foreach>`

### if 标签

> `<if>`
>
> 单条件动态查询

根据实体类的不同取值，使用不同的SQL语句来进行查询。比如在`id`如果不为0时根据id查询，如果`username`不同时为空还要加入用户名作为条件：

```xml
<!-- 查询操作 resultType:结果集类型-->
<select id="select" parameterType="com.meng.pojo.User" resultType="com.meng.pojo.User">
    <!-- 编写SQL语句 -->
    SELECT * FROM tb_user 
    <where>
        <if test="id != 0">
            AND id = #{id}
        </if>
        <if test="username != null">
            AND username = #{username}
        </if>
    </where>
</select>
```

> 存在的问题：第一个条件不需要逻辑运算符（`AND`）
>
> 解决方法:
>
> - 使用恒等式`WHERE 1=1`让所有条件格式都一样
> - 使用`<where>`标签替换`WHERE`关键字

### choose 标签

> `<choose(when, otherwise)>`
>
> 多条件动态查询
>
> 类似Java里的`switch`语句，`when`类似`case`，`otherwise`类似`default`

```xml
<!-- 查询操作-->
<select id="select" resultType="user">
    SELECT * FROM tb_user WHERE
    <choose>
        <when test="username == null">
            id=2
        </when>
        <when test="username != null and username != '' ">
            id=3
        </when>
        <otherwise>
            1=1
        </otherwise>
    </choose>
</select>
```

### trim 标签

> 存在的问题：`if`标签子元素的sql语句不统一，容易出现sql语句的错误拼接，比如第一个条件不需要逻辑运算符（`AND`）

- `<where>` ：自动解决`WHERE`下的逻辑运算符问题

```xml
<select id="select" parameterType="user" resultType="user">
    SELECT * FROM tb_user WHERE
    <!-- where元素只会在子元素返回任何内容的情况下才插入 “WHERE” 子句。而且，若子句的开头为 “AND” 或 “OR”，where 元素也会将它们去除 -->
    <where>
        <if test="id != 0">
            AND id = #{id}
        </if>
        <if test="username != null">
            AND username = #{username}
        </if>
    </where>
</select>
```

- `<set>` ：动态更新语句(`UPDATE`)，可以动态包含需要更新的列，忽略其它不更新的列

```xml
<update id="update">
    UPDATE tb_user
    <!-- 这个例子中，set元素会动态地在行首插入SET关键字，并会删掉额外的逗号 -->
    <set>
        <if test="username != null">username=#{username},</if>
        <if test="password != null">password=#{password},</if>
    </set>
    WHERE id=#{id}
</update>
```

- `<trim>` ： 定制功能

```xml
<select id="select" parameterType="user" resultType="user">
    SELECT * FROM tb_user WHERE
    <!--
 		prefixOverrides：自动忽略的文本序列
		prefix：自动插入的文本序列 
		案例实现了和<where>标签等同的效果
		注意AND和OP后面的空格是必须的，不然就出现ADNid这种情况
 	-->
    <trim prefix="WHERE" prefixOverrides="AND |OR ">
        <if test="id != 0">
            id = #{id}
        </if>
        <if test="username != null">
            username = #{username}
        </if>
    </trim>
</select>

<update id="update">
    UPDATE tb_user
    <!-- 实现类似<set>标签的功能 -->
    <trim prefix="SET" suffixOverrides=",">
        <if test="username != null">username=#{username},</if>
        <if test="password != null">password=#{password},</if>
    </trim>
    WHERE id=#{id}
</update>
```

### foreach 标签

循环执行sql的拼接操作，例如`SELECT * FROM tb_user WHERE id IN(1,2,5)`

```xml
<!-- 查询操作 传入的参数是一个List集合-->
<select id="select" resultType="User">
    <!-- SELECT * FROM tb_user WHERE id IN(?, ?, ?) -->
    SELECT * FROM tb_user
    <where>
        <!--
			index:指定一个名字，用于表示在迭代过程中，每次迭代到 
            open:开始的语句
            close:结束的语句
            open和close拼接成了id IN()
            item:中间的元素
            separator: 分隔符
            这里在sql语句的id IN()里面循环拼接了值
        -->
        <if test="list !=null">
            <foreach collection="list" open="id IN(" close=")" item="id" separator=",">
                #{id}
            </foreach>
        </if>
    </where>
</select>
```

测试：

```java
 // 查询
 List<Integer> ids = new ArrayList<>();
 ids.add(1);
 ids.add(2);
 ids.add(3);
 List<User> users = userMapper.select(ids);
 System.out.println(users);
```

### sql片段抽取

> 抽取sql片段，可以在不同的地方复用

```xml
<!-- sql片段抽取 -->
<sql id="selectUser">
    SELECT * FROM tb_user
</sql>
<!-- 查询操作-->
<select id="select" resultType="User">
    <!-- include标签调用sql片段 -->
    <include refid="selectUser"></include>
</select>
```

## 分页插件

> 分页助手PageHelper是将分页的复杂操作进行封装，使用简单的方式即可获得分页的相关数据。
>
> [MyBatis 分页插件 PageHelper](https://pagehelper.github.io/)

**步骤：**

 - 导入通用PageHelper依赖

```xml
<dependency>
    <groupId>com.github.pagehelper</groupId>
    <artifactId>pagehelper</artifactId>
    <version>5.3.0</version>
</dependency>

<!-- springboot分页插件,springboot用这个不用再配置 -->
<!-- pagehelper分页插件依赖 -->
<dependency>
            <groupId>com.github.pagehelper</groupId>
            <artifactId>pagehelper-spring-boot-starter</artifactId>
            <version>1.4.6</version>
</dependency>
```

 - 在mybaits核心配置文件中配置分页插件

```xml
<!-- 分页插件 -->
<plugins>
    <!-- com.github.pagehelper为PageHelper类所在包名 -->
    <plugin interceptor="com.github.pagehelper.PageInterceptor"></plugin>
</plugins>
```

​		Spring配置文件中配置分页插件

```xml
<bean id="sqlSessionFactory" class="org.mybatis.spring.SqlSessionFactoryBean">
  <!-- 注意其他配置 -->
  <property name="plugins">
    <array>
      <bean class="com.github.pagehelper.PageInterceptor">
        <property name="properties">
          <value></value>
        </property>
      </bean>
    </array>
  </property>
</bean>
```

 - 测试分页数据获取

```java
// 设置分页相关参数：当前页和每页显示条数
PageHelper.startPage(1, 2);
// 获得结果集
List<User> users = userMapper.select();

Iterator<User> iterator = users.iterator();
while (iterator.hasNext()) {
    System.out.println(iterator.next());
}

// 获得与分页相关的参数
PageInfo<User> pageInfo = new PageInfo<>(users);
System.out.println("当前页:" + pageInfo.getPageNum() + '\n' +
        ", 每页显示条数:" + pageInfo.getPageSize() + '\n' +
        ", 总条数:" + pageInfo.getTotal() + '\n' +
        ", 总页数:" + pageInfo.getPages() + '\n' +
        ", 上一页:" + pageInfo.getPrePage() + '\n' +
        ", 下一页:" + pageInfo.getNextPage() + '\n' +
        ", 是否是第一页:" + pageInfo.isIsFirstPage() + '\n' +
        ", 是否是最后一页:" + pageInfo.isIsLastPage()
);
```

## 代码生成器

```xml
<dependency>
    <groupId>org.mybatis.generator</groupId>
    <artifactId>mybatis-generator-core</artifactId>
    <version>1.4.0</version>
</dependency>
```

```xml
<plugin>
    <groupId>org.mybatis.generator</groupId>
    <artifactId>mybatis-generator-maven-plugin</artifactId>
    <version>1.3.2</version>
    <configuration>
          <!--配置文件的位置-->
            <configurationFile>src/main/resources/mybatis-generator.xml</configurationFile>
            <verbose>true</verbose>
            <overwrite>true</overwrite>
    </configuration>
    <executions>
            <execution>
              <id>Generate MyBatis Artifacts</id>
              <goals>
                <goal>generate</goal>
              </goals>
            </execution>
    </executions>
</plugin>
```

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE generatorConfiguration
        PUBLIC "-//mybatis.org//DTD MyBatis Generator Configuration 1.0//EN"
        "http://mybatis.org/dtd/mybatis-generator-config_1_0.dtd">

<generatorConfiguration>
    <!--    windows下路径, D:\downloads\xxx.jar-->
    <classPathEntry location="C:\Users\lenovo\Desktop\Java\jar\mysql-connector-java-8.0.28.jar" />

    <context id="DB2Tables" targetRuntime="MyBatis3">

        <!--覆盖生成XML文件，用不了-->
        <!--<plugin type="org.mybatis.generator.plugins.UnmergeableXmlMappersPlugin" />-->

        <commentGenerator>
            <property name="suppressAllComments" value="true"/>
        </commentGenerator>

        <jdbcConnection driverClass="com.mysql.cj.jdbc.Driver"
                        connectionURL="jdbc:mysql://127.0.0.1:3306/mall?characterEncoding=utf-8"
                        userId="root"
                        password="123456">
        </jdbcConnection>

        <javaTypeResolver >
            <property name="forceBigDecimals" value="false" />
        </javaTypeResolver>

        <javaModelGenerator targetPackage="com.chongming.mall.pojo" targetProject="src/main/java">
            <property name="enableSubPackages" value="true" />
            <!--            <property name="trimStrings" value="true" />-->
        </javaModelGenerator>

        <sqlMapGenerator targetPackage="mappers"  targetProject="src/main/resources">
            <property name="enableSubPackages" value="true" />
        </sqlMapGenerator>

        <javaClientGenerator type="XMLMAPPER" targetPackage="com.chongming.mall.dao"  targetProject="src/main/java">
            <property name="enableSubPackages" value="true" />
        </javaClientGenerator>

                <table tableName="mall_order" domainObjectName="Order" enableCountByExample="false" enableDeleteByExample="false" enableSelectByExample="false" enableUpdateByExample="false"/>
                <table tableName="mall_order_item" domainObjectName="OrderItem" enableCountByExample="false" enableDeleteByExample="false" enableSelectByExample="false" enableUpdateByExample="false"/>
                <table tableName="mall_user" domainObjectName="User" enableCountByExample="false" enableDeleteByExample="false" enableSelectByExample="false" enableUpdateByExample="false"/>
                <table tableName="mall_category" domainObjectName="Category" enableCountByExample="false" enableDeleteByExample="false" enableSelectByExample="false" enableUpdateByExample="false"/>
                <table tableName="mall_product" domainObjectName="Product" enableCountByExample="false" enableDeleteByExample="false" enableSelectByExample="false" enableUpdateByExample="false">
                    <columnOverride column="detail" jdbcType="VARCHAR" />
                    <columnOverride column="sub_images" jdbcType="VARCHAR" />
                </table>
        <table tableName="mall_shipping" domainObjectName="Shipping" enableCountByExample="false" enableDeleteByExample="false" enableSelectByExample="false" enableUpdateByExample="false"/>

    </context>
</generatorConfiguration>
```



## 模板

### 配置文件

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>
    <!-- 加载外部properties文件 -->
    <properties resource="jdbc.properties"></properties>
	<!-- 类型别名 -->
    <typeAliases>
        <package name="com.meng.pojo"/>
    </typeAliases>
    <!-- 分页插件 -->
    <plugins>
        <!-- com.github.pagehelper为PageHelper类所在包名 -->
        <plugin interceptor="com.github.pagehelper.PageInterceptor"></plugin>
    </plugins>

    <environments default="development">
        <environment id="development">
            <transactionManager type="JDBC"/>
            <dataSource type="POOLED">
                <!-- ${属性名}进行动态配置 -->
                <property name="driver" value="${jdbc.driver}"/>
                <property name="url" value="${jdbc.url}"/>
                <property name="username" value="${jdbc.username}"/>
                <property name="password" value="${jdbc.password}"/>
            </dataSource>
        </environment>
    </environments>
    <!-- 加载SQL映射文件，包扫描 -->
    <mappers>
        <package name="com.meng.mapper"/>
    </mappers>
</configuration>
```

### 数据库配置

```properties
jdbc.driver=com.mysql.cj.jdbc.Driver
jdbc.url=jdbc:mysql://localhost:3306/mybatis?serverTimezone=UTC&useSSL=false&allowPublicKeyRetrieval=true
jdbc.username=root
jdbc.password=123456
```

### 映射文件

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN" "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<!-- namespace:命名空间 写Mapper的权限名 -->
<mapper namespace=""> </mapper>
```

