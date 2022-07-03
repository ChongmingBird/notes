# 【Maven】

## 概念

> **Apache Maven：** 是一个项目管理和构建工具，它基于项目对象模型(POM)的概念，通过一小段描述信息来管理项目的构建、报告和文档。
>
> 官网 ：http://maven.apache.org/ 

### 作用

Maven是专门用于管理和构建Java项目的工具，它的主要功能有：

- **提供一套标准化的项目结构：**

  不同IDE之间，项目结构不一样，不通用，但使用Maven构建的项目结构完全一样，在不同的IDE中可以通用

![image-20220426174946205](https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-master/image-20220426174946205.png)

- **提供一套标准化的构建流程（编译、测试、打包、发布...)：**

![image-20220426175130316](https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-master/image-20220426175130316.png)

- **提供了一套依赖管理机制：**

​		**依赖管理：** 管理项目所依赖的第三方资源(jar包、插件...)				![image-20220426175347365](https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-master/image-20220426175347365.png)

### Maven模型

**Maven模型：**

1. 项目对象模型 (Project Object Model)
2. 依赖管理模型(Dependency)
3. 插件(Plugin)



- ![image-20210726155759621](https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-master/image-20210726155759621.png)

上图红框所示部分，就是用来完成 `标准化构建流程` 。

如需要编译，Maven提供了一个编译插件供使用，需要打包，Maven就提供了一个打包插件供使用等

- ![image-20210726160928515](https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-master/image-20210726160928515.png)

上图中红框所示部分，项目对象模型就是将我们自己抽象成一个对象模型，有自己专属的坐标，如下图所示是一个Maven项目：

![image-20210726161340796](https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-master/image-20210726161340796.png)

依赖管理模型则是使用坐标来描述当前项目依赖哪些第三方jar包，如下图所示：

![image-20210726161616034](https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-master/image-20210726161616034.png)

### 仓库

创建Maven项目时，在项目中使用坐标来指定项目的依赖，其实依赖jar包是存储在我们的本地仓库中。而项目运行时从本地仓库中拿需要的依赖jar包。

**仓库分类：**

* **本地仓库：** 自己计算机上的一个目录
* **中央仓库：** 由Maven团队维护的全球唯一的仓库
  * 地址： https://repo1.maven.org/maven2/
* **远程仓库(私服)：** 一般由公司团队搭建的私有仓库

当项目中使用坐标引入对应依赖jar包后，首先会查找本地仓库中是否有对应的jar包：

* 如果有，则在项目直接引用;
* 如果没有，则去中央仓库中下载对应的jar包到本地仓库。

![image-20210726162553653](https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-master/image-20210726162553653.png)

如果还可以搭建远程仓库，将来jar包的查找顺序则变为：

> 本地仓库 --> 远程仓库--> 中央仓库







## 配置仓库

### 本地仓库

配置本地仓库

修改conf/settings.xml中的 `<localRepository>`为一个指定目录作为本地仓库，用来存储jar包。

```xml
<localRepository>
    D:\Environment\apache-maven-3.8.1\maven-repo
</localRepository>
```



### 阿里云仓库

> 中央仓库在国外，访问速度慢，使用国内镜像的远程仓库可以加快访问速度
>
> 阿里云Maven中央仓库为阿里云云效提供的公共代理仓库，帮助研发人员提高研发生产效率，使用阿里云Maven中央仓库作为下载源，速度更快更稳定

**配置方法：**

打开 Maven 的配置文件(windows机器一般在maven安装目录的conf/settings.xml)，在`<mirrors></mirrors>`标签中添加 mirror 子节点:

```xml
<mirror>
    <id>aliyunmaven</id>
    <mirrorOf>*</mirrorOf>
    <name>阿里云公共仓库</name>
    <url>https://maven.aliyun.com/repository/public</url>
</mirror>
```

### 华为云仓库

> Mybatis分页插件最新依赖阿里云仓库里没有，但华为云仓库里有

```xml
<mirror> 
    <id>huaweicloud</id> 
    <mirrorOf>*</mirrorOf> 
    <url>https://mirrors.huaweicloud.com/repository/maven/</url> 
</mirror>
```



## 基本使用

### 常用命令

> * compile ：编译
> * clean：清理
> * test：测试
> * package：打包
> * install：安装

使用上面命令需要在磁盘上进入到项目的 `pom.xml`的同级目录下，打开命令提示符使用：

**编译命令演示：**

```shell
compile ：编译
```

执行上述命令可以看到：

* 从阿里云下载编译需要的插件的jar包，在本地仓库也能看到下载好的插件
* 在项目下会生成一个 `target` 目录

![image-20210726171047324](https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-master/image-20210726171047324.png)

项目下会出现一个 `target` 目录，编译后的字节码文件就放在该目录下

<img src="https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-master/image-20210726171346824.png" alt="image-20210726171346824" style="zoom:80%;" />

**清理命令演示：**

```shell
mvn clean
```

执行上述命令可以看到

* 从阿里云下载清理需要的插件jar包
* 删除项目下的 `target` 目录

<img src="https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-master/image-20210726171558786.png" alt="image-20210726171558786" style="zoom:80%;" />

**打包命令演示：**

```
mvn package
```

执行上述命令可以看到：

* 从阿里云下载打包需要的插件jar包
* 在项目的 `terget` 目录下有一个jar包（将当前项目打成的jar包）

<img src="https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-master/image-20210726171747125.png" alt="image-20210726171747125" style="zoom:80%;" />

**测试命令演示：**

```shell
mvn test  
```

该命令会执行所有的测试代码。执行上述命令效果如下

<img src="https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-master/image-20210726172343933.png" alt="image-20210726172343933" style="zoom:80%;" />

**安装命令演示：**

```shell
mvn install
```

该命令会将当前项目打成jar包，并安装到本地仓库。执行完上述命令后到本地仓库查看结果如下：

<img src="https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-master/image-20210726172709112.png" alt="image-20210726172709112" style="zoom:80%;" />

### 生命周期

> **Maven 构建项目生命周期：** 一次构建过程经历了多少个事件

**Maven 对项目构建的生命周期划分为3套：**

* clean：清理工作。
* default：核心工作，例如编译，测试，打包，安装等。
* site：产生报告，发布站点等。这套声明周期一般不会使用。

同一套生命周期内，执行后边的命令，前面的所有命令会自动执行。例如默认（default）生命周期如下：

<img src="https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-master/image-20210726173153576.png" alt="image-20210726173153576" style="zoom:80%;" />

执行 `install`（安装）命令时，它会先执行 `compile`命令，再执行 `test ` 命令，再执行 `package` 命令，最后执行 `install` 命令。

执行 `package` （打包）命令时，它会先执行 `compile` 命令，再执行 `test` 命令，最后执行 `package` 命令。

**其他一些基本不使用的命令：**<img src="https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-master/image-20210726173619353.png" alt="image-20210726173619353" style="zoom:80%;" />

## IDEA使用

### 配置Maven环境

* **选择 IDEA中 File --> Settings**

  <img src="https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-master/image-20210726174202898.png" alt="image-20210726174202898" style="zoom:80%;" />

* **搜索 maven** 

  <img src="https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-master/image-20210726174229396.png" alt="image-20210726174229396" style="zoom:80%;" />

* **设置 IDEA 使用本地安装的 Maven，并修改配置文件路径**

  <img src="https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-master/image-20210726174248050.png" alt="image-20210726174248050" style="zoom:70%;" />

### 坐标

**坐标：**

- Maven 中的坐标是资源的唯一标识

* 使用坐标来定义项目或引入项目中需要的依赖

**Maven 坐标主要组成：**

* groupId：定义当前Maven项目隶属组织名称（通常是域名反写，例如：com.itheima）
* artifactId：定义当前Maven项目名称（通常是模块名称，例如 order-service、goods-service）
* version：定义当前项目版本号

**如下图就是使用坐标表示一个项目：**

![image-20210726174718176](https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-master/image-20210726174718176.png)

**注意：**

* 上面所说的资源可以是插件、依赖、当前项目。
* 当前项目如果被其他的项目依赖时，也是需要坐标来引入的。

### 创建Maven项目

* 创建模块，选择Maven，点击Next

  <img src="https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-master/image-20210726175049876.png" alt="image-20210726175049876" style="zoom:90%;" />

* 填写模块名称，坐标信息，点击finish，创建完成

  <img src="https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-master/image-20210726175109822.png" alt="image-20210726175109822" style="zoom:80%;" />

  创建好的项目目录结构如下：

  ![image-20210726175244826](https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-master/image-20210726175244826.png)

### 导入Maven项目

**导入别人的项目，查看他们的代码：**

* 选择右侧Maven面板，点击 + 号

  <img src="https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-master/image-20210726182702336.png" alt="image-20210726182702336" style="zoom:70%;" />

* 选中对应项目的pom.xml文件，双击即可

  <img src="https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-master/image-20210726182648891.png" alt="image-20210726182648891" style="zoom:70%;" />

* 如果没有Maven面板，选择

  View --> Appearance --> Tool Window Bars

  <img src="https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-master/image-20210726182634466.png" alt="image-20210726182634466" style="zoom:80%;" />

​		可以通过下图所示进行命令的操作：

<img src="https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-master/image-20210726182902961.png" alt="image-20210726182902961" style="zoom:80%;" />

### Maven-Helper插件

* 选择 IDEA中 File --> Settings

  <img src="https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-master/image-20210726192212026.png" alt="image-20210726192212026" style="zoom:80%;" />

* 选择 Plugins

  <img src="https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-master/image-20210726192224914.png" alt="image-20210726192224914" style="zoom:80%;" />

* 搜索 Maven，选择第一个 Maven Helper，点击Install安装，弹出面板中点击Accept

  <img src="https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-master/image-20210726192244567.png" alt="image-20210726192244567" style="zoom:80%;" />

* 重启 IDEA

安装完该插件后可以通过 选中项目右键进行相关命令操作，如下图所示：

<img src="https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-master/image-20210726192430371.png" alt="image-20210726192430371" style="zoom:80%;" />

 

## 依赖管理

> Maven仓库：https://mvnrepository.com

###   引入依赖

**使用坐标引入jar包的步骤：**

* 在项目的 pom.xml 中编写`<dependencies>`标签

* 在`<dependencies>`标签中使用`<dependency>`引入坐标

* 定义坐标的`groupId`，`artifactId`，`version`

  <img src="https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-master/image-20210726193105765.png" alt="image-20210726193105765" style="zoom:70%;" />

* 点击刷新按钮，使坐标生效

  <img src="https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-master/image-20210726193121384.png" alt="image-20210726193121384" style="zoom:60%;" />



**快捷方式导入jar包的坐标：**

每次需要引入jar包，都去对应的网站进行搜索是比较麻烦的，接下来给大家介绍一种快捷引入坐标的方式

* 在 pom.xml 中 按 alt + insert，选择 Dependency

  <img src="https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-master/image-20210726193603724.png" alt="image-20210726193603724" style="zoom:60%;" />

* 在弹出的面板中搜索对应坐标，然后双击选中对应坐标

  <img src="https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-master/image-20210726193625229.png" alt="image-20210726193625229" style="zoom:60%;" />

* 点击刷新按钮，使坐标生效

  ![](https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-master/image-20210726193121384.png)

**自动导入设置：**

上面每次操作都需要点击刷新按钮，让引入的坐标生效。当然我们也可以通过设置让其自动完成

* 选择 IDEA中 File --> Settings

  <img src="https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-master/image-20210726193854438.png" alt="image-20210726193854438" style="zoom:60%;" />

* 在弹出的面板中找到 Build Tools

  <img src="https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-master/image-20210726193909276.png" alt="image-20210726193909276" style="zoom:80%;" />

* 选择 Any changes，点击 ok 即可生效

### 依赖范围

> 通过设置坐标的依赖范围(scope)，可以设置对应jar包的作用范围：编译环境、测试环境、运行环境。

如下图所示给 `junit` 依赖通过 `scope` 标签指定依赖的作用范围。 那么这个依赖就只能作用在测试环境，其他环境下不能使用。

<img src="https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-master/image-20210726194703845.png" alt="image-20210726194703845" style="zoom:70%;" />

  **`scope`取值：**

|  依赖范围  | 编译classpath | 测试classpath | 运行classpath |       例子        |
| :--------: | :-----------: | :-----------: | :-----------: | :---------------: |
| `compile`  |       Y       |       Y       |       Y       |      logback      |
|   `test`   |       -       |       Y       |       -       |       Junit       |
| `provided` |       Y       |       Y       |       -       |    servlet-api    |
| `runtime`  |       -       |       Y       |       Y       |     jdbc驱动      |
|  `system`  |       Y       |       Y       |       -       | 存储在本地的jar包 |

* `compile`：作用于编译环境、测试环境、运行环境。
* `test`：作用于测试环境。典型的就是Junit坐标，以后使用Junit时，都会将scope指定为该值
* `provided`：作用于编译环境、测试环境。我们后面会学习 `servlet-api` ，在使用它时，必须将 `scope` 设置为该值，不然运行时就会报错
* `runtime` ：作用于测试环境、运行环境。jdbc驱动一般将 `scope` 设置为该值，当然不设置也没有任何问题

> 注意：
>
> 如果引入坐标不指定 `scope` 标签时，默认就是 compile  值。大部分jar包都是使用默认值。