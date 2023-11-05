# 【Mybatis-plus】

**自用笔记，记于2021年5月，mybatis-plus更新较快，可能落伍，仅供参考**

## 简介

> **MyBatis-Plus** 是一个 **MyBatis** 的增强工具，在 MyBatis 的基础上只做增强不做改变，为简化开发、提高效率而生。

**特性：**

- **无侵入**：只做增强不做改变，引入它不会对现有工程产生影响，如丝般顺滑
- **损耗小**：启动即会自动注入基本 CURD，性能基本无损耗，直接面向对象操作
- **强大的 CRUD 操作**：内置通用 Mapper、通用 Service，仅仅通过少量配置即可实现单表大部分 CRUD 操作，更有强大的条件构造器，满足各类使用需求。
- **支持 Lambda 形式调用**：通过 Lambda 表达式，方便的编写各类查询条件，无需再担心字段写错
- **支持主键自动生成**：支持多达 4 种主键策略（内含分布式唯一 ID 生成器 - Sequence），可自由配置，完美解决主键问题
- **支持 ActiveRecord 模式**：支持 ActiveRecord 形式调用，实体类只需继承 Model 类即可进行强大的 CRUD 操作
- **支持自定义全局通用操作**：支持全局通用方法注入（ Write once, use anywhere ）
- **内置代码生成器**：采用代码或者 Maven 插件可快速生成 Mapper 、 Model 、 Service 、 Controller 层代码，支持模板引擎，更有超多自定义配置等您来使用
- **内置分页插件**：基于 MyBatis 物理分页，开发者无需关心具体操作，配置好插件之后，写分页等同于普通 List 查询
- **分页插件支持多种数据库**：支持 MySQL、MariaDB、Oracle、DB2、H2、HSQL、SQLite、Postgre、SQLServer 等多种数据库
- **内置性能分析插件**：可输出 Sql 语句以及其执行时间，建议开发测试时启用该功能，能快速揪出慢查询
- **内置全局拦截插件**：提供全表 delete 、 update 操作智能分析阻断，也可自定义拦截规则，预防误操作

**框架结构：**

![framework](https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-master/mybatis-plus-framework.jpg)

## 快速入门

> 地址：[快速开始 | MyBatis-Plus (baomidou.com)](https://baomidou.com/guide/quick-start.html#初始化工程)
>
> 使用第三方组件：
>
> 1. 导入对应的依赖
> 2. 研究依赖如何配置
> 3. 代码如何编写
> 4. 提高拓展技术能力！
>

### 构建数据库

现有一张 `User` 表，其表结构如下：

| id   | name   | age  | email              |
| ---- | ------ | ---- | ------------------ |
| 1    | Jone   | 18   | test1@baomidou.com |
| 2    | Jack   | 20   | test2@baomidou.com |
| 3    | Tom    | 28   | test3@baomidou.com |
| 4    | Sandy  | 21   | test4@baomidou.com |
| 5    | Billie | 24   | test5@baomidou.com |

其对应的数据库 Schema 脚本如下：

```sql
DROP TABLE IF EXISTS user;

CREATE TABLE user
(
	id BIGINT(20) NOT NULL COMMENT '主键ID',
	name VARCHAR(30) NULL DEFAULT NULL COMMENT '姓名',
	age INT(11) NULL DEFAULT NULL COMMENT '年龄',
	email VARCHAR(50) NULL DEFAULT NULL COMMENT '邮箱',
	PRIMARY KEY (id)
);
-- 真实开发中，还需要的字段（阿里巴巴开发手册）：
--version（乐观锁）、deleted（逻辑删除）、gmt_create（创建时间）、gmt_modified（更新时间）
```

其对应的数据库 Data 脚本如下：

```sql
DELETE FROM user;

INSERT INTO user (id, name, age, email) VALUES
(1, 'Jone', 18, 'test1@baomidou.com'),
(2, 'Jack', 20, 'test2@baomidou.com'),
(3, 'Tom', 28, 'test3@baomidou.com'),
(4, 'Sandy', 21, 'test4@baomidou.com'),
(5, 'Billie', 24, 'test5@baomidou.com');
```

### 初始化工程

- 创建一个空的SpringBoot

### 添加依赖

引入 Spring Boot Starter 父工程：

```xml
<parent>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-parent</artifactId>
    <version>2.5.0</version>
    <relativePath/>
</parent>
```

引入 `spring-boot-starter`、`spring-boot-starter-test`、`mybatis-plus-boot-starter`、`h2` 依赖：

```xml
<dependencies>
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter</artifactId>
    </dependency>
    
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-test</artifactId>
        <scope>test</scope>
    </dependency>
    
    <dependency>
    <groupId>org.projectlombok</groupId>
    <artifactId>lombok</artifactId>
    <version>1.18.20</version>
    <scope>provided</scope>
</dependency>
    
    <!--mybatis—plus是国人开发的mybatis增强版，不是官方的-->
    <dependency>
    <groupId>com.baomidou</groupId>
    <artifactId>mybatis-plus-boot-starter</artifactId>
    <version>3.4.3</version>
    <classifier>sources</classifier>
    <type>java-source</type>
</dependency>
    
    <dependency>
        <groupId>com.h2database</groupId>
        <artifactId>h2</artifactId>
        <scope>runtime</scope>
    </dependency>
    
     <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
            <scope>runtime</scope>
     </dependency>
</dependencies>
```

说明：使用mybatis-plus 可以节省大量的代码，尽量不要同时导入mybatis和mybatis-plus

### 配置

连接数据库

properties版：

```properties
spring.datasource.username=root
spring.datasource.password=2428028346
spring.datasource.url=jdbc:mysql://localhost:3306/数据库名?useUnicode=true&characterEncoding=UTF-8&serverTimezone=Asia/Shanghai&useSSL=true
spring.datasource.driver-class-name=com.mysql.cj.jdbc.Driver
```

yaml版：

```yaml
spring:
  datasource:
    driver-class-name: com.mysql.cj.jdbc.Driver
    username: root
    password: 2428028346
    url: jdbc:mysql://localhost:3306/数据库名?useUnicode=true&characterEncoding=UTF-8&serverTimezone=Asia/Shanghai&useSSL=true
```

在 Spring Boot 启动类中添加 `@MapperScan` 注解，扫描 Mapper 文件夹：

```java
@SpringBootApplication
@MapperScan("com.baomidou.mybatisplus.samples.quickstart.mapper")
public class Application {
    public static void main(String[] args) {
        SpringApplication.run(QuickStartApplication.class, args);
    }
}
```

- mybatis流程：创建pojo-dao（连接mybatis、配置mapper.xml文件）-service-controller
- mybatis-plus流程：创建pojo-实现mapper接口-使用

### 编码

- **pojo包User类**

```java
//用lombok插件简化了set/get方法
@Data
@AllArgsConstructor
@NoArgsConstructor
public class User {
    private Long id;
    private String name;
    private Integer age;
    private String email;
}
```

- **Mapper包UserMapper接口**

```java
@Mapper
public interface UserMapper extends BaseMapper<User> {
    /**
     * 所有的CRUD操作已经编写完成了
     */
}

```

- **测试类**

```java
@SpringBootTest
class MybatisApplicationTests {
    /**
     * 继承了BaseMapper，所有的方法都来自父类，我们也可以编写自己的拓展方法
     */
    @Autowired
    private UserMapper userMapper;

    @Test
    void contextLoads() {
        List<User> users = userMapper.selectList(null);
        users.forEach(System.out::println);
    }
}
```

## 配置日志

所有的sql现在是不可见的，若希望知道它如何执行，需要打开日志。

```yaml
# 数据库连接配置-yaml语法
spring:
  datasource:
    driver-class-name: com.mysql.cj.jdbc.Driver
    username: root
    password: 123456
    url: jdbc:mysql://localhost:3306/数据库名?useUnicode=true&characterEncoding=UTF-8&serverTimezone=Asia/Shanghai&useSSL=true
# 配置日志
mybatis-plus:
  configuration:
    log-impl: org.apache.ibatis.logging.stdout.StdOutImpl
    # 是否开启自动驼峰命名规则映射，即将数据库列名中的下划线命名，转换成属性值的驼峰命名
    map-underscore-to-camel-case: false

```

得到的日志：

![](https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-master/images/20210606060507.jpg)



## CRUD

### INSERT

```java
@Test
//测试插入
public void testInsert(){
    User user = new User();
    user.setName("重明鸟");
    user.setAge(20);
    user.setEmail("2428028346@qq.com");
    int result = userMapper.insert(user);
    System.out.println(result); //受影响的行数
    System.out.println(user); 
}
```

![](https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-master/images/20210606061306.jpg)

**没有设置id，却发现id自动生成**

这里数据库插入的id默认值是由雪花算法生成的全局唯一ID，ASSIGN_ID

#### 主键策略

![](https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-master/images/20210608112055.jpg)

`@TableId(type = IdType.XXX)`

```java
public enum IdType {
    AUTO(0), 
    NONE(1), 
    INPUT(2),
    ASSIGN_ID(3),
    ASSIGN_UUID(4);
    ...
}
```



|              值               |                             描述                             |
| :---------------------------: | :----------------------------------------------------------: |
|             AUTO              |                         数据库ID自增                         |
|             NONE              | 无状态,该类型为未设置主键类型(注解里等于跟随全局,全局里约等于 INPUT) |
|             INPUT             |                    insert前自行set主键值                     |
|           ASSIGN_ID           | 分配ID(主键类型为Number(Long和Integer)或String)(since 3.3.0),使用接口`IdentifierGenerator`的方法`nextId`(默认实现类为`DefaultIdentifierGenerator`雪花算法) |
|          ASSIGN_UUID          | 分配UUID,主键类型为String(since 3.3.0),使用接口`IdentifierGenerator`的方法`nextUUID`(默认default方法) |
|   ID_WORKER<br />（已废弃）   |     分布式全局唯一ID 长整型类型(please use `ASSIGN_ID`)      |
|     UUID<br />（已废弃）      |           32位UUID字符串(please use `ASSIGN_UUID`)           |
| ID_WORKER_STR<br />（已废弃） |     分布式全局唯一ID 字符串类型(please use `ASSIGN_ID`)      |

**注意，字段为id时，@TableId的默认值是IdType.ASSIGN_ID，其余时候是IdType.NONE**

### UPDATE

```java
//测试更新
@Test
public void testUpdate() {
	User user = new User();
    //通过条件自动拼接sql
	user.setId(5L);
	user.setName("重明鸟");
    //虽然名字是updateByID,但参数是一个对象
	int i = userMapper.updateById(user);
	System.out.println(i);
}
```

![](https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-master/images/20210608115242.jpg)



### SELECE

#### 单个查询

```java
// 测试查询
@Test
public void testSelectById() {
    User user = userMapper.selectById(1L);
    System.out.println(user.toString());
}
```

#### 批量查询

```java
// 测试批量查询
@Test
public void testSelectById() {
    List<User> users = userMapper.selectBatchIds(Arrays.asList(1, 2, 3));
    users.forEach(System.out::println);
}
```

#### 条件查询

- **map操作**

```java
// 多条件查询
@Test
public void testSelectById() {
    HashMap<String, Object> map = new HashMap<>();
   // 可以放入多个条件
    map.put("name", "重明鸟");
    map.put("age", 3);
    List<User> users = userMapper.selectByMap(map);
    users.forEach(System.out::println);
}
```



#### 分页查询

> 分页在网站使用的十分之多！
>
> 1.  原始的limit进行分页
> 2.  pageHelper第三方插件
> 3.  Mybatis-plus也内置了分页插件
>

**使用方法：**

​	1、配置拦截器组件

```java
/**
 * 新的分页插件,一缓和二缓遵循mybatis的规则,需要设置 MybatisConfiguration#useDeprecatedExecutor = false 避免缓存出现问题(该属性会在旧插件移除后一同移除)
 */
@Bean
public MybatisPlusInterceptor paginationInterceptor() {
    MybatisPlusInterceptor interceptor = new MybatisPlusInterceptor();
    interceptor.addInnerInterceptor(new PaginationInnerInterceptor(DbType.H2));
    return interceptor;
}
```

​	2、直接使用

```java
// 测试分页查询
@Test
public void testPage() {
    // 参数一：当前当前页
    // 参数二：页面大小
    Page<User> page = new Page<>(2,5);
    userMapper.selectPage(page, null);
    page.getRecords().forEach(System.out::println);
}
```

### DELETE

#### 单个删除

```java
// 测试删除
@Test
public void testDeleteById() {
    userMapper.deleteById(7L);
}
```

#### 批量删除

```java
// 测试批量删除
@Test
public void testDeleteById() {
    userMapper.deleteBatchIds(Arrays.asList(1L, 2L, 3L));
}
```

#### 条件删除

```java
// 测试删除
@Test
public void testDeleteById() {
    HashMap<String, Object> hashMap = new HashMap<>();
    hashMap.put("name", "德玛西亚");
    hashMap.put("age", 25);
    userMapper.deleteByMap(hashMap);
}
```

#### 逻辑删除

- 物理删除：从数据库中直接移除
- 逻辑删除：在数据库中没有被移除，而是通过一个变量来让他失效 ！deleted = 0 => deleted = 1
- 可以让管理员能够直接查看被删除的记录！防止数据的丢失。类似于回收站！

> 说明:
>
> 只对自动注入的sql起效:
>
> - 插入: 不作限制
> - 查找: 追加where条件过滤掉已删除数据,且使用 wrapper.entity 生成的where条件会忽略该字段
> - 更新: 追加where条件防止更新到已删除数据,且使用 wrapper.entity 生成的where条件会忽略该字段
> - 删除: 转变为 更新
>
> 例如:
>
> - 删除: `update user set deleted=1 where id = 1 and deleted=0`
> - 查找: `select id,name,deleted from user where deleted=0`
>
> 字段类型支持说明:
>
> - 支持所有数据类型(推荐使用 `Integer`,`Boolean`,`LocalDateTime`)
> - 如果数据库字段使用`datetime`,逻辑未删除值和已删除值支持配置为字符串`null`,另一个值支持配置为函数来获取值如`now()`
>
> 附录:
>
> - 逻辑删除是为了方便数据恢复和保护数据本身价值等等的一种方案，但实际就是删除。
> - 如果你需要频繁查出来看就不应使用逻辑删除，而是以一个状态去表示。

**测试：**

```
1. 在数据表中增加一个deleted字段
2. 修改pojo类——最新版本 需要添加注解即可
```

```java
//逻辑删除注解
@TableLogic
private int deleted;
```

3. 测试

```java
@Test
public void testDelete() {
	userMapper.deleteById(5L);
}
```

![](https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-master/images/20210609154628.jpg)

**实质上是一个update操作**

记录依旧在数据库中，但值已经变了

![](https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-master/images/20210609154929.jpg)

但现在我们已经查找不到这条记录了

## 自动填充

创建时间、修改时间！这些操作一般都是自动化完成的，我们不希望手动更新。

《阿里巴巴开发手册》：gmt_create、gmt_modified，几乎所有的表都需要有，并且要求自动化。

### 方法一：数据库级别

1. 在表中新增字段：gmt_create、gmt_modified，并设置默认值(CURRENT_TIMESTAMP，CURRENT_TIMESTAMP+根据当前时间戳更新)
2. 实体类也要添加属性

**项目中不允许中途修改数据库**

![](https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-master/images/20210608120107.jpg)

### 方法二：代码级别

1. 删除数据库中的默认值、更新操作
2. 实体类字段属性上需要增加注解

```java
// 字段添加填充内容
@TableField(fill = FieldFill.INSERT)
private Date gmtCreate;
@TableField(fill = FieldFill.INSERT_UPDATE)
private Date gmtModified;
```

3. 编写处理器（handler包下）

```java
@Slf4j
// 一定要把处理器添加到IOC容器中！
@Component 
public class MyMetaObjectHandler implements MetaObjectHandler {
    // 插入时的填充策略
    @Override
    public void insertFill(MetaObject metaObject) {
        log.info("start insert fill...");
        this.setFieldValByName("gmtCreate", new Date(), metaObject);
        this.setFieldValByName("gmtModified", new Date(), metaObject);
    }
    // 更新时的填充策略
    @Override
    public void updateFill(MetaObject metaObject) {
        log.info("start update fill...");
        this.setFieldValByName("gmtModified", new Date(), metaObject);
    }
}
```

## 乐观锁

- **悲观锁：**

  当要对数据库中的一条数据进行修改的时候，为了避免同时被其他人修改，最好的办法就是直接对该数据进行加锁以防止并发。

  这种借助数据库锁机制，在修改数据之前先锁定，再修改的方式被称之为悲观并发控制【Pessimistic Concurrency Control，缩写“PCC”，又名“悲观锁”】。

- **乐观锁：**

  乐观锁是相对悲观锁而言的，乐观锁假设数据一般情况不会造成冲突，所以在数据进行提交更新的时候，才会正式对数据的冲突与否进行检测，如果冲突，则返回给用户异常信息，让用户决定如何去做。乐观锁适用于读多写少的场景，这样可以提高程序的吞吐量。

当要更新一条记录的时候，我们希望这条记录没有被别人更新。**乐观锁实现方式：**

- 取出记录时，获取当前version
- 更新时，带上这个version
- 执行更新时， set version = newVersion where version = oldVersion
- 如果version不对，就更新失败

举例：

```sql
-- 乐观锁：
-- 先查询，获得版本号vetsion = 1 
-- A
update user set name = "dongmeng", version = version + 1 
where id = 2 and version = 1

-- B 如果B线程抢先完成，这个时候version = 2，会导致A修改失败
update user set name = "dongmeng", version = version + 1 
where id = 2 and version = 1
```

**乐观锁配置步骤：**

1、给数据表加上乐观锁字段`version`，默认值设置为1（版本号为1）

![](https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-master/images/20210608173212.jpg)

2、同步实体类

```java
//乐观锁Version注解
@Version
private Integer version;
```

3、注册组件

```java
//可以选择在这里扫描我们的 mapper文件夹
@MapperScan("com.dm.mapper")
//开启事务管理，默认开启
@EnableTransactionManagement
//配置类
@Configuration
public class MyBatisPlusConfig {
    //注册乐观锁插件
    @Bean
    public MybatisPlusInterceptor mybatisPlusInterceptor() {
        MybatisPlusInterceptor interceptor = new MybatisPlusInterceptor();
        interceptor.addInnerInterceptor(new OptimisticLockerInnerInterceptor());
        return interceptor;
    }
}
```

4、测试

- 成功案例

```java
@Test
public void testOptimisticLocker1() {
	// 1.查询用户信息
	User user = userMapper.selectById(1L);
	// 2.修改用户信息
	user.setName("艾欧尼亚");
	user.setAge(33);
	user.setEmail("564651612@163.com");
	// 3.执行更新操作
	userMapper.updateById(user);
}
```

![](https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-master/images/20210608175103.jpg)

version自动+1

- 失败案例

```java
// 测试乐观锁成功案例！
@Test
public void testOptimisticLocker2() {
	// 线程 1
	User user1 = userMapper.selectById(1L);
	user1.setName("艾欧尼亚");
	user1.setAge(33);
	user1.setEmail("564651612@163.com");

	// 模拟另外一个线程执行了插队操作
	User user2 = userMapper.selectById(1L);
	user2.setName("巨神峰");
	user2.setAge(26);
	user2.setEmail("9999912@163.com");
	userMapper.updateById(user2);
	//如果没有乐观锁，下面的操作会覆盖插队线程的操作
	userMapper.updateById(user1);
}
```

![](https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-master/images/20210608180155.jpg)

## 条件构造器

**官方文档：**

[条件构造器 | MyBatis-Plus (baomidou.com)](https://baomidou.com/guide/wrapper.html#abstractwrapper)

**测试：**

```java
@Test
void contextLoads() {
    // 查询name不为空，邮箱不为空，且年龄大于等于12岁的用户
    QueryWrapper<User> wrapper = new QueryWrapper<>();
    wrapper
            .isNotNull("name")
            .isNotNull("email")
            .ge("age",12);
    //查询一个结构用selectOne
    userMapper.selectList(wrapper).forEach(System.out::println);
}
```

![](https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-master/images/20210611154836.jpg)

**方法大全（具体详见官方文档）：**

![](https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-master/images/20210611160842.png)

## 代码生成器

> AutoGenerator 是 MyBatis-Plus 的代码生成器，通过 AutoGenerator 可以快速生成 Entity、Mapper、Mapper XML、Service、Controller 等各个模块的代码，极大的提升了开发效率

### 添加依赖

- 添加 代码生成器 依赖

```xml
<dependency>
    <groupId>com.baomidou</groupId>
    <artifactId>mybatis-plus-generator</artifactId>
    <version>3.4.1</version>
</dependency>
```

- 添加 模板引擎 依赖

```xml
<dependency>
    <groupId>org.apache.velocity</groupId>
    <artifactId>velocity-engine-core</artifactId>
    <version>2.3</version>
</dependency>
```

### 编写配置

### 构建代码生成器

```java
package com.dm.demo;

import com.baomidou.mybatisplus.annotation.IdType;
import com.baomidou.mybatisplus.generator.AutoGenerator;
import com.baomidou.mybatisplus.generator.config.GlobalConfig;

/**
 * @ClassName: DongCode
 * @Description: 代码自动生成器
 * @Author: ChongMing
 * @Date: 2021年06月19日 16:15
 * @Version: 1.0
 */
public class DongCode {
    public static void main(String[] args) {
        // 首先需要构建一个代码生成器对象
        AutoGenerator mpg = new AutoGenerator();
        
        // 配置写在中间
      
        //执行操作
        mpg.execute(); 
    }
}
```

#### 全局配置

```java
// 全局配置(generation下的包)
GlobalConfig gc = new GlobalConfig();
// 获取当前项目路径
String projectPath = System.getProperty("user.dir");
// 输出到当前项目目录的位置
gc.setOutputDir(projectPath+"/src/main/java");
// 设置作者信息
gc.setAuthor("ChongMing");
// 是否打开资源管理器
gc.setOpen(false);
// 是否覆盖原有的
gc.setFileOverride(false);
// 设置主键类型
gc.setIdType(IdType.ASSIGN_ID);
// 设置swagger文档
gc.setSwagger2(true);
// 把配置丢入构造器里
mpg.setGlobalConfig(gc);
```

#### 数据源配置

```java
// 2.设置数据源
// 数据源就是用户名连接密码这些
DataSourceConfig dsc = new DataSourceConfig();
dsc.setUrl("jdbc:mysql://localhost:3306/mp?useUnicode=true&characterEncoding=UTF-8&serverTimezone=Asia/Shanghai&useSSL=true");
dsc.setDriverName("com.mysql.cj.jdbc.Driver");
dsc.setUsername("root");
dsc.setPassword(null);
// 数据库类型
dsc.setDbType(DbType.MYSQL);
// 把配置丢入构造器里
mpg.setDataSource(dsc);
```

#### 包配置

```java
// 3.配置包
PackageConfig pc = new PackageConfig();
// 设置软件包
pc.setParent("com.dm");
// 设置模块，代码最终会放到com.dm.blog下
pc.setModuleName("blog");
// 设置各个包名
pc.setEntity("pojo");
pc.setMapper("mapper");
pc.setService("service");
pc.setController("controller");
// 把配置丢入构造器里
mpg.setPackageInfo(pc);
```

#### 策略配置

```java
// 4.策略配置
StrategyConfig sc = new StrategyConfig();
// 设置要映射的表名，也就是要读取的表，可以设置多个
sc.setInclude("user");
// 包命名下划线转驼峰
sc.setNaming(NamingStrategy.underline_to_camel);
// 数据表列名下划线转驼峰
sc.setColumnNaming(NamingStrategy.underline_to_camel);
// 是否自动生成lombok注解
sc.setEntityLombokModel(true);
// 逻辑删除，设置代表逻辑删除的字段名
sc.setLogicDeleteFieldName("deleted");
// 自动填充配置
TableFill gmtCreate = new TableFill("gmt_create", FieldFill.INSERT);
TableFill gmtModified = new TableFill("gmt_modified", FieldFill.UPDATE);
ArrayList<TableFill> tableFills = new ArrayList<>();
tableFills.add(gmtCreate);
tableFills.add(gmtModified);
// 把策略自动填充策略丢入配置中
sc.setTableFillList(tableFills);
// 乐观锁配置
sc.setVersionFieldName("version");
// 把配置丢入构造器里
mpg.setStrategy(sc);
```

### 执行

执行后所有东西都自动生成了

![QQ截图20210619171446](https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-master/img/QQ截图20210619171446.png)

### 模板

```java
 @Test
    void contextLoads() {//需要构建一个代码生成器对象
        AutoGenerator autoGenerator = new AutoGenerator();
    /*
    配置策略
     */
        // 1、全局配置
        GlobalConfig config = new GlobalConfig();
        String projectPath = System.getProperty("user.dir");
        config.setOutputDir(projectPath+"/src/main/java");
        config.setAuthor("ChongMing");
        config.setOpen(false);  //生成完是否打开资源管理器
        config.setFileOverride(false);  //是否覆盖原来生成的
        config.setIdType(IdType.ASSIGN_UUID);  //全局ID的生成方式
        config.setDateType(DateType.ONLY_DATE); //设置日期类型
        config.setSwagger2(true);   //自动配置swagger 文档
        autoGenerator.setGlobalConfig(config);
        //2、设置数据源配置
        DataSourceConfig sourceConfig = new DataSourceConfig();
        sourceConfig.setUrl("jdbc:mysql://localhost:3306/yvnuncle?useUnicode=true&characterEncoding=UTF-8&serverTimezone=Asia/Shanghai&useSSL=false");
                sourceConfig.setUsername("root");
        sourceConfig.setPassword("123456");
        sourceConfig.setDriverName("com.mysql.cj.jdbc.Driver");
        sourceConfig.setDbType(DbType.MYSQL);
        autoGenerator.setDataSource(sourceConfig);
        //3、配置包
        PackageConfig pc = new PackageConfig();
        pc.setModuleName("yvnUncle");
        pc.setParent("com.yvnze.yvnUncle");
        pc.setEntity("entity");
        pc.setMapper("mapper");
        pc.setController("controller");
        pc.setService("service");
        autoGenerator.setPackageInfo(pc);
        //4、策略配置
        StrategyConfig stc = new StrategyConfig();
        stc.setInclude("share");   //设置要映射的表明，这里有多少各表名就能映射多少个表
        stc.setNaming(NamingStrategy.underline_to_camel);  //内置包的命名规则  下划线转驼峰命名
        stc.setColumnNaming(NamingStrategy.underline_to_camel);  //内置列的命名规则  下划线转驼峰命名
        stc.setEntityLombokModel(true);  //是否使用Lombok
        stc.setLogicDeleteFieldName("deleted");  //设置逻辑删除的名字
        stc.setVersionFieldName("version"); //设置乐观锁的名字
        //自动填充配置
        TableFill gmt_create = new TableFill("gmt_create", FieldFill.INSERT);
        TableFill gmt_modified = new TableFill("gmt_modified", FieldFill.INSERT_UPDATE);
        ArrayList<TableFill> tableFills = new ArrayList<>();
        tableFills.add(gmt_create);
        tableFills.add(gmt_modified);
        stc.setTableFillList(tableFills);
        stc.setRestControllerStyle(true);   //开启Controller 的驼峰命名
        stc.setControllerMappingHyphenStyle(true); //在url 中使用下划线
        autoGenerator.setStrategy(stc);
        //执行代码生成器
    }
```

