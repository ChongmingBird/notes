# 【MySQL基础篇】

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

​	在这种隔离级别下，一个事务会读到另一个事务更新后但未提交的数据，如果另一个事务回滚，那么当前事务读到的数据就是脏数据，这就是脏读（Dirty Read）

##### Read Committed

​	在Read Committed隔离级别下，一个事务可能会遇到不可重复读（Non Repeatable Read）的问题。

​	不可重复读是指，在一个事务内，多次读同一数据，在这个事务还没有结束时，如果另一个事务恰好修改了这个数据，那么，在第一个事务中，两次读取的数据就可能不一致。

##### Repeatable Read

​	在Repeatable Read隔离级别下，一个事务可能会遇到幻读（Phantom Read）的问题。

​	幻读是指，在一个事务中，第一次查询某条记录，发现没有，但是，当试图更新这条不存在的记录时，竟然能成功，并且，再次读取同一条记录，它就神奇地出现了。

##### Serializable

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

**MySQL索引数据结构对经典的B+Tree进行了优化。在原B+Tree的基础上，增加一个指向相邻叶子节点的链表指针，就形成了带有顺序指针的B+Tree，提高区间访问的性能，利于范围查询和排序。**

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

如上图所示的联合索引，若需使索引生效，查询时最左边的列，也就是profession字段必须存在，且若无age字段，则status会失效

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
  -- 索引长度为47，索引仅上校profession
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

## 视图

## 存储过程

## 触发器

## 锁

## InnoDB引擎

## MySQL管理

































