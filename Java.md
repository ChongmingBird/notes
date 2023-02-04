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

### JDK8

![image-20230203153422248](https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-masterimage-20230203153422248.png)



![image-20230203153727786](https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-masterimage-20230203153727786.png)

### JDK9

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

# 【集合】

## ArrayList

![image-20230203152810424](https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-masterimage-20230203152810424.png)



