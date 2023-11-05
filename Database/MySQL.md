# 【MySQL基础篇】

```properties
spring.datasource.username=root
spring.datasource.password=123456
spring.datasource.url=jdbc:mysql://localhost:3306/mall?useUnicode=true&characterEncoding=UTF-8&serverTimezone=Asia/Shanghai&useSSL=true
```

## 简介

> **数据库：**
>
> - 有组织地存档数据的仓库
> - 英文：DataBase，简称DB
>
> **数据库管理系统：**
>
> - 管理数据库的大型软件
> - 英文：DataBase Management System， 简称DBMS
>
> **关系型数据库：** 关系型数据库是建立在关系模型上的数据库。简单来说，关系型数据库是由多张互相连接的二维表组成的数据库。
>
> ​		表的每一行称为记录（Record），记录是一个逻辑意义上的数据
>
> ​		表的每一列称为字段（Column），同一个表的每一行记录都拥有相同的若干字段。
>
> **优点：**
>
> - 都是使用表结构，格式一致，易于维护
> - 使用通用SQL语言操作，使用方便，可用于复杂查询
> - 数据存储在磁盘中，安全
>
> **SQL：**
>
> - 英文：Structured Query Language，简称SQL，结构化查询语言
> - 操作关系型数据库的编程语言
> - 定义操作所有关系型数据库的统一标准
>
> **MySQL：** 开源免费的中小型数据库(管理系统)，是关系型数据库。
>
> ​	Sun公司收购了MySQL，然后Sun公司又被Oracle收购
>
> ****

## 常用指令

> Server version: 8.0.19 MySQL Community Server - GPL

**登录：**

| 操作 |                             指令                             |
| :--: | :----------------------------------------------------------: |
| 登录 | `mysql -u<帐号>  -p<密码> -h<ip(本机省略) -p<端口号(默认3306)>` |
| 断开 |                        `exit`或`quit`                        |
| 清屏 |                     `mysql> system cls;`                     |

​	注意`EXIT`仅仅断开了客户端和服务器的连接，MySQL服务器仍然继续运行。

​	**数据库：**

|        操作        |          指令          |
| :----------------: | :--------------------: |
|   列出所有数据库   |   `SHOW DATABASES;`    |
| 创建一个新的数据库 | `CREATE DATABASE xxx;` |
|   删除一个数据库   |  `DROP DATABASE xxx;`  |
|  切换/使用数据库   |       `USE xxx;`       |

​	**表：**

|         操作         |           指令           |
| :------------------: | :----------------------: |
| 列出当前数据库所有表 |      `SHOW TABLES;`      |
|   查看一个表的结构   |       `DESC xxx;`        |
| 查看创建表的SQL语句  | `SHOW CREATE TABLE xxx;` |
|        创建表        |   `CREATE TABLE xxx;`    |
|        删除表        |    `DROP TABLE xxx;`     |

​	**创建表：**

```sql
CREAT TABLE 'xxx' {
	-- AUTO_INCREMENT定义列为自增的属性，一般用于主键，数值会自动加1。
	'xxx1' bigint(20) UNSIGNED AUTO_INCREMENT, 	
	'xxx2' VARCHAR(100) NOT NULL,
	'XXX3' VARCHAR(40) NOT NULL,
	'XXX3' DATE,	
	-- PRIMARY KEY关键字用于定义列为主键。 您可以使用多列来定义主键，列间以逗号分隔。
	RRIMARY KET('XXX1')  -- 没有逗号！！！！！
	-- 设置存储引擎，CHARSET 设置编码。
}ENGINE = InnoDB DEFAULT CHARSET=utf8;
```

## 数据类型

MySQL数据类型大致可以分成3类：

- 数值
- 日期
- 字符串

![image-20220424174003908](https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-master/image-20220424174003908.png)

**简单说明：**

| 名称           | 类型       | 说明                                                         |
| :------------- | :--------- | :----------------------------------------------------------- |
| `INT`          | 整型       | 小整数值                                                     |
| `DOUBLE(M,N)`  | 浮点型     | 双精度浮点数，M是总长度，N是小数点后保留位数                 |
| `DECIMAL(M,N)` | 高精度小数 | 由用户指定精度的小数，使用字符串保存                         |
| `CHAR(N)`      | 定长字符串 | 存储指定长度的字符串，例如，CHAR(100)总是存储100个字符的字符串，**性能高，空间换时间** |
| `VARCHAR(N)`   | 变长字符串 | 存储可变长度的字符串，例如，VARCHAR(100)可以存储0~100个字符的字符串，**性能低，时间换空间** |
| `BOOLEAN`      | 布尔       | 存储True或者False                                            |
| `DATE`         | 日期       | 存储日期，例如，2018-06-22                                   |
| `TIME`         | 时间       | 存储时间，例如，12:20:59                                     |

## SQL简介

**英文：Structured Query Language，简称SQL**

SQL是结构化查询语言，一门操作关系型数据库的编程语言

它定义了操作所有关系型数据库的统一标准

对于同一个需求，每一种数据库操作的方式可能存在一些不一样的地方，称之为“方言”

1. SQL语言可以单行或多行书写，**以分号结尾**
2. MySQL数据库的SQL语句不区分大小写，关键字建议使用大写
3. 注释：

```sql
-- 单行注释
# MySQL特有的单行注释
/*
 * 多行注释
 */
```

**SQL分类：**

- **DDL 数据定义语言(Data Definition Language):** 用来定义数据库的对象，如数据库、表、列等 
- **DML 数据操纵语言(Data Manipulation Language):** 用来对数据库中的数据进行增删改查
- **DQL 数据查询语言(Data Query Language):** 用来查询数据库中表的记录（数据）
- **DCL 数据控制语言(Data Control Language):** 重来定义数据库的访问权限和安全等级，以及创建用用户

> 关系数据库的基本操作就是增删改查，即CRUD：Create、Delete、Update、Retrieve。

## DDL

> **DDL 数据定义语言(Data Definition Language):** 用来定义数据库的对象，如数据库、表、列等 

### 操作数据库

#### 查询库

```sql
SHOW DATABASES;

-- 查询成功，初始四个数据库为mysql自带的数据库
+--------------------+
| Database           |
+--------------------+
| information_schema |
| mysql              |
| performance_schema |
| sys                |
+--------------------+
4 rows in set (0.00 sec)
```

**自带的数据库: **

|       数据库名       |                             说明                             |
| :------------------: | :----------------------------------------------------------: |
| `information_schema` | 记录了mysql有哪些库有哪些表的信息，使用特殊的"视图"来存储，不存在物理的文件 |
|      `mysql  `       |               存储数据库核心信息，如权限、安全               |
| `performance_schema` |                      存储性能相关的信息                      |
|        `sys`         |                      存储系统相关的信息                      |

#### 创建库

```sql
CREATE DATABASE 数据库名;

-- 创建成功
Query OK, 1 row affected (0.01 sec)
```

**如果需要判断无同名数据库时再创建：**

```sql
CREATE DATABASE IF NOT EXISTS 数据库名;
-- 已存在数据库，未创建
Query OK, 1 row affected, 1 warning (0.01 sec)

CREATE DATABASE IF NOT EXISTS 数据库名;
-- 未存在数据库，创建
Query OK, 1 row affected (0.01 sec)
```

#### 删除库

```sql
DROP DATABASE 数据库名;
```

**如果需要判断数据库是否存在再删除：**

```sql
DROP DATABASE IF EXISTS 数据库名;
```

#### 切换库

```sql
USE 数据库名;
```

**查询当前使用的数据库：**

```sql
SELECT DATABASE();
```

![image-20220424175628609](https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-master/image-20220424175628609.png)

### 操作表

#### 查询表

**查询库内所有表：**

```sql
SHOW TABLES;
```

![image-20220424172925118](https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-master/image-20220424172925118.png)

**查询某个表的结构：**

```sql
DESC 表名;
```

![image-20220424172955127](https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-master/image-20220424172955127.png)

**查询表的建表语句：**

```mysql
SHOW CREATE TABLE 表名;
```

#### 创建表

```sql
CREATE TABLE 表名 (
	字段名1  数据类型1,
    字段名2  数据类型2,
    ......
    -- 最后一个字段不需要逗号
    字段名n  数据类型n 
);
```

![image-20220424173717172](https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-master/image-20220424173717172.png)

**创建案例：**

![image-20220424174936662](https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-master/image-20220424174936662.png)

```sql
CREATE TABLE stu(
	id INT,
    name VARCHAR(10),
    gender CHAR(1),
    birthday DATE,
    score DOUBLE(5,2),
    email VARCHAR(64),
    tel VARCHAR(15),
    status TINYINT
);
```

![image-20220424175324694](https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-master/image-20220424175324694.png)

#### 删除表

```sql
DROP TABLE 表名;
```

**删除前判断是否存在：**

```sql
DROP TABLE IF EXISTS 表名;
```

#### 修改表

**修改表名：**

```sql
ALTER TABLE 表名 RENAME TO 新的表名;
```

**添加一列：**

```sql
ALTER TABLE 表名 ADD 列名 数据类型;
```

**修改数据类型：**

```sql
ALTER TABLE 表名 MODIFY 列名 新数据类型;
```

**修改列名和数据类型：**

```sql
ALTER TABLE 表名 CHANGE 列名 新列名 新数据类型
```

**删除列：**

```sql
ALTER TABLE 表名 DROP 列名;
```

## DML

> **DML 数据操纵语言(Data Manipulation Language):** 用来对数据库中的数据进行增删改查

### 添加数据

**给指定列添加数据:**

```sql
INSERT INTO 表名(列名1,列名2,...) VALUES(值1, 值2,...);

-- 给所有列增添数据时可以省略列名
INSERT INTO stu values(值1, 值2,...);
```

![image-20220424195644626](https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-master/image-20220424195644626.png)

**批量添加： 用逗号隔开**

```sql
INSERT INTO 表名(列名1,列名2,...) VALUES(值1, 值2,...),(值1, 值2,...),(值1, 值2,...),...;
```

![image-20220424200317533](https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-master/image-20220424200317533.png)

**注意：**

- 自增主键可以由数据库自己推算，有默认值的字段在`INSERT`语句中不必出现

- 字段顺序不必和数据库表的字段顺序一致，但值的顺序必须和字段顺序一致。

### 修改数据

**修改表数据：**

```sql
UPDATE 表名 SET 列名=值1,列名=值2,... [WHERE 条件];
```

![image-20220424200844666](https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-master/image-20220424200844666.png)

**注意： 如果不写WHERE条件，会把所有的记录全部修改！**

### 删除数据

```sql
DELETE FROM 表名 WHERE ...;
```

**注意：**

- 如果`WHERE`条件没有匹配到任何记录，`DELETE`语句不会报错，也不会有任何记录被删除。
- 最后，要特别小心的是，和`UPDATE`类似，不带`WHERE`条件的`DELETE`语句会删除整个表的数据，最好也先用`SELECT`语句来测试`WHERE`条件是否筛选出了期望的记录集，然后再用`DELETE`删除

## DQL

> **DQL 数据查询语言(Data Query Language):** 用来查询数据库中表的记录（数据）

**查询语法（编写顺序）：**

```sql
SELECT [DISTINCT] 字段列表 FROM 列表表名

WHERE
	条件
GROUP BY
	分组字段
HAVING
	分组后条件
ORDER BY
	排序字段 [ASC/DESC]
LIMIT
	起始索引，查询条目数
;	
```

### 基本查询

```sql
SELECT 列1, 列2,...(简称字段列表) FROM 表名;
```

**注意：**

- `*` 替代列名可以查询到所有字段，但项目中尽量不要使用`*`，写全列名可以明确需要查找的数据
- `DISTINCT` 可以去除重复记录，写在字段列表前面，如:`SELECT DISTINCT 字段列表 ...`或者`SELECT COUNT(DISTINCT 字段列表) ...`
- `列名 as/空格 别名` 可以给查询出的列名起别称
- 如果不使用FROM字句，则SELECT会计算出其后跟着的表达式的结果，通常可以用来判断当前到数据库的连接是否有效。许多检测工具会执行一条`SELECT 1;`来测试数据库连接。

![image-20220424202227605](https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-master/image-20220424202227605.png)

### 条件查询

```sql
SELECT 字段列表 FROM 表名 WHERE 条件;
```

**条件：**

|         符号         |                  功能                   |
| :------------------: | :-------------------------------------: |
|         `>`          |                  大于                   |
|         `<`          |                  小于                   |
|         `>=`         |                大于等于                 |
|         `<=`         |                小于等于                 |
|         `=`          |                  等于                   |
|      `!=`或`<>`      |                 不等于                  |
| `BETWEEN ... AND...` |          在某个范围内(都包含)           |
|      `IN(...)`       |                 多选一                  |
|    `LIKE` 占位符     | 模糊查询:  `_`单个字符，`%`多个任意字符 |

|     符号      |   功能   |
| :-----------: | :------: |
|   `IS NULL`   |  是NULL  |
| `IS NOT NULL` | 不是NULL |
|  `AND`或`&&`  |   并且   |
|  `OR`或`||`   |   或者   |
|  `NOT`或`!`   | 非、不是 |

**注意：** 比对字段是否为NULL要使用`IS NULL`和`IS NOT NULL`，不能用`NULL`

**条件表达式：**

- 和 `<条件1> AND <条件2>`
- 或 `<条件1> OR <条件2>`
  - 简化或写法: `字段 IN(值1,值2,...)`
- 否 `NOT <条件>` 
  - `NOT id = 2`等价于`id <>2`
- 条件运算按照`NOT`、`AND`、`OR`的优先级进行，要组合三个或者更多的条件并改变优先级，就需要用小括号`()`表示如何进行条件运算。

**模糊查询案例：**

- 查询第一个是'马'的学员信息:

```sql
SELECT * FROM stu WHERE name LIKE '马%';
```

- 查询第二个字是'花'的学员信息:

```sql
SELECT * FROM stu WHERE name LIKE '_花%';
```

- 查询名字中包含'德'的学员信息：

```sql
SELECT * FROM stu WHERE name LIKE '%德%';
```

### 排序查询

```sql
SELECT * FROM students ORDER BY 字段名1 排序方式1, 字段名1 排序方式2; 
```

查询时结果集通常是按照主键排序，若要其根据其他条件排序，就可以加上`ORDER BY`子句。

- `ORDER BY 列名 ASC` 升序排序（从小到大），`ASC` 为默认值
- `ORDER BY 列名 DESC` 降序排序（从大到小）
- 如果列有相同的数据，要进一步排序，就可以继续添加列名，也就是说如果有多个排序条件，只有当前一个条件值一样时才会根据第二条件排序。


**注：**`ORDER BY`子句要放到`WHERE`子句后面

**案例：**

- 查询学生信息，按照年龄升序排列

```sql
SELECT * FROM stu ORDER BY age ASC;
```

- 查询学生信息，按照数学成绩降序排列

```sql
SELECT * FROM stu ORDER BY age DESC;
```

- 查询学生信息，按照数学成绩降序排列，如果成绩一样，按照英语成绩升序排列

```sql
SELECT * FROM stu ORDER BY math DESC, english ASC;
```

### 聚合函数

**概念：** 将一列数据作为一个整体，进行纵向计算

**聚合函数语法：**	

```sql
SELECT 聚合函数名(列名) FROM 表;
```

**聚合函数：**

| 函数          | 说明                                                         |
| :------------ | :----------------------------------------------------------- |
| `COUNT(字段)` | 计算列的行数(一般选不为`NULL`的字段，通常为主键)，使用`*`可以统计到所有行 |
| `SUM(字段)`   | 计算某一列的合计值，该列必须为数值类型                       |
| `AVG(字段)`   | 计算某一列的平均值，该列必须为数值类型                       |
| `MAX(字段)`   | 计算某一列的最大值                                           |
| `MIN(字段)`   | 计算某一列的最小值                                           |

**注意： **

- `null`值不参与所有聚合函数运算

- `MAX()`和`MIN()`函数并不限于数值类型。如果是字符类型，`MAX()`和`MIN()`会返回排序最后和排序最前的字符。
- 如果聚合查询携带`WHERE`条件但没有匹配到任何行，`COUNT()`会返回0，而`SUM()`、`AVG()`、`MAX()`和`MIN()`会返回`NULL`
- 如果需要分组使用聚合函数，可以使用`GROUP BY`，要显示分组字段，可以在查询中加入分组字段

```sql
SELECT 分组字段,聚合函数名(列名) 别名 FROM 表 GROUP BY 分组字段;
```

**案例：**

![image-20220424211514727](https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-master/image-20220424211514727.png)

注意第5条英语成绩为`NULL`，以该字段为参数不会参与到聚合函数运算

- 统计班级共有多少个学生

```sql
-- id是不会为NULL的字段
SELECT COUNT(id) FROM stu;
```

![image-20220424211535698](https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-master/image-20220424211535698.png)

如果参数是`english`，统计的数据是7，因为有一条记录的`english`为`NULL`

- 查询数学成绩最高分和最低分

```sql
-- 最高分
SELECT MAX(math) FROM stu;
-- 最低分
SELECT MIN(math) FROM stu;
```

![image-20220424211559200](https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-master/image-20220424211559200.png)

- 查询数学成绩的总分

```sql
SELECT SUM(math) FROM stu;
```

![image-20220424211634257](https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-master/image-20220424211634257.png)

- 查询数学成绩的平均分

```sql
SELECT AVG(math) FROM stu;
```

![image-20220424211721362](https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-master/image-20220424211721362.png)

### 分组查询

分组查询使用`GROUP BY`

```sql
SELECT 字段列表 FROM 表名 [WHERE 分组前的条件限定] GROUP BY 分组字段名 [HAVING 分组后条件过滤];
```

**`WHERE`和`HAVING`的区别：**

- 执行顺序：`WHERE`>聚合函数>`HAVING`
- 执行时机不同：`WHERE`在分组之前限定，不满足的不参与分组，`HAVING`在分组之后限定，是对分组结果进行过滤
- 可判断的条件不一样：`WHERE`不能对聚合函数进行判断，`HAVING`可以

**案例：**

- 查询男同学和女同学各自的数学平均分

```sql
SELECT sex, AVG(math) FROM stu GROUP BY sex;
```

![image-20220424212843547](https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-master/image-20220424212843547.png)	

**注意：** 分组之后，查询的字段为聚合函数和分组字段，查询其他字段无任何意义，比如这里哪怕查询了name，结果集中也不会显示

- 查询男同学和女同学各自的数学平均分，以及各自人数

```sql
SELECT sex 性别, AVG(math) 平均分, COUNT(*) 人数 FROM stu GROUP BY sex;
```

![image-20220424213340588](https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-master/image-20220424213340588.png)

- 查询男同学和女同学各自的数学平均分，以及各自人数，要求：分数低于70分的不参与分组

```sql
SELECT sex 性别, AVG(math) 平均分, COUNT(*) 人数 FROM stu WHERE math > 70 GROUP BY sex;
```

![image-20220424213512861](https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-master/image-20220424213512861.png)

- 查询男同学和女同学各自的数学平均分，以及各自人数，要求：分数低于70分的不参与分组，分组之后人数大于2个的

```sql
SELECT sex 性别, AVG(math) 平均分, COUNT(id) 人数 FROM stu WHERE math > 70 GROUP BY sex HAVING COUNT(id) > 2;
```

![image-20220424213910479](https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-master/image-20220424213910479.png)

### 分页查询

分页查询使用关键字`LIMIT M OFFSET <N>`实现，在`MySQL`中`OFFSET`可省略为`<M>, <N> `，这是`MySQL`的方言(不同数据库分页关键字一般不一样)

```sql
SELECT 字段列表 FROM 表名 LIMIT 起始索引, 查询条目数;
```

**注意： 上述语句中的起始索引是从0开始**

**起始索引计算公式：**

```sql
起始索引 = (当前页码 - 1) * 每页显示的条数
```

**案例：**

- 从第0条开始查询，查询3条数据

```sql
SELECT * FROM stu LIMIT 0, 3;
```

- 每页显示3条数据，查询第1页数据

```sql
SELECT * FROM stu LIMIT 0, 3;
```

![image-20220424214750481](https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-master/image-20220424214750481.png)

- 每页显示3条数据，查询第2页数据，起始索引(2-1)*3=3

```sql
SELECT * FROM stu LIMIT 3, 3;
```

![image-20220424214809642](https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-master/image-20220424214809642.png)

- 每页显示3条数据，查询第3页数据，起始索引(3-1)*3=6（当前页不足3条，有多少显示多少）

```sql
select * from stu limit 6, 3;
```

![image-20220424214827977](https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-master/image-20220424214827977.png)

### 练习

```sql
-- 查询年龄为20，21，22，23岁的女性员工信息
SELECT * FROM emp WHERE gender = '女' and age in(20,21,22,23);

-- 查询性别为男，年龄在20-40岁（含）以内，姓名为三个字的员工
SELECT * FROM emp WHERE gender = '男' AND (age BETWEEN 20 AND 40) AND name like '___';

-- 统计员工表中，年龄小于60岁的，男性员工和女性员工的人数
SELECT gender, COUNT(*) FROM emp WHERE age < 60 GROUP BY gender;

-- 查询所有年龄小于或等于35岁员工的姓名和年龄，并对查询结果按年龄升序排序，如果年龄相同按入职时间降序排序
SELECT name,age FROM emp WHERE name <= 35 ORDER BY age ASC,entrydate DESC;

-- 查询性别为男，且年龄在20-40岁（含）以内的前五个员工信息，对查询的结果按年龄升序排序，年龄相同按入职时间升序排序
SELECT * FROM emp WHERE gender = '男' AND (age BETWEEN 20 AND 40) ORDER BY age ASC , entrydate ASC LIMIT 0,5;
```

### 执行顺序

```sql
-- 执行顺序
FROM 
	字段列表
WHERE 
	条件列表
GROUP BY 
	分组字段列表
HAVING
	分组后条件列表
SELECT
	字段列表
ORDER BY
	排序字段列表
LIMIT
	分页参数
```

## DCL

> **DCL 数据控制语言(Data Control Language):** 用来管理数据库用户、控制数据库的访问权限
>
> - 控制哪些用户能访问数据库
> - 控制用户对数据库有哪些访问权限

### 用户管理

#### 查询用户

**mysql的用户信息存在user表中，且该表只能本机访问**

```sql
USE mysql; 
SELECT * FROM user;
```

#### 创建用户

**mysql用户使用'用户名@主机名'标识**

```sql
CREATE USER '用户名'@'主机名' IDENTIFIED BY '密码';
```

**例：**

```mysql
-- 创建用户 chong，只能够在当前主机localhost访问，密码 123456;
CREATE USER 'chong'@'localhost' IDENTIFIED BY '123456';
-- 注意，此时的用户没有分配权限
```

```mysql
-- 创建用户 ming，可以在任意主机访问该数据库，密码 123456;
CREATE USER 'ming'@'%' IDENTIFIED BY '123456';
-- %代表任意主机都能访问
```

![image-20221025194832511](https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-masterimage-20221025194832511.png)

#### 修改用户密码

```sql
ALTER USER '用户名'@'主机名' IDENTIFIED WITH mysql_native_password BY '新密码';
```

**例：**

```mysql
-- 修改用户 ming的访问密码为 1234;
ALTER USER 'ming'@'%' IDENTIFIED with mysql_native_password BY '1234';
```

#### 删除用户

```sql
DROP USER '用户名'@'主机名';
```

**例：**

```mysql
-- 删除 chong@localhost;
DROP USER 'chong'@'localhost';
```

### 权限控制

#### 权限列表

**MySQL中定义了很多种权限，但常用的就以下几种：**

|        权限        |        说明        |
| :----------------: | :----------------: |
| ALL，ALLPRIVILEGED |      所有权限      |
|       SELECT       |      查询数据      |
|       INSERT       |      插入数据      |
|       UPDATE       |      修改数据      |
|       DELETE       |      删除数据      |
|       ALTER        |       修改表       |
|        DROP        | 删除数据库/表/视图 |
|       CREATE       |   创建数据库/表    |

#### 查询权限

```mysql
SHOW GRANTS FOR '用户名'@'主机名';
```

**例：**

```mysql
-- 查询权限
SHOW GRANTS FOR 'ming'@'%';
```

![image-20221025202828640](https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-masterimage-20221025202828640.png)

此时该没有权限

#### 授予权限

```mysql
GRANT 权限列表 ON 数据库.表名 TO '用户名'@'主机名';
```

**例：**

```mysql
-- 授予所有权限
GRANT ALL ON tablespace.* TO 'ming'@'%';
```

![image-20221025202927209](https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-masterimage-20221025202927209.png)

此时用户多了一条对于数据库'tablespace'的全部权限

#### 撤销权限

```mysql
REVOKE 权限列表 ON 数据库名.表名 FROM '用户名'@'主机名';
```

**例：**

```mysql
-- 撤销所有权限
REVOKE ALL ON tablespace.* FROM 'ming'@'%';
```

![image-20221025203823578](https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-masterimage-20221025203823578.png)

刚才新增的权限被删除了

## 函数

> 函数是指一端可以直接被另一端程序调用的程序或代码。
>
> MySQL中内置了很多函数

### 字符串函数

**相对常用的函数：**

|            函数            |                           功能                            |
| :------------------------: | :-------------------------------------------------------: |
|   `CONCAT(S1,S2,...,Sn)`   |                        字符串拼接                         |
|        `LOWER(str)`        |                   将字符串全部转为小写                    |
|        `UPPER(str)`        |                   将字符串全部转为大学                    |
|     `LPAD(str,n,pad)`      | 左填充，用字符串pad对str的左边进行填充，达到n个字符串长度 |
|     `RPAD(str,n,pad)`      | 右填充，用字符串pad对str的右边进行填充，达到n个字符串长度 |
|        `TRIM(str)`         |                去掉字符串头部和尾部的空格                 |
| `SUBSTRING(str,start,len`) | 返回字符串str从start位置起始(从1开始)的len个长度的字符串  |

**例：**

```mysql
-- CONCAT
SELECT CONCAT('hello','mysql');

-- LOWER
SELECT LOWER('HELLO');

-- UPPER
SELECT UPPER('hello');

-- LPAD
SELECT LPAD('01',5,'-');

-- RPAD
SELECT RPAD('01',5,'-');

-- TRIM
SELECT TRIM(' Hello MySQL ');

-- SUBSTRING
SELECT SUBSTRING('Hello MySQL',1,3); -- HEL

-- 将企业员工的工号统一为5位数
UPDATE emp set workerno = LPAD(workno,5,0);
```

### 数值函数

**常见的数值函数：**

|     函数     |                功能                |
| :----------: | :--------------------------------: |
|  `CEIL(x)`   |              向上取整              |
|  `FLOOR(x)`  |              向下取整              |
| `MOD(x，y)`  |            返回x/y的模             |
|   `RAND()`   |         返回0~1内的随机数          |
| `ROUND(x,y)` | 求参数x的四舍五入的值，保留y位小数 |

**例：**

```mysql
-- CEIL
SELECT CEIL(1.1); -- 2

-- FLOOR
SELECT FLOOR(1.9); -- 1

-- MOD
SELECT MOD(7,4); -- 3

-- RAND
SELECT RAND();

-- ROUND
SELECT ROUND(2.344,2); -- 2.34


-- 案例: 通过数据库的函数，生成一个六位数的随机验证码。
SELECT LPAD(ROUND(RAND()*1000000,0),6,'0');
```

### 日期函数

|               函数                |                       功能                        |
| :-------------------------------: | :-----------------------------------------------: |
|            `CURDATE()`            |                   获得当前日期                    |
|            `CURTIME()`            |                   获得当前时间                    |
|              `NOW()`              |                返回当前日期和时间                 |
|           `YEAR(date)`            |                获取指定date的年份                 |
|           `MONTH(date)`           |                获取指定date的月份                 |
|            `DAY(date)`            |                获取指定date的日期                 |
| `DAY_ADD(date,INTERVALexpr type)` | 返回一个日期/时间值加上一个时间间隔expr后的时间值 |
|      `DATEDIFF(date1,date2)`      |    返回起始时间date1和结束时间date2之间的天数     |

**例：**

```mysql
-- CURDATE()
SELECT CURDATE();

-- CURTIME()
SELECT CURTIME();

-- NOW()
SELECT NOW();

-- YEAR , MONTH , DAY
SELECT YEAR(NOW());

SELECT MONTH(NOW());

SELECT DAY(NOW());

-- DATE_ADD
SELECT DATE_ADD(NOW(), INTERVAL 70 YEAR );

-- DATEDIFF
SELECT DATEDIFF('2021-10-01', '2021-12-01');

-- 案例: 查询所有员工的入职天数，并根据入职天数倒序排序。
SELECT name, DATEDIFF(CURDATE(), entrydate) AS 'entryday' FROM emp ORDER BY entrydays DESC;
```

### 流程函数

流程控制函数可以在SQL语句中实现条件筛选，从而提高语句的效率

**常见的流程控制函数：**

|                            函数                             |                       功能                        |
| :---------------------------------------------------------: | :-----------------------------------------------: |
|                       `IF(value,t,f)`                       |           如果value为true，返回t，否则f           |
|                   `IFNULL(value1,value2)`                   |    如果value不为空，返回value1，否则返回value2    |
|    `CASE WHEN [val1] THEN [res1] ...ELSE [default] END`     |    如果val1为true，返回res1,...否则返回default    |
| `CASE [expr] WHEN [val1] THEN [res1] ...ELSE [default] END` | 如果expr的值等于val1，返回res1,...否则返回default |

**例：**

```mysql
-- IF
SELECT IF(FALSE, 'OK', 'ERROR');

-- IFNULL
SELECT IFNULL('OK','DEFAULT'); -- OK
SELECT IFNULL('','DEFAULT'); -- (空串)
SELECT IFNULL(NULL,'DEFAULT'); -- DEFAULT

-- CASE WHEN THEN ELSE END
-- 需求: 查询EMP表的员工姓名和工作地址 (北京/上海 ----> 一线城市 , 其他 ----> 二线城市)
SELECT
    name,
    (CASE workaddress WHEN '北京' THEN '一线城市' WHEN '上海' THEN '一线城市' ELSE '二线城市' END) AS '工作地址'
FROM EMP;
```

## 约束

### 概念

- 约束是作用于表中列上的规则，用于限制加入表的数据
- 约束的存在保证了数据库中数据的正确性、有效性、完整性
- 例如，约束id字段，让它不可重复

```mysql
CREATE TABLE user (
	id int PRIMARY KEY AUTO_INCREMENT COMMENT '主键',
	name varchar(10) NOT NULL UNIQUE COMMENT '姓名',
	age int CHECK (age > 0 AND age <= 120) COMMENT '年龄',
	status char(1) DEFAULT '1' COMMENT '状态',
	gender char(1) COMMENT '性别'
) COMMENT '用户表';
```

### 分类

**SQL中约束的分类：**

| 约束名称 |    关键字     |                             描述                             |
| :------: | :-----------: | :----------------------------------------------------------: |
| 非空约束 |  `NOT NULL`   |                 保证列中所有数据不能为`NULL`                 |
| 唯一约束 |   `UNIQUE`    |                   保证列中所有数据各不相同                   |
| 主键约束 | `PRIMARY KEY` |           主键是一行数据的唯一标识，要求非空且唯一           |
| 检查约束 |    `CHECK`    |       保证列中的值满足某一条件，**MySQL 8.0.16后支持**       |
| 默认约束 |   `DEFAULT`   | 保存数据时，未指定值则采用默认值(不给值时才会给默认值，如果给值`NULL`，结果还是`NULL`) |
| 外键约束 | `FOREIGN KEY` | 外键用来将两个表的数据之间建立链接，保证数据的一致性和完整性 |

**注意：** MySQL 8.0.16 后才支持检查约束

### 非空约束

> **概念：** 非空约束用于保证列中所有数据不能有NULL值

**语法：**

* 添加约束

  ```sql
  -- 创建表时添加非空约束
  CREATE TABLE 表名(
      列名 数据类型 NOT NULL,
      …
  ); 
  ```

  ```sql
  -- 建完表后添加非空约束
  ALTER TABLE 表名 MODIFY 字段名 数据类型 NOT NULL;
  ```

* 删除约束

  ```sql
  ALTER TABLE 表名 MODIFY 字段名 数据类型;
  ```

### 唯一约束

> **概念：** 唯一约束用于保证列中所有数据各不相同

**语法：**

* 添加约束

  ```sql
  -- 创建表时添加唯一约束
  CREATE TABLE 表名(
     列名 数据类型 UNIQUE [AUTO_INCREMENT],
     -- AUTO_INCREMENT: 当不指定值时自动增长
     …
  ); 
  
  -- 在建表语句末尾添加唯一约束
  CREATE TABLE 表名(
     列名 数据类型,
     …
     CONSTRAINT 约束名称 UNIQUE(字段名,...)
  ); 
  ```

  ```sql
  -- 建完表后添加唯一约束
  ALTER TABLE 表名 MODIFY 字段名 数据类型 UNIQUE;
  ```

* 删除约束

  ```sql
  ALTER TABLE 表名 DROP INDEX 字段名;
  ```

### 主键约束

> **概念：** 主键是一行数据的唯一标识，要求非空且唯一
>
> **一张表只能有一个主键，但可以由多个字段组成联合主键，满足整体非空唯一**

**语法：**

* 添加约束

  **AUTO_INCREMENT：** 主键自动增长

  ```sql
  -- 创建表时添加主键约束
  CREATE TABLE 表名(
      列名 数据类型 PRIMARY KEY [AUTO_INCREMENT],
      …
  ); 
  -- 建表末尾添加主键约束
  CREATE TABLE 表名(
      列名 数据类型,
      CONSTRAINT 约束名称 PRIMARY KEY(字段名,...)
  ); 
  ```

  ```sql
  -- 建完表后添加主键约束
  ALTER TABLE 表名 ADD PRIMARY KEY(列名,...);
  ```

* 删除约束

  ```sql
  ALTER TABLE 表名 DROP PRIMARY KEY;
  ```

### 默认约束

> **概念：** 保存数据时，未指定值则采用默认值

**语法：**

* 添加约束

  ```sql
  -- 创建表时添加默认约束
  CREATE TABLE 表名(
      列名 数据类型 DEFAULT 默认值,
      …
  ); 
  ```

  ```sql
  -- 建完表后添加默认约束
  ALTER TABLE 表名 ALTER 列名 SET DEFAULT 默认值;
  ```

* 删除约束

  ```sql
  ALTER TABLE 表名 ALTER 列名 DROP DEFAULT;
  ```

### 检查约束

> **概念：** 保存数据时，对数据进行检查

**语法：**

- 添加约束

  ```mysql
  -- 创建表时添加检查约束
  CREATE TABLE 表名(
      列名 数据类型 CHECK(检查条件),
      …
  ); 
  -- 建表末尾添加检查约束
  CREATE TABLE 表名(
  	列名 数据类型,
      …
  	CONSTRAINT 约束名 CHECK(检查条件)
  );
  ```

  ```mysql
  -- 建完表后添加检查约束
  ALTER TABLE 表名 ADD CHECK(检查条件);
  ```

- 删除约束

  ```mysql
  ALTER TABLE 表名 DROP CONSTRAINT 约束名;
  ```

### 外键约束

#### 概述

> 外键用来让两个表的数据之间建立链接，保证数据的一致性和完整性。

比如员工表中有员工所属的部门id，部门表中有各部门及其id，此时员工表中的部门id和部门表中的部门id是有联系的（逻辑上的）。

如果删除了部门表中的某一个部门id，可能会导致员工表中的部门id出现错误，

所以需要通过外键让这两张表产生数据库层面的关系（物理层面的联系），这样只删除部门表中的1号部门时，数据将无法删除。

#### 语法

**添加外键约束：**

```sql
-- 创建表时添加外键约束
CREATE TABLE 表名(
    列名 数据类型,
    …
    CONSTRAINT 外键名称 FOREIGN KEY(外键列名) REFERENCES 主表(主表列名) 
); 
```

```sql
-- 建完表后添加外键约束
ALTER TABLE 表名 ADD CONSTRAINT 外键名称 FOREIGN KEY (外键字段名称) REFERENCES 主表名称(主表列名称);
```

**删除外键约束：**

```sql
ALTER TABLE 表名 DROP FOREIGN KEY 外键名称;
```

#### 案例

**创建员工表和部门表，并添加上外键约束：**

```sql
-- 删除表
DROP TABLE IF EXISTS emp;
DROP TABLE IF EXISTS dept;

-- 部门表
CREATE TABLE dept(
	id INT PRIMARY KEY AUTO_INCREMENT,
	dep_name VARCHAR(20),
	addr VARCHAR(20)
);
-- 员工表 
CREATE TABLE emp(
	id INT PRIMARY KEY AUTO_INCREMENT,
	name VARCHAR(20),
	age INT,
	dep_id INT,

	-- 添加外键 dep_id,关联 dept 表的id主键
	CONSTRAINT fk_emp_dept FOREIGN KEY(dep_id) REFERENCES dept(id)	
);
```

![image-20220425150556745](https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-master/image-20220425150556745.png)

![image-20220425150618211](https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-master/image-20220425150618211.png)

**添加数据：**

```sql
-- 添加 2 个部门
insert into dept(dep_name,addr) values
('研发部','广州'),('销售部', '深圳');

-- 添加员工,dep_id 表示员工所在的部门
INSERT INTO emp (name, age, dep_id) VALUES 
('张三', 20, 1),
('李四', 20, 1),
('王五', 20, 1),
('赵六', 20, 2),
('孙七', 22, 2),
('周八', 18, 2);
```

![image-20220425150732866](https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-master/image-20220425150732866.png)

**此时删除 `研发部` 这条数据，会发现无法删除：**

```sql
DELETE FROM emp WHERE dep_name = '研发部';
```

![image-20220425151056508](https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-master/image-20220425151056508.png)



#### 级联

> MySQL利用外键级联删除、更新
>
> MySql支持外键的存储引擎只有InnoDB
>
> 在创建外键的时候，要求父表必须有对应的索引，子表在创建外键的时候也会自动创建对应的索引。

|    行为     |                             说明                             |
| :---------: | :----------------------------------------------------------: |
| NOT ACTION  | 当在父表中删除/更新对应记录时，首先检查该记录是否有对应外键，如果有则不允许删除/更新。与RESTRICT一致) 默认行为 |
|  RESTRICT   | 当在父表中删除/更新对应记录时，首先检查该记录是否有对应外键，如果有则不允许删除/更新。(与NO ACTION一致) 默认行为 |
|   CASCADE   | 当在父表中删除/更新对应记录时，首先检查该记录是否有对应外键，如果有，则也删除/更新外键在子表中的记录。 |
|  SET NULL   | 当在父表中删除对应记录时，首先检查该记录是否有对应外键，如果有则设置子表中该外键值为null（这就要求该外键允许取null）。 |
| SET DEFAULT | 父表有变更时，子表将外键列设置成一个默认的值 (MySQL默认引擎Innodb不支持) |

**语法：**

```mysql
ALTER TABLE 表名 ADD CONSTRAINT 外键名称 FOREIGN KEY(外键列名) REFERENCES 主表名(主表列名) [ON DELETE 行为名] [ON UPDATE 行为名];
```

**案例：**

```mysql
-- 主表dept中id删除和更新时，同时删除/更新子表emp的外键值demt_id
ALTER TABLE emp	ADD CONSTRAINT fk_emp_dept_id FOREIGN KEY (dept_id) REFERENCES dept(id) ON DELETE CASCADE ON UPDATE CASCADE;

-- 主表dept中id删除和更新时，同时设置子表emp的外键值demt_id为NULL
ALTER TABLE emp	ADD CONSTRAINT fk_emp_dept_id FOREIGN KEY (dept_id) REFERENCES dept(id) ON DELETE CASCADE ON UPDATE CASCADE;
```

## 多表查询

### 简介

> 多表查询（笛卡尔查询）：从多张表中一次性的查询出想要的数据

**语法：**

```sql
SELECT * FROM 表1, 表2;

-- 例：
SELECT * FROM students, classes;
```

- **两表分别数据：**

![image-20220425174923021](https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-master/image-20220425174923021.png)

- **一次性查询：**

![image-20220425175030328](https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-master/image-20220425175030328.png)

结果集仍是二维表，为两张表的行与列两两拼在一起返回（有A,B两个集合，会 **取尽A,B所有的组合情况** ），所以列数是两表列之积，行为两表行数之积，其结果可能会非常大。

**多表查询主要操作就是消除无效数据**

### 分类

**笛卡尔积：** 取A,B两个集合的所有组合情况

**多表查询分类：**

- **连接查询：**
  - 内连接：相当于查询A,B交集数据
  - 外连接：
    - 左外连接：相当于查询A表所有数据和交集部分数据
    - 右外连接：相当于查询B表所有数据和交集部分数据
- **子查询**

### 内连接

> **内连接：相当于查询A,B交集数据**

**语法：**

```sql
-- 隐式内连接
SELECT 字段列表 FROM 表1,表2… WHERE 条件;

-- 显示内连接
SELECT 字段列表 FROM 表1 INNER JOIN 表2 ON 条件;
```

**案例：**

- 隐式内连接：

  ```sql
  -- 可以给表起别名，但起完别名不能再用原名，因为DQL语句FROM先执行
  SELECT
  	t1.name,
  	t1.gender,
  	t2.dname
  FROM
  	emp t1,
  	dept t2
  WHERE
  	t1.dep_id = t2.did;
  ```

- 显示内连接：

  ```sql
  SELECT 
  	emp.name, 
  	emp.gender, 
  	dept.dname 
  FROM 
  	emp 
  	INNER JOIN dept ON emp.dep_id = dept.did;
  ```

![image-20220425180405344](https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-master/image-20220425180405344.png)

### 外连接

> **左外连接：相当于查询A表所有数据和AB交集部分数据**
>
> **右外连接：相当于查询B表所有数据和AB交集部分数据**

**语法：**

```sql
-- 左外连接
SELECT 字段列表 FROM 表1 LEFT OUTER JOIN 表2 ON 条件;

-- 右外连接
SELECT 字段列表 FROM 表1 RIGHT OUTER JOIN 表2 ON 条件;
```

**案例：**

- 查询emp表所有数据和对应的部门信息（左外连接）：

```sql
SELECT * FROM emp LEFT OUTER JOIN dept ON emp.dep_id = dept.did;
```

![image-20220425180741076](https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-master/image-20220425180741076.png)

结果显示查询到了 **左表（emp）** 中所有的数据及两张表能关联的数据（如图，尽管小白龙没有关联 **右表(dept)** ，但还是查出来了）

- 查询dept表所有数据和对应的员工信息（右外连接）:

```sql
SELECT * FROM emp RIGHT OUTER JOIN dept ON emp.dep_id = dept.did;
```

![image-20220425181045343](https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-master/image-20220425181045343.png)

结果显示查询到了右表（dept）中所有的数据及两张表能关联的数据（如图，没和 **右表(dept)** 关联的小白龙没有查出来，但没有和 **左表(emp)** 关联的销售部查出来了）

### 自连接

> 概念：查询中单张表自己和自己连接
>
> 虽然查询的是同一张表，但可以看成对两张一样的表进行查询
>
> 必须使用别名

**案例：**

```mysql
-- 查询员工及其所属领导的名字
SELECT a.name, b.name FROM emp a, emp b WHERE a.manager_id = b.id;
-- 查询员工及其所属领导的名字，如果员工没有领导，也需要查询出来
SELECT a.name, b.name FROM emp a LEFT OUTER JOIN emp b ON a.manager_id = b.id;
```

### 联合查询

> 概念：把多次查询的结果合并起来，形成一个新的查询结果集
>
> **联合查询要求多张表的列数必须保持一直，字段类型也需要保持一致**
>
> UNION ALL 会将所有数据直接合并在一起
>
> UNION 会将数据去重后合并

**语法：**

```mysql
SELECT 字段列表 FROM 表A ...
UNION [ALL]
SELECT 字段列表 FROM 表B ...;
```

**案例：**

```mysql
-- 将薪资低于5000的员工,和年龄大于50岁的员工全部查询出来
SELECT * FROM emp WHERE salary < 5000
UNION ALL -- 添加ALL将不合并重复记录
SELECT * FROM emp WHERE age > 50;
```

如图，1~5条为薪资低于5000的员工记录，6~9条为年龄大于50岁的员工记录，联合查询直接合并两个记录

![image-20221026210315919](https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-masterimage-20221026210315919.png)

## 子查询

> 概念：查询中嵌套查询，称嵌套查询为子查询
>
> 子查询根据查询结果不同，作用不同：
>
>  * **单行单列：** 子查询语句作为条件值，使用 `= > < `等进行条件判断
>
>    ```sql
>    SELECT 字段列表 FROM 表 WHERE 字段名 = (子查询);
>    ```
>
>  * **多行单列：** 子查询语句作为条件值，使用`IN`等关键字进行条件判断
>
>    ```sql
>    SELECT 字段列表 FROM 表 WHERE (字段列表) IN (子查询);
>    ```
>
>  * **多行多列：** 子查询语句作为虚拟表
>
>    ```sql
>    SELECT 字段列表 FROM (子查询) WHERE 条件;
>    ```
>
> 分类：
>
> - 标量子查询（子查询结果为单个值）
> - 列子查询（子查询结果为一列）
> - 行子查询（子查询结果为一行）
> - 表子查询（子查询为多行多列）

**例如，查询工资A高于员工B的员工信息：**

- 查询员工B的工资

```sql
SELECT salary FROM emp WHERE name = 'B'
```

- 查询工资高于员工B员工信息

```sql
SELECT * FROM emp WHERE salary > 3600;
```

**两个需求合二为一即子查询：**

```sql
SELECT * FROM emp WHERE salary > (SELECT salary FROM emp WHERE name = 'B');
```

![image-20220425182033357](https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-master/image-20220425182033357.png)

### 标量子查询

> 子查询返回的结果是单个值（数字、字符串、日期等），最简单的形式，这种子查询称为标量子查询。
>
> 常用的操作符：`=`、 `<>`、 `>`、 `>=`、 `<`、 `<=` 

```mysql
-- 查询在"方东白"入职之后的员工信息
SELECT *
FROM emp
WHERE entrydate > (
	SELECT entrydate
	FROM emp
	WHERE name = '方东白'
);
```

### 列子查询

> 子查询返回的结果是一列（可以是多行），这种子查询称为列子查询。
>
> 常用的操作符：`IN`、`NOT IN`、`ANY`、`SOME`、`ALL`
>
> | 操作符 |                  描述                  |
> | :----: | :------------------------------------: |
> |   IN   |       在指定的集合范围内，多选一       |
> | NOT IN |          不在指定的集合范围内          |
> |  ANY   |  子查询返回列表中，有任意一个满足即可  |
> |  SOME  | 与ANY等同，使用SOME的地方都可以使用ANY |
> |  ALL   |    子查询返回列表的所有值都必须满足    |

```mysql
-- 查询比研发部其中任意一人工资高的员工信息
SELECT *
FROM emp
WHERE salary > ANY (
	SELECT salary
	FROM emp
	WHERE dept_id = (
		SELECT id
		FROM dept
		WHERE name = '研发部'
	)
);
```

### 行子查询

> 子查询返回的结果是一行（可以是多列），这种子查询称为行子查询。
>
> 常用的操作符：`=` 、`<>` 、`IN` 、`NOT IN`

```mysql
-- 查询与"张无忌"的薪资及直属领导相同的员工信息
SELECT *
FROM emp
WHERE (salary, managerid) = (
	SELECT salary, managerid
	FROM emp
	WHERE name = '张无忌'
);
```

### 表子查询

> 子查询返回的结果是多行多列，这种子查询称为表子查询。
>
> 常用的操作符：`IN`
>
> 每一个派生表必须起别名

```mysql
-- 查询与 "鹿杖客" , "宋远桥" 的职位和薪资相同的员工信息
SELECT *
FROM emp
WHERE (job, salary) IN (
	SELECT job, salary
	FROM emp
	WHERE name = '鹿杖客' OR name = '宋远桥'
);


-- 查询入职日期是 "2006-01-01" 之后的员工信息, 及其部门信息
-- 子查询结果集别名e，部门表别名d
SELECT e.*, d.*
FROM (
	SELECT *
	FROM emp
	WHERE entrydate > '2006-01-01'
) e
	LEFT OUTER JOIN dept d ON e.dept_id = d.id;
```

## 数据库设计

### 简介

**软件的研发步骤：**

<img src="https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-master/image-20210724130925801.png" alt="image-20210724130925801" style="zoom:80%;" />

**数据库设计概念：**

* 数据库设计就是根据业务系统的具体需求，结合我们所选用的DBMS(数据库管理系统)，为这个业务系统构造出最优的数据存储模型。
* 建立数据库中的表结构以及表与表之间的关联关系的过程。
* 有哪些表？表里有哪些字段？表和表之间有什么关系？

**数据库设计的步骤：**

* **需求分析：**（数据是什么? 数据具有哪些属性? 数据与属性的特点是什么）

* **逻辑分析：**（通过ER图对数据库进行逻辑建模，不需要考虑我们所选用的数据库管理系统）

  ER(Entity/Relation)图：

  <img src="https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-master/image-20210724131210759.png" alt="image-20210724131210759" style="zoom:80%;" />

* **物理设计：**（根据数据库自身的特点把逻辑设计转换为物理设计）

* **维护设计：**（1.对新的需求进行建表；2.表优化）

**表关系：**

* **一对一**
  * 如：用户 和 用户详情
  * 一对一关系多用于表拆分，将一个实体中经常使用的字段放一张表，不经常使用的字段放另一张表，用于提升查询性能

* **一对多**
  * 如：部门 和 员工

  * 一个部门对应多个员工，一个员工对应一个部门。

* **多对多**
  * 如：商品 和 订单
  * 一个商品对应多个订单，一个订单包含多个商品。

### 一对一关系

> **一对一**
>
> * 如：用户 和 用户详情
> * 一对一关系多用于表拆分，将一个实体中经常使用的字段放一张表，不经常使用的字段放另一张表，用于提升查询性能

**实现方式：** 在任意一方加入外键，关联另一方主键，并且设置外键为`UNIQUE`

```sql
CREATE TABLE tb_user_desc (
	id INT PRIMARY KEY auto_increment,
	city VARCHAR(20),
	edu VARCHAR(10),
	income INT,
	status CHAR(2),
	des VARCHAR(100)
);

create table tb_user (
	id INT PRIMARY KEY auto_increment,
	photo VARCHAR(100),
	nickname VARCHAR(50),
	age INT,
	gender CHAR(1),
	desc_id INT UNIQUE,
	-- 添加外键
	CONSTRAINT fk_user_desc FOREIGN KEY(desc_id) REFERENCES tb_user_desc(id)	
);
```

![image-20220425154555757](https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-master/image-20220425154555757.png)

### 一对多关系

> **一对多**
>
> * 如：部门 和 员工
> * 一个部门对应多个员工，一个员工对应一个部门。

**实现方式：** 在多的一方建立外键，指向一的一方的主键

```sql
-- 部门表
CREATE TABLE dept(
    -- 部门表是一，id为组建
	id INT PRIMARY KEY auto_increment,
	dep_name VARCHAR(20),
	addr VARCHAR(20)
);
-- 员工表 
CREATE TABLE emp(
	id INT PRIMARY KEY auto_increment,
	name VARCHAR(20),
	age INT,
	dep_id INT,

	-- 员工表是多，建立外键，dep_id，关联dept表的id主键
	CONSTRAINT fk_emp_dept FOREIGN KEY(dep_id) REFERENCES dept(id)	
);
```

![image-20220425152957367](https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-master/image-20220425152957367.png)

### 多对多关系

> **多对多**
>
> * 如：商品 和 订单
> * 一个商品对应多个订单，一个订单包含多个商品。

**实现方式：** 建立第三张中间表，中间表至少包含两个外键，分别关联双方的主键

```sql
-- 订单表
CREATE TABLE tb_order(
    -- 订单表的主键
    id INT PRIMARY KEY auto_increment,
	payment DOUBLE(10,2),
	payment_type TINYINT,
	status TINYINT
);

-- 商品表
CREATE TABLE tb_goods(
    -- 商品表的主键
	id INT PRIMARY KEY auto_increment,
	title VARCHAR(100),
	price DOUBLE(10,2)
);

-- 订单商品中间表
CREATE TABLE tb_order_goods(
	id INT PRIMARY KEY auto_increment,
	-- 订单表id，需要作为外键关联到订单表主键
    order_id INT,
    -- 商品表id，需要作为外键关联到商品表主键
	goods_id INT,
    -- 在订单表中可以存放当前商品购买的数量
	count INT
);

-- 建完表后，添加外键
ALTER TABLE tb_order_goods ADD CONSTRAINT fk_order_id FOREIGN KEY(order_id) REFERENCES tb_order(id);
ALTER tb_order_goods ADD CONSTRAINT fk_goods_id FOREIGN KEY(goods_id) REFERENCES tb_goods(id);
```

![image-20220425153849088](https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-master/image-20220425153849088.png)

**场景：** 如果用户需要创建一个订单，选中3个商品，只需要在中间表中添加3条记录，每条记录记录下用户id和选中的商品id以及商品数量即可。

## 事务

### 简介

> 数据库的事务（Transaction）是一种机制、一个操作序列，包含了一组数据库操作命令
>
> 事务把所有的命令作为一个整体一起向系统提交或撤销操作请求，即这一组数据库命令要么同时成功，要么同时失败
>
> 事务是不可分割的工作逻辑单元

**在执行SQL语句的时候，某些业务要求，一系列操作必须全部执行，而不能仅执行一部分。**

例如，一个转账操作：

```sql
-- 从id=1的账户给id=2的账户转账100元
-- 第一步：将id=1的A账户余额减去100
UPDATE accounts SET balance = balance - 100 WHERE id = 1;
-- 第二步：将id=2的B账户余额加上100
UPDATE accounts SET balance = balance + 100 WHERE id = 2;
```

这两条SQL语句必须全部执行，或者，由于某些原因，如果第一条语句成功，第二条语句失败，就必须全部撤销。

**这种把多条语句作为一个整体进行操作的功能，被称为数据库事务。**

数据库事务可以确保该事务范围内的所有操作都可以 **全部成功** 或者 **全部失败** 。如果事务失败，那么效果就和没有执行这些SQL一样，不会对数据库数据有任何改动。

### 四大特性

**数据库事务具有ACID这4个特性：**

- **A：Atomic 原子性** 事务是不可分割的最小操作单元，要么全部执行，要么全部不执行；
- **C：Consistent 一致性** 事务完成后，所有数据的状态都是一致的，即A账户只要减去了100，B账户则必定加上了100；
- **I：Isolation 隔离性** 如果有多个事务并发执行，同时操作一批数据，每个事务作出的修改必须与其他事务隔离，隔离性越强，即看不见别的事务对数据的修改，同时性能越低；
- **D：Duration 持久性** 即事务完成后，对数据库数据的修改被持久化存储。

### 语法

**隐性事务：** 对于单条SQL语句，数据库系统自动将其作为一个事务执行，这种事务被称为隐式事务。

**显性事务：** 要手动把多条SQL语句作为一个事务执行，使用`BEGIN`开启一个事务，使用`COMMIT`提交一个事务，这种事务被称为显式事务

```sql
-- 开启事务，也可以使用BEGIN
START TRANSACTION;

-- 提交事务
COMMIT;
-- 回滚事务
ROLLBACK;
```

例如，把上述的转账操作作为一个显式事务：

```sql
BEGIN;
UPDATE accounts SET balance = balance - 100 WHERE id = 1;
UPDATE accounts SET balance = balance + 100 WHERE id = 2;
COMMIT;

-- COMMIT是指提交事务，即试图把事务内的所有SQL所做的修改永久保存。如果COMMIT语句执行失败了，整个事务也会失败。

-- 有些时候，我们希望主动让事务失败，这时，可以用ROLLBACK回滚事务，整个事务会失败:
BEGIN;
UPDATE accounts SET balance = balance - 100 WHERE id = 1;
UPDATE accounts SET balance = balance + 100 WHERE id = 2;
ROLLBACK;
```

### 并发事务问题

#### 脏读

**读取未提交数据**

A事务读取B事务尚未提交的数据，此时如果B事务发生错误并执行回滚操作，那么A事务读取到的数据就是脏数据。

就好像原本的数据比较干净、纯粹，此时由于B事务更改了它，这个数据变得不再纯粹。这个时候A事务立即读取了这个脏数据，但事务B良心发现，又用回滚把数据恢复成原来干净、纯粹的样子，而事务A却什么都不知道，最终结果就是事务A读取了此次的脏数据，称为脏读。

这种情况常发生于转账与取款操作中：

| 时间顺序 |                     转账事务                      |                     取款事务                     |
| :------: | :-----------------------------------------------: | :----------------------------------------------: |
|    1     |                                                   |                     开始事务                     |
|    2     |                     开始事务                      |                                                  |
|    3     |                                                   |               查询账户余额为2000元               |
|    4     |                                                   |          取款1000元，余额被更改为1000元          |
|    5     |               查询账户余额为1000元                |                                                  |
|    6     |                                                   | 取款操作发生未知错误，事务回滚，余额变更为2000元 |
|    7     | 转入2000元，余额被更改为3000元（脏读的1000+2000） |                                                  |
|    8     |                     提交事务                      |                                                  |
|   备注   |      按照正确逻辑，此时账户余额应该为4000元       |                                                  |

####  不可重复读

**前后多次读取，数据内容不一致**

**不可重复读强调 数据的修改**

事务A在执行读取操作，由整个事务A比较大，前后读取同一条数据需要经历很长的时间。

而在事务A第一次读取数据，比如此时读取了小明的年龄为20岁，事务B执行更改操作，将小明的年龄更改为30岁，此时事务A第二次读取到小明的年龄时，发现其年龄是30岁，和之前的数据不一样了，也就是数据不重复了，系统不可以读取到重复的数据，称为不可重复读。

| 时间顺序 |                      事务A                      |        事务B         |
| :------: | :---------------------------------------------: | :------------------: |
|    1     |                    开始事务                     |                      |
|    2     |          第一次查询，小明的年龄为20岁           |                      |
|    3     |                                                 |       开始事务       |
|    4     |                    其他操作                     |                      |
|    5     |                                                 | 更改小明的年龄为30岁 |
|    6     |                                                 |       提交事务       |
|    7     |          第二次查询，小明的年龄为30岁           |                      |
|   备注   | 按照正确逻辑，事务A前后两次读取到的数据应该一致 |                      |

#### 幻读

事务A在执行读取操作，需要两次统计数据的总量，前一次查询数据总量后，此时事务B执行了新增数据的操作并提交后，这个时候事务A读取的数据总量和之前统计的不一样，就像产生了幻觉一样，平白无故的多了几条数据，成为幻读。

**幻读强调 数据的增删**

| 时间顺序 |                        事务A                        |     事务B     |
| :------: | :-------------------------------------------------: | :-----------: |
|    1     |                      开始事务                       |               |
|    2     |             第一次查询，数据总量为100条             |               |
|    3     |                                                     |   开始事务    |
|    4     |                      其他操作                       |               |
|    5     |                                                     | 新增100条数据 |
|    6     |                                                     |   提交事务    |
|    7     |             第二次查询，数据总量为200条             |               |
|   备注   | 按照正确逻辑，事务A前后两次读取到的数据总量应该一致 |               |

### 事务隔离级别

#### 分类

对于两个并发执行的事务，如果涉及到操作同一条记录的时候，可能会发生问题。因为并发操作会带来数据的不一致性，包括**脏读、不可重复读、幻读** 等。数据库系统提供了隔离级别来让我们有针对性地选择事务的隔离级别，避免数据不一致的问题。

​	SQL标准定义了4种隔离级别，分别对应可能出现的数据不一致的情况：

| Isolation Level  | 脏读（Dirty Read） | 不可重复读（Non Repeatable Read） | 幻读（Phantom Read） |
| :--------------: | :----------------: | :-------------------------------: | :------------------: |
| Read Uncommitted |        Yes         |                Yes                |         Yes          |
|  Read Committed  |         -          |                Yes                |         Yes          |
| Repeatable Read  |         -          |                 -                 |         Yes          |
|   Serializable   |         -          |                 -                 |          -           |

##### Read Uncommitted

​	Read Uncommitted是隔离级别最低的一种事务级别。

​	**读未提交，事务A可以读到事务B没有提交的数据；一个事务未提交，其变更对其他事物可见**

​	在这种隔离级别下，一个事务会读到另一个事务更新后但未提交的数据，如果另一个事务回滚，那么当前事务读到的数据就是脏数据，这就是脏读（Dirty Read）

##### Read Committed

​	**读已提交，事务A可以读到事务B已经提交的数据；一个事务未提交，其变更对其他事务不可见**

​	在Read Committed隔离级别下，一个事务可能会遇到不可重复读（Non Repeatable Read）的问题。不可重复读是指，在一个事务内，多次读同一数据，在这个事务还没有结束时，如果另一个事务恰好修改了这个数据，那么，在第一个事务中，两次读取的数据就可能不一致。

##### Repeatable Read

​	**可重复读，事务A执行过程中的数据是一致的；未提交的变更对其他事物不可见**

​	在Repeatable Read隔离级别下，一个事务可能会遇到幻读（Phantom Read）的问题。

​	幻读是指，在一个事务中，第一次查询某条记录，发现没有，但是，当试图更新这条不存在的记录时，竟然能成功，并且，再次读取同一条记录，它就神奇地出现了。

​	**可重复读保证了数据的修改不会影响当前事务读到的数据，但对于数据增删没有要求，所以如果有数据增删了，就会出现幻读**

##### Serializable

​	**串行化，对应一个记录会加读写锁，出现冲突时，后访问的事务必须等前一个事物执行完成才能继续执行**

​	Serializable是最严格的隔离级别。在Serializable隔离级别下，所有事务按照次序依次执行，因此，脏读、不可重复读、幻读都不会出现。

​	虽然Serializable隔离级别下的事务具有最高的安全性，但是，由于事务是串行执行，所以效率会大大下降，应用程序的性能会急剧降低。如果没有特别重要的情景，一般都不会使用Serializable隔离级别。

#### 查看事务隔离级别

```mysql
SELECT @@TRANSACTION_ISOLATION;
```

#### 设置事务隔离级别

```mysql
SET 作用范围 TRANSACTION ISOLATION LEVEL 事务隔离级别
-- session，会话级别，当前客户端窗口有效
-- global，所有客户端会话窗口有效
SET [ SESSION | GLOBAL ] TRANSACTION ISOLATION LEVEL { READ UNCOMMITTED | READ COMMITTED | REPEATABLE READ | SERIALIZABLE }
```

## 其他语句

### 插入或替换

​	如果我们希望插入一条新记录（INSERT），但如果记录已经存在，就先删除原记录，再插入新记录。此时，可以使用`REPLACE`语句，这样就不必先查询，再决定是否先删除再插入：

```sql
REPLACE INTO students (id, class_id, name, gender, score) VALUES (1, 1, '小明', 'F', 99);
```

​	若`id=1`的记录不存在，`REPLACE`语句将插入新记录，否则，当前`id=1`的记录将被删除，然后再插入新记录。

### 插入或更新

​	如果我们希望插入一条新记录（INSERT），但如果记录已经存在，就更新该记录，此时，可以使用`INSERT INTO ... ON DUPLICATE KEY UPDATE ...`语句：

```sql
INSERT INTO students (id, class_id, name, gender, score) VALUES (1, 1, '小明', 'F', 99) ON DUPLICATE KEY UPDATE name='小明', gender='F', score=99;
```

​	若`id=1`的记录不存在，`INSERT`语句将插入新记录，否则，当前`id=1`的记录将被更新，更新的字段由`UPDATE`指定。

### 插入或忽略

​	如果我们希望插入一条新记录（INSERT），但如果记录已经存在，就啥事也不干直接忽略，此时，可以使用`INSERT IGNORE INTO ...`语句：

```sql
INSERT IGNORE INTO students (id, class_id, name, gender, score) VALUES (1, 1, '小明', 'F', 99);
```

​	若`id=1`的记录不存在，`INSERT`语句将插入新记录，否则，不执行任何操作。

### 快照

​	如果想要对一个表进行快照，即复制一份当前表的数据到一个新表，可以结合`CREATE TABLE`和`SELECT`：

```sql
-- 对class_id=1的记录进行快照，并存储为新表students_of_class1:
CREATE TABLE students_of_class1 SELECT * FROM students WHERE class_id=1;
```

​	新创建的表结构和`SELECT`使用的表结构完全一致。

### 写入查询结果集

​	如果查询结果集需要写入到表中，可以结合`INSERT`和`SELECT`，将`SELECT`语句的结果集直接插入到指定表中。

​	例如，创建一个统计成绩的表`statistics`，记录各班的平均成绩：

```sql
CREATE TABLE statistics (
    id BIGINT NOT NULL AUTO_INCREMENT,
    class_id BIGINT NOT NULL,
    average DOUBLE NOT NULL,
    PRIMARY KEY (id)
);
```

​	然后，我们就可以用一条语句写入各班的平均成绩：

```sql
INSERT INTO statistics (class_id, average) SELECT class_id, AVG(score) FROM students GROUP BY class_id;
```

​	确保`INSERT`语句的列和`SELECT`语句的列能一一对应，就可以在`statistics`表中直接保存查询的结果：

```sql
> SELECT * FROM statistics;
+----+----------+--------------+
| id | class_id | average      |
+----+----------+--------------+
|  1 |        1 |         86.5 |
|  2 |        2 | 73.666666666 |
|  3 |        3 | 88.333333333 |
+----+----------+--------------+
3 rows in set (0.00 sec)
```

### 强制使用指定索引

​	在查询的时候，数据库系统会自动分析查询语句，并选择一个最合适的索引。但是很多时候，数据库系统的查询优化器并不一定总是能使用最优索引。如果我们知道如何选择索引，可以使用`FORCE INDEX`强制查询使用指定的索引。例如：

```sql
> SELECT * FROM students FORCE INDEX (idx_class_id) WHERE class_id = 1 ORDER BY id DESC;
```

​	指定索引的前提是索引`idx_class_id`必须存在。

# 【MySQL进阶篇】

## 存储引擎

### MySQL体系结构

![image-20221027090054535](https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-masterimage-20221027090054535.png)

- **连接层**

​		最上层是一些客户端和链接服务，包含本地sock通信和大多数基于客户端/服端工具实现的类似于TCP/IP的通信。主要完成一些类似于连接处理、授权认证、及相关的安全方案。在该层上引入了线程池的概念，为通过认证安全接入的客户端提供线程。同样在该层上可以实现基于SSL的安全链接。服务器也会为安全接入的每个客户端验证它所具有的操作权限。

- **服务层**

​		第二层架构主要完成大多数的核心服务功能，如SQL接口，并完成缓存的查询，SQL的分析和优化，部分内置函数的执行。所有跨存储引擎的功能也在这一层实现，如过程、函数等。在该层，服务器会解析查询并创建相应的内部解析树，并对其完成相应的优化如确定表的查询的顺序，是否利用索引等，最后生成相应的执行操作。如果是SELECT语句，服务器还会查询内部的缓存，如果缓存空间足够大，这样在解决大量读操作的环境中能够很好的提升系统的性能。

- **引擎层**

​		存储引擎层，存储引擎真正的负责了MySQL中数据的存储和提取，服务器通过API和存储引擎进行通信。不同的存储引擎具有不同的功能，这样我们可以根据自己的需要，来选取合适的存储引擎。 **数据库中的索引是在存储引擎层实现的，固不同的存储引擎，索引结构不同。**

- **存储层**

​		数据存储层，主要是将数据(如: redolog、undolog、数据、索引、二进制日志、错误日志、查询日志、慢查询日志等)存储在文件系统之上，并完成与存储引擎的交互。和其他数据库相比，MySQL有点与众不同，它的架构可以在多种不同场景中应用并发挥良好作用。主要体现在存储引擎上，插件式的存储引擎架构，将查询处理和其他的系统任务以及数据的存储提取分离。这种架构可以根据业务的需求和实际需要选择合适的存储引擎。

### 简介

> 存储引擎是MySQL数据库的核心。
>
> 存储引擎就是存储数据、建立索引、更新/查询数据等技术的实现方式。存储引擎是基于表的，而不是基于库的，所以存储引擎也可被称为表类型。我们可以在创建表的时候，来指定选择的存储引擎，如果没有指定将自动选择默认的存储引擎。
>
> MySQL5.5版本后默认使用InnoDB引擎

- 建表时指定存储引擎

  ```mysql
  CREATE TABLE 表名(
  	字段1 字段1类型 [ COMMENT 字段1注释 ] ,
  	......
  	字段n 字段n类型 [COMMENT 字段n注释 ]
  ) ENGINE = 存储引擎 [ COMMENT 表注释 ] ;
  
  -- 案例
  CREATE TABLE my_myisam(
      id INT,
      name varchar(10)
  ) ENGINE = MyISAM COMMENT '设置存储引擎为MyISAM';
  ```

- 查询当前数据库支持的存储引擎

  ```mysql
  -- 查询建表语句(可以看到存储引擎)
  SHOW CREATE TABLE 表名;
  
  -- 查询当前数据库支持的存储引擎
  SHOW ENGINES;
  ```

  ![image-20221027091145914](https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-masterimage-20221027091145914.png)

### 种类

#### InnoDB

##### 简介

> InnoDB是一种兼顾高可靠性和高性能的通用存储引擎，在MySQL5.5之后，InnoDB是默认的MySQL存储引擎。

##### 特点

- DML操作遵循ACID模型，支持事务
- 行级锁，提高并发访问性能
- 支持外键FOREIGN KEY约束，保证数据的完整性和准确性

##### 文件

InnoDB引擎的每张表都会对应一个表空间文件(表名.ibd)，存储该表的结构(早期存储在frm，新版存储在sdi，sdi融入ibd文件中)、数据和索引。

参数：innodb_file_per_table (每一张表是否都对应一个表空间文件)

```mysql
-- 可以看到这个参数的value是on，固每一张表都对应一个表空间文件
SHOW VARIABLES LIKE 'innodb_file_per_table';
```

MySQL数据存放目录：`\MySQL\MySQL Server 8.0\Data`，不同文件夹代表不同的库，库中有表对应的表空间文件：

![image-20221027092539988](https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-masterimage-20221027092539988.png)

每一个ibd文件对应一张表，该文件基于二进制存储，使用MySQL提供的`ibd2sdi`指令可以从ibd文件中提取sid信息，sdi数据字典信息就包含该表的表结构。

![image-20221027092945239](https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-masterimage-20221027092945239.png)

##### 逻辑存储结构

![image-20221027093040719](https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-masterimage-20221027093040719.png)

- **表空间(TableSpace)：**

​		InnoDB存储引擎逻辑结构的最高层，ibd文件其实就是表空间文件，在表空间中可以包含多个Segment段。

- **段(Segment)：**

​		表空间是由各个段组成的，常见的段有数据段、索引段、回滚段等。InnoDB中对于段的管理，都是引擎自身完成，不需要人为对其控制，一个段中包含多个区。

- **区(Extent)：**

​		区是表空间的单元结构，每个区的大小为1M。默认情况下，InnoDB存储引擎页大小为16K，即一个区中一共有64个连续的页。

- **页(Page)：**

​		页是组成区的最小单元， **页也是InnoDB存储引擎磁盘管理的最小单元** ，每个页的大小默认为16KB。为了保证页的连续性，InnoDB存储引擎每次从磁盘申请 4-5个区。

- **行(Row)：**

​		InnoDB存储引擎是面向行的，也就是说数据是按行进行存放的，在每一行中除了定义表时所指定的字段以外，还包含两个隐藏字段。

#### MyISAM

##### 简介

> MyISAM是MySQL早期的默认存储引擎。

##### 特点

- 不支持事务，不支持外键
- 支持表锁，不支持行锁
- 访问速度快

##### 文件

![image-20221027093814517](https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-masterimage-20221027093814517.png)

- `xxx.sdi`：存储表结构信息
- `xxx.MYD`：存储数据
- `xxx.MYI`：存储索引

#### MEMORY

##### 简介

> MEMORY引擎的表数据时存储在内存中的，由于受到硬件问题、或断电问题的影响，只能将这些表作为临时表或缓存使用。

##### 特点

- 内存存放，访问速度快
- 会吃hash索引(默认)

##### 文件

- `xxx.sdi`：存储表结构信息

#### 总结

|     特点     |     InnoDB      | MyISAM | MEMORY |
| :----------: | :-------------: | :----: | :----: |
|   存储限制   |      64TB       |   有   |   有   |
|   事务安全   |      支持       |   -    |   -    |
|    锁机制    |      行锁       |  表锁  |  表锁  |
|  B+Tree索引  |      支持       |  支持  |  支持  |
|   Hash索引   |        -        |   -    |  支持  |
|   全文索引   | 支持(5.6版本后) |  支持  |   -    |
|   空间使用   |       高        |   低   |  N/A   |
|   内存使用   |       高        |   低   |  中等  |
| 批量插入速度 |       低        |   高   |   高   |
|   支持外键   |      支持       |   -    |   -    |

> **面试题:**
>
> InnoDB引擎与MyISAM引擎的区别?
>
> ①. InnoDB引擎, 支持事务, 而MyISAM不支持。
>
> ②. InnoDB引擎, 支持行锁和表锁(表锁由MySQL支持), 而MyISAM仅支持表锁, 不支持行锁。
>
> ③. InnoDB引擎, 支持外键, 而MyISAM是不支持的。
>
> 主要是上述三点区别，当然也可以从索引结构、存储限制等方面，更加深入的回答，具体参考如下官方文档：
>
> **https://dev.mysql.com/doc/refman/8.0/en/innodb-introduction.html**
>
> **https://dev.mysql.com/doc/refman/8.0/en/myisam-storage-engine.html**

### 存储引擎选择

在选择存储引擎时，应该根据应用系统的特点选择合适的存储引擎。对于复杂的应用系统，还可以根据实际情况选择多种存储引擎进行组合。

- **InnoDB：**

​		是Mysql的默认存储引擎，支持事务、外键。如果应用对事务的完整性有比较高的要求，在并发条件下要求数据的一致性，数据操作除了插入和查询之外，还包含很多的更新、删除操作，那么InnoDB存储引擎是比较合适的选择。

- **MyISAM：**

​		如果应用是以读操作和插入操作为主，只有很少的更新和删除操作，并且对事务的完整性、并发性要求不是很高，那么选择这个存储引擎是非常合适的。

​		比如业务系统中日志相关数据，电商平台中足迹和评论相关数据（现在一般被NoSQL数据库MongoDB取代）。

- **MEMORY:**

​		将所有数据保存在内存中，访问速度快，通常用于临时表及缓存。MEMORY的缺陷就是对表的大小有限制，太大的表无法缓存在内存中，而且无法保障数据的安全性（一般被NoSQL数据库Redis取代）。

## 索引

### 介绍

> 索引（index）是帮助MySQL高效获取数据的数据结构(有序)。
>
> 在数据之外，数据库系统还维护着满足特定查找算法的数据结构，这些数据结构以某种方式引用（指向）数据，这样就可以在这些数据结构上实现高级查找算法，这种数据结构就是索引。

### 演示

```mysql
-- 在云服务器上创建一个可以远程访问的MySQL用户，方便datagrip连接
CREATE user 'root'@'%' IDENTIFIED WITH mysql_native_password BY '1234';

-- 授予权限
GRANT ALL ON *.* TO 'root'@'%';
```

表结构及其数据：

![image-20221029081018438](https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-masterimage-20221029081018438.png)

需要执行的SQL语句：`SELECT * FROM user WHERE age = 45;`

- **无索引情况：**

![image-20221029081125341](https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-masterimage-20221029081125341.png)

无索引时，就需要从第一行开始扫描，一直扫描到最后一行，即全表扫描，性能很低。

- **有索引情况(二叉树为例子)**

针对于这张表建立了索引，假设索引结构就是二叉树，那么也就意味着，会对age这个字段建立一个二叉树的索引结构。

![image-20221029081310365](https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-masterimage-20221029081310365.png)		

此时进行查询时，只需要扫描三次就可以找到数据了，极大地提高了查询的效率。

PS：并非索引的真实结构

### 特点

|                           优势                            |                        劣势                        |
| :-------------------------------------------------------: | :------------------------------------------------: |
|           提高数据解锁的效率，降低数据的IO成本            |                  索引需要占用空间                  |
| 通过索引对数据进行排序，降低数据排序的成本，降低CPU的消耗 | 索引大大提高了查询效率，同时也降低了更新表的速度。 |

### 索引结构

#### 分类

MySQL的索引是在存储引擎层实现的，不同的存储索引有不同的索引结构，主要包含以下几种：

|          索引           |                             描述                             |
| :---------------------: | :----------------------------------------------------------: |
|     **B+Tree索引**      |         最常见的索引结构，大部分引擎都支持B+Tree索引         |
|      **Hash索引**       | 底层数据结构使用哈希表实现的，但不支持范围查询，只能进行精确匹配 |
|  **R-Tree(空间索引)**   | 空间索引是MyISAM引擎的一个特殊索引类型，主要用于地理空间数据类型，通常使用较少 |
| **Full-text(全文索引)** | 是一种通过建立倒排索引,快速匹配文档的方式。类似于Lucene,Solr,ES |

不同存储引擎对于索引结构的支持：

|          索引           |    InnoDB     | MyISAM | Memory |
| :---------------------: | :-----------: | :----: | :----: |
|     **B+Tree索引**      |     支持      |  支持  |  支持  |
|      **Hash索引**       |    不支持     | 不支持 |  支持  |
|  **R-Tree(空间索引)**   |    不支持     |  支持  | 不支持 |
| **Full-text(全文索引)** | 5.6版本后支持 |  支持  |  支持  |

平时所说的索引，如果没有明确指明，都是指B+树结构组织的索引

#### B+Tree索引

> 树是包含n（n为整数，大于0）个结点， n-1条边的有穷集。
>
> ![image-20221029085056694](https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-masterimage-20221029085056694.png)
>
> **特点：**
>
> - 每个结点或者无子结点或者只有有限个子结点；
> - 有一个特殊的结点,它没有父结点，称为根结点；
> - 每一个非根节点有且只有一个父结点；
> - 树里面没有环路
>
> **术语：**
>
> - 结点的度：一个结点含有的子结点个数
> - 树的度：一棵树中最大结点的度
> - 父结点：若一个结点含有子结点，则这个结点称为其子结点的父结点
> - 孩子：结点的子树的根
> - 深度：对于任意结点n，n的深度为从根到n的唯一路径长，其根结点的深度为0
> - 高度：对于任意结点n，n的高度为从n到一片树叶的最长路径长，所有树叶的高度为0
>
> **种类(部分)：**
>
> - 二叉树：每个节点最多含有两个子树的树称为二叉树；
> - 二叉查找树：首先它是一颗二叉树，若左子树不空，则左子树上所有结点的值均小于它的根结点的值；若右子树不空，则右子树上所有结点的值均大于它的根结点的值；左、右子树也分别为二叉排序树；
> - 满二叉树：叶节点除外的所有节点均含有两个子树的树被称为满二叉树；
> - 完全二叉树：如果一颗二叉树除去最后一层节点为满二叉树，且最后一层的结点依次从左到右分布
> - 平衡二叉树（AVL）：一棵空树或它的左右两个子树的高度差的绝对值不超过1，并且左右两个子树都是一棵平衡二叉树
>
> **多路查找树：** 每一个结点的孩子数可以多余两个，并且每一个结点可以存储多个元素，所有元素之间存在某种特定的排序关系
>
> - 2-3树：一个2结点包含一个元素和两个子结点，一个3结点包含一小一大两个元素和三个子结点
>
>   ![image-20221031093450518](https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-masterimage-20221031093450518.png)
>
> - 2-3-4树：一个4结点包含小中大三个元素和四个子结点
>

**B树：** 

B树是一种平衡的多路查找树，结点最大的孩子数目称为B树的阶。2-3树是3阶B树，2-3-4树是4阶B树

**一个m阶的B树具有如下属性：**

- 如果根结点不是叶子结点，则其至少有两棵子树
- 如果一个非根的分支结点都有k-1个元素和k个孩子，其中⌈m/2⌉≤k≤m，每一个叶子结点n都有k-1个元素，其中⌈m/2⌉≤k≤m（即一个结点上最多有m-1个关键字,m个子树）
- 所有叶子结点都位于同一层次
- 所有分支结点包含下列信息数据(n,A0,K1,A1,K2,A2,...,Kn,An)
  - Ki：关键字(key)，且Ki<Ki-1，(i=1,2,...,n-1)
  - Ai：指向子树根结点的指针，(i=0,1,...,n)，
    - Ai-1所指子树中所有结点的关键字均小于Ki
    - Ai所指子树中所有结点的关键字均大于Ki
  - n：关键字的个数（或n+1为子树的个数）

![image-20221031095217916](https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-masterimage-20221031095217916.png)

一棵最大度数为5(5阶)的B树，最大存储4个key，5个指针

![image-20221031095743766](https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-masterimage-20221031095743766.png)



**B+树：** 

B+树是B树的变体，也是一棵多路查找树

**一个m阶的B+树具有如下特点：**

- 树中每个结点至多有m个子结点(即至多有m-1个关键字)，非根节点关键字个数范围：⌈m/2⌉-1≤k≤m-1
- 所有的叶子结点中包含了全部元素的信息，及指向含这些元素记录的指针。且叶子结点本身依关键字的大小，自小而大顺序链接。
- 所有分支结点可以看成是索引，结点中仅含有其子树中的最大(或最小)关键字

![image-20221031141942123](https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-masterimage-20221031141942123.png)

相比于B树，B+树有以下三点区别：

- 所有的数据都会出现在叶子结点
- 叶子结点形成一个单向链表
- 非叶子结点仅仅起到索引数据的作用，具体的数据存放在叶子结点中

**MySQL索引数据结构对经典的B+Tree进行了优化。在原B+Tree的基础上，增加一个指向相邻叶子节点的链表指针，就形成了带有顺序指针的B+Tree（叶子结点形成环形链表），提高区间访问的性能，利于范围查询和排序。**

![image-20221031142818053](https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-masterimage-20221031142818053.png)

#### Hash索引

**结构：** 

哈希索引就是采用一定的hash算法，将键值换算成新的hash值，映射到对应的槽位上，然后存储在hash表中。

![image-20221031143023989](https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-masterimage-20221031143023989.png)

如果两个或多个键值映射到同一个槽位上，就产生了hash冲突(哈希碰撞)，可以通过链表解决。

![image-20221031143118169](https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-masterimage-20221031143118169.png)

**特点：**

- Hash索引只能用于对等比较(=,in)，不支持范围查询(between,>,<,...)
- 无法利用索引完成排序操作
- 查询效率高，通常只需要一次检索(前提是不出现hash碰撞)，效率通常要高于B+Tree

**存储引擎支持：**

在Mysql中，支持hash索引的是Memory存储引擎。而InnoDB中具有自适应hash功能，在特定情况下会跟回B+Tree索引自动构建成hash索引

### 索引分类

|   分类   |                         含义                         |              特点              |   关键字   |
| :------: | :--------------------------------------------------: | :----------------------------: | :--------: |
| 主键索引 |                针对表中主键创建的索引                |    默认自动创建，只能有一个    | `PRIMARY`  |
| 唯一索引 |           避免同一个表中某数据列中的值重复           | 添加约束时自动创建，可以有多个 |  `UNIQUE`  |
| 常规索引 |                   快速定位特定数据                   |           可以有多个           |            |
| 全文索引 | 全文索引查找的是文本中的关键字，而不是比较索引中的值 |           可以有多个           | `FULLTEXT` |

### 聚集索引&二级索引

**在InnoDB存储引擎中，根据索引的存储形式，可以分为以下两种：**

|             分类              |                            含义                            |         特点         |
| :---------------------------: | :--------------------------------------------------------: | :------------------: |
| **聚集索引(Clustered Index)** | 将数据存储与索引放到了一块，索引结构的叶子结点保存了行数据 | 必须有，且只能有一个 |
| **二级索引(Secondary Index)** | 将数据与索引分开存储，索引结构的叶子结点关联的是对应的主键 |     可以存在多个     |

聚集索引选取规则：

- 如果存在主键，主键索引就是聚集索引。
- 如果不存在主键，将使用第一个唯一（UNIQUE）索引作为聚集索引。

- 如果表没有主键，或没有合适的唯一索引，则InnoDB会自动生成一个rowid作为隐藏的聚集索引。

聚集索引和二级索引的具体结构如下：

![image-20221031144645054](https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-masterimage-20221031144645054.png)

- 聚集索引的叶子结点下挂的是这一行的数据
- 二级索引的叶子结点下面挂的是该字段对应的主键值



执行SQL语句时，具体的查找过程：

![image-20221031144837080](https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-masterimage-20221031144837080.png)

1. 由于是根据name字段进行查询，所以先根据name='Arm'到name字段的二级索引进行匹配查找。但是在二级索引中只能查找到Arm对应的主键值10
2. 由于查询返回的数据是*，所以根据主键值10，到聚集索引中查找到10对应的记录，最终找到10对应的行row
3. 最终拿到这一行的数据并返回

> 回表查询：这种先到二级索引中查找数据，找到主键值，然后再到聚集索引中根据主键值，获取数据的方式，就称之为回表查询。

### 语法

- **创建索引：**

```mysql
-- UNIQUE 创建唯一索引
-- FULLTEXT 创建全文索引

-- 单列索引：关联一个列
-- 联合索引：关联多列
CREATE [ UNIQUE | FULLTEXT ] INDEX 索引名 ON 表名(列名列表);
```

- **查看索引：**

```mysql
-- SHOW INDEX FROM 表名\G; 表太长会在命令行中变形，可以转换成键值显示
SHOW INDEX FROM 表名;
```

- **删除索引：**

```mysql
DROP INDEX 索引名 ON 表名;
```

**索引命名格式：** `idx_表名_列名`

**案例：**

```mysql
-- name字段为姓名字段，该字段的值可能会重复，为该字段创建索引。
CREATE INDEX idx_user_name ON tb_user(name);

-- phone手机号字段的值，是非空，且唯一的，为该字段创建唯一索引。
CREATE UNIQUE INDEX idx_user_phone ON tb_user(phone);

-- 为profession、age、status创建联合索引。
CREATE INDEX idx_user_pro_age_sta ON tb_user(profession,age,status);

-- 为email建立合适的索引来提升查询效率。
CREATE INDEX idx_user_email ON tb_user(email);

-- 删除idx_user_email索引
DROP INDEX idx_user_email ON tb_user;
```

![image-20221101091757458](https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-masterimage-20221101091757458.png)

### 验证索引效率

- 无索引查询

```mysql
-- 根据sn字段查询
SELECT * FROM tb_sku WHERE sn = '100000003145001';
-- 耗时特别长，用了两分钟
1 row in set (2 min 1.94 sec)
```

- 为sn字段构建索引

```mysql
-- 为sn字段构建索引，需要构建B+树索引结构，耗时较长
CREATE INDEX idx_sku_sn ON tb_sku(sn);

Query OK, 0 rows affected (4 min 21.72 sec)
```

- 有索引查询

```mysql
SELECT * FROM tb_sku WHERE sn = '100000003145001';
-- 耗时显著减少
1 row in set (0.00 sec)
```

### 性能分析工具

#### SQL执行频率

MySQL 客户端连接成功后，通过`show [session|global] status`命令可以提供服务器状态信息，查看当前数据库的INSERT、UPDATE、DELETE、SELECT的访问频次：

```mysql
-- SESSION 查看当前会话
-- GLOBAL 查询全局数据
-- Com______：1个下划线+6个模糊匹配字符(下划线)
SHOW [SESSION|GLOBAL] STATUS LIKE 'Com______';

-- Com_delete: 删除次数
-- Com_insert: 插入次数
-- Com_select: 查询次数
-- Com_update: 更新次数
```

**案例：**

```mysql
mysql> SHOW GLOBAL STATUS LIKE 'Com_______';
+---------------+--------+
| Variable_name | Value  |
+---------------+--------+
| Com_binlog    | 0      |
| Com_commit    | 66     |
| Com_delete    | 0      |
| Com_import    | 0      |
| Com_insert    | 177    |
| Com_repair    | 0      |
| Com_revoke    | 0      |
| Com_select    | 161481 |
| Com_signal    | 0      |
| Com_update    | 8      |
| Com_xa_end    | 0      |
+---------------+--------+
11 rows in set (0.00 sec)
```

通过上述指令可以查看到当前数据库到底是以查询为主，还是以增删改为主，从而为数据库优化提供参考依据。 

如果是以增删改为主，我们可以考虑不对其进行索引的优化。如果是以查询为主，那么就要考虑对数据库的索引进行优化了。

#### 慢查询日志

慢查询日志记录了所有执行时间超过指定参数（long_query_time，单位：秒，默认10秒）的所有SQL语句的日志。

MySQL的慢查询日志默认没有开启：

```mysql
-- 查看慢查询参数是否开启
mysql> SHOW VARIABLES LIKE 'slow_query_log';
+----------------+-------+
| Variable_name  | Value |
+----------------+-------+
| slow_query_log | OFF   |
+----------------+-------+
```

- 开启慢查询日志，需要在MySQL的配置文件（/etc/my.cnf）中配置如下信息：


```shell
# 开启MySQL慢日志查询开关
slow_query_log=1
#配置日志地址
slow-query-log-file=/var/lib/mysql/localhost-slow.log
# 设置慢日志的时间为2秒，SQL语句执行时间超过2秒，就会视为慢查询，记录慢查询日志
long_query_time=2
```

- 通过命令开启慢查询（临时，MySQL重启后失效）

```mysql
SET GLOBAL slow_query_log='ON';
SET GLOBAL long_query_time=2;
SET GLOBAL slow_query_log_file='/var/lib/mysql/localhost-slow.log'
```

重启Mysql服务：`systemctl restart mysqld `

**测试：**

```mysql
# 在目录/www/server/data下找到慢日志文件mysql-slow.log
# 实时输出慢日志尾部写入的内容
tail -f localhost-slow.log

-- 输入查询
SELECT COUNT(*) FROM tb_sku;
-- 用时两分钟
| COUNT(*) |
+----------+
|  9998112 |
+----------+
1 row in set (2 min 9.83 sec)

-- 慢日志输出的日志信息：

# Time: 2022-11-01T02:29:27.489483Z
# User@Host: root[root] @  [60.223.17.56]  Id: 133100
# Query_time: 130.072571  Lock_time: 0.000153 Rows_sent: 1  Rows_examined: 0
SET timestamp=1667269637;
/* ApplicationName=DataGrip 2022.2.5 */ SELECT COUNT(*) FROM tb_sku;

```

通过慢查询日志，可以定位出执行效率比较低的SQL，从而有针对性的进行优化。

#### PROFILE

`SHOW PROFILES`能够显示时间都耗费到哪里。

通过have_profiling参数，可以看到当前MySQL是否支持profile操作：

```mysql
SELECT @@have_profiling;

-- 数据库是支持profile操作
mysql> SELECT @@have_profiling;
+------------------+
| @@have_profiling |
+------------------+
| YES              |
+------------------+
1 row in set, 1 warning (0.00 sec)
```

但数据库默认关闭profiling操作，可以通过set语句在session/global级别开启profiling：

```mysql
-- 数据库默认profiling开关关闭
mysql> SELECT @@profiling;
+-------------+
| @@profiling |
+-------------+
|           0 |
+-------------+
1 row in set, 1 warning (0.00 sec)

-- 开启profiling
SET profiling = 1;
```

开启后，执行一系列SQL操作，可以通过如下指令查看指令的执行耗时：

```mysql
-- 查看每一条SQL的耗时基本情况
SHOW PROFILES;
-- 查看指定query_id的SQL语句各个阶段的耗时情况
SHOW PROFILE FOR QUERY query_id;
-- 查看指定query_id的SQL语句CPU的使用情况
SHOW PROFILE CPU FOR QUERY query_id;
```

**案例：**

```mysql
-- 执行SQL语句
SELECT * FROM tb_user;
SELECT * FROM tb_user WHERE name = '白起';
SELECT COUNT(*) FROM tb_sku;
SELECT * FROM tb_user WHERE id = 1;
```

- `SHOW PROFILES;` ：查看每一条SQL的耗时情况

![image-20221101104540151](https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-masterimage-20221101104540151.png)

> 注意根据name查询是id查询的10倍，因为name查询是回表查询

- `SHOW PROFILE FOR QUERY 4;`：查看指定id=4的SQL语句各个阶段的耗时情况

![image-20221101104823114](https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-masterimage-20221101104823114.png)

- `SHOW PROFILE CPU FOR QUERY;`：查看指定id的SQL语句的CPU使用情况

![image-20221101104903554](https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-masterimage-20221101104903554.png)

#### EXPLAIN

通过`EXPLAIN`或者`DESC`命令可以获取MySQL如何执行SELECT语句的信息，包括在SELECT语句执行过程中表如何连接和连接的顺序。

**语法：**

```mysql
-- 直接在SELEC语句之前加上关键字 EXPLAIN/DESC
[EXPLAIN/DESC] SELECT 字段列表 FROM 表名 WHERE 条件 ;
```

![image-20221101105330968](https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-masterimage-20221101105330968.png)

**执行计划各列字段含义：** (*表示重点关注)

| 字段            | 含义                                                         |
| :-------------- | :----------------------------------------------------------- |
| `id`            | SELECT查询的序列号，表示查询中执行SELECT子句或者是操作表的顺序（id相同，执行顺序从上到下；id不同，值越大，越先执行） |
| `select_type`   | 表示 SELECT 的类型，常见的取值有：<br />SIMPLE（简单表，无表连接或子查询）<br />PRIMARY（主查询，即外层的查询）<br />UNION（联合查询中的第二个或后面的查询语句）<br />SUBQUERY（SELECT/WHERE之后的语句包含子查询）<br />等 |
| <br />*`type`   | 表示连接类型，性能由好到差的连接类型为<br />NULL<br />system<br />const<br />eq_ref<br />ref<br />range<br />index<br />all |
| *`possible_key` | 显示这张表上可能用到的一个或多个索引                         |
| *`key`          | 实际使用的索引，如果为NULL，则没有使用索引                   |
| *`key_len`      | 表示索引中使用的字节数，该值为索引字段最大可能长度，并非实际使用长度，在不损失精确性的前提下，长度越短越好 |
| `rows`          | MySQL认为必须要执行查询的行数，在innodb引擎的表中，是一个估计值，可能并不总是准确的 |
| `filtered`      | 表示返回结果的行数占需读取行数的百分比，filtered的值越大越好 |
| `Extra`         | 额外信息                                                     |

### 索引使用规则

#### 最左前缀法则

若使用联合索引，要遵守最左前缀法则

**最左前缀法则:** 

查询要从索引的最左列开始，且不能跳过索引中的列。若跳跃某一列，索引将会部分失效（后面的字段索引失效）

![image-20221104105930733](https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-masterimage-20221104105930733.png)

如上图所示的联合索引，若需使索引生效，查询时最左边的列，也就是profession字段必须存在，且若无age字段（比如只有profession和），则status会失效

**案例：**

- ```mysql
  -- 索引生效
  EXPLAIN SELECT * FROM tb_user WHERE profession='软件工程' AND age='31' AND status='0';
  ```

  ![image-20221104133233620](https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-masterimage-20221104133233620.png)

  注意此时索引长度为54，此时联合索引完全生效

- ```mysql
  -- 缺少最左列professing，索引失效
  EXPLAIN SELECT * FROM tb_user WHERE age='31' AND status='0';
  ```

  ![image-20221104133316000](https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-masterimage-20221104133316000.png)

  注意此时索引失效，查询采用全表扫描

- ```mysql
  -- 索引仅生效于针对profession的匹配，因为缺少age，所以针对status的匹配失效
  EXPLAIN SELECT * FROM tb_user WHERE profession='软件工程' AND status='0';
  ```

  ![image-20221104133423389](https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-masterimage-20221104133423389.png)

  注意此时索引虽然生效，但索引长度减少到47，即索引部分生效。

- ```mysql
  -- 索引长度为54，索引全部生效
  EXPLAIN SELECT * FROM tb_user WHERE profession='软件工程' AND age='31' AND status='0';
  -- 索引长度为49，索引仅生效profession和age
  EXPLAIN SELECT * FROM tb_user WHERE profession='软件工程' AND age='31';
  -- 索引长度为47，索引仅 profession
  EXPLAIN SELECT * FROM tb_user WHERE profession='软件工程';
  ```

  根据索引长度，可以推断出profession字段索引长度为47，age字段索引长度为2，status字段索引长度为5，联合索引总长度54。

> 最左前缀法则中，最左边的列，是指在查询时，联合索引的最左边的字段(即是第一个字段)必须存在， 与编写SQL时条件编写的先后顺序无关

#### 范围查询

- 联合查询中，当范围查询使用`>`，`<`，范围查询右侧的字段索引失效

```mysql
-- status字段索引失效
EXPLAIN SELECT * FROM tb_user WHERE profession = '软件工程' AND age > 30 AND status = '0';
```

![image-20221107194348179](https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-masterimage-20221107194348179.png)

联合索引总长度为54，这里仅使用了49，说明status字段的索引失效

- 联合查询中，当范围查询使用`>=`，`<=`，联合索引全部生效

```mysql
-- 联合索引全部生效
EXPLAIN SELECT * FROM tb_user WHERE profession = '软件工程' AND age >= 30 AND status = '0';
```

![image-20221107194707086](https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-masterimage-20221107194707086.png)

联合查询使用的索引长度为49，联合索引全部生效

**所以在业务允许的情况下，应当尽量使用`<=`，`>=`运算符，避免使用`<`，`>`运算符**

#### 索引失效情况

- **在索引列上进行运算操作，索引会失效**

  ```mysql
  -- 对phone字段进行等值匹配，索引生效，key_len = 46
  EXPLAIN SELECT * FROM tb_user WHERE phone = '17799990015';
  -- 对phone字段进行函数运算，索引失效，key_len = NULL
  EXPLAIN SELECT * FROM tb_user WHERE substring(phone,10,2) = '15';
  ```

- **字符串类型字段不加引号，索引会失效**

  ```mysql
  -- status字段等值匹配添加单引号，索引生效，key_len = 54
  EXPLAIN SELECT * FROM tb_user WHERE profession = '软件工程' AND age >= 30 AND status = '0';
  -- status字段等值匹配未添加单引号，索引失效，key_len = 49
  EXPLAIN SELECT * FROM tb_user WHERE profession = '软件工程' AND age >= 30 AND status = 0;
  ```

- **进行模糊查询时，头部模糊匹配，索引失效；尾部模糊匹配，索引不会失效**

  ```mysql
  -- 尾部模糊匹配，索引生效
  EXPLAIN SELECT * FROM tb_user WHERE profession like '软件%';
  -- 头部模糊匹配，索引失效
  EXPLAIN SELECT * FROM tb_user WHERE profession like '%工程';
  ```

- **使用OR关键字，若OR左侧与右侧字段有一边没有建立索引，那么涉及到的索引均不会生效**

  ```mysql
  -- 尽管id字段存在索引，但age字段没有，故索引不会使用，key_len = 0
  EXPLAIN SELECT * FROM tb_user WHERE id = 10 OR age = 23;
  EXPLAIN SELECT * FROM tb_user WHERE age = 23 OR id = 10;
  -- 为age字段建立索引，两侧索引均使用，key_len = 4,2
  EXPLAIN SELECT * FROM tb_user WHERE id = 10 OR age = 23;
  ```

- **如果MySQL评估使用索引比全表更慢，则不使用索引**

  ```mysql
  -- 手机号分布范围为尾号0000~0023
  -- 使用索引
  EXPLAIN SELECT * FROM tb_user WHERE phone >= '17799990015';
  -- 因为手机尾号均大于0000，MySQL评估全表扫描更快，故未使用索引
  EXPLAIN SELECT * FROM tb_user WHERE phone >= '17799990000';
  ```

  `IS NULL`与`IS NOT NULL`同理，是否使用索引取决于表中数据分布，由MySQL评估全表扫描和索引哪个快

#### SQL提示

> 1. 执行SQL：`EXPLAIN SELECT * FROM tb_user WHERE profession = '软件工程';`
>
>    ![image-20221107202508527](https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-masterimage-20221107202508527.png)
>
>    查询结果显示SQL查询使用了联合索引
>
> 2. 执行SQL：`CREATE INDEX idx_user_pro on tb_user(profession);`
>
>    为profession字段创建单列索引
>
> 3. 再次执行SQL：`EXPLAIN SELECT * FROM tb_user WHERE profession = '软件工程';`
>
>    ![](https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-masterimage-20221107202508527.png)
>
>    查询结果仍显示使用联合索引

**SQL提示：优化数据库的一个重要手段，简单来说就是在SQL语句中加入一些人为的提示来达到优化操作的目的**

- `USE INDEX(索引名)`：建议MySQL使用哪一个索引完成此次索引（仅建议，是否使用需看MySQL评估）

  ```mysql
  EXPLAIN SELECT * FROM tb_user USE INDEX(idx_user_pro) WHERE profession = '软件工程';
  ```

- `IGNORE INDEX(索引名)`：忽略指定的索引

  ```mysql
  EXPLAIN SELECT * FROM tb_user IGNORE INDEX(idx_user_pro_age_sta) WHERE profession = '软件工程';
  ```

- `FORCE INDEX(索引名)`：强制使用指定的索引

  ```mysql
  EXPLAIN SELECT * FROM tb_user FORCE INDEX(idx_user_pro) WHERE profession = '软件工程';
  ```

#### 覆盖索引

**尽量使用覆盖索引，减少`SELECT *`**

**覆盖索引:** 查询时使用了索引，并且需要返回的数据在该索引结构中已经能够找到，而无需再进行回表查询。（查询所需的数据列从索引中就可以得到）

> `tb_user`表中有一个联合索引`idx_user_pro_age_sta`，该索引关联了profession、age、status三个字段，该索引同时也是二级索引，索引B+Tree的叶子结点下挂着的是这一条记录的主键id。
>
> 当查询时返回的数据在id、profession、age与status中时，直接从二级索引中直接返回数据
>
> 当查询的数据在上述范围之外，就需要通过主键id去扫描聚集索引，再获取额外的数据，即回表操作。
>
> 若一直使用`SELECT *`查询返回所有字段，很容易造成回表查询。
>
> 使用`EXPLAIN`关键字获取SQL执行信息，从Extra列可以得到是否使用覆盖索引
>
> |          Extra           |                             含义                             |
> | :----------------------: | :----------------------------------------------------------: |
> | Using where；Using Index | 查找使用了索引，但是需要的数据都在索引列中能找到，所以不需要回表查询数据 |
> |  Using index condition   |             查询时使用了索引，但需要回表查询数据             |
>
> 如图使用聚集索引：
>
> ![image-20221107204717819](https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-masterimage-20221107204717819.png)

#### 前缀索引

> 当字段数据类型为字符串（varchar，text，longtext）时，有时候需要索引很长的字符串，这会让索引变得很大，查询时浪费大量磁盘IO，影响查询效率。
>
> 此时可以只将字符串的一部分前缀建立索引，大大节约索引空间，提高索引效率

**语法：**

```mysql
-- table_name 表名
-- column 列名
-- n 索引使用的前缀长
CREATE INDEX idx_xxxx ON table_name(colunm(n));
```

**前缀长度：**

前缀长度可以根据索引的选择性来决定

索引的选择性：不重复的索引值（基数）和数据表的记录总数的比值，索引的选择性越高则查询效率越高。如唯一索引的选择性是1，具有最好的索引选择性，性能最好。

```mysql
-- 计算email字段的索引选择性
SELECT COUNT(DISTINCT email) / COUNT(*) FROM tb_user;
-- 计算email字段的前缀索引的索引选择性
SELECT COUNT(DISTINCT SUBSTRING(email,1,8)) / COUNT(*) FROM tb_user;
```

**前缀索引的查询流程：**



![image-20221107210442912](https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-masterimage-20221107210442912.png)

执行查询是会截取email前五个字符匹配，前五个字符匹配到后，回表查询匹配全部字符。

注意B+Tree叶子结点是一个链表，若匹配到多个项，会依次回表查询并匹配全部字符。

**案例：**

为tb_user表的email字段，建立长度为5的前缀索引。

```mysql
CREATE INDEX idx_email_5 ON tb_user(email(5));
```

Sub_part显示前缀索引截取的长度

![image-20221107205706892](https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-masterimage-20221107205706892.png)

#### 单列/联合索引

- **单列索引:** 一个索引仅包含单个列
- **联合索引:** 一个索引包含了多个列

在业务场景中，如果存在多个查询条件，考虑针对于查询字段建立索引时，建议建立联合索引，而非单列索引。

#### 索引设计原则

1. 针对数据量较大，且查询比较频繁的表建立索引。
2. 针对常作为查询条件（WHERE）、排序（ORDER BY）、分组（GROUP BY）操作的字段建立索引。
3. 尽量选择区分度高的列作为索引，尽量建立唯一索引，区分度越高，使用的效率越高。
4. 如果是字符串类型的字段，且字段的长度较长，可以针对于字段的特点，建立前缀索引。
5. 尽量使用联合索引，减少单列索引。查询时，联合索引很多时候可以覆盖索引，节省存储空间，避免回表查询，提高查询效率。
6. 要控制索引的数量，索引并不是多多益善，索引越多，维护索引结构的代价也越大，会影响删改的效率
7. 如果索引列不能存储NULL值，请在创建表时使用`NOT NULL`约束它。当优化器知道每列是否包含NULL值时，它可以更好的确定哪个索引能最有效地用于查询。

## SQL优化

### 插入数据

```mysql
insert into tb_test values(1,'tom');
insert into tb_test values(2,'cat');
insert into tb_test values(3,'jerry');
.....
```

**如果需要一次性往数据表中插入多条记录，可以从三个方面优化：**

- **批量插入**

  ```mysql
  Insert into tb_test values(1,'Tom'),(2,'Cat'),(3,'Jerry');
  ```

- **手动提交事务**

  避免事务的频繁开启和提交

  ```mysql
  start transaction;
  insert into tb_test values(1,'Tom'),(2,'Cat'),(3,'Jerry');
  insert into tb_test values(4,'Tom'),(5,'Cat'),(6,'Jerry');
  insert into tb_test values(7,'Tom'),(8,'Cat'),(9,'Jerry');
  commit;
  ```

- **主键顺序插入（性能优于乱序插入）：**

  ```mysql
  主键乱序插入 : 8 1 9 21 88 2 4 15 89 5 7 3
  主键顺序插入 : 1 2 3 4 5 7 8 9 15 21 88 89
  ```

****

**大批量插入数据：**

如果一次性需要插入大批量数据(比如: 几百万的记录)，使用insert语句插入性能较低，此时可以使 用MySQL数据库提供的load指令进行插入。操作如下：

```mysql
-- 客户端连接服务端时，加上参数 -–local-infile
mysql –-local-infile -u root -p

-- 设置全局参数local_infile为1，开启从本地加载文件导入数据的开关
set global local_infile = 1;

-- 执行load指令将准备好的数据，加载到表结构中
load data local infile '/root/sql1.log' into table tb_user fields
terminated by ',' lines terminated by '\n' ;
```

![image-20230525134930268](https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-masterimage-20230525134930268.png)

### 主键优化

#### 数据组织方式

- **在InnoDB存储引擎中，表数据都是根据主键顺序组织存放的，这种存储方式的表称为索引组织表 (index organized table IOT)。（聚簇索引）**

  ![image-20230525135635870](https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-masterimage-20230525135635870.png)

  行数据，都是存储在聚集索引的叶子节点上的

- **InnoDB逻辑存储结构：**

  ![image-20221027093040719](https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-masterimage-20221027093040719.png)

  在InnoDB引擎中，数据行是记录在逻辑结构 page 页中的，而每一个页的大小是固定的，默认16K。 那也就意味着， 一个页中所存储的行也是有限的，如果插入的数据行row在该页存储不小，将会存储到下一个页中，页与页之间会通过指针连接。

  页可以为空，也可以填充一半，也可以填充100%。每个页包含了2-N行数据（如果一行数据过大，会行溢出），根据主键排列。

  ![image-20230525140305647](https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-masterimage-20230525140305647.png)

#### **页分裂**

- **主键顺序插入效果：**

  从磁盘中申请页，主键顺序插入

  - 如果第一个页没满，继续往第一页插入
  - 当第一个也写满之后，再写入第二个页，页与页之间会通过指针连接

- **主键乱序插入效果：** 

  **页分裂：**

  假如1#，2#页都已经写满了，存放了如图所示的数据：

  ![image-20230525140325052](https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-masterimage-20230525140325052.png)

  此时再插入id为50的记录，因为索引结构的叶子结点是有顺序的。按照顺序，应该存储在47之后

  ![image-20230525140452851](https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-masterimage-20230525140452851.png)

  但是47所在的1#页，已经写满了，存储不了50对应的数据了。 那么此时会开辟一个新的页 3#。

  同时并不会直接将50存入3#页，而是会将1#页后一半的数据，移动到3#页，然后在3#页，插入50。

  ![image-20230525140519342](https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-masterimage-20230525140519342.png)

  移动数据，并插入id为50的数据之后，那么此时，这三个页之间的数据顺序是有问题的。 1#的下一个 页，应该是3#， 3#的下一个页是2#。 所以，此时，需要重新设置链表指针。

  ![image-20230525140600201](https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-masterimage-20230525140600201.png)

  **页分裂是比较耗费性能的操作**

#### **页合并**

目前表中已有数据的索引结构(叶子节点)如下：

![image-20230525140742507](https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-masterimage-20230525140742507.png)

当对已有数据进行删除：删除一行记录时，实际上记录并没有被物理删除，只是记录被标记（flaged）为删除并且它的空间变得允许被其他记录声明使用。

![image-20230525140759898](https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-masterimage-20230525140759898.png)

当页中删除的记录达到 MERGE_THRESHOLD（默认为页的50%），InnoDB会开始寻找最靠近的页（前或后）看看是否可以将两个页合并以优化空间使用。

![image-20230525140845659](https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-masterimage-20230525140845659.png)

**这里发生的合并页的现象，称之为页合并**

> MERGE_THRESHOLD：合并页的阈值，可以自己设置，在创建表或者创建索引时指定。

#### **索引设计原则**

- **满足业务需求的情况下，尽量降低主键的长度**
  - 因为二级索引只存储主键，主键过长会让二级索引占用过多空间
- **插入数据时，尽量选择顺序插入，选择使用AUTO_INCREMENT自增主键**
  - 为维护主键索引数据结构，主键乱序插入会导致页分裂现象
- **尽量不要使用UUID做主键或者其他自然主键，如身份证号**
  - 无序的主键在插入时是乱序插入，更容易发生页分裂
  - UUID、身份证号之类的主键长度较长，浪费IO
- **业务操作时，避免对主键的修改**

### **Order By优化**

**MySQL的排序，有两种方式：**

- **Using filesort :** 

  通过表的索引或全表扫描，读取满足条件的数据行，然后在排序缓冲区sort buffer中完成排序操作。

  所有不是通过索引直接返回排序结果的排序都叫 FileSort 排序。

- **Using Index：**

  通过有序索引顺序扫描直接返回有序数据，这种情况即为 using index，不需要 额外排序，操作效率高。

**对于以上的两种排序方式，Using index的性能高，而Using filesort的性能低。所以在优化排序操作时，尽量要优化为 Using index。**

****

**Order By 优化原则：**

- **根据排序字段建立合适的索引，多字段排序时，也遵循最左前缀法则**

  不满足最左前缀法则，索引部分生效，EXPLAIN解析结果extra字段会标注`Using index,Using filesort`

- **尽量使用覆盖索引**

  覆盖索引即查询索引中的值已经足够返回结果，不需要回表；

  没有使用覆盖索引，无法根据索引优化排序

- **多字段排序，一个升序一个降序，此时需要注意联合索引在创建时的规则（ASC/DESC，默认升序ASC）**

  单索引升序索引，倒序排序字段会反向扫描索引，EXPLAIN查看执行信息时，会在extra字段标注`Backward index scan`

  联合索引排序时，如果创建默认升序的联合索引，并且第一个索引升序排序，第二个索引降序排序，会出现`Using filesort`，因为需要额外的排序

- **如果不可避免的出现filesort，大数据量排序时，可以适当增大排序缓冲区大小 sort_buffer_size(默认256k)**

### **Group By优化**

**排序方式(EXPLAIN分析得到的extra字段信息)：**

- **Using temporary**

  使用了临时表，没使用索引，性能低

- **Using index**

  使用了索引，性能高

**Group By优化原则**

- **分组操作时，可以通过索引来提高效率**

- **分组操作时，索引的使用也应当满足最左前缀法则**

  where出现第一个字段，group by第二个字段，也是满足最左前缀法则

### **Limit优化**

在数据量比较大时，如果进行limit分页查询，在查询时，越往后，分页查询效率越低。

![image-20230525143908135](https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-masterimage-20230525143908135.png)

因为，当在进行分页查询时，如果执行 limit 2000000,10 ，此时需要MySQL排序前2000010记录，仅仅返回 2000000 - 2000010 的记录，**其他记录丢弃，查询排序的代价非常大 。** 

****

**优化思路：**

- **索引优化：**

  一般分页查询时，通过创建覆盖索引能够比较好地提高性能（可以在子表中使用覆盖索引）

  - **子查询优化：**

    **先在子查询中，用覆盖索引通过索引执行分页查询出主键，再从以子表结果去主表回表拿数据**

  ```mysql
  explain select * from tb_sku t , (select id from tb_sku order by id limit 2000000,10) a where t.id = a.id);
  ```

  - **延迟关联：**

    和上述的子查询做法类似，可以使用JOIN，先在索引列上完成分页操作，然后再回表获取所需要的列。

  ```mysql
  select a.* from t5 a inner join (select id from t5 order by text limit 1000000, 10) b on a.id=b.id;
  ```

- **记录上次查询结束的位置：**

  使用某种变量记录上一次数据的位置，分页时直接从这个变量的位置开始扫描，从而避免Mysql扫描大量数据再抛弃的操作：

  ```mysql
  select * from t5 where id>=1000000 limit 10;
  ```

### Count优化

数据量很大的时候，执行count操作时，是非常耗时的：

- MyISAM 引擎把一个表的总行数存在了磁盘上，因此执行count(*)的时候会直接返回这个数，效率很高； 

  但是如果是带条件的count，MyISAM也慢。

- InnoDB 引擎就麻烦了，它执行 count(*) 的时候，需要把数据一行一行地从引擎里面读出来，然后累积计数。

count() 是一个聚合函数，对于返回的结果集，一行行地判断，如果 count 函数的参数不是 NULL，累计值就加 1，否则不加，最后返回累计值。

| COUNT用法 | 含义                                                         |
| :-------: | :----------------------------------------------------------- |
| **主键**  | InnoDB 引擎会遍历整张表，把每一行的主键id值都取出来，返回给服务层。 服务层拿到主键后，直接按行进行累加(主键不可能为null) |
| **字段**  | **没有not null约束** : InnoDB 引擎会遍历整张表把每一行的字段值都取出 来，返回给服务层，服务层判断是否为null，不为null，计数累加。<br />**有not null约束**：InnoDB 引擎会遍历整张表把每一行的字段值都取出来，返回给服务层，直接按行进行累加。 |
|  **\***   | InnoDB引擎并不会把全部字段取出来，而是专门做了优化，不取值，服务层直接按行进行累加。 |
|   **1**   | InnoDB 引擎遍历整张表，但不取值。服务层对于返回的每一行，放一个数字“1”进去，直接按行进行累加。 |

> 按照效率排序的话，count(字段) < count(主键 id) < count(1) ≈ count(\*)，所以尽 量使用 count(\*)。
>
> 目前基于磁盘的数据库或者搜索引擎（比如Lucene）的性能瓶颈主要都是在IO阶段，相比于CPU和RAM，IO操作实在太慢了，所以这类系统的优化方向也都都是类似的——尽一切可能减少IO的次数（所以很多用ES的程序在性能优化到极限的时候选择直接上SSD）。
>
> **这里统计行数的操作，查询优化器的优化方向就是选择能够让IO次数最少的索引，也就是基于占用空间最小的字段所建的索引**（每次IO读取的数据量是固定的，索引占用的空间越小所需的IO次数也就越少）。
>
> 而Innodb的主键索引是聚簇索引（包含了KEY，除了KEY之外的其他字段值，事务ID和MVCC回滚指针）所以**主键索引一定会比二级索引（包含KEY和对应的主键ID）大**，也就是说在有二级索引的情况下，一**般COUNT()都不会通过主键索引来统计行数，在有多个二级索引的情况下选择占用空间最小的。**

### Update优化

- **在执行下面的SQL语句时**

  ```mysql
  update course set name = 'javaEE' where id = 1 ;
  ```

  会锁定id=1的这一行数据，然后事务提交之后，行锁释放

- **但是当执行下面的SQL时：**

  ```mysql
  update course set name = 'SpringBoot' where name = 'PHP' ;
  ```

  当开启多个事务，在执行上述的SQL时，因为name字段没有索引，此时行锁会升级为表锁，性能大大降低

> InnoDB的行锁是针对索引加的锁，不是针对记录加的锁 ,并且该索引不能失效，否则会从行锁升级为表锁 。

## 视图

### 介绍

视图（View）是一种虚拟存在的表。

视图中的数据并不在数据库中实际存在，行和列数据来自定义视图的查询中使用的表，并且是在使用视图时动态生成的。 

通俗的讲，视图只保存了查询的SQL逻辑，不保存查询结果。所以在创建视图的时候，主要的工作就落在创建这条SQL查询语句上。

### 视图作用

- **简单**

  视图不仅可以简化用户对数据的理解，也可以简化他们的操作。

  那些被经常使用的查询可以被定义为视图，从而使得用户不必为以后的操作每次指定全部的条件。

- **安全**

  数据库可以授权，但不能授权到数据库特定行和特定的列上。

  通过视图用户只能查询和修改他们所能见到的数据

  **数据独立**

  视图可帮助用户屏蔽真实表结构变化带来的影响。

### 语法

- **创建视图：**

  ```mysql
  CREATE [OR REPLACE] VIEW 视图名称[(列名列表)] AS SELECT语句 [ WITH [
  CASCADED | LOCAL ] CHECK OPTION ]
  ```

- **查询视图：**

  ```mysql
  查看创建视图语句：
  SHOW CREATE VIEW 视图名称;
  查看视图数据：
  SELECT * FROM 视图名称 ...... ;
  ```

- **修改视图：**

  ```mysql
  方式一：
  CREATE [OR REPLACE] VIEW 视图名称[(列名列表)] AS SELECT语句 [ WITH [ CASCADED | LOCAL ] CHECK OPTION ]
  方式二：
  ALTER VIEW 视图名称[(列名列表)] AS SELECT语句 [ WITH [ CASCADED | LOCAL ] CHECK OPTION ]
  ```

- **删除视图**

  ```mysql
  DROP VIEW [IF EXISTS] 视图名称 [,视图名称] ...
  ```

### 检查选项

在创建视图时，指定了条件 id<=10 ；

此时在视图中执行了操作，insert了id为17和id为6的数据

在视图中查询，查询不到id=17的数据，但是基表中已经确实插入

id=17，显然不符合视图定义的条件，**如果在定义视图时，希望不符合条件的操作不被执行，可以借助视图的检查选项**

**当使用`WITH CHECK OPTION`子句创建视图时，MySQL会通过视图检查正在更改的每个行，例如插入，更新，删除，以使其符合视图的定义。** 

**MySQL允许基于另一个视图创建视图，它还会检查依赖视图中的规则以保持一致性。**

为了确定检查的范围，mysql提供了两个选项：`CASCADED` 和 `LOCAL` ，默认值为 `CASCADED` 

- `CASCADED`：级联

  **当我们在操作视图的时候，cascaded检查选项会递归地检查所依赖的视图的条件，无论这些视图是否指定检查选项**

  比如，v2视图是基于v1视图的，如果在v2视图创建的时候指定了检查选项为 cascaded，但是v1视图创建时未指定检查选项。 

  则在执行检查时，不仅会检查v2，还会级联检查v2的关联视图v1。

  ```mysql
  # case1
  # 创建一个基于students表的视图
  create or replace view v1 as select id,name from students where id<=20;
  # 由于没有检查选项，所以插入id>20的数据也会插入成功
  insert into v1 values(21,'john');	# 插入成功
  
  # case2
  # 创建一个基于v1的视图，并添加cascaded检查选项
  create or replace view v2 as select id,name from v1 where id>10 with cascaded check option;
  
  # 添加检查选项后，再插入数据，MySQL就会判断插入数据是否满足条件，
  # 由于此视图是基于v1的，所以现在可以插入的id值为 10<id<=20。
  insert into v2 values（22，'lucy');	# 插入失败
  
  # case3
  # 创建一个基于v2的视图
  create or replace v3 as select id,name from v2 where id<=15;
  
  # 由于v3没有添加检查选项，但v3是基于v2的，所以现在可以插入的id值依然为 10<id<=20。
  insert into v3 values(18,'Tom');	# 插入成功
  
  insert into v3 values(24,'kobe');	# 插入失败
  ```

- `LOCAL`：本地

  **当我们在操作当前视图时，local检查选项是递归地查找当前视图所依赖的视图是否有检查选项，如果有，则检查；如果没有，就不做检查**

  比如，v2视图是基于v1视图的，如果在v2视图创建的时候指定了检查选项为 local ，但是v1视图创建时未指定检查选项。 

  则在执行检查时，只会检查v2，不会检查v2的关联视图v1。

  ```mysql
  # case1
  # 创建一个基于students表的视图
  create or replace view v1 as select id,name from students where id<=20;
  
  insert into v1 values(21,'john');	# 插入成功
  
  # case2
  # 创建一个基于v1的视图，并添加local检查选项
  create or replace view v2 as select id,name from v1 where id>10 with local check option;
  
  # 添加检查选项后，再插入数据，MySQL就会判断插入数据是否满足条件，
  # 由于此视图是基于v1的，v1没有检查选项，所以现在可以插入的id值为 id>10。
  insert into v2 values（22，'lucy');	# 插入成功
  
  #case3
  #创建一个基于v2的视图
  create or replace v3 as select id,name from v2 where id<=15;
  
  #由于v3没有添加检查选项，但v3是基于v2的，所以现在可以插入的id值依然为 id>10。
  insert into v3 values(18,'Tom');	# 插入成功
  ```

### 视图的更新

要使视图可更新，视图中的行与基础表中的行之间必须存在一对一的关系。

**如果视图包含以下任何一项，则该视图不可更新：**

- 聚合函数或窗口函数
- `DISTINCT`
- `GROUP BY`
- `HAVING`
- `UNION`或者`UNION ALL`

## 存储过程

存储过程是事先经过编译并存储在数据库中的一段SQL语句的集合，调用存储过程可以简化应用开发人员的很多工作，减少数据在数据库和应用服务器之间的传输，对于提高数据处理的效率是有好处的。 

存储过程思想上很简单，就是数据库SQL语言层面的代码封装与重用。

![image-20230525160727331](https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-masterimage-20230525160727331.png)

**特点：**

- **封装，复用：**

  可以把某一业务SQL封装在存储过程中，需要用到的时候直接调用即可

- **可以接收参数，也可以返回数据：**

  在存储过程中，可以传递参数，也可以接收返回值

- **减少网络交互，效率提升：**

  如果涉及到多条SQL，每执行一次都是一次网络传输。而如果封装在存储过程中，就只需要网络交互一次就够了

### 语法

1. **创建**

   ```mysql
   CREATE PROCEDURE 存储过程名称 ([ 参数列表 ])
   BEGIN
   -- SQL语句
   END ;
   ```

2. **调用**

   ```mysql
    CALL 名称 ([ 参数 ]);
   ```

3. **查看**

   ```mysql
    -- 查询指定数据库的存储过程及状态信息
   SELECT * FROM INFORMATION_SCHEMA.ROUTINES WHERE ROUTINE_SCHEMA = 'xxx';
   -- 查询某个存储过程的定义
   SHOW CREATE PROCEDURE 存储过程名称 ; 
   ```

4. **删除**

   ```mysql
    DROP PROCEDURE [ IF EXISTS ] 存储过程名称;
   ```

**演示示例：**

```mysql
-- 存储过程基本语法
-- 创建
create procedure p1()
begin
select count(*) from student;
end;

-- 调用
call p1();

-- 查看
select * from information_schema.ROUTINES where ROUTINE_SCHEMA = 'itcast';
show create procedure p1;

-- 删除
drop procedure if exists p1;
```

### 变量

在MySQL中变量分为三种类型：系统变量、用户定义变量、局部变量。

#### 系统变量

系统变量是MySQL服务器提供，不是用户定义的，属于服务器层面。

分为全局变量（GLOBAL）、会话变量（SESSION）。

- **查看系统变量：**

  ```mysql
   -- 查看所有系统变量
  SHOW [ SESSION | GLOBAL ] VARIABLES;
  -- 可以通过LIKE模糊匹配方式查找变量
  SHOW [ SESSION | GLOBAL ] VARIABLES LIKE '......'; 
  -- 查看指定变量的值
  SELECT @@[SESSION | GLOBAL] 系统变量名; 
  ```

- **设置系统变量：**

  ```mysql
  SET [ SESSION | GLOBAL ] 系统变量名 = 值 ;
  SET @@[SESSION | GLOBAL] 系统变量名 = 值 ;
  ```

> 注意: 
>
> 如果没有指定SESSION/GLOBAL，默认是SESSION，会话变量。 
>
> A. 全局变量(GLOBAL): 全局变量针对于所有的会话。 
>
> B. 会话变量(SESSION): 会话变量针对于单个会话，在另外一个会话窗口就不生效了。

**演示示例：**

```mysql
-- 查看系统变量
show session variables ;
show session variables like 'auto%';
show global variables like 'auto%';
select @@global.autocommit;
select @@session.autocommit;
-- 设置系统变量
set session autocommit = 1;
insert into course(id, name) VALUES (6, 'ES');
set global autocommit = 0;
select @@global.autocommit;
```

#### 用户定义变量

用户定义变量是用户根据需要自己定义的变量，用户变量不用提前声明，在用的时候直接用 "@变量名" 使用就可以。

其作用域为当前连接。

**赋值：**

- 方式一：

  ```mysql
  SET @var_name = expr [, @var_name = expr] ... ;
  SET @var_name := expr [, @var_name := expr] ... ;
  ```

  赋值时，可以使用`=`，也可以使用`:=`。

- 方式二

  ```mysql
  SELECT @var_name := expr [, @var_name := expr] ... ;
  SELECT 字段名 INTO @var_name FROM 表名;
  ```

**使用：**

> 注意: 用户定义的变量无需对其进行声明或初始化，只不过获取到的值为NULL。

```mysql
SELECT @var_name ;
```

**演示示例：**

```mysql
-- 赋值
set @myname = 'itcast';
set @myage := 10;
set @mygender := '男',@myhobby := 'java';
select @mycolor := 'red';
select count(*) into @mycount from tb_user;
-- 使用
select @myname,@myage,@mygender,@myhobby;
select @mycolor , @mycount;
select @abc; -- NULL
```

#### 局部变量

局部变量是根据需要定义的在局部生效的变量，访问之前，需要DECLARE声明。

可用作存储过程内的局部变量和输入参数，局部变量的范围是在其内声明的BEGIN ... END块。

**声明：**

```mysql
DECLARE 变量名 变量类型 [DEFAULT ... ] ;
```

**赋值：**

```mysql
SET 变量名 = 值 ;
SET 变量名 := 值 ;
SELECT 字段名 INTO 变量名 FROM 表名 ... ;
```

**演示示例：**

```mysql
-- 声明局部变量 - declare
-- 赋值
create procedure p2()
begin
	declare stu_count int default 0;
	select count(*) into stu_count from student;
	select stu_count;
end;
call p2();
```

### if

if 用作条件判断，具体的语法结构为：

```mysql
IF 条件1 THEN
	.....
ELSEIF 条件2 THEN -- 可选
	.....
ELSE -- 可选
	.....
END IF;
```

在if条件判断的结构中，

ELSEIF结构可以有多个，也可以没有。

ELSE结构可以有，也可以没有。

### 参数

参数的类型，主要分为以下三种：`IN`、`OUT`、`INOUT`。

具体含义如下：

| 类型  |                     含义                     | 备注 |
| :---: | :------------------------------------------: | :--: |
|  IN   |   该类参数作为输入，也就是需要调用时传入值   | 默认 |
|  OUT  | 该类参数作为输出，也就是该参数可以作为返回值 |      |
| INOUT |    既可以作为输入参数，也可以作为输出参数    |      |

**用法：**

```mysql
CREATE PROCEDURE 存储过程名称 ([ IN|OUT|INOUT 参数名 参数类型 ])
BEGIN
	-- SQL语句
END ;
```

**演示示例：**

```mysql
create procedure p4(in score int, out result varchar(10))
begin
	if score >= 85 then
		set result := '优秀';
	elseif score >= 60 then
		set result := '及格';
	else
		set result := '不及格';
	end if;
end;
-- 定义用户变量 @result来接收返回的数据, 用户变量可以不用声明
call p4(18, @result);
select @result;
```

### case

case结构及作用和流程控制函数很类似

**语法1：**

```mysql
-- 含义：当
-- case_value的值为when_value1时，执行statement_list1；
-- 当值为 when_value2时，执行statement_list2，否则就执行statement_list
CASE case_value
	WHEN when_value1 THEN statement_list1
	[ WHEN when_value2 THEN statement_list2] ...
	[ ELSE statement_list ]
END CASE;
```

**语法2：**

```mysql
-- 含义：
-- 当条件search_condition1成立时，执行statement_list1，
-- 当条件search_condition2成立时，执行statement_list2，否则就执行statement_list
CASE
	WHEN search_condition1 THEN statement_list1
	[WHEN search_condition2 THEN statement_list2] 
	...
	[ELSE statement_list]
END CASE;
```

### while

while循环是有条件的循环控制语句。

满足条件后，再执行循环体中的SQL语句。

具体语法为：

```mysql
-- 先判定条件，如果条件为true，则执行逻辑，否则，不执行逻辑
WHILE 条件 DO
	SQL逻辑...
END WHILE;
```

### repeat

repeat是有条件的循环控制语句,

当满足until声明的条件的时候，则退出循环 。

具体语法为：

```mysql
-- 先执行一次逻辑，然后判定UNTIL条件是否满足，如果满足，则退出。
-- 如果不满足，则继续下一次循环
REPEAT
	SQL逻辑...
UNTIL 条件
END REPEAT;
```

### loop

LOOP 实现简单的循环，如果不在SQL逻辑中增加退出循环的条件，可以用其来实现简单的死循环。 

LOOP可以配合一下两个语句使用：

- LEAVE：配合循环使用，退出循环
- ITERATE：必须用在循环中，作用是跳过当前循环剩下的语句，直接进入下一次循环

```mysql
[begin_label:] LOOP
	SQL逻辑...
END LOOP [end_label];
```

```mysql
LEAVE label; -- 退出指定标记的循环体
ITERATE label; -- 直接进入下一次循环
```

### 游标

游标（CURSOR）是用来存储查询结果集的数据类型 , 在存储过程和函数中可以使用游标对结果集进行循环的处理。

游标的使用包括游标的声明、OPEN、FETCH 和 CLOSE，其语法分别如下：

- 声明游标

  ```mysql
  DECLARE 游标名称 CURSOR FOR 查询语句 ;
  ```

- 打开游标

  ```mysql
  OPEN 游标名称 ;
  ```

- 获取游标记录

  ```mysql
  FETCH 游标名称 INTO 变量 [, 变量 ] ;
  ```

- 关闭游标

  ```mysql
  CLOSE 游标名称 ;
  ```

**演示示例：**

根据传入的参数uage，来查询用户表tb_user中，所有的用户年龄小于等于uage的用户姓名（name）和专业（profession），并将用户的姓名和专业插入到所创建的一张新表 (id,name,profession)中。

```mysql
-- 逻辑:
-- A. 声明游标, 存储查询结果集
-- B. 准备: 创建表结构
-- C. 开启游标
-- D. 获取游标中的记录
-- E. 插入数据到新表中
-- F. 关闭游标
create procedure p11(in uage int)
begin
	declare uname varchar(100);
	declare upro varchar(100);
	-- 声明游标
	declare u_cursor cursor for select name,profession from tb_user where age <=uage;
	drop table if exists tb_user_pro;
	-- 创建表结构
	create table if not exists tb_user_pro(id int primary key auto_increment,name varchar(100),profession varchar(100));
	-- 开启游标
	open u_cursor;
	while true do
		-- 获取游标中的记录
		fetch u_cursor into uname,upro;
		-- 插入数据到新表中
		insert into tb_user_pro values (null, uname, upro);
	end while;
	-- 关闭游标
	close u_cursor;
end;
call p11(30);
```

上述的存储过程，最终在调用的过程中，会报错，之所以报错是因为上面的while循环中，并没有退出条件。

当游标的数据集获取完毕之后，再次获取数据，就会报错，从而终止了程序的执行。

但是此时，tb_user_pro表结构及其数据都已经插入成功了

要想解决这个问题，就需要通过MySQL中提供的条件处理程序 Handler 来解决

### 条件处理程序

条件处理程序（Handler）可以用来定义在流程控制结构执行过程中遇到问题时相应的处理步骤。

具体语法为：

```mysql
DECLARE handler_action HANDLER FOR condition_value [, condition_value]... statement ;
-- handler_action 的取值：
	CONTINUE: 继续执行当前程序
	EXIT: 终止执行当前程序
-- condition_value 的取值：
	SQLSTATE sqlstate_value: 状态码，如 02000
	SQLWARNING: 所有以01开头的SQLSTATE代码的简写
	NOT FOUND: 所有以02开头的SQLSTATE代码的简写
	SQLEXCEPTION: 所有没有被SQLWARNING 或 NOT FOUND捕获的SQLSTATE代码的简写
```

**演示示例：**优化游标一节中的代码

根据传入的参数uage，来查询用户表tb_user中，所有的用户年龄小于等于uage的用户姓名 （name）和专业（profession），并将用户的姓名和专业插入到所创建的一张新表 (id,name,profession)中。

- **通过SQLSTATE指定具体的状态码**

  ```mysql
  -- 逻辑:
  -- A. 声明游标, 存储查询结果集
  -- B. 准备: 创建表结构
  -- C. 开启游标
  -- D. 获取游标中的记录
  -- E. 插入数据到新表中
  -- F. 关闭游标
  create procedure p11(in uage int)
  begin
  	declare uname varchar(100);
  	declare upro varchar(100);
  	declare u_cursor cursor for select name,profession from tb_user where age <= uage;
  	-- 声明条件处理程序：当SQL语句执行抛出的状态码为02000时，将关闭游标u_cursor，并退出
  	declare exit handler for SQLSTATE '02000' close u_cursor;
  	drop table if exists tb_user_pro;
  	create table if not exists tb_user_pro(
  		id int primary key auto_increment,
  		name varchar(100),
  		profession varchar(100)
      );
  	open u_cursor;
  	while true do
  		fetch u_cursor into uname,upro;
  		insert into tb_user_pro values (null, uname, upro);
  	end while;
  	close u_cursor;
  end;
  call p11(30);
  ```

- **通过SQLSTATE的代码简写方式 NOT FOUND**

  02 开头的状态码，代码简写为 NOT FOUND

  ```mysql
  create procedure p12(in uage int)
  begin
  	declare uname varchar(100);
  	declare upro varchar(100);
  	declare u_cursor cursor for select name,profession from tb_user where age <= uage;
  	-- 声明条件处理程序：当SQL语句执行抛出的状态码为02开头时，将关闭游标u_cursor，并退出
  	declare exit handler for not found close u_cursor;
  	drop table if exists tb_user_pro;
  	create table if not exists tb_user_pro(
  		id int primary key auto_increment,
      	name varchar(100),
  		profession varchar(100)
  	);
  	open u_cursor;
  	while true do
  		fetch u_cursor into uname,upro;
  		insert into tb_user_pro values (null, uname, upro);
  	end while;
  	close u_cursor;
  end;
  call p12(30);
  ```

> 具体的错误状态码，可以参考官方文档： 
>
> [MySQL :: MySQL 8.0 Reference Manual :: 13.6.7.2 DECLARE ... HANDLER Statement](https://dev.mysql.com/doc/refman/8.0/en/declare-handler.html)
>
> [MySQL :: MySQL 8.0 Error Reference :: 2 Server Error Message Reference](https://dev.mysql.com/doc/mysql-errors/8.0/en/server-error-reference.html)

### 存储函数

存储函数是有返回值的存储过程，存储函数的参数只能是IN类型的。

具体语法如下：

```mysql
CREATE FUNCTION 存储函数名称 ([ 参数列表 ])
RETURNS type [characteristic ...]
BEGIN
	-- SQL语句
	RETURN ...;
END ;
```

 **characteristic说明：** 

- `DETERMINISTIC`：相同的输入参数总是产生相同的结果
- `NO SQL`：不包含SQL语句。
- `READS SQL DATA`：包含读取数据的语句，但不包含写入数据的语句

## 触发器

### 介绍

触发器是与表有关的数据库对象，指在`insert`/`update`/`delete`之前(BEFORE)或之后(AFTER)，触发并执行触发器中定义的SQL语句集合。

触发器的这种特性可以协助应用在数据库端确保数据的完整性，日志记录，数据校验等操作。

使用别名OLD和NEW来引用触发器中发生变化的记录内容，这与其他的数据库是相似的。现在触发器还只支持行级触发，不支持语句级触发。

|   触发器类型    |                       NEW 和 OLD                       |
| :-------------: | :----------------------------------------------------: |
| INSERT 型触发器 |             NEW 表示将要或者已经新增的数据             |
| UPDATE 型触发器 | OLD 表示修改之前的数据，NEW 表示将要或已经修改后的数据 |
| DELETE 型触发器 |             OLD 表示将要或者已经删除的数据             |

### 语法

- **创建：**

  ```mysql
  CREATE TRIGGER trigger_name
  BEFORE/AFTER INSERT/UPDATE/DELETE
  ON tbl_name FOR EACH ROW -- 行级触发器
  BEGIN
  	trigger_stmt ;
  END;
  ```

- **查看：**

  ```mysql
  SHOW TRIGGERS;
  ```

- **删除：**

  ```mysql
  -- 如果没有指定schema_name，默认为当前数据库
  DROP TRIGGER {schema_name.}trigger_name;
  ```

### 演示示例

通过触发器记录tb_user表的数据变更日志，将变更日志插入到日志表user_logs中

**表结构准备：**

```mysql
create table user_logs(
    id int(11) not null auto_increment,
    operation varchar(20) not null comment '操作类型, insert/update/delete',
    operate_time datetime not null comment '操作时间',
    operate_id int(11) not null comment '操作的ID',
    operate_params varchar(500) comment '操作参数',
    primary key(`id`)
)engine=innodb default charset=utf8;
```

**插入数据触发器**

```mysql
CREATE TRIGGER tb_user_insert_trigger AFTER INSERT ON tb_user for FOR EACH row
begin
	INSERT INTO 
		user_logs(id, operation, operate_time, operate_id, operate_params)
	VALUES
		(null, 'insert', now(), new.id, concat('插入的数据内容为:
                                       id=',new.id,',name=',new.name, ', phone=', NEW.phone, ', email=', NEW.email, ',
                                       profession=', NEW.profession));
END;
```

**测试：**

```mysql
-- 查看
show triggers ;
-- 插入数据到tb_user
insert into tb_user(id, name, phone, email, profession, age, gender, status,
                    createtime) VALUES (26,'三皇子','18809091212','erhuangzi@163.com','软件工
                                        程',23,'1','1',now());
```

检查日志表中的数据是否可以正常插入，以及插入数据的正确性。

## 锁

> 锁是计算机协调多个进程或线程并发访问某一资源的机制。
>
> 在数据库中，除传统的计算资源（CPU、 RAM、I/O）的争用以外，数据也是一种供许多用户共享的资源。
>
> 如何保证数据并发访问的一致性、有效性是所有数据库必须解决的一个问题，锁冲突也是影响数据库并发访问性能的一个重要因素。从这个角度来说，锁对数据库而言显得尤其重要，也更加复杂。  

### 分类

**MySQL中的锁，按照锁的粒度分，分为以下三类：**

- 全局锁：锁定数据库中的所有表
- 表级锁：每次操作锁住整张表
- 行级锁：锁住某一行的数据

### 全局锁

#### 介绍

全局锁就是对整个数据库实例加锁，加锁后整个实例就处于只读状态，后续的DML的写语句，DDL语句，已经更新操作的事务提交语句都将被阻塞。

其典型的使用场景是做**全库的逻辑备份**，对所有的表进行锁定，从而获取一致性视图，保证数据的完整性

**未加全局锁：**

![image-20230417140047873](https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-masterimage-20230417140047873.png)

**添加全局锁：**

![image-20230417140205558](https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-masterimage-20230417140205558.png)

#### 语法

**全局锁（FTWRL）:** `flush tables with read lock`

```mysql
-- 加锁
flush tables with read lock;
-- 解锁
unlock tables;
```

#### 特点

数据库总加全局锁，是一个比较重的操作，存在以下问题：

- 如果在主库上备份，那么在备份期间都不能执行更新，业务基本停摆
- 如果在从库上备份，那么在备份期间不能执行主库同步过来的二进制日志(binlog)，造成主从延迟

**其他解决数据备份的方法：**

1. 在InnoDB引擎中，我们可以在备份时加上参数`--single-transaction`参数来完成不加锁的一致性数据备份

   ```mysql
   mysqldump --single-transaction -uroot –p123456 itcast > itcast.sql
   ```

   当 mysqldump 使用参数–single-transaction 的时候，导数据之前就会启动一个事务（可重复读隔离级别），来确保拿到一致性视图。

   **前提是数据库使用的是支持事务的引擎**

2. `set global readonly=true`让全库进入只读状态

   - 在有些系统中，readonly的值会被用来做其他逻辑，比如用来判断一个库是主库还是备库。因此，修改global变量的方式影响面更大，不建议使用

   - 在异常处理机制上,

     如果执行 FTWRL 命令之后由于客户端发生异常断开，那么 MySQL 会自动释放这个全局锁，整个库回到可以正常更新的状态。

     而将整个库设置为 readonly 之后，如果客户端发生异常，则数据库就会一直保持 readonly 状态，这样会导致整个库长时间处于不可写状态，风险较高。

**一般还是使用FTWRL的方式进行数据备份**

### 表级锁

#### 介绍

表级锁，每次操作锁住整张表。

锁定粒度大，发生锁冲突的概率最高，并发度最低。

应用在MyISAM、 InnoDB、BDB等存储引擎中。 

**对于表级锁，主要分为以下三类：**

- 表锁
- 元数据锁(meta data lock, MDL)
- 意向锁

#### 表锁

**两类：**

- 表共享读锁，read lock

  不会阻塞其他客户端的读，但会阻塞写

- 表独占写锁，write lock

  即阻塞其他客户端的读，又阻塞其他客户端的写

**语法：**

```mysql
-- 加锁
lock tables 表名... read/write;
-- 解锁，客户端断开连接也会解锁
unlock tables;
```

#### 元数据锁

meta data lock , 元数据锁，简写MDL。

 MDL加锁过程是系统自动控制，无需显式使用，在访问一张表的时候会自动加上。

**元数据理解为表的表结构。元数据锁的主要作用是维护表元结构的数据一致性，当一张表涉及未提交的事务时，不可以修改这张表的表结构**

在MySQL5.5中引入了MDL，

- 当对一张表进行增删改查的时候，加MDL读锁(共享)
- 当对表结构进行变更操作的时候，加MDL写锁(排他)

常见的SQL操作时，所添加的元数据锁：

![image-20230417142228675](https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-masterimage-20230417142228675.png)

#### 意向锁

为了避免DML在执行时，加的行锁与表锁的冲突。InnoDB中引入了意向锁，使得表锁不用检查每行数据是否加锁，使用意向锁来减少表锁的检查。

**在对涉及的行加行锁时，同时也会对该表加上意向锁**

**此时其他客户端在对这张表加表锁的时候，会根据表上所加的意向锁来判定是否可以成功加锁，从而不用逐行判断行锁情况**

- **意向共享锁（IS）：**

  语句`select ... lock in share mode`添加

  与表锁共享锁 (read)兼容，与表锁排他锁(write)互斥

- **意向排它锁（IX）：**

  语句`insert`、`update`、`delete`、`select...for update`添加

  与表锁共享锁(read)及排他锁(write)都互斥，意向锁之间不会互斥

- 读读兼容，读写互斥，写写互斥，意向锁间不互斥

查看意向锁及行锁的加锁情况：

```mysql
select object_schema,object_name,index_name,lock_type,lock_mode,lock_data 
from
performance_schema.data_locks;
```

### 行级锁

#### 介绍

行级锁，每次操作锁住对应的行数据。

锁定粒度最小，发生锁冲突的概率最低，并发度最高。

应用在 InnoDB存储引擎中。

InnoDB的数据是基于索引组织的，行锁是通过对索引上的**索引项加锁**来实现的，而不是对记录加的锁。

对于行级锁，主要分为以下三类：

- **行锁（Record Lock）：**

  锁定单个行记录的锁，防止其他事务对此行进行update和delete。在 RC（读已提交）、RR（可重复读）隔离级别下都支持

  ![image-20230417143516854](https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-masterimage-20230417143516854.png)

- **间隙锁（Gap Lock）：**

  锁定索引记录间隙（不含该记录），确保索引记录间隙不变，防止其他事务在这个间隙进行insert，产生幻读。

  在RR隔离级别下都支持。

  ![image-20230417143626039](https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-masterimage-20230417143626039.png)

- **临建锁（Next-Key Lock）：**

  行锁和间隙锁组合，同时锁住数据，并锁住数据前面的间隙Gap。 在RR隔离级别下支持。

#### 行锁

InnoDB实现了以下两种类型的行锁：

- **共享锁（S）：**允许一个事务去读一行，阻止其他事务获得相同数据集的排它锁

- **排它锁（X）：**允许获取排他锁的事务更新数据，阻止其他事务获得相同数据集的共享锁和排他锁

- 兼容度：

  ![image-20230417143937375](https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-masterimage-20230417143937375.png)

常见的SQL语句，在执行时，所加的行锁如下：

![image-20230417144003941](https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-masterimage-20230417144003941.png)

默认情况下，InnoDB在REPEATABLE READ事务隔离级别运行，InnoDB使用 next-key 锁进行搜索和索引扫描，以防止幻读。 

**针对唯一索引进行检索时，对已存在的记录进行等值匹配时，将会自动优化为行锁。** 

**InnoDB的行锁是针对于索引加的锁，不通过索引条件检索数据，那么InnoDB将对表中的所有记录加锁，此时就会升级为表锁。**

可以通过以下SQL，查看意向锁及行锁的加锁情况：

```mysql
select object_schema,object_name,index_name,lock_type,lock_mode,lock_data 
from
performance_schema.data_locks
```

#### 间隙锁&临键锁

默认情况下，InnoDB在 REPEATABLE READ事务隔离级别运行，InnoDB使用 next-key 锁进行搜索和索引扫描，以防止幻读。

- 索引上的等值查询（唯一索引），给不存在的记录加锁时, 行锁优化为间隙锁。

- 索引上的等值查询(非唯一普通索引)，向右遍历至最后一个满足结果的值时，next-key lock 退化为间隙锁。

  ![image-20230417144714519](https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-masterimage-20230417144714519.png)

- 索引上的范围查询(唯一索引)，会访问到不满足条件的第一个值为止。

> 注意：间隙锁唯一目的是防止其他事务插入间隙。
>
> 间隙锁可以共存，一个事务采用的间隙锁不会阻止另一个事务在同一间隙上采用间隙锁。
>
> 间隙锁解决了可重复度隔离级别下当前读的幻读问题（快照读的幻读通过MVCC解决）

## InnoDB引擎

### 逻辑存储结构

InnoDB的逻辑存储结构如下图所示：

![image-20221027093040719](https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-masterimage-20221027093040719.png)

- **表空间（TableSpace）：**

  表空间是InnoDB存储引擎逻辑结构的最高层。

  如果用户启用了参数innodb_file_per_table(在8.0版本中默认开启) ，则每张表都会有一个表空间（xxx.ibd）

  一个mysql实例可以对应多个表空间，用于存储记录、索引等数据（用段来存储）

- **段（Segment）：**

  段，分为：

  - 数据段（Leaf node segment）
  - 索引段（Non-leaf node segment）
  - 回滚段 （Rollback segment）

  InnoDB是索引组织表，数据段就是B+树的叶子节点， 索引段即为B+树的非叶子节点。

  段用来管理多个Extent（区）

- **区（Extent）：**

  区，表空间的单元结构，每个区的大小为1M，默认情况下，InnoDB存储引擎区中一共有64个连续的页

- **页（Page）：**
  页，是InnoDB存储引擎磁盘管理的最小单元，每个页的大小默认为16kb；

  为了保证页的连续性，InnoDB存储引擎每次从磁盘申请4-5个区

- **行（Row）：**

  行，InndoDB 存储引擎数据是按行进行存放的

  在行中，默认有两个隐藏字段：

  - `Trx_id`：每次对某条记录进行改动时，都会把对应的事务id赋值给trx_id隐藏列
  - `Roll_pointer`：每次对某条记录进行改动时，都会把旧的版本写入到undo日志中，然后这个隐藏列就相当于一个指针，可以通过它来找到该记录修改前的信息

![image-20230526153731555](https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-master202305261537745.png)

### 架构

> MySQL5.5 版本开始，默认使用InnoDB存储引擎，它擅长事务处理，具有崩溃恢复特性，在日常开发中使用非常广泛。
>
> 下面是InnoDB架构图，左侧为内存结构，右侧为磁盘结构
>
> ![img](https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-master202305261615769.png)

#### 内存架构

![image-20230526160328952](https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-master202305261603045.png)

InnoDB的内存结构，主要分为四块

- **Buffer Pool：缓冲池** 
- **Change Buffer：更改缓冲区**
- **Adaptive Hash Index：自适应哈希索引**
- **Log Buffer：日志缓冲区**

##### Buffer Pool

> InnoDB存储引擎基于磁盘文件存储，访问物理硬盘和在内存中进行访问，速度相差很大。
>
> 为了尽可能弥补这两者之间的I/O效率的差值，就需要把经常使用的数据加载到缓冲池中，避免每次访问都进行磁盘I/O。
>
> 在InnoDB的缓冲池中不仅缓存了索引页和数据页，还包含了undo页、插入缓存、自适应哈希索引，以及InnoDB的锁信息等等。

**缓冲池 Buffer Pool，是主内存中的一个区域，里面可以缓存磁盘上经常操作的真实数据**，在执行增删改查操作时，先操作缓冲池中的数据（若缓冲池没有数据，则从磁盘加载并缓存），然后再以一定频率刷新到磁盘，从而减**少磁盘IO，加快处理速度。**

**缓冲池以Page页为单位，底层采用链表数据结构管理Page。根据状态，将Page分为三种类型：**

- **free page：**空闲page，未被使用
- **clean page：**被使用page，数据没有被修改过
- **dirty page：**脏页，被使用page，数据被修改过，也就是缓冲中的数据和磁盘的数据产生了不一致

> 在专用服务器上，通常将多达80％的物理内存分配给缓冲池。
>
> 参数设置：`show variables like 'innodb_buffer_pool_size'`

##### Change Buffer

**Change Buffer，更改缓冲区（针对于非唯一的二级索引页）**

在执行DML语句时，如果这些数据Page没有在Buffer Pool中，不会直接操作磁盘，而会将数据变更存在更改缓冲区 Change Buffer中，在未来数据被读取时，再将数据合并恢复到Buffer Pool中，再将合并后的数据刷新到磁盘中。

**Change Buffer的意义：**

二级索引结构图：

![image-20230526155141495](https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-master202305261551602.png)

与聚集索引不同，二级索引通常是非唯一的，并且数据（主键和索引值）以相对随机的顺序插入二级索引。

同样，删除和更新可能会影响索引树中不相邻的二级索引页。如果每一次都操作磁盘，会造成大量的磁盘IO。

**有了ChangeBuffer之后，我们可以在缓冲池中进行合并处理，减少磁盘IO。**

##### Adaptive Hash Index

**自适应hash索引，用于优化对Buffer Pool数据的查询。**

MySQL的**innoDB引擎中虽然没有直接支持hash索引**，但是提供了一个自适应hash索引的功能。

hash索引在进行等值匹配时，一般性能是要高于B+树的，因为hash索引一般只需要一次IO即可；而B+树，可能需要几次匹配，所以hash索引的效率要高。

但是hash索引又不适合做范围查询、模糊匹配等。 

**InnoDB存储引擎会监控对表上各索引页的查询，如果观察到在特定的条件下hash索引可以提升速度，则建立hash索引，称之为自适应hash索引。** 

自适应哈希索引，无需人工干预，是系统根据情况自动完成。

> 自适应哈希索引开关的参数：`adaptive_hash_index`
>
> `show variables like '%hash_index%'`
>
> ![image-20230526155737445](https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-master202305261557555.png)
>
> InnoDB默认开启自适应哈希索引

##### Log Buffer

**Log Buffer：日志缓冲区，用来保存要写入到磁盘中的log日志数据（redo log 、undo log）， 默认大小为16MB。**

日志缓冲区的日志会定期刷新到磁盘中。如果需要更新、插入或删除许多行的事务，增加日志缓冲区的大小可以节省磁盘 I/O。

> **参数：**
>
> - `innodb_log_buffer_size`：缓冲区大小
>
> -  `innodb_flush_log_at_trx_commit`：
>
>   日志刷新到磁盘时机，取值主要包含以下三个：
>
>   - 1，日志在每次事务提交时写入并刷新到磁盘，默认值。
>   - 0，每秒将日志写入并刷新到磁盘一次。
>   - 2，日志在每次事务提交后写入，并每秒刷新到磁盘一次
>
> `show variables like 'innodb_flush_log_at_trx_commit'`：
>
> ![image-20230526160222538](https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-master202305261602626.png)

#### 磁盘架构

![image-20230526160527120](https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-master202305261605232.png)

##### System Tablespace

**系统表空间，是更改缓冲区的存储区域。**

(在MySQL5.x版本中还包含InnoDB数据字典、undolog等)

如果表是在系统表空间创建（而不是在每个表文件或通用表空间中创建的，即独立表空间关闭），它也可能包含表和索引数据。

> 参数：`innodb_data_file_path`
>
> ![image-20230526160920202](https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-master202305261609298.png)
>
> 系统表空间，默认的文件名叫ibdata1

##### File-Per-Table Tablespace

**独立表空间**，如果开启了innodb_file_per_table开关，**则每个表的文件表空间包含单个InnoDB表的数据和索引，并存储在文件系统上的单个数据文件中。**

> 开关参数：`innodb_file_per_table`，该参数默认开启

##### General Tablespaces

**通用表空间**，需要通过 CREATE TABLESPACE 语法创建通用表空间，在创建表时，可以指定该表空间。

**创建表空间：**

```mysql
CREATE TABLESPACE ts_name ADD DATAFILE 'file_name' ENGINE = engine_name;
```

**创建表时指定表空间：**

```mysql
 CREATE TABLE xxx ... TABLESPACE ts_name;
```

##### Undo Tablespaces

撤销表空间，MySQL实例在初始化时会自动创建两个默认的undo表空间（初始大小16M），用于存储undo log日志。

##### Temporary Tablespaces

InnoDB使用会话临时表空间和全局临时表空间。

存储用户创建的临时表等数据

#####  Doublewrite Buffer Files

双写缓冲区，innoDB引擎将数据页从Buffer Pool刷新到磁盘前，先将数据页写入双写缓冲区文件中，便于系统异常时恢复数据。

![image-20230526161746261](https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-master202305261617341.png)

##### Redo Log

重做日志，是用来实现事务的持久性。

该日志文件由两部分组成：重做日志缓冲（redo log buffer）以及重做日志文件（redo log）

前者是在内存中，后者在磁盘中。

当事务提交之后会把所有修改信息都会存到该日志中, 用于在刷新脏页到磁盘时发生错误, 进行数据恢复使用。 

以循环方式写入重做日志文件，涉及两个文件：

![image-20230526161844186](https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-master202305261618263.png)

#### 后台线程

![image-20230526162611877](https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-master202305261626050.png)

后台线程的作用是将缓冲区的数据在适时的时候刷入到磁盘中

**InnoDB的后台线程分为四类：**

- **Master Thread**

  核心后台线程，负责调度其他线程，还负责将缓冲池中的数据异步刷新到磁盘中, 保持数据的一致性，还包括脏页的刷新、合并插入缓存、undo页的回收

- **IO Thread**

  在InnoDB存储引擎中大量使用了AIO（异步IO）来处理IO请求, 这样可以极大地提高数据库的性能。

  而IO Thread主要负责这些IO请求的回调。

  |       线程类型       | 默认个数 |            职责            |
  | :------------------: | :------: | :------------------------: |
  |     Read thread      |    4     |         负责读操作         |
  |     Write thread     |    4     |         负责写操作         |
  |      Log thread      |    1     | 负责将日志缓冲区刷新到磁盘 |
  | Insert buffer thread |    1     |  负责将写缓冲区刷新到磁盘  |

  > `show engine innodb status \G;`：查看Innodb状态信息，其中包含IO thread
  >
  > ![image-20230526162502619](https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-master202305261625723.png)

- **Purge Thread**

  主要用于回收已经提交的事务的undo log。

  在事务提交之后，undo log可能不用，就用它来回收

- **Page Cleaner Thread**

  协助Master Thread刷新脏页到磁盘的线程，它可以减轻Master Thread的工作压力，减少阻塞。

### 事务原理

#### 事务基础

事务是一组操作的集合，它是一个不可分割的工作单位，事务会把所有的操作作为一个整体一起向系统提交或撤销操作请求，即这些操作要么同时成功，要么同时失败。

**特性：**ACID

- **原子性（Atomicity）：**

  事务是不可分割的最小操作单元，要么全部成功，要么全部失败。

- **一致性（Consistency）：**

  事务完成时，必须使所有的数据都保持一致状态。

- **隔离性（Isolation）：**

  数据库系统提供的隔离机制，保证事务在不受外部并发操作影响的独立环境下运行。

- **持久性（Durability）：**

  事务一旦提交或回滚，它对数据库中的数据的改变就是永久的。

**事务的实现原理：**

![image-20230526163648138](https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-master202305261636237.png)

#### bin log

> bin log不是InnoDB引擎特性，是数据用于数据恢复和主从复制的，但属于三大日志（面试比较重要）

**binlog用于记录数据库执行的写入性操作(不包括查询)信息，以二进制的形式保存在磁盘中。**

**binlog是mysql的逻辑日志，并且由Server层进行记录**，使用任何存储引擎的mysql数据库都会记录binlog日志。

- 逻辑日志：可以简单理解为记录的就是sql语句。
- 物理日志：因为mysql数据最终是保存在数据页中的，物理日志记录的就是数据页变更，即数据最终的+1-1的原始逻辑（不包含向where那样定位用的逻辑）。

binlog是通过追加的方式进行写入的，可以通过max_binlog_size参数设置每个binlog文件的大小，当文件大小达到给定值之后，会生成新的文件来保存日志。

**应用场景：**

- 主从复制：在Master端开启binlog，然后将binlog发送到各个Slave端，Slave端重放binlog从而达到主从数据一致。
- 数据恢复：通过使用mysqlbinlog工具来恢复数据。

#### redo log

**重做日志，记录的是事务提交时数据页的物理修改，是用来实现事务的持久性。** 

**redo log实际上记录数据页的变更，即物理日志**

**redo log包括两部分：**

- 内存中的日志缓冲(redo log buffer)
- 磁盘上的日志文件(redo log file)。

**WAL(Write-Ahead Logging) 技术：**

mysql每执行一条DML语句，先将记录写入redo log buffer，后续某个时间点再一次性将多个操作记录写到redo log file(刷新redo log到磁盘，而不是buffer pool中的脏页)。

> **为什么每次提交事务要刷新redo log而不是buffer pool中的脏页？**
>
> 因为在业务操作中，操作数据一般都是随机读写磁盘的，而不是顺序读写磁盘。 
>
> 而redo log在往磁盘文件中写入数据，由于是日志文件，所以都是顺序写的。
>
> 顺序写的效率，要远大于随机写。 
>
> 这种先写日志的方式，称之为WAL（Write-Ahead Logging）。

**redo log解决的问题：**

- 缓冲区中修改的数据，即脏页，会在一段时间后通过后台线程刷新到磁盘中

- **如果脏页在刷新的过程中出错，但事务已经提交，就会破坏事务的持久性**

- 引入redo log，对缓冲区的数据进行增删改时，会将操作的数据页的变化，记录到redo log buffer中。

- **如果在脏页刷新时出错，可以通过redo log进行恢复，从而保证事务的持久性**

  ![image-20230526164508257](https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-master202305261645360.png)

- 而如果脏页成功刷新到磁盘，或者涉及到的数据已经落盘，此时redolog就没有作用了，就可以删除了。

  所以存在的两个redolog文件是循环写的。

![img](https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-masterv2-6db2466f157a91021b28f70adbe79791_r.jpg)

#### undo log

**回滚日志，用于记录数据被修改前的信息，用来实现事务的原子性 **

作用包含两个 :

- 提供回滚(保证事务的原子性) 
- MVCC(多版本并发控制) 。

**undo log和redo log记录物理日志不一样，是逻辑日志。**

可以认为当delete一条记录时，undo log中会记录一条对应的insert记录，反之亦然。

当执行rollback时，就可以从undo log中的逻辑记录读取到相应的内容并进行回滚。

**undo log销毁**：

undo log在事务执行时产生，事务提交时，并不会立即删除undo log，因为这些日志可能还用于MVCC。 

**undo log存储**：

undo log采用段的方式进行管理和记录，存放在rollback segment回滚段中，内部包含1024个undo log segment。

#### 两阶段提交

假设执行一条SQL语句：

1. 写入redo log，处于prepare状态
2. 写bin log
3. 修改redo log，状态为commit

**原因：**

- 先写redo log后写bin log，若中间系统崩溃，事务有效，数据写入正常，但基于bin log做数据恢复和主从复制缺少该事务，数据不一致
- 先写bin log 后写redo log，若中间系统崩溃，该事务无效，但基于bin log做数据恢复和主从复制会多出该事务，数据不一致

**崩溃恢复：**

- redo log 里面的事务是完整的，也就是已经有了 commit 标识，则直接提交；

- 如果 redo log 里面的事务只有完整的 prepare，则判断对应的事务 binlog 是否存在并完整：

  - 是，则提交事务；

  - 否，回滚事务。

    由于此时 binlog 还没写，redo log 也还没提交，所以崩溃恢复的时候，这个事务会回滚。这时候，binlog 还没写，所以也不会传到备库。

#### MVCC

**多版本并发控制（MVCC，Multi-Version Concurrency Control）可以解决读-写并发冲突（非阻塞读），提高并发性能。**

> MVCC不解决【可重复读】隔离级别下的幻读问题（第一次读和第二次读中间，有新数据插入，第二次读的比第二次读的多）
>
> 幻读问题通过间隙锁解决，即读取的时候锁住数据的间隙，防止插入

简单来说，MVCC就是存储了同一条数据的不同历史版本链，不同事务可以访问不同的数据版本（版本快照）（适合读已提交和可重复读）

**当前读和快照读：**

- **当前读：**

  就是读取记录的当前最新版本，读取时要保证其他事物不能修改记录(增删改查和`select...for update/ lock in share mode`，即加锁)

- **快照读：**

  读取MVCC版本链中的某个快照版本，不需要加锁

  - **读已提交：**ReadView会在事务中的每一个SELECT语句查询发送前生成
  - **可重复读：**ReadView会在事务的第一个SELECT语句查询发送前生成，且之后的SELECT都是基于这个ReadView
  - **串行化：**快照读会退化为当前读

##### 隐藏字段

在InnoDB引擎中，会为表额外增加至少两个字段：

- `DB_TRX_ID`：创建或者修改的数据的事务ID
- `DB_ROLL_PTR`：回滚指针，指向记录的上一个版本（在undo log中）
- `DB_ROW_ID`：隐藏主键，如果表结构没有指定主键，将会生成该隐藏字段

多版本数据链使用UNDO（回滚）日志实现，回滚日志存储了修改值的记录的原值（即旧版本）

> `ibd2sdi ibd文件`查看表空间文件，可以找到表的隐藏字段

##### 版本链

在修改数据的时候，

MySQL除了执行redo log（物理日志）和bin log（逻辑日志）的两阶段提交，还会记录一个undo log，用于事务回滚；

- 当insert时，产生的undo log日志只在回滚时需要，在事务提交后，可被立即删除
- 当update/delete时，产生的undo log日志不仅在回滚时需要，在快照读时也需要，不会被立即删除

MVCC的版本链就是在undo log上形成的；

![image-20230526171129789](https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-master202305261711951.png)

##### 版本快照

**Read View 读视图**

读视图是事务进行快照读产生的读视图，记录并维护系统当前活跃事务的id数组（事务先后顺序通过事务id大小判断）

**ReadView中包含了4个核心字段：**

|       字段       |                      含义                      |
| :--------------: | :--------------------------------------------: |
|     `m_ids`      |              当前活跃的事务ID集合              |
|   `min_trx_id`   |                 最小活跃事务id                 |
|   `max_trx_id`   | 预分配事务ID，当前最大活跃事务ID+1(事务ID自增) |
| `creator_trx_id` |             ReadView创建者的事务ID             |

##### 快照生成

某个事务进行快照读时可以读到哪个版本的数据，ReadView有一套算法：

- 版本未提交，不可见
- 版本已提交，在视图创建后提交，不可见
- 版本已提交，在视图创建前提交，可见

当查询发生时生成ReadView，查询操作沿着undo log链表从最新版本向老版本遍历：

- `trx_id == creator_trx_id`

  当前事务id == 创建快照版本事务id

  可以访问该版本，因为数据是当前事务更改的

- `trx_id < min_trx_id`

  当前事务id < 最小活跃事务id

  可以访问，说明数据已经提交了

- `trx_id > max_trx_id`

  当前事务id > 预分配事务id

  不可以访问该版本，因为当前事务id是在快照版本生成之后才开启的

- `min_trx_id <= trx_id <= max_trx_id && trx_id NOT IN (m_ids)`

  当前事务id在最小最大活跃事务之间，且不在当前活跃的事务ID集合中，可以访问该版本

![img](https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-master8485522-d5afd5fcad183f18.png)

- **上面最大最小的指针标反了**
- 绿色范围可以访问（delete_flag也不应为true，否则这个版本是已经被删除的）
- 浅绿色范围只能访问自己修改的未commit版本
- 蓝色范围不允许访问

**总结：**只有当前事务修改的未commit版本和所有已提交事务版本允许被访问

**注意：**UPDATE操作是基于当前读的值进行修改，而不是视图

# 【个人总结】

## redo log、buffer pool

- **redo log：**

  采用WAL，write ahead log，对缓冲区的增删改，会先写入redo log buffer，目的是为了减少磁盘写

  1. redolog和binlog都是顺序写，磁盘顺序写比随机写速度要快
  2. 组提交机制，大幅度降低磁盘的IOPS消耗

  redo log 用于实现事务的持久性，通过**两阶段提交**解决数据库的崩溃恢复：

  - **将redo log buffer中的数据写入redo log file，prepare状态**
  - **提交bin log**
  - **提交redo log file，commit状态**

  脏页刷新入磁盘，不是redo log做的，redo log只是帮助数据恢复

- buffer Pool

  是主内存中的一个区域，里面可以缓存磁盘上经常操作的真实数据，**增删改查时先操作缓冲池中的数据（修改的数据称为脏页），在一定策略刷新到磁盘中，目的是为了减少磁盘IO**



**bin log写入策略：**

事务执行时写入binlog cache，事务提交时写入binlog文件：实际上是先将binlog cache刷入文件系统的page cache，再按照策略异步刷入磁盘）

**binlog 什么时候刷新到磁盘跟参数 sync_binlog 相关。**

- 如果设置为0，则表示MySQL不控制binlog的刷新，由文件系统去控制它缓存的刷新；
- 如果设置为不为0的值，则表示每 sync_binlog 次事务，MySQL调用文件系统的刷新操作刷新binlog到磁盘中。（设置为N表示累计N个事务才fsync）
- 设为1是最安全的，在系统故障时最多丢失一个事务的更新，但是会对性能有所影响。

**redo log写入策略：**

与binlog一样，事务执行时写入redo log cache，然后write到文件系统化的page cache，最后由文件系统fsync（持久化）到磁盘（InnoDB有一个后台线程，每隔一秒轮询上诉操作）

**写入策略：innodb_flush_log_at_trx_commit参数**

- 0，事务每次提交都只留在redo log buffer
- 1，事务每次提交都持久化到磁盘
- 2，事务每次提交都写到page cache



**为了让组提交的效果更好，mysql做了优化：**

- redo log prepare，write，写入page cache

- binlog write，写入page cache

- redo log prepare，fsync，page cache持久化到磁盘

- binlog fsync，page cache持久化到磁盘

  **如果第四步执行的时候，有多个事务完成了第二步操作，这里就会通过组提交将他们一起持久化**

- redolog commit



## 唯一索引、普通索引

- 唯一索引和普通索引在更新时，非空和查找判断下一条记录的操作对性能影响微乎其微

- **记录要更新的目标页不在内存中时：**

  - 唯一索引需要从磁盘中读取目标页到buffer pool中，判断会否有非空冲突再插入值，语句执行结束。
  - 普通索引则是将数据的修改记录到change buffer中，在按照策略刷新到buffer pool中，语句执行结束

  将磁盘读入内存涉及随机IO的访问，是数据库里面成本很高的操作之一，所以相比之下普通索引性能会更好。

## 加锁原则

**两段锁协议：**所有的事务必须分两个阶段对数据项加锁和解锁。

即事务分两个阶段，第一个阶段是获得封锁。事务可以获得任何数据项上的任何类型的锁，但是不能释放；第二阶段是释放封锁，事务可以释放任何数据项上的任何类型的锁，但不能申请。

- 第一阶段是获得封锁的阶段，称为扩展阶段：其实也就是该阶段可以进入加锁操作，在对任何数据进行读操作之前要申请获得S锁，在进行写操作之前要申请并获得X锁，加锁不成功，则事务进入等待状态，直到加锁成功才继续执行。就是加锁后就不能解锁了。

- 第二阶段是释放封锁的阶段，称为收缩阶段：当事务释放一个封锁后，事务进入封锁阶段，在该阶段只能进行解锁而不能再进行加锁操作。

**加锁原则：**

- 原则 1：加锁的基本单位是 next-key lock。next-key lock 是左开右闭区间(select、update都是加锁)

  - next-key lock加锁分两段执行，先加间隙锁，再加行锁

- 原则 2：查找过程中访问到的对象（索引）才会加锁。

- 优化 1：索引上的等值查询，给唯一索引加锁的时候，next-key lock 退化为行锁；如果不是唯一索引，需要访问到第一个不满足条件的值，此时next-key lock会退化为间隙锁；

- 优化 2：索引上的等值查询，向右遍历时且最后一个值不满足等值条件的时候，next-key lock 退化为间隙锁（条件已经不满足，不需要加行锁）

  ```mysql
  -- values (5,5,5)、(10,10,10)、(15,15,15)
  -- 锁住c索引树上的(5,10]和(10,15)
  select id from t where c = 10 lock in share mode;
  ```

- 一个bug：唯一索引上的范围查询，会访问到不满足条件的第一个值为止

**优化总结：**

- 删除数据的时候尽量加limit，不仅可以控制删除数据的条数，让操作更安全，还可以减小加锁的范围（缩小右侧的间隙锁）

- 

