# 【JDBC】

## 简介

> **JDBC ：Java DataBase Connectivity —Java数据库连接**
>
> JDBC就是使用Java语言操作关系型数据库的一套API

JDBC相关类库：`java.sql.*`

JDBC本质上是SUN公司制定好的一套接口，纯`interface`，放在包`java.sql.*`中

```java
public interface JDBC {
	void getConnection();
}
```

MySQL数据库厂家对SUN公司制定的JDBC接口进行了实现，而这个代码只能又MySQL厂家进行编写，因为只有MySQL厂家才知道MySQL的秘密。

```java
public class MySQL implements JDBC(){
	//获取连接
	public void getConnection(){
   		 //MySQL厂家编写↓
		System.out.println("通过MySQL数据库的小秘密，连接了MySQL的数据库");
	}
}
```

其他数据库厂家如Oracle同理。	

JDBC是体现接口作用的非常经典的例子，我们不需要关心底层是mysql还是oracle，还是DB2还是Sybase，因为JDBC接口的出现让程序降低了耦合度，Java程序员写一套JDBC程序，可以连接任意一个品牌的数据库。

```java
public class JavaProgramer{
    //面向JDBC接口写代码
    //更换数据库时只需要改变new的对象，如new Oracle
    JDBC jdbc = new MySQL;
    //连接数据库
    
    //调用方法的时候，面向JDBC接口调用，JDBC中接口中有什么方法就调用什么方法
    /*
    * 这几百行代码都是面向JDBC接口调用的，更换数据库时不需要修改！
    */
}
```

JDBC整个程序结构中有三波人：

- **SUN公司，制定JDBC接口**

- **java.sql.*下面所有接口都有实现类，这些实现类是数据库厂家编写的，如MySQL：**

  mysql-connector-java-8.0.23.jar，这个jar包有一个专业的术语，叫做**mysql的驱动**

  jar包中有很多.class字节码文件，这事mysql数据库厂家写的接口实现！

  如果连接的是oracle，则需要oracle的jar包

- **Java程序员，面向JDBC接口写代码**

![image-20210725130537815](https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-master/image-20210725130537815.png)

## 	快速入门

- **0.创建工程，导入驱动jar包**

* **1.注册驱动**

  ```java
  Class.forName("com.mysql.cj.jdbc.Driver");
  ```

  `com.mysql.jdbc.Driver` 是 mysql-connector-java 5中的，
  `com.mysql.cj.jdbc.Driver` 是 mysql-connector-java 6中的

* **2.获取连接**

  ```java
  Connection conn = DriverManager.getConnection(url, username, password);
  ```

  Java代码需要发送SQL给MySQL服务端，就需要先建立连接

  8.0以上驱动包需要在url里修改时区`?serverTimezone=UTC`

* **3.定义SQL语句**

  ```java
  String sql =  "UPDATE ...";
  ```

* **4.获取执行SQL对象**

  执行SQL语句需要SQL执行对象，而这个执行对象就是Statement对象

  ```java
  Statement stmt = conn.createStatement();
  ```

* **5.执行SQL**

  ```java
  stmt.executeUpdate(sql);  
  ```

* **6.处理返回结果**

* **7.释放资源**

**案例：**

```java
// 1.注册驱动，JDK6，即JDBC4.0后的版本无需再显示注册，获取连接时会自动的查找合适的驱动类，并初始化当前使用相同驱动的类
Class.forName("com.mysql.cj.jdbc.Driver");

// 2.获取连接 8.0以上驱动包需要改时区
String url = "jdbc:mysql://127.0.0.1:3306/sqldb?serverTimezone=UTC";
String username = "xxxx";
String password = "xxxx";
Connection conn = DriverManager.getConnection(url, username, password);

// 3.定义sql, 注意不用sql语句后面不用写分号
String sql = "UPDATE stu SET name = '李华' WHERE id = 1";

// 4.获取执行sql的对象 Statement
Statement stmt = conn.createStatement();

// 5.执行sql, 返回值为受影响的行数，失败返回0
int count = stmt.executeUpdate(sql);

// 6.处理结果
System.out.println(count);

// 7.释放资源 后开的先释放
stmt.close();
conn.close();
```

## API详解

### 1.DriverManager

> **DriverManager（驱动管理类）作用：**
>
> 1. 注册驱动
> 2. 获取数据库连接

#### 注册驱动

![image-20220425193822113](https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-master/image-20220425193822113.png)

注册连接时的代码：

```java
Class.forName("com.mysql.cj.jdbc.Driver");
```

在Driver类中，静态代码块调用了注册的方法，从而注册驱动

```java
public class Driver extends NonRegisteringDriver implements java.sql.Driver {
    public Driver() throws SQLException {
    }

    static {
        try {
            // 静态代码块调用了注册的方法
            DriverManager.registerDriver(new Driver());
        } catch (SQLException var1) {
            throw new RuntimeException("Can't register driver!");
        }
    }
}
```

#### 获取数据库连接

![image-20220425193657787](https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-master/image-20220425193657787.png)

**参数说明：**

* **url ： 连接路径**

  > 语法：`jdbc:mysql://ip地址(域名):端口号/数据库名称?参数键值对1&参数键值对2…`
  >
  > 示例：`jdbc:mysql://127.0.0.1:3306/db1`
  >
  > 细节：
  >
  > * 如果连接的是本机mysql服务器，并且mysql服务默认端口是3306，则url可以简写为：`jdbc:mysql:///数据库名称?参数键值对`
  >
  > * 配置`useSSL=false`参数，禁用安全连接方式，解决警告提示

* **user ：用户名**

* **poassword ：密码**

### 2.Connection

> Connection（数据库连接对象）作用：
>
> * 获取执行 SQL 的对象
> * 管理事务

#### 获取执行对象

`Connection`可以通过以下方法获得不同的执行对象：

- 普通执行SQL对象

  ```java
  // 方法返回Statement
  Statement createStatement()
  ```

* 预编译SQL的执行SQL对象：防止SQL注入

  ```java
  // 方法返回PreparedStatement
  PreparedStatement prepareStatement(sql)
  ```

  通过这种方式获取的 `PreparedStatement` SQL语句执行对象可以防止SQL注入。

* 执行存储过程的对象

  ```java
  // 方法返回CallableStatement
  CallableStatement prepareCall(sql)
  ```

  通过这种方式获取的 `CallableStatement` 执行对象是用来执行存储过程的，而存储过程在MySQL中不常用

#### 管理事务

> MySQL事务管理的操作：
>
> * 开启事务 ： BEGIN; 或者 START TRANSACTION;
> * 提交事务 ： COMMIT;
> * 回滚事务 ： ROLLBACK;

**Connection几口中定义了3个对应的方法：**

|               方法名                |                     返回值                      |   效果   |
| :---------------------------------: | :---------------------------------------------: | :------: |
| `setAutoCommit(boolean autoCommit)` | `true`为自动提交；`false`为手动提交，即开始事务 | 开启事务 |
|             `commit()`              |                     `void`                      | 提交事务 |
|            `rollback()`             |                     `void`                      | 回滚事务 |

**案例：**

```java
try {
    // 1.开启事务
    conn.setAutoCommit(false);  
    // 执行sql, 返回值为受影响的行数，失败返回0
    int count1 = stmt.executeUpdate(sql1);    
    // 2.提交事务
    conn.commit();   
    // 处理结果
    System.out.println(count1);
} catch (Exception e) {
    // 3.回滚事务
    conn.rollback();
}
```

### 3.Statement

> **Statement作用：执行SQL语句**

**方法：**

```java
/** 
 * 执行DML、DDL语句
 * DML语句返回影响的行数
 * DDL语句执行成功也可能返回0
 */
int executeUpdate(sql);

/**
 * 执行DQL语句
 * 返回值：ResultSet结果集
 */
ResultSet executeQuery(sql);
```

### 4.ResultSet

> **ResultSet（结果集对象）作用：封装了SQL查询语句的结果**
>
> 执行DQL语句后就会返回该对象：
>
> ```java
> ResultSet  executeQuery(sql);
> ```

**光标：** 最开始指向字段名，每往后走一步相当于跳到下一条记录

![image-20220426073849602](https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-master/image-20220426073849602.png)

**从ResultSet中查询结果数据：**

- `next()` : 移动光标

```java
/**
 * 将光标从当前位置向前移动一行 
 * 判断当前行是否为有效行
 * true:有效航，当前行有数据
 * false ： 无效行，当前行没有数据
 */
boolean next();
```

- `getXxx(参数)` ： 获取光标当前行的指定列数据

```java
/**
 * xxx：数据类型；如： int getInt(参数)
 * 要是获取的数据是什么类型的xxx就是对应类型；
 * 参数有两种：
 * int类型的参数：列的编号，从1开始
 * String类型的参数：列的名称 
 */
xxx getXxx(参数);
```

**案例：** 

比如结果集如下：

|  id  | name |
| :--: | :--: |
|  1   | 张三 |
|  2   | 李四 |

获取这两行数据，封装成一个对象，放入`ArrayList`集合中：

```java
// 查询得到结果集对象
ResultSet rs = stmt.executeQuery(sql);
// 创建集合,User对象成员变量和表中列一一对应
List<User> list = new ArrayList<>();
// 获取数据
while(rs.next()) {
    // 可以使用列的名称
    int id = rs.getInt("id");
    // 也可以使用列的编号，第2列就是'name'列
    String name = rs.getString(2);
    // 封装对象到集合中
    User user = new User(id, name);
    list.add(user);
}

// ResultSet对象需要关闭释放资源
try{
    ...
} catch(Exception e) {
    // 注意先开后关
    rs.close();
    stmt.close();
    conn.close();
}
```

### 5.PreparedStatement

> **PreparedStatement作用：预编译SQL语句并执行，预防SQL注入问题**

#### sql注入

**定义：** sql注入即是指web应用程序对用户输入数据的合法性没有判断或过滤不严，攻击者可以在web应用程序中事先定义好的查询语句的结尾上添加额外的SQL语句，在管理员不知情的情况下实现非法操作，以此来实现欺骗数据库服务器执行非授权的任意查询，从而进一步得到相应的数据信息。

**根本原因：** 用户不是一般的用户，用户是懂得程序的，输入的用户名信息以及密码信息中含有SQL语句的关键字，这个SQL语句的关键字和底层的SQL语句进行“字符串拼接”，导致原SQL语句的含义被扭曲了。**最主要的原因是：用户提供的信息参与了SQL语句的编译！**

**主要因素：** 程序先进行SQL语句的拼接，再进行SQL语句的编译

**解决方案：**

- **java.sql.Statement接口：** 先进行字符串的拼接，然后再进行sql语句的编译。

​		优点：可以使用Statement进行sql语句的拼接

​		缺点：因为拼接的存在，导致可能给不法分子机会，导致SQL注入。

- **java.sql.PrepareStatement接口：** 先进行sql语句的编译，再进行sql语句的传值。

​		优点：避免SQL注入

​		缺点：没办法进行sql语句的拼接，只能给sql语句传值。

#### 使用          

**获取 PreparedStatement 对象**

```java
String sql = "SELECT * FROM tb_user WHERE id = ?";
PreparedStatement pstmt
    = conn.prepareStatement(sql);
```

上面的sql语句中参数使用`?`进行占位，在执行之前要设置这些`?`的值。

**设置参数值：**

```java
// 给第1个 ? 赋值为2，变成 WHERE id = 2
pstmt.setInt(1, 2);
```

> `setXxx(参数1，参数2)`：给`? `赋值
>
> * Xxx：数据类型 ； 如 setInt (参数1，参数2)
>
> * 参数：
>
>   * 参数1：`?`的位置编号，从1开始
>
>   * 参数2：`?`的值

**执行SQL语句：**

```java
ResultSet rs = pstmt.executeQuery();
```

> `executeUpdate();`：执行DDL语句和DML语句
>
> `executeQuery(); `：执行DQL语句
>
> **注意：** 调用这两个方法时不需要传递SQL语句，因为获取SQL语句执行对象时已经对SQL语句进行预编译了。

#### 原理

**Java代码操作数据库流程：**

![image-20210725195756848](https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-master/image-20210725195756848.png)

* **将sql语句发送到MySQL服务器端**

* **MySQL服务端会对sql语句进行如下操作**

  * **检查SQL语句：** 检查SQL语句的语法是否正确。

  * **编译SQL语句：** 将SQL语句编译成可执行的函数。

    检查SQL和编译SQL花费的时间比执行SQL的时间还要长。如果只是重新设置参数，那么检查SQL语句和编译SQL语句将不需要重复执行。这样就提高了性能。

  * **执行SQL语句**

**`PreparedStatement`原理：**

* 在获取`PreparedStatement`对象时，将sql语句发送给mysql服务器进行检查，编译（这些步骤很耗时）
* 执行时就不用再进行这些步骤，速度更快
* 如果sql模板一样，则只需要进行一次检查、编译

## 配置文件

> 将连接数据库的信息统一配置到配置文件中

IDEA：在`src`文件夹下新建包(package)，名为`resources`，新建文件`db.properties`

```properties
### mysql connectivity configuration ###
driver = com.mysql.cj.jdbc.Driver
url = jdbc:mysql://127.0.0.1:3306/数据库名?serverTimezone=UTC&useSSL=false
username = root
password = xxxxxx
```

```java
//资源绑定器
ResourceBundle bundle = ResourceBundle.getBundle("resources/db");
//通过属性配置文件拿到信息
String url = bundle.getString("url");
String driver = bundle.getString("driver");
String username = bundle.getString("username");
String password = bundle.getString("password");
```

**思路：**

**将连接数据库的可变化的4条信息写到配置文件中。**

**以后想连接其他数据库的时候，可以直接修改配置文件，不用修改java程序。**

## 连接池

### 简介

> 数据库连接池是个容器，负责分配、管理数据库连接(Connection)
>
> 它允许应用程序重复使用一个现有的数据库连接，而不是再重新建立一个；它还会释放空闲时间超过最大空闲时间的数据库连接，避免因为没有释放数据库连接而引起的数据库连接遗漏
>
> **好处：**
>
> * 资源重用
> * 提升系统响应速度
> * 避免数据库连接遗漏

之前的代码中使用连接是没有使用都创建一个`Connection`对象，使用完毕就会将其销毁。

这样重复创建销毁的过程是特别耗费计算机的性能，及消耗时间

而数据库使用了数据库连接池后，就能达到Connection对象的复用，如下图：

![image-20210725210432985](https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-master/image-20210725210432985.png)

连接池是在一开始就创建好了一些连接（`Connection`）对象存储起来。用户需要连接数据库时，不需要自己创建连接，而只需要从连接池中获取一个连接进行使用，使用完毕后再将连接对象归还给连接池；这样就可以起到资源重用，也节省了频繁创建连接销毁连接所花费的时间，从而提升了系统响应的速度。

### 实现

**标准接口：** `DataSource`

官方(SUN) 提供的数据库连接池标准接口，由第三方组织实现此接口。该接口提供了获取连接的功能：

```java
Connection getConnection()
```

那么以后就不需要通过 `DriverManager` 对象获取 `Connection` 对象，而是通过连接池（`DataSource`）获取 `Connection` 对象。

**常见的数据库连接池:**

* DBCP
* C3P0
* Druid

现在使用更多的是Druid，它的性能比其他两个好一些。

**Druid（德鲁伊）**

* Druid连接池是阿里巴巴开源的数据库连接池项目 
* 功能强大，性能优秀，是Java语言最好的数据库连接池之一

### Druid连接池

- **导入jar包 druid-1.1.12.jar：**

​		添加druid的jar包放到项目的lib目录下，添加为库文件

* **定义配置文件：**

  ​	`resources`目录下新建文件`druid.properties`

  ```properties
  driverClassName=com.mysql.cj.jdbc.Driver
  url=jdbc:mysql://127.0.0.1:3306/数据库名?serverTimezone=UTC&useSSL=false
  username=root
  password=xxxxxx
  # 初始化连接数量
  initialSize=5
  # 最大连接数
  maxActive=10
  # 最大等待时间
  maxWait=3000
  ```

* 加载配置文件

  ```java
  Properties prop = new Properties();
  // 可以打印一下当前工程目录
  System.out.println(System.getProperty("user.dir"));
  // 读取配置文件
  prop.load(new FileInputStream("src/resources/druid.properties"));
  ```

* 获取数据库连接池对象

  ```java
  // 获取数据库链接连接池对象
  DataSource dataSource = DruidDataSourceFactory.createDataSource(prop);
  ```

* 获取连接

  ```java
  // 获取数据库链接
  Connection connection = dataSource.getConnection();
  ```

### Druid配置详解




|                    属性                    |                             说明                             |          建议值           |
| :----------------------------------------: | :----------------------------------------------------------: | :-----------------------: |
|                   `url`                    |   数据库的jdbc连接地址。一般为连接oracle/mysql。示例如下：   |                           |
|                                            |    mysql : jdbc:mysql://ip:port/dbname?option1&option2&…     |                           |
|                                            |        oracle : jdbc:oracle:thin:@ip:port:oracle_sid         |                           |
|                                            |                                                              |                           |
|                 `username`                 |                      登录数据库的用户名                      |                           |
|                 `password`                 |                     登录数据库的用户密码                     |                           |
|               `initialSize`                |            启动程序时，在连接池中初始化多少个连接            |        10-50已足够        |
|                `maxActive`                 |                连接池中最多支持多少个活动会话                |                           |
|                 `maxWait`                  | 程序向连接池中请求连接时,超过maxWait的值后，认为本次请求失败，即连接池 |            100            |
|                                            |         没有可用连接，单位毫秒，设置-1时表示无限等待         |                           |
|        `minEvictableIdleTimeMillis`        | 池中某个连接的空闲时长达到 N 毫秒后, 连接池在下次检查空闲连接时，将 |        见说明部分         |
|                                            |               回收该连接,要小于防火墙超时设置                |                           |
|                                            |   net.netfilter.nf_conntrack_tcp_timeout_established的设置   |                           |
|      `timeBetweenEvictionRunsMillis`       |    检查空闲连接的频率，单位毫秒, 非正整数时表示不进行检查    |                           |
|                `keepAlive`                 | 程序没有close连接且空闲时长超过 minEvictableIdleTimeMillis,则会执 |           true            |
|                                            | 行validationQuery指定的SQL,以保证该程序连接不会池kill掉,其范围不超 |                           |
|                                            |                  过minIdle指定的连接个数。                   |                           |
|                 `minIdle`                  |          回收空闲连接时，将保证至少有minIdle个连接.          |     与initialSize相同     |
|             `removeAbandoned`              | 要求程序从池中get到连接后, N 秒后必须close,否则druid 会强制回收该 |   false,当发现程序有未    |
|                                            | 连接,不管该连接中是活动还是空闲, 以防止进程不会进行close而霸占连接。 | 正常close连接时设置为true |
|          `removeAbandonedTimeout`          | 设置druid 强制回收连接的时限，当程序从池中get到连接开始算起，超过此 |  应大于业务运行最长时间   |
|                                            |            值后，druid将强制回收该连接，单位秒。             |                           |
|               `logAbandoned`               |    当druid强制回收连接后，是否将stack trace 记录到日志中     |           true            |
|              `testWhileIdle`               | 当程序请求连接，池在分配连接时，是否先检查该连接是否有效。(高效) |           true            |
|             `validationQuery`              | 检查池中的连接是否仍可用的 SQL 语句,drui会连接到数据库执行该SQL, 如果 |                           |
|                                            |         正常返回，则表示连接可用，否则表示连接不可用         |                           |
|               `testOnBorrow`               |  程序 **申请** 连接时,进行连接有效性检查（低效，影响性能）   |           false           |
|               `testOnReturn`               |  程序 **返还** 连接时,进行连接有效性检查（低效，影响性能）   |           false           |
|          `poolPreparedStatements`          |                缓存通过以下两个方法发起的SQL:                |           true            |
|                                            |    public PreparedStatement prepareStatement(String sql)     |                           |
|                                            |    public PreparedStatement prepareStatement(String sql,     |                           |
|                                            |         int resultSetType, int resultSetConcurrency)         |                           |
| `maxPoolPrepareStatementPerConnectionSize` |                  每个连接最多缓存多少个SQL                   |            20             |
|                 `filters`                  |                这里配置的是插件,常用的插件有:                |      stat,wall,slf4j      |
|                                            |                    监控统计: filter:stat                     |                           |
|                                            |              日志监控: filter:log4j 或者 slf4j               |                           |
|                                            |                   防御SQL注入: filter:wall                   |                           |
|            `connectProperties`             |         连接属性。比如设置一些连接池统计方面的配置。         |                           |
|                                            |    druid.stat.mergeSql=true;druid.stat.slowSqlMillis=5000    |                           |
|                                            |                 比如设置一些数据库连接属性:                  |                           |



## 工具类封装

**为了便于以后的开发，封装一个JDBC的工具类**

新建package`utils`，新建class`DBUtil`

```java
public class DBUtil {
    
    // 工具类中的构造方法一般都是私有化来防止new对象，因为工具类中的方法都是静态的，不需要new对象，直接用“类.方法名”的方式调用
    private DButil(){}
    
    // 类加载时绑定资源文件
    private static ResourceBundle bundle = ResourceBundle.getBundle("resources/db");
    
    // 注册驱动，不需要
    static {
        try {
        Class.forName(bundle.getString("driver"));
        } catch(ClassNotFoundException e) {
            e.printStackTrace();
        }
    }
    
    /**
     *获取数据库连接对象
     *@return 新的连接对象
     *@throws SQLException
     */
    public static Connection getConnection() throws SQLException {
        String url = bundle.getConnection("url");
        String user = bundle.getConnection("user");
        String password = bundle.getConnection("password");
        Connection conn = DriverMannager.getConnection(url,user,password);
        return conn;
    }
    
    /**
     *释放资源
     *这里用Statement而不是PreparedStatement，写父类，因为多态，子类也可以传过来
     *@param conn 连接对象
     *@param stmt 数据库操作对象
     *@param rs 查询结果集
     */
    public static void close(Connection conn, Statement stmt, ResultSet rs) {
        if(rs != null) {
                try{
                   	rs.close();
                } catch(SQLException e){
                    e.printStackTrace();
                }                    
           	}           
            if(stmt != null) {
                try{
                    stmt.close();
                } catch(SQLException e){
                    e.printStackTrace();
                }                
            }
            if(conn != null) {
                try{
                   	conn.close();
                } catch(SQLException e){
                    e.printStackTrace();
                }                    
    		}
    }
}
```

调用语句：

```java
Connection conn = null;
PreparedStatement ps = null;
ResultSet rs = null;
try{
    //一.获取连接
    conn = DBUtil.getConnection();
    // 执行sql语句
} catch(Exception e) {
    e.printStackTrace();
} finally {
    //二.释放资源
    DBUtil.close(conn, ps, rs);
}
```
