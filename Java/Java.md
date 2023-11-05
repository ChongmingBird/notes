# 【IDEA】

## 快捷键

|     快捷键     |               操作名称               |
| :------------: | :----------------------------------: |
|       F7       |         单步调试（进入函数）         |
|       F8       |        单步调试（不进入函数）        |
|    Shift+F7    |           选择要进入的函数           |
|    Shift+F8    |               跳出函数               |
|     Alt+F8     |          执行表达式查看结果          |
|     Alt+F9     |              运行到断点              |
|       F9       | 继续执行，进入下一个断点或执行完程序 |
|   Shift+F10    |                 运行                 |
|     Ctrl+D     |             复制到下一行             |
| Ctrl+Alt+Enter |               创建空行               |
|     ALT+←      |            返回上一级源码            |
|    Ctrl+F12    |           查看源码方法列表           |
|  Ctrl+Shift+=  |             展开所有代码             |
|  Ctrl+Shift+-  |             折叠所有代码             |
|     ALT+7      |             查看类结构图             |
|     Ctrl+h     |             查看类继承树             |
|                |                                      |
|                |                                      |
|                |                                      |

# 【String】

## StringBuilder

![image-20230203150210935](https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-masterimage-20230203150210935.png)

- Java特殊处理：打印StringBuilder打印的是属性值而不是地址值

## StringJoiner

![image-20230203151027789](https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-masterimage-20230203151027789.png)

![image-20230203151045457](https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-masterimage-20230203151045457.png)



# 【接口】

## 接口升级

**JDK8**

![image-20230203153422248](https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-masterimage-20230203153422248.png)



![image-20230203153727786](https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-masterimage-20230203153727786.png)

**JDK9**

![image-20230203153839108](https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-masterimage-20230203153839108.png)

- 普通的私有方法为默认方法服务
- 静态的私有方法为静态方法服务

## 适配器设计模式

- 设计模式：一套被反复使用、多数人知晓的、经过分类编目的代码设计经验的总结（套路）

**适配器设计模式：** 当一个接口中抽放方法过多，但只需要使用其中一部分时。 

在接口和实现类中添加一个类`InterAdapeter`(可以创建为抽象接口)，对接口中的所有方法进行空实现。

当想要实现接口中的某一方法，可以让实现类继承适配器类，并实现指定方法。

为了避免其他类创建适配器类的对象，中间的适配器类用abstract进行修饰。

# 【内部类】

## 分类

- 成员内部类：写在成员位置，属于外部类的成员

  ```java
  class car {
      int a;
      // 成员内部类，可以被一些修饰符修饰
      // 用static修饰的是静态内部类
      // JDK16之前不能定义静态变量
      class enginer {
          int b;
      }
  }
  ```

- 静态内部类：

  

- 局部内部类

- **匿名内部类**

## 匿名内部类

![image-20230203161437291](https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-masterimage-20230203161437291.png)

**匿名的这个类就是`{}`的部分，`new Inter()`是创建了这个匿名的类，即实现了Inter接口的类**

# 【常用类】

## Math

![image-20230204134006059](https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-masterimage-20230204134006059.png)

```java
public static int abs(int a)					// 返回参数的绝对值
public static double ceil(double a)				// 返回大于或等于参数的最小整数
public static double floor(double a)			// 返回小于或等于参数的最大整数
public static int round(float a)				// 按照四舍五入返回最接近参数的int类型的值
public static int max(int a,int b)				// 获取两个int值中的较大值
public static int min(int a,int b)				// 获取两个int值中的较小值
public static double pow (double a,double b)	// 计算a的b次幂的值
public static double random()					// 返回一个[0.0,1.0)的随机值
```

## System

![image-20230204133941556](https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-masterimage-20230204133941556.png)

```java
// 获取当前时间所对应的毫秒值（当前时间为0时区所对应的时间即就是英国格林尼治天文台旧址所在位置）
public static long currentTimeMillis()		
// 终止当前正在运行的Java虚拟机，0表示正常退出，非零表示异常退出
public static void exit(int status)
// 进行数组元素copy
public static native void arraycopy(Object src,  int  srcPos, Object dest, int destPos, int length);
```



## Runtime

![image-20230204133603105](https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-masterimage-20230204133603105.png)

## Object

- Object是Java中的顶级父类。所有的类都直接或简介地继承与Object类
- Object类中的方法可以被所有子类访问

**构造方法：无参构造**

![image-20230204134755119](https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-masterimage-20230204134755119.png)

可以重写Object类的成员方法满足需要

- `toString()`：默认打印地址值

- `equals()`：默认比较地址值

  - String中的equals方法，先判断参数是否为字符串，再比较内部属性。非字符串直接返回false
  - StringBuilde使用Object的equals方法

- `clone()`：把A对象属性值完全拷贝给B对象

  - **浅克隆：** 基本数据类型拷贝值，引用数据类型拷贝地址(Object类中的方法为浅克隆)

  - **深克隆:** 基本数据类型拷贝值，引用数据类型重新创建新对象

  - **使用方法：**

    1. 重写Object的clone方法（父类为protected方法，无法在lang包外使用）
    2. 让javabean类实现Cloneable接口(标记该类可克隆)

  - **第三方深克隆工具：** Gson.jar

    ```java
    // Student st1, st2;
    Gson gson = new Gson();
    String s = gson.toJson(st1)
    st2 = gson.fromJson(s, Student.class)
    ```

## Objects

**Objects是一个工具类，提供了一些工具方法**

![image-20230204142355425](https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-masterimage-20230204142355425.png)

## BigInteger

![image-20230204143535097](https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-masterimage-20230204143535097.png)

* 如果BigInteger表示的数字没有超出long的范围，可以用静态方法获取。

  ```java
  // 静态方法获取BigInteger的对象，内部有优化
  // 1.能表示范围比较小，只能在long的取值范围之内
  // 2.在内部对常用的数字: -16 ~ 16 进行了优化
  // 提前把-16~16 先创建好BigInteger的对象，如果多次获取不会重新创建新的。
  BigInteger bd5 = BigInteger.valueOf(16);
  BigInteger bd6 = BigInteger.valueOf(16);
  System.out.println(bd5 == bd6); //true
  ```

* 如果BigInteger表示的超出long的范围，可以用构造方法获取。

* 对象一旦创建，BigInteger内部记录的值不能发生改变。

* 只要进行计算都会产生一个新的BigInteger对象

**成员方法：**

![image-20230204143915977](https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-masterimage-20230204143915977.png)

**底层存储方式：**

```java
public class BigInteger extends Number implements Comparable<BigInteger> {
    // 符号位-1为负，0或1为正
	final int signum;
    /** 
     * BigInteger会把一个很大的数拆分成多段，每段都放入数组中
     * 数组中最多能存储元素个数：21亿多
     * 数组中每一位能表示的数字：42亿多
     * 理论上，BigInteger能表示的最大数字为：42亿的21亿次方
     * 但是还没到这个数字，电脑的内存就会撑爆，所以一般认为BigInteger是无限的
     */
    final int[] mag;
    ...
}
```

BigInteger会把原数转换为二进制补码，然后从右向左32位为一组，再把每组都转换为对应十进制数，存入数组。

![bigInteger的底层原理](https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-masterbigInteger%E7%9A%84%E5%BA%95%E5%B1%82%E5%8E%9F%E7%90%86.png)

## BigDecimal

使用浮点数进行数学运算时，容易产生精度丢失问题

Java就给我们提供了BigDecimal供我们进行数据运算

 ![1576134383441](https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-master1576134383441.png)

![image-20230204150259059](https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-masterimage-20230204150259059.png)

- 使用BigDecimal类型的数据进行除法运算的时候，得到的结果是一个无限循环小数，会报错。可以使用另一个重载的方法

- ```java
  BigDecimal divide(BigDecimal divisor, int scale, int roundingMode)
      
  divisor:		除数对应的BigDecimal对象；
  scale:			精确的位数；
  roundingMode:	取舍模式；
  取舍模式被封装到了RoundingMode这个枚举类中，在这个枚举类中定义了很多种取舍方式。最常见的取舍方式有如下几个：
  // 数轴举例    
  UP(远离零方向舍入), DOWN(向零方向舍入), FLOOR(直接删除), HALF_UP(四舍五入), HALF_DOWN(五舍六入)    
  ```

**底层存储方式：**

BigDecimal把数据看成字符串（包括符号位），遍历得到里面的每一个字符，把这些字符在ASCII码表上的值，都存储到数组中。

![image-20230204151447061](https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-masterimage-20230204151447061.png)

## JDK7时间相关类

### Date

`java.util.Date`类表示特定的瞬间，精确到毫秒。

**构造方法：**

- `public Date()`：从运行程序的此时此刻到时间原点经历的毫秒值,转换成Date对象，分配Date对象并初始化此对象，以表示分配它的时间（精确到毫秒）。
- `public Date(long date)`：将指定参数的毫秒值date转换成Date对象，分配Date对象并初始化此对象，以表示自从标准基准时间（称为“历元（epoch）”，即1970年1月1日00:00:00 GMT）以来的指定毫秒数。
- 简单来说：使用无参构造，可以自动设置当前系统时间的毫秒时刻；指定long类型的构造参数，可以自定义毫秒时刻

> tips: 由于中国处于东八区（GMT+08:00）比格林尼治时间（GMT）快8小时的时区，当格林尼治标准时间为0:00时，东八区的标准时间为08:00。

**常用方法：** Date类中的多数方法已经过时，常用的方法如下

- `public long getTime()` 把日期对象转换成对应的时间毫秒值。
- `public void setTime(long time)` 修改日期对象的毫秒值为方法参数

### SimpleDateFormat

`java.text.SimpleDateFormat` 是日期/时间格式化类，通过这个类可以完成日期和文本之间的转换, **也就是可以在Date对象与String对象之间进行来回转换。**

- **格式化**：按照指定的格式，把Date对象转换为String对象。
- **解析**：按照指定的格式，把String对象转换为Date对象。

**构造方法：**

DateFormat为抽象类，不能直接使用，需要常用的子类`java.text.SimpleDateFormat`。

这个类需要一个模式（格式）来指定格式化或解析的标准。

- `public SimpleDateFormat(String pattern)`：用给定的模式和默认语言环境的日期格式符号构造SimpleDateFormat。参数pattern是一个字符串，代表日期时间的自定义格式。

- **格式规则：**

  - | 标识字母（区分大小写） | 含义 |
    | ---------------------- | ---- |
    | y                      | 年   |
    | M                      | 月   |
    | d                      | 日   |
    | H                      | 时   |
    | m                      | 分   |
    | s                      | 秒   |

  - `yyyy-MM-dd HH:mm:ss` -> `2023-02-04 17:35:26`

  > 备注：更详细的格式规则，可以参考SimpleDateFormat类的API文档。

**常用方法：**

- `public String format(Date date)`：将Date对象格式化为字符串。
- `public Date parse(String source)`：将字符串解析为Date对象。

### Calendar

- `java.util.Calendar`类表示一个“日历类”，可以进行日期运算。

  它是一个抽象类，不能创建对象，可以使用它的子类：`java.util.GregorianCalendar`类。

- 有两种方式可以获取GregorianCalendar对象：

  - 直接创建GregorianCalendar对象；

  - 通过Calendar的静态方法getInstance()方法获取GregorianCalendar对象

    ```java
    /**
     * 底层原理：
     * 根据系统的不同时区获取不同的日历对象
     * 把时间中的纪元、年、月、日等放入一个数组当中
     */
    Calendar calendar = Calendar.getInstance();
    ```

![](https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-masterimage-20230204174457352.png)

**星期：周日(1) 周一(2) 周二(3) …… 周六(7)**

**字段：**

| 常量                   | 含义                                                         |
| :--------------------- | :----------------------------------------------------------- |
| `YEAR`                 | 当前年                                                       |
| `MONTH`                | 当前月(从0开始)                                              |
| `DAY_OF_MONTH`         | 当前日                                                       |
| `DATE`                 | 当前日(与DAY_OF_MONTH意思一样)                               |
| `HOUR_OF_DAY`          | 当前小时(24小时制)                                           |
| `HOUR`                 | 当前小时(12小时制)                                           |
| `MINUTE`               | 当前分钟                                                     |
| `SECOND`               | 当前秒                                                       |
| `DAY_OF_WEEK`          | 当前星期几(用数字（1~7）表示（星期日~星期六），用的时候需要 -1) |
| `AM_PM`                | 当前是上午还是下午(0-上午；1-下午)                           |
| `WEEK_OF_YEAR`         | 当前年的第几周                                               |
| `WEEK_OF_MONTH`        | 当前月的星期数                                               |
| `DAY_OF_WEEK_IN_MONTH` | 当前月的第几个星期                                           |
| `DAY_OF_YEAR`          | 当前年的第几天                                               |

1. 在获取月份时，`Calendar.MONTH + 1` 的原因：
   - Java中的月份遵循了罗马历中的规则：当时一年中的月份数量是不固定的，第一个月是JANUARY。
   - 而Java中Calendar.MONTH返回的数值其实是当前月距离第一个月有多少个月份的数值 **（即Calendar中的月份是0-11）** JANUARY在Java中返回“0”，所以我们需要+1。

2. 在获取星期几 `Calendar.DAY_OF_WEEK – 1` 的原因：
   - Java中`Calendar.DAY_OF_WEEK`其实表示：一周中的第几天，所以他会受到 **第一天是星期几** 的影响
   - 有些地区以星期日作为一周的第一天，而有些地区以星期一作为一周的第一天，需要区分这两种，**当周日为第一天时，所得的星期需要-1**
   - 所以Calendar.DAY_OF_WEEK需要根据本地化设置的不同而确定是否需要 “-1”（默认周日为第一天）
   - Java中设置不同地区的输出可以使用 `Locale.setDefault(Locale.地区名) `来实现。
   - 可以主动设置每周的第一天：`Calendar.setFirstDayOfWeek(Calendar.MONDAY)`

## JDK8时间相关类

![image-20230205154010887](https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-masterimage-20230205154010887.png)

**JDK8中的时间类均为不可变对象，当对一个时间对象进行修改时，均是产生了一个新的对象**

### Date类

#### ZoneId

![image-20230205154229294](https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-masterimage-20230205154229294.png)

```java
// 1.获取所有的时区名称
Set<String> zoneIds = ZoneId.getAvailableZoneIds();
Iterator<String> iterator = zoneIds.iterator();
while (iterator.hasNext()) {
    System.out.println(iterator.next());
}

// 2.获取当前系统的默认时区
ZoneId zoneId = ZoneId.systemDefault();
System.out.println(zoneId);//Asia/Shanghai

// 3.获取指定的时区
ZoneId zoneId1 = ZoneId.of("Asia/Pontianak");
System.out.println(zoneId1);// Asia/Pontianak
```

#### Instant

![image-20230205154721530](https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-masterimage-20230205154721530.png)



```java
/**
 * static Instant now() 获取当前时间的Instant对象(标准时间)
 * static Instant ofXxxx(long epochMilli) 根据(秒/毫秒/纳秒)获取Instant对象
 * ZonedDateTime atZone(ZoneIdzone) 指定时区
 * boolean isxxx(Instant otherInstant) 判断系列的方法
 * Instant minusXxx(long millisToSubtract) 减少时间系列的方法
 * Instant plusXxx(long millisToSubtract) 增加时间系列的方法
 */
// 1.获取当前时间的Instant对象(标准时间)
Instant now = Instant.now();
System.out.println(now);

// 2.根据(秒/毫秒/纳秒)获取Instant对象(参数为距时间原点过去的时间)
Instant instant1 = Instant.ofEpochMilli(0L);
System.out.println(instant1); // 1970-01-01T00:00:00.001Z

Instant instant2 = Instant.ofEpochSecond(1L);
System.out.println(instant2); // 1970-01-01T00:00:01Z

Instant instant3 = Instant.ofEpochSecond(1L, 1000000000L);
System.out.println(instant3); 1970-01-01T00:00:02Z

// 3. 指定时区
ZonedDateTime time = Instant.now().atZone(ZoneId.of("Asia/Shanghai"));
System.out.println(time);

// 4.isXxx 判断
Instant instant4=Instant.ofEpochMilli(0L);
Instant instant5 =Instant.ofEpochMilli(1000L);

// 5.用于时间的判断
// isBefore:判断调用者代表的时间是否在参数表示时间的前面
boolean result1=instant4.isBefore(instant5);
System.out.println(result1); // true

// isAfter:判断调用者代表的时间是否在参数表示时间的后面
boolean result2 = instant4.isAfter(instant5);
System.out.println(result2); // false

// 6.Instant minusXxx(long millisToSubtract) 减少时间系列的方法
Instant instant6 =Instant.ofEpochMilli(3000L);
System.out.println(instant6); // 1970-01-01T00:00:03Z

Instant instant7 =instant6.minusSeconds(1);
System.out.println(instant7); // 1970-01-01T00:00:02Z
```

#### ZoneDateTime

> ZoneDateTime  带时区的时间

![image-20230205155726327](https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-masterimage-20230205155726327.png)

### 日期格式化类

#### DateTimeFormatter

> DateTimeFormatter   用于时间的格式化和解析

![image-20230205160121389](https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-masterimage-20230205160121389.png)

```java
// 获取时间对象
ZonedDateTime time = Instant.now().atZone(ZoneId.of("Asia/Shanghai"));
// 解析/格式化器
DateTimeFormatter dtf1=DateTimeFormatter.ofPattern("yyyy-MM-dd HH:mm;ss EE a");
// 格式化
System.out.println(dtf1.format(time));
```

### 日历类

![image-20230205160420472](https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-masterimage-20230205160420472.png)

![image-20230205160401290](https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-masterimage-20230205160401290.png)

![image-20230205160558291](https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-masterimage-20230205160558291.png)

### 工具类

![image-20230205160716072](https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-masterimage-20230205160716072.png)

#### Duration

```java
// 本地日期时间对象。
LocalDateTime today = LocalDateTime.now();
System.out.println(today);

// 指定的的日期时间对象
LocalDateTime birthDate = LocalDateTime.of(2000, 1, 1, 0, 0, 0);
System.out.println(birthDate);

// 第二个参数减第一个参数
Duration duration = Duration.between(birthDate, today);
System.out.println("相差的时间间隔对象:" + duration);

System.out.println("============================================");
System.out.println(duration.toDays());		// 两个时间差的天数
System.out.println(duration.toHours());		// 两个时间差的小时数
System.out.println(duration.toMinutes());	// 两个时间差的分钟数
System.out.println(duration.toMillis());	// 两个时间差的毫秒数
System.out.println(duration.toNanos());		// 两个时间差的纳秒数
```

#### Period

```java
// 当前本地 年月日
LocalDate today = LocalDate.now();
System.out.println(today);

// 指定的 年月日
LocalDate birthDate = LocalDate.of(2000, 1, 1);
System.out.println(birthDate);

// 第二个参数减第一个参数
Period period = Period.between(birthDate, today);

System.out.println("相差的时间间隔对象:" + period);
System.out.println(period.getYears());		// 返回相差的年数
System.out.println(period.getMonths());		// 返回相差的月数(相差1年3个月，得到的是相差3个月)
System.out.println(period.getDays());		// 返回相差的天数
System.out.println(period.toTotalMonths());	// 返回相差的总月数
```

#### ChronoUnit

```java
// 当前时间
LocalDateTime today = LocalDateTime.now();
System.out.println(today);
// 指定时间
LocalDateTime birthDate = LocalDateTime.of(2000, 1, 1,0, 0, 0);
System.out.println(birthDate);

System.out.println("相差的年数:" + ChronoUnit.YEARS.between(birthDate, today));
System.out.println("相差的月数:" + ChronoUnit.MONTHS.between(birthDate, today));
System.out.println("相差的周数:" + ChronoUnit.WEEKS.between(birthDate, today));
System.out.println("相差的天数:" + ChronoUnit.DAYS.between(birthDate, today));
System.out.println("相差的时数:" + ChronoUnit.HOURS.between(birthDate, today));
System.out.println("相差的分数:" + ChronoUnit.MINUTES.between(birthDate, today));
System.out.println("相差的秒数:" + ChronoUnit.SECONDS.between(birthDate, today));
System.out.println("相差的毫秒数:" + ChronoUnit.MILLIS.between(birthDate, today));
System.out.println("相差的微秒数:" + ChronoUnit.MICROS.between(birthDate, today));
System.out.println("相差的纳秒数:" + ChronoUnit.NANOS.between(birthDate, today));
System.out.println("相差的半天数:" + ChronoUnit.HALF_DAYS.between(birthDate, today));
System.out.println("相差的十年数:" + ChronoUnit.DECADES.between(birthDate, today));
System.out.println("相差的世纪(百年)数:" + ChronoUnit.CENTURIES.between(birthDate, today));
System.out.println("相差的千年数:" + ChronoUnit.MILLENNIA.between(birthDate, today));
System.out.println("相差的纪元数:" + ChronoUnit.ERAS.between(birthDate, today));
```

## 包装类

### 概述

> 包装类：基本数据类型的引用类型
>
> - 包装类都是final修饰无法继承
> - 数字类型的父类都是Number
> - 当包装类作为类属性时,其默认值都为Null

Java提供了两个类型系统，基本类型与引用类型。

使用基本类型效率高，然而很多情况，会创建对象使用，因为对象可以做更多的功能

| 基本类型 | 对应的包装类（位于java.lang包中） |
| :------: | :-------------------------------: |
|   byte   |               Byte                |
|  short   |               Short               |
|   int    |              Integer              |
|   long   |               Long                |
|  float   |               Float               |
|  double  |              Double               |
|   char   |             Character             |
| boolean  |              Boolean              |

### 获取包装类对象

- 构造方法：`包装类(参数)`

  - 通过构造方法创建的对象，每一次创建的都是新的对象

- 静态方法：`包装类.valueOf(参数)`

  - 通过静态方法构造的方法，如`Integer.valueOf()`方法基于减少对象创建次数和节省内存的考虑，缓存了[-128,127]（常量池）之间的数字。

    此数字范围内传参则直接返回缓存中的对象。在此之外，通过构造方法创建。

### 常量池

Java虚拟机会默认将一些简单的字面量对象放到常量池中,来减少频繁的内存操作,是一种优化机制

- `String`类型的所有字面量都会进入常量池
- `Byte`、`Short`、`Integer`、`Long`四种类型字面量在大于等于-128且小于等于127时对象将进入常量池
- `Character`类型不能为负数,字面量大于等于0 且 小于等于127时对象将进入常量池
- `Boolean`类型两个字面量都会进入常量池
- `Float`和`Double`类型不会进入常量池

**常量池会在第一次创建范围内的对象时被创建并缓存**

### 自动装箱/拆箱

由于经常要做基本类型与包装类之间的转换，从Java 5（JDK 1.5）开始，基本类型与包装类的装箱、拆箱动作可以自动完成。例如：

```java
// 自动装箱，相当于Integer i = Integer.valueOf(4);
Integer i = 4;
// 等号右边：将i对象转成基本数值(自动拆箱) i.intValue() + 5;
i = i + 5;
// 加法运算完成后，再次装箱，把基本数值转成对象。
```

**自动装箱的时机：**

1. 将包装类型变量直接赋值给对应基础类型时,系统会自动进行拆箱操作
2. 当要访问这个对象的真实数据值时会进行自动拆箱,例如要输出对象的值
3. 当要对包装类的实际值进行数学运算时,会自动拆箱,例如比较大小

需要注意的是,当使用判断是否相等时,符号任意一边是基础数据类型另外一边都会自动拆箱(不会出问题),

但如果**两边都是包装类型，会比较对象引用是否相同** (可能产生问题)

```
Integer i1 = new Integer(10);
Integer i2 = new Integer(10);
System.out.println(i1 == i2); //结果为false
```

### 字符串与数据类型相互转换

**String与包装类的相互转换**

```java
// 使用包装类提供的valueof()方法来将字符串转为对象的包装类
System.out.println(Float.valueOf("1.1ff"));
System.out.println(Boolean.valueOf("true"));

// String提供了静态方法valueOf(),将其他任意类型转换为字符串类型
System.out.println(String.valueOf('c'));
System.out.println(String.valueOf(true));
```

**基本数据类型转换为String类型（Integer为例）：**

- 方式一：直接在数字后加一个空字符串
- 方式二：通过String类静态方法valueOf()

**String类型转换为基本数据类型（Integer为例）：**

- 方式一：先将字符串数字转成Integer，再调用valueOf()方法

- 方式二：通过Integer静态方法parseInt()进行转换

  除Character类之外，其他所有包装类都具有parseXxx静态方法可以将字符串参数转换为对应的基本类型：

  - `public static byte parseByte(String s)`：将字符串参数转换为对应的byte基本类型。
  - `public static short parseShort(String s)`：将字符串参数转换为对应的short基本类型。
  - `public static int parseInt(String s)`：将字符串参数转换为对应的int基本类型。
  - `public static long parseLong(String s)`：将字符串参数转换为对应的long基本类型。
  - `public static float parseFloat(String s)`：将字符串参数转换为对应的float基本类型。
  - `public static double parseDouble(String s)`：将字符串参数转换为对应的double基本类型。
  - `public static boolean parseBoolean(String s)`：将字符串参数转换为对应的boolean基本类型。

# 【泛型】

## 简介

**泛型：** JDK5引入的特性，可以在编译阶段约束操作的数据类型，并进行检查

**泛型的格式：** <数据类型>

**Java中泛型标记符：**

- `E` - Element (在集合中使用，因为集合中存放的是元素)
- `T` - Type（Java 类）
- `K` - Key（键）
- `V` - Value（值）
- `N` - Number（数值类型）
- `?` - 表示不确定的 java 类型

**泛型的好处：**

- 统一数据类型
- 把运行时期遇到的问题提前到了编译时期，从而避免了强制类型转换可能出现的异常，因为这些在编译阶段就能确定下来

**泛型擦除：** `.java`文件中存在泛型，编译时会进行泛型检查，生成的`.class`文件中泛型被转换成了相应的数据类型

**细节：**

- 泛型只支持引用数据类型
- 指定泛型具体类型后，可以传入该类类型以及其子类类型
- 不写泛型，泛型默认是Object，即`<> = <Object>`

## 泛型类

```java
// 类上定义的泛型可以在所有方法上使用
修饰符 class 类名<类型> {
    
}
```

```java
// E相当于一个变量，用于记录数据的类型
public class ArrayList<E> {    
    void add(E e) {
        ...
    }
}
// 相当于将String赋值给E，从而确定list对象的泛型E的数据类型是String
/* ArrayList：
    add(E e) -> add(String e) */
ArrayList<String> list = new ArrayList<>();
```

## 泛型方法

```java
// 定义在方法上的泛型只能在方法体内使用
修饰符 <类型> 返回值类型 方法名(类型 变量名){
    
}
```

```java
public <T> void show (T t){
    T t1 = t;
    System.out.println(t)
}
// 调用show方法
show(String t);
// 【T t1 = t】 -> 【String t1 = t】
```

## 泛型接口

```java
修饰符 interface 接口名<类型> {
    ...
}

public interface List<E> {
    void add(E e);
}

/* 1.实现类给出具体类型 */	
public class MyList1 implements List<String> {
	void add(String e)  
}

/* 2.实现类延续泛型，创建实现类对象时确定具体类型*/
public class MyList2 implements List<E>{
    void add(E e);
} 
MyList2 list = new MyList2<String>();
// add(E e) -> add(String e)
```

## 通配符

- `<? extends E>`：表示可以传递E或E的所有子类型
- `<? super E>`：表示可以传递E或E的所有父类型

# 【正则表达式】

[教程：learn-regex](https://github.com/ziishaned/learn-regex/blob/master/translations/README-cn.md)

[在线测试：regex101](https://regex101.com/)

**IDEA插件：** any-rule

## JavaAPI

`java.util.regex` 是一个用正则表达式所订制的模式来对字符串进行匹配工作的类库包。

它包括两个类：Pattern 和 Matcher。

Pattern 对象是正则表达式编译后在内存中的表示形式，因此，正则表达式字符串必须先被编译为 Pattern 对象，然后再利用该 Pattern 对象创建对应的 Matcher 对象。执行匹配所涉及的状态保留在 Matcher 对象中，多个 Matcher 对象可共享同一个 Pattern 对象。

**典型的调用顺序：**

```java
// 将一个字符串(正则表达)编译成 Pattern 对象
// Pattern构造方法私有，不能直接创建
Pattern p = Pattern.compile("a*b");
// 使用 Pattern 对象创建 Matcher 对象
Matcher m = p.matcher("aaaaab");
boolean b = m.matches(); // 返回true
```

上面定义的 Pattern 对象可以多次重复使用。

如果某个正则表达式仅需一次使用，则可直接使用 Pattern 类的静态 matches() 方法

```java
boolean b = Pattern.matches ("a*b","aaaaab");

// String类型变量可以直接进行正则匹配
"a".matches("[a-zA-Z]");
```

这种语句每次都需要重新编译新的 Pattern 对象，不能重复利用已编译的 Pattern 对象，所以效率不高。

**Matcher常用方法：**

| 名称        | 说明                                                         |
| ----------- | ------------------------------------------------------------ |
| find()      | 返回目标字符串中是否包含与 Pattern 匹配的子串                |
| group()     | 返回上一次与 Pattern 匹配的子串                              |
| start()     | 返回上一次与 Pattern 匹配的子串在目标字符串中的开始位置      |
| end()       | 返回上一次与 Pattern 匹配的子串在目标字符串中的结束位置加 1  |
| lookingAt() | 返回目标字符串前面部分与 Pattern 是否匹配，只有匹配到的字符串在最前面才返回true |
| matches()   | 返回整个目标字符串与 Pattern 是否匹配                        |
| reset()     | 将现有的 Matcher 对象应用于一个新的字符序列。                |

## 元字符

|  元字符   | 描述                                                         |
| :-------: | :----------------------------------------------------------- |
|   **.**   | **句号匹配任意单个字符除了换行符。**                         |
|  **[ ]**  | **字符种类。匹配方括号内的任意字符。**                       |
| **[^ ]**  | **否定的字符种类。匹配除了方括号里的任意字符**               |
|   *****   | **匹配>=0个重复的，在*号之前的字符。**<br />`a*` 匹配0或更多个以a开头的字符。<br />`[a-z]*` 匹配一个行中所有以小写字母开头的字符串。<br />`.*`匹配所有字符 |
|   **+**   | **匹配>=1个重复的+号前的字符。**<br />`c.+t` 匹配以首字母`c`开头以`t`结尾，中间跟着至少一个字符的字符串。 |
|   **?**   | **标记?之前的字符为可选。**<br />`[T]?he` 匹配字符串 `he` 和 `The` |
| **{n,m}** | **匹配num个大括号之前的字符或字符集 (n <= num <= m).**<br />`[0-9]{2,3}` 匹配最少 2 位最多 3 位 0~9 的数字 |
| **(xyz)** | **字符集，匹配与 xyz 完全相等的字符串.**                     |
|  **\|**   | **或运算符，匹配符号前或后的字符.**                          |
|  **\\**   | **转义字符,用于匹配一些保留的字符 `[ ] ( ) { } . * + ? ^ $ \ |`** |
|  **&&**   | **且运算符，匹配符号前和后的交集**                           |
|   **^**   | **从开始行开始匹配.**<br />`^` 用来检查匹配的字符串是否在所匹配字符串的开 |
|   **$**   | **从末端开始匹配.**<br />同理于 `^` 号，`$` 号用来匹配字符是否是最后一个 |

**Java中默认从头匹配到尾，可以省略^和$**

## 简写字符集

| 简写 | 描述                                               |
| ---- | :------------------------------------------------- |
| .    | 除换行符外的所有字符                               |
| \w   | 匹配所有字母数字，等同于 `[a-zA-Z0-9_]`            |
| \W   | 匹配所有非字母数字，即符号，等同于： `[^\w]`       |
| \d   | 匹配数字： `[0-9]`                                 |
| \D   | 匹配非数字： `[^\d]`                               |
| \s   | 匹配所有空格字符，等同于： `[\t\n\f\r\p{Z}]`       |
| \S   | 匹配所有非空格字符： `[^\s]`                       |
| \f   | 匹配一个换页符                                     |
| \n   | 匹配一个换行符                                     |
| \r   | 匹配一个回车符                                     |
| \t   | 匹配一个制表符                                     |
| \v   | 匹配一个垂直制表符                                 |
| \p   | 匹配 CR/LF（等同于 `\r\n`），用来匹配 DOS 行终止符 |

## 零宽度断言（前后预查）

先行断言和后发断言（合称 lookaround）都属于**非捕获组**

用于匹配模式，但不包括在匹配列表中。**即作为匹配的判断条件，但本身不作为被匹配的字符串**

当需要一个模式的前面或后面有另一个特定的模式时，就可以使用它们。

例如，从 `$4.44` 和 `$10.88` 中获得所有以 `$` 字符开头的数字，我们将使用以下的正则表达式 `(?<=\$)[0-9\.]*`。意思是：获取所有前面是 `$` 的`.`或数字。

零宽度断言如下：

| 符号 | 描述            |
| ---- | --------------- |
| ?=   | 正先行断言-存在 |
| ?!   | 负先行断言-排除 |
| ?<=  | 正后发断言-存在 |
| ?<!  | 负后发断言-排除 |

### 正先行断言

`?=...`正先行断言，表示第一部分表达式之后必须跟着 `?=...`定义的表达式。

**返回结果只包含满足匹配条件的第一部分表达式。**

定义一个正先行断言要使用 `()`。在括号内部使用一个问号和等号： `(?=...)`。

正先行断言的内容写在括号中的等号后面。 

例如，表达式 `(T|t)he(?=\sfat)` 匹配 `The` 和 `the`，且其后紧跟着`(空格)fat`。

**注意，匹配到的字符串是`The`或`the`，虽然判断了`  /sfat`**

### 负先行断言

负先行断言 `?!` 用于筛选所有匹配结果，筛选条件为其后不跟随着断言中定义的格式。

 正先行断言定义和负先行断言一样，区别就是 `=` 替换成 `!` 也就是 `(?!...)`。

表达式 `(T|t)he(?!\sfat)` 匹配 `The` 和 `the`，且其后不跟着 `(空格)fat`。

### 正后发断言

正后发断言 记作`(?<=...)` 用于筛选所有匹配结果，筛选条件为其前跟随着断言中定义的格式。 

例如，表达式 `(?<=(T|t)he\s)(fat|mat)` 匹配 `fat` 和 `mat`，且其前跟着`空格`+ `The` 或 `the`。

### 负后发断言

负后发断言 记作 `(?<!...)` 用于筛选所有匹配结果，筛选条件为其前不跟随着断言中定义的格式。 

例如，表达式 `(?<!(T|t)he\s)(cat)` 匹配 `cat`，且其前不跟着`空格`+ `The` 或 `the`。

## 标志

标志也叫模式修正符，因为它可以用来修改表达式的搜索结果

正则表达式是包含在 **两个斜杠`/ /`之间** 的一个或多个字符，在后一个斜杠的后面，可以指定一个或多个选项。 

这些标志可以任意的组合使用，它也是整个正则表达式的一部分。

| 标志 | 描述                                                         |
| ---- | ------------------------------------------------------------ |
| i    | 忽略大小写。                                                 |
| g    | 全局搜索。即不仅仅返回第一个匹配的，而是返回全部             |
| m    | 多行修饰符，执行一个多行匹配<br />可以让锚点元字符 `^` `$` 在每行的起始都进行匹配。 |

- 表达式 `/The/gi` 表示在全局搜索 `The`，在后面的 `i` 将其条件修改为忽略大小写，则变成搜索 `the` 和 `The`，`g` 表示全局搜索。

- 表达式 `/.at(.)?$/gm` 表示除换行符外任意字符+`at`+ 末尾可选除换行符外任意字符。

  根据 `m` 修饰符，现在表达式匹配每行的结尾。

  ```java
  "/.at(.)?$/gm" => The fat    <- 匹配fat
                  cat sat      <- 匹配sat
                  on the mat.  <- 匹配mat
  ```

## 贪婪匹配/惰性匹配

正则表达式默认采用贪婪匹配模式，在该模式下意味着会匹配尽可能长的子串。我们可以使用 `?` 将贪婪匹配模式转化为惰性匹配模式。

```java
// 匹配结果是 The fat cat sat on the mat
"/(.*at)/" => The fat cat sat on the mat
```

```java
// 匹配结果是The fat
"/(.*?at)/" => The fat cat sat on the mat
```

# 【函数式接口】

函数式接口(Functional Interface)就是一个有且仅有一个抽象方法，但是可以有多个非抽象方法的接口。

函数式接口可以被隐式转换为 lambda 表达式。

- 只包含一个抽象方法的接口，称为函数式接口
- 可以通过Lambda表达式创建该接口的对象（若Lambda表达式抛出一个收检异常—非运行时异常—那么该异常需要在目标接口的抽象方法上进行声明）
- 可以在接口上使用`@FunctionalInterface`注解，检查该接口是否为函数式接口，同时也会在javadoc中包含对它是函数式接口的声明
- `java.util.function`包下定义了Java 8的丰富的函数式接口

|        函数式接口        | 参数类型 | 返回类型 |                             用途                             |
| :----------------------: | :------: | :------: | :----------------------------------------------------------: |
|  Consumer<T> 消费型接口  |    T     |   void   |    对类型为T的对象应用操作，包含方法：`void accept(T t)`     |
|  Supplier<T> 供给型接口  |    无    |    T     |            返回类型为T的对象，包含方法:`T get()`             |
| Function<T,R> 函数型接口 |    T     |    R     | 对类型为T的对象应用操作，并返回结果。结果是R类型的对象。包含方法:`R apply(T t)` |
| Predicate<T> 断定型接口  |    T     | boolean  | 确定类型为T的对象是否满足其约束，并返回boolean值。包含方法:      `boolean test(T t)` |

# 【Lambda表达式】

Lambda 表达式，也可称为闭包，它是推动 Java 8 发布的最重要新特性。

**Lambda的本质：** 作为函数式接口的实例。

如果一个接口中，只声明了一个抽象方法，则此接口就被称为函数式接口

## **举例**

- 例一：

```java
@Test
public void test1() {
    // 正常写法(匿名内部类，即Runnable)
    Runnable r1 = new Runnable() {
        @Override
        public void run() {
            System.out.println("Hello World!");
        }
    };
    r1.run();
    
    // Lambda表达式写法：
    Runnable r2 = () -> System.out.println("Hello World!");
    r2.run();
}
```

​	运行结果：

```shell
Hello World!
Hello World!
```

- 例二：

```java
public void test2() {
    // 写法一：正常写法
    Comparator<Integer> com1 = new Comparator<Integer>() {
        @Override
        public int compare(Integer o1, Integer o2) {
            return Integer.compare(o1,o2);
        }
    };
    System.out.println(com1.compare(11,22));
    System.out.println("*********");
    
    // 写法二：Lamabda表达式写法
    Comparator<Integer> com2 = (o1,o2) -> Integer.compare(o1,o2);
    System.out.println(com2.compare(33,22));
    System.out.println("*********");
    
    // 写法三：方法引用
    Comparator<Integer> com3 = Integer :: compare;
    System.out.println(com3.compare(33,44));
}
```

​	运行结果：

```shell
-1
*********
1
*********
-1
```

## 语法

**lambda 表达式的语法格式如下：**

```java
(parameters) -> expression
或
(parameters) ->{ statements; }

举例：(o1,o2) -> Integer.compare(o1,o2)
格式：
     -> ：Lambda操作符或箭头操作符
     ->左边：Lambda形参列表（其实就是接口中抽象方法的形参列表）
     ->右边：Lambda体（其实就是重写的抽象方法的方法体）
```

**Lambda表达式的重要特征:**

- 可选类型声明：不需要声明参数类型，编译器可以统一识别参数值。
- 可选的参数圆括号：一个参数无需定义圆括号，但多个参数需要定义圆括号。
- 可选的大括号**：**如果主体包含了一个语句，就不需要使用大括号。
- 可选的返回关键字**：**如果主体只有一个表达式返回值则编译器会自动返回值，大括号需要指定明表达式返回了一个数值。

****

**Lambda表达式的本质：** 作为接口的实例

****

**具体使用（六种）：**

- **语法格式一：** 无参，无返回值

```java
// 正常写法
Runnable r1 = new Runnable() {
    @Override
    public void run() {
        System.out.println("Hello World!");
    }
};
r1.run();

// Lambda表达式写法：
Runnable r2 = () -> System.out.println("Hello World!");
r2.run();
```

- **语法格式二：** 有参，无返回值

```java
// 正常写法:
Consumer<String> con1 = new Consumer<String>() {
    @Override
    public void accept(String s) {
        System.out.println(s);
    }
};
con1.accept("Hello World!");

// Lambda表达式写法
Consumer<String> con2 = (String s) -> { System.out.println(s); };
con2.accept("Hello World!");
```

- **语法格式三： **数据类型可省略（“类型推断”）

```java
// 不省略写法
Consumer<String> con2 = (String s) -> { System.out.println(s); };
con2.accept("Hello World!");

// 省略数据类型
Consumer<String> con2 = (s) -> { System.out.println(s); };
con2.accept("Hello World!");
```

- **语法格式四：** 单参数时可以省略小括号

```java
// 有括号写法
Consumer<String> con2 = (String s) -> { System.out.println(s); };
con2.accept("Hello World!");

// 无括号写法
Consumer<String> con2 = s -> { System.out.println(s); };
con2.accept("Hello World!");
```

- **语法格式五：** 多参数多执行语句，且有返回值

```java
// 
Comparator<Integer> com2 = (o1,o2) -> { 
    System.out.println(o1);
    System.out.println(o2);
    return o1.compareTo(o2);
};
```

- **语法格式六：** lambda体只有一条语句时，return与大括号若有，都可以省略

```java
Comparator<Integer> com2 = (o1,o2) -> o1.compareTo(o2);
```

# 【方法引用】

方法引用是隐式借用已经存在的方法作为现成的执行逻辑，而不必在`lambda`表达式中显式调用该方法，或者重写这一部分代码。

简单来说，方法引用是对`lambda`表达式的一种更加简便的写法。

- 当要传递给Lambda体的操作，已经有实现的方法了，就可以使用方法引用。

  **要求：** 接口中抽象方法的形参列表和返回值类型，与方法引用的形参列表和返回值类型相同！

- 方法引用就是Lambda表达式，也就是函数式接口的一个实例，通过方法的名字来指向一个方法

- 格式：使用操作符`::`将类（或对象）与方法名分隔开来。

- 使用场景：

  - `对象::实例方法名`

    ```java
    // Consumer接口中的void accept(T t)
    // PrintStream中的void println(T t)与accept方法结构一样，可视为实现了该抽象方法
    // 使用println(T t)方法替换accpet(T t)方法
    
    // lambda表达式
    Consumer<String> con1 == str -> System.out.println(str);
    // 方法引用
    // out是System的一个静态成员字段，类型为Printtream
    PrintStream ps = System.out
    Consumer<String> con2 = ps::println
    ```
  
  - `类::静态方法名`
  
    ```java
    // Comparator接口中的int compare(T t1, T t2)
    // Integer中的int compare(T t1, T t2)
    
    // lambda表达式
    Comparator<Integer> com1 = (t1,t2) -> Integer.compare(t1, t2);
    // 方法引用
    Comparator<Integer> com2 = Integer::compare;
    ```
  
    ```java
    // Function接口中的R apply(T t)
    // Math中的Long round(Double d)
    // 上述返回值和形参列表相同
    
    // lambda表达式
    Function<Double,Long> func1 = d -> Math.round(d);
    // 方法引用
    Function<Double,Long> func2 = Math::round;
    ```
  
  - `类::实例方法名`
  
    接口中的抽象方法有两个参数，类的实例方法仅含有一个参数
  
    **可以视为类作为第一个参数，为方法的调用者，实例方法中的参数作为第二个参数，来实现抽象方法，也可以使用方法引用**
  
    ```java
    // Comparator接口中的int compare(T t1, T t2)
    // String中的int t1.compare(t2)
    
    // lambda表达式
    Comparator<String> com1 = (s1,s2) -> s1.compareTo(s2)
    // 方法引用
    Comparator<String> com2 = s1::compareTo;
    ```
  
  - `类::new` 
  
    - 构造器引用
  
    ```java
    // Supplier中的T get()，有返回值无参数
    // Employee空参构造器: Employee()，无参数返回Employee对象
    
    // lambda表达式
    Supplier<Employee> sup1 = () -> new Employee();
    // 方法引用
    Supplier<Employee> sup2 = Employee::new;
    ```
  
    ```java
    // Function中的R apply(T t)
    // Employee的有参构造器 Employee(int id)
    
    // lambda表达式
    Function<Employee> func1 = (id) -> new Employee(id);
    // 方法引用
    Function<Employee> func2 = Employee::new;
    ```
  
    - 数组引用
  
    ```java
    // Function中的R apply(T t)
    // 将数组看做一个特殊的类，创建数组即使用其构造方法
    
    // lambda表达式
    Function<Integer,String[]> func1 = length -> new String[length];
    // 数组引用
    Function<Integer,String[]> func2 = String[]::new;
    ```

# 【集合/数据结构】

## API

### Queue队列

LIFO(先进先出)

| 方法/区别 |  抛出异常   | 返回特殊值 |
| :-------: | :---------: | :--------: |
|   插入    |  `add(e)`   | `offer(e)` |
|   删除    | `remove()`  |  `poll()`  |
|   查看    | `element()` |  `peek()`  |

### List集合

### Set集合





### Map集合

## 集合体系结构

- `Collection`：单列集合，一次添加一个数据
  - `List`：接口，添加的元素有序、可重复、有索引
    - ArrayList
    - LinkedList
    - Vector
  - `Set`：接口，添加的元素无序、不重复、无索引
    - HashSet
      - LinkedHashSet
    - TreeSet
  - `Queue`
- `Map`：双列集合，一次添加一对数据

## Collection

Collection是单列集合的顶层父接口，它的功能是全部单列集合都可以继承使用的

### 成员方法

![image-20230206101037580](https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-masterimage-20230206101037580.png)



```java
/**
 * public boolean add(E e)             添加
 * public void clear()                 清空
 * public boolean remove(E e)          删除
 * public boolean contains(Object obj) 判断是否包含
 * public boolean isEmpty()            判断是否为空
 * public int size()                   集合长度
 */

/* 
	注意点：Collection是一个接口,不能直接创建他的对象。
	实现类以ArrayList为例子
*/	

Collection<String> coll = new ArrayList<>();

// 1.添加元素
// 细节1：List系列集合中添加数据，那么方法永远返回true，因为List系列的是允许元素重复的。
// 细节2：Set系列集合中添加数据,如果当前要添加元素不存在，方法返回true;如果当前要添加的元素已经存在，方法返回false
// 		 因为Set系列的集合不允许重复。
coll.add("aaa");
coll.add("bbb");
coll.add("ccc");
System.out.println(coll);

// 2.清空
coll.clear();

// 3.删除
// 细节1：因为Collection里面定义的是共性的方法，所以此时不能通过索引进行删除。只能通过元素的对象进行删除。
// 细节2：方法会有一个布尔类型的返回值，删除成功返回true，删除失败返回false
// 如果要删除的元素不存在，就会删除失败。
System.out.println(coll.remove("aaa"));
System.out.println(coll);

// 4.判断元素是否包含
// 细节：底层是依赖equals方法进行判断是否存在的,所以需要根据需要重写equals方法
// 如果存的是自定义对象，没有重写equals方法，那么默认使用Object类中的equals方法进行判断，而Object类中equals方法，依赖地址值进行判断。
// 需求：如果同姓名和同年龄，就认为是同一个学生。
// 所以，需要在自定义的Javabean类中，重写equals方法。
boolean result1 = coll.contains("bbb");
System.out.println(result1);

// 5.判断集合是否为空
boolean result2 = coll.isEmpty();
System.out.println(result2); // false

// 6.获取集合的长度
coll.add("ddd");
int size = coll.size();
System.out.println(size);
```

### 通用遍历方式

set集合没有所以，无法通过普通的for循环遍历

**通用遍历方式：**

- 迭代器遍历

- 增强for遍历
- lambda表达式遍历

#### 迭代器遍历

![image-20230206103152271](https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-masterimage-20230206103152271.png)

```java
// 1.创建集合并添加元素
Collection<String> coll = new ArrayList<>();
coll.add("aaa");
coll.add("bbb");
coll.add("ccc");
coll.add("ddd");
// 2.获取迭代器对象
// 迭代器好比一个指针，默认指向集合的0索引处
Iterator<String> it = coll.iterator();
// 3.利用循环不断获取集合中的元素
while (it.hasNext()) {
    // next()：获取元素并移动指针
    System.out.println(it.next());
}
```

**细节：**

- 迭代器当前位置没有元素，会报错`NoSuchElementException`

- 迭代器遍历完毕，指针不会复位，再次遍历需重新获取`Iterator`对象

- 循环中只能用一次`next()`方法

- 迭代器遍历时，不能用集合的方法进行添加和删除

  迭代器提供了`remove()`方法，可以删除当前元素

#### 增强for遍历

- 增强for的底层就是迭代器，可以简化迭代器的代码书写
- 只有单列集合和数组才能使用增强for进行遍历

```java
for(元素的数据类型 变量名 : 数组或集合) {
    
}
```

```java
// 1.创建集合并添加元素
Collection<String> coll = new ArrayList<>();
coll.add("aaa");
coll.add("bbb");
coll.add("ccc");

// 2.增强for遍历
// idea快速生成方式: coll.for
for (String s : coll) {
    System.out.println(s);
}
```

**细节：**

- 修改增强for中的变量，不会改变集合中原本的数据

#### forEach遍历

![image-20230206105746430](https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-masterimage-20230206105746430.png)

```java
// 1.创建集合并添加元素
Collection<String> coll = new ArrayList<>();
coll.add("张三");
coll.add("李四");
coll.add("王五");

// 2.forEach遍历
// 方法底层会遍历集合，依次得到每一个元素
// 然后把每一个元素作为参数传递给下面的accept方法

// 匿名内部类写法
coll.forEach(new Consumer<String>() {
    @Override
    public void accept(String s) {
        System.out.println(s);
    }
});

// Lambda表达式写法
coll.forEach(s -> System.out.println(s));

// 方法引用写法
coll.forEach(System.out::println);
```

## List集合

`List`：接口，添加的元素有序、可重复、有索引

- ArrayList：数组实现；无参空数组，有参默认10，1.5倍
- LinkedList：链表实现
- Vector：线程安全，底层也是对象数组；无参10长度，2倍扩

### 成员方法

![image-20230206110642650](https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-masterimage-20230206110642650.png)

```java
/*
	List系列集合独有的方法：
		void add(int index,E element)       在此集合中的指定位置插入指定的元素
		E remove(int index)                 删除指定索引处的元素，返回被删除的元素
		E set(int index,E element)          修改指定索引处的元素，返回被修改的元素
		E get(int index)                    返回指定索引处的元素
*/

// 1.创建集合
List<String> list = new ArrayList<>();

// 2.添加元素
list.add("aaa");
list.add("bbb");
list.add("ccc");

// 添加：void add(int index,E element)    
// 在此集合中的指定位置插入指定的元素
// 细节：原来索引上的元素会依次往后移
list.add(1,"QQQ");

// 删除：E remove(int index)                 
// 删除指定索引处的元素，返回被删除的元素
String remove = list.remove(0);
System.out.println(remove); // aaa

// 修改：E set(int index,E element)          
// 修改指定索引处的元素，返回被修改的元素
String result = list.set(0, "QQQ");
System.out.println(result);

// 获取：E get(int index)                    
// 返回指定索引处的元素
String s = list.get(0);
System.out.println(s);

// 3.打印集合
System.out.println(list);
```

**细节：**

- 删除元素时：`remove()`有两个方法，

  - 通过索引删除元素`remove(Int index)`
  - 通过元素本身来删除元素`remove(Object o)`

  如下所示，当remove中传参1时，是删除1这个元素，还是删除1索引下的元素？

  ```java
  ArrayList<Object> list = new ArrayList<>();
  list.add(0);
  list.add(2);
  list.add(1);
  
  list.remove(1);
  ```

  **当方法发生重载时，会自动调用实参和形参类型一致的方法，所以：**

  - 删除1索引下的元素：使用`Int index = 1`参数
  - 删除1这个元素：使用`Integer object = 1`参数

### 遍历方式

List系列集合的五种遍历方式及使用场景：

- 迭代器：遍历过程中需要删除元素
- 增强for：仅遍历
- Lambda表达式：仅遍历
- 普通for循环：操作索引
- 列表迭代器：遍历过程中需要添加元素

```java
// 创建集合并添加元素
List<String> list = new ArrayList<>();
list.add("aaa");
list.add("bbb");
list.add("ccc");

// 1.迭代器
Iterator<String> it = list.iterator();
while(it.hasNext()){
    String str = it.next();
    System.out.println(str);
}

//2 .增强for
for (String s : list) {
    System.out.println(s);
}

// 3.Lambda表达式
list.forEach(s->System.out.println(s) );

// 4.普通for循环
for (int i = 0; i < list.size(); i++) {
    String s = list.get(i);
    System.out.println(s);
}

// 5.列表迭代器，迭代器的子接口
// 获取一个列表迭代器的对象，里面的指针默认也是指向0索引的
// 额外添加了一个方法：在遍历的过程中，可以添加元素
ListIterator<String> it = list.listIterator();
while(it.hasNext()){
    String str = it.next();
    if("bbb".equals(str)){
        // 添加元素
        it.add("qqq");
    }
}
System.out.println(list);
```

**列表迭代器`ListIiterator`：**

```java
boolean hasNext();     //检查是否有下一个元素
E next();              //返回下一个元素
boolean hasPrevious(); //check是否有previous(前一个)element(元素)
E previous();          //返回previous element
int nextIndex();       //返回下一element的Index
int previousIndex();   //返回当前元素的Index
void remove();         //移除一个elment
void set(E e);         //set()方法替换访问过的最后一个元素 注意用set设置的是List列表的原始值
void add(E e);         //添加一个element
```

### ArrayList

`Collection`：单列集合，一次添加一个数据

- `List`：接口，添加的元素有序、可重复、有索引
  - ArrayList
  - LinkedList
  - Vector

**ArrayList** 拥有上层的所有方法

**底层原理：**

- 利用空参创建的集合，在底层创建一个默认长度为0的数组

  ```java
  // 数组名称：elementData
  // 数组指针：size，表示当前元素个数、下一个元素被存入的下标
  public ArrayList() {
      // DEFAULTCAPACITY_EMPTY_ELEMENTDATA：数组长度为0的数组
      this.elementData = DEFAULTCAPACITY_EMPTY_ELEMENTDATA;
  }
  ```

- 添加元素时，如果数组已满，会调用`grow()`方法：

  ```java
  // minCapacity：数组最小容量
  private Object[] grow(int minCapacity) {
      int oldCapacity = elementData.length; // 旧数组容量
      // 判断数组已经存在(而不是默认的0长度数组)
      if (oldCapacity > 0 || elementData != DEFAULTCAPACITY_EMPTY_ELEMENTDATA) {
          // ArraysSupport.newLength() 返回需要的数组长度：Math.max(minGrowth, prefGrowth) + oldLength
          int newCapacity = ArraysSupport.newLength(oldCapacity, // 旧容量 oldLength
                  minCapacity - oldCapacity, // 最小需要扩容的容量 minGrowth
                  oldCapacity >> 1           // 默认扩容的容量 prefGrowth，默认扩容旧容量的一半
          // 根据扩充后的数组长度创建新数组并复制元素
          return elementData = Arrays.copyOf(elementData, newCapacity);
      } else {
          // DEFAULT_CAPACITY 数组默认长度，10
          return elementData = new Object[Math.max(DEFAULT_CAPACITY, minCapacity)];
      }
  }
  ```

  - 添加第一个元素时（此时`minCpacity`为1），底层会创建一个新的长度为10的数组

  - 当数组长度存满后，再添加元素，扩容数组

    - 当数组存满后，会创建一个新数组，数组长度是原来的1.5倍

    - 如果一次添加多个元素，1.5倍还放不下，则新创建数组的长度以实际为准

### LinkedList

`Collection`：单列集合，一次添加一个数据

- `List`：接口，添加的元素有序、可重复、有索引
  - `ArrayList`
  - LinkedList
  - Vector

**LinkedList** 拥有上层的所有方法

底层数据结构是双链表，查询慢，增删快，但如果操作的是首尾元素，速度也是极快的

LinkedList提供了很多首尾操作的特有API：

![image-20230304135435487](https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-masterimage-20230304135435487.png)

**底层原理：**

- 成员变量：

  ```java
  // 链表大小
  transient int size = 0;
  // 头结点
  transient Node<E> first;
  // 尾结点
  transient Node<E> last;
  
  // 结点
  private static class Node<E> {
      E item;
      Node<E> next;
      Node<E> prev;
      Node(Node<E> prev, E element, Node<E> next) {
          this.item = element;
          this.next = next;
          this.prev = prev;
      }
  }
  ```

- 添加元素：

  ```java
  public boolean add(E e) {
      linkLast(e);
      return true;
  }
  
  void linkLast(E e) {
      final Node<E> l = last; // 临时变量，存旧尾结点
      final Node<E> newNode = new Node<>(l, e, null); // 添加的新结点
      last = newNode; // 修改链表尾指针为新结点 
      if (l == null)
          first = newNode; // 空链表则链表头指针也指向新结点
      else
          l.next = newNode; // 修改旧尾结点的尾指针为新结点
      size++;
      modCount++;
  }
  ```

### 迭代器源码

**Iterator是集合的一个内部类**

```java
// 创建一个内部类对象
public Iterator<E> iterator() {
    return new Itr();
}

private class Itr implements Iterator<E> {
    int cursor; // 迭代器的指针，默认指向0索引
    int lastRet = -1; // 上一次操作的索引
    // modCount 记录集合变化的次数，每次add或remove都是会增
    // 当创建迭代器对象时，迭代器对象会持有这个次数
    // 迭代器遍历时，会校验这个次数：checkForComodification()
    // 如果迭代器持有的modCount与集合的不一致，说明迭代器迭代的过程中集合发生了并发修改，会抛出并发修改异常
    int expectedModCount = modCount; 
    
    Itr() {}
    
    public boolean hasNext() {
        // 当指针指向末索引后一位(即null位置)，返回false
        return cursor != size;
    }
    
    @SuppressWarnings("unchecked")
    public E next() {
        checkForComodification(); // 校验modCount
        int i = cursor; // 记录当前指针指向的索引位置
        if (i >= size)
            throw new NoSuchElementException();
        Object[] elementData = ArrayList.this.elementData;
        if (i >= elementData.length)
            throw new ConcurrentModificationException();
        cursor = i + 1; // 移动指针
        return (E) elementData[lastRet = i]; // 记录操作的索引，并返回此索引的元素
    }
}    
```

## Set集合

`Set`：接口，添加的元素 **无序** 、 **不重复** 、无索引

- HashSet
  - LinkedHashSet 
- TreeSet 

Set接口中的方法基本上与Collection的API一致

![image-20230206101037580](https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-masterimage-20230206101037580.png)

### HashSet

- HashSet集合底层采取哈希表存储数据
- JDK8以前：数组+链表
- JDK8之后：数组+链表+红黑树

**哈希值：**

- 是根据hashCode方法计算出int类型的整数
- 该方法定义在Object类中，所有对象都可以进行调用，默认使用地址值进行计算
- 一般情况下，会重写hashCode方法，利用对象内部的属性值计算哈希值
- 哈希碰撞：
  - 相同属性/地址的哈希值一定相同
  - 不同属性/地址的哈希值也可能相同，即发生哈希碰撞

**HashSet底层原理：**

- 创建一个默认长度16，默认加载因子为0.75的数组，命名为table
  - 加载因子：数组的扩容时机 ↓
  - 当数组存入了`16×0.75=12`个元素时，数组长度会自动扩容到原来的两倍
  - JDK8以后，当`数组长度>=64，链表长度>8`时，会把当前位置的链表转为红黑树
- 存入元素时，通过`int index = (数组长度-1) & 哈希值`计算出应存入元素在数组中的下标
- 判断当前位置是否为null，如果是null直接存入
- 如果当前位置不是null，表示有元素，则调用equlas方法比较两个元素是否相同；
  - 元素相同，不存入；
  - 元素相同，形成链表（已有链表时equals会比较链表中每一个元素）
  - JDK8以前：头插法，新元素存入数组，老元素挂在新元素的下面
  - JDK8以后：尾插法，新元素直接挂在老元素下面
- 如果集合中存储的是自定义对象，必须重写hashCode和equals方法：目的是比较对象的属性值，而不是地址值

**加载因子为什么是0.75：**

- 加载因子太大，发生hash冲突的次数太多，影响效率
- 加载因子大小，发生扩容的次数太多，浪费空间

**树化阈值为什么是8：**

- 树化是不正常现象（预防恶意攻击），很耗性能，要尽量避免

- key碰撞问题，每一次碰撞都是独立随机事件，碰撞n次发生的概率符合泊松分布

- 在加载因子为0.75的情况下，发生hash碰撞的概率大概是0.5，再此条件下，发生8次hash碰撞的概率已经足够小，满足需求

### LinkedHashSet

- LinkedHashSet：**有序** 、不重复、无索引
- 有序指的是保证存储与取出的元素顺序一致
- **原理：**底层数据结构依然是哈希表，只是每个元素使用双向链表记录存储的顺序。

![image-20230315171505872](https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-masterimage-20230315171505872.png)

### TreeSet

- TreeSet：不重复、无索引、可排序

- **可排序：** 默认按照元素从小到大的顺序进行存储

  - 数值类型：数字大小从小到大排列
  - 字符、字符串类型：按照字符在ASCII表中的数字升序进行排序，字符串按照首字母、次字母....的顺序排序

- 底层使用红黑树实现

- **比较方法：**

  - 自然排序：javabean类实现Comparable接口

    ```java
    public class Student implements Comparable<Student>{
        int id;
        int age;
    
        public Student(int id, int age) {
            this.id = id;
            this.age = age;
        }
    
        @Override
        public int compareTo(Student o) {
            // 主要判断条件
            int i = this.id - o.id;
            // 次要判断条件
            i = (i==0 ? i : this.age-o.age);
            return i;
        }
    }
    ```

  - 比较器排序：集合构造方法接收Comparator的实现类对象，重写compare(T o1,T o2)方法

    ```java
    TreeSet<Student> ts = new TreeSet<>(new Comparator<Student>() {
        @Override
        public int compare(Student o1, Student o2) {
            return o1.age-o2.age;
        }
    });
    ```

  - 两种排序方法同时存在时，使用比较器排序

## Map集合

### 常用API

![image-20230315174933757](https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-masterimage-20230315174933757.png)

![image-20230315175018757](https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-masterimage-20230315175018757.png)

**遍历方式：**

- 键找值

  ```java
  // 获取所有键，把这些键放到一个单列集合中
  Set<String> keySet = map.keySet();
  for (String key : keySet) {
      System.out.println(key + "=" + map.get(key));
  }
  ```

- 依次获取所有键值对对象(entry对象)

  ```java
  // Entry是Map接口的子接口，里面存储key和value
  Set<Map.Entry<String, String>> entries = map.entrySet();
  for (Map.Entry entry: entries) {
      System.out.println(entry.getKey() + "=" + entry.getValue());
  }
  ```

- Lambda表达式遍历

  ```java
  map.forEach(
          (key, value) -> 
                  System.out.println(key + "=" + value)
  );
  ```

### HashMap

- 键：无序、不重复、无索引
- 底层原理与HashSet一致，底层数据结构都是哈希表结构，默认数组长度16，增长因子0.75，根据key计算存储位置，相同key会覆盖value；
- 当数组长度大于等于64（否则会先尝试扩容），链表长度大于8时会转为红黑树

### LinkedHashMap

- 键：有序、不会重复、无索引
- 底层原理与LinkedHashSet一致，底层数据结构是哈希表，但额外使用双向链表链接元素

### TreeMap

![image-20230315193612379](https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-masterimage-20230315193612379.png)

### 源码解析

