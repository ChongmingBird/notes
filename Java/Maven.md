# 【Maven基础】

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

​		**依赖管理：** 管理项目所依赖的第三方资源(jar包、插件...)				

![image-20220426175347365](https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-master/image-20220426175347365.png)

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





## Maven命令

> * compile ：编译
> * clean：清理
> * test：测试
> * package：打包
> * install：安装

使用上面命令需要在磁盘上进入到项目的 `pom.xml`的同级目录下，打开命令提示符使用：

### compile

```shell
compile ：编译
```

执行上述命令可以看到：

* 从阿里云下载编译需要的插件的jar包，在本地仓库也能看到下载好的插件
* 在项目下会生成一个 `target` 目录

![image-20210726171047324](https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-master/image-20210726171047324.png)

项目下会出现一个 `target` 目录，编译后的字节码文件就放在该目录下

<img src="https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-master/image-20210726171346824.png" alt="image-20210726171346824" style="zoom:80%;" />

### clean

```shell
mvn clean
```

执行上述命令可以看到

* 从阿里云下载清理需要的插件jar包
* 删除项目下的 `target` 目录

<img src="https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-master/image-20210726171558786.png" alt="image-20210726171558786" style="zoom:80%;" />

### package

```
mvn package
```

执行上述命令可以看到：

* 从阿里云下载打包需要的插件jar包
* 在项目的 `terget` 目录下有一个jar包（将当前项目打成的jar包）

<img src="https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-master/image-20210726171747125.png" alt="image-20210726171747125" style="zoom:80%;" />

### test

```shell
mvn test  
```

该命令会执行所有的测试代码。执行上述命令效果如下

<img src="https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-master/image-20210726172343933.png" alt="image-20210726172343933" style="zoom:80%;" />

### install

```shell
mvn install
```

该命令会将当前项目打成jar包，并安装到本地仓库。执行完上述命令后到本地仓库查看结果如下：

<img src="https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-master/image-20210726172709112.png" alt="image-20210726172709112" style="zoom:80%;" />

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

如下图所示给 `junit` 依赖通过 `<scope>` 标签指定依赖的作用范围。 那么这个依赖就只能作用在测试环境，其他环境下不能使用。

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
* `provided`：作用于编译环境、测试环境。 `servlet-api` ，在使用它时，必须将 `scope` 设置为该值，不然运行时就会报错
* `runtime` ：作用于测试环境、运行环境。jdbc驱动一般将 `scope` 设置为该值，当然不设置也没有任何问题

> 注意：
>
> 如果引入坐标不指定 `scope` 标签时，默认就是 compile  值。大部分jar包都是使用默认值。

### 依赖传递

**依赖具有传递性：**

- **直接依赖：**在当前项目中通过依赖配置建立的依赖关系
- **间接依赖：**被当前项目当做资源的资源，如果依赖其他资源，则当前项目间接依赖其他资源

![image-20230524162931024](https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-masterimage-20230524162931024.png)

**依赖传递冲突：**

- **路径优先：**当依赖中出现相同的资源时，层级越深，优先级越低，层级越浅，优先级越高
- **声明优先：**当资源在相同层级被依赖时，配置顺序靠前的覆盖配置顺序靠后的
- **特殊优先：**当同级配置相同资源的不同版本，后配置的覆盖先配置的

![image-20230524163223534](https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-masterimage-20230524163223534.png)

### 可选依赖

**可选依赖此依赖对外隐藏当前所依赖的资源--不透明**

```xml
<dependency>
    <groupId>junit</groupId>
    <artifactId>junit</artifactId>
    <version>4.12</version>
    <!-- 配置依赖可选 -->
    <optional>true</optional>
</dependency>
```

### 排除依赖

**排除依赖指主动断开依赖的资源，被排除的资源无需指定版本--不需要**

```xml
<dependency>
    <groupId>junit</groupId>
    <artifactId>junit</artifactId>
    <version>4.12</version>
    <!-- 配置需要排除的依赖 -->
    <exclusions>
        <exclusion>
            <groupId>org.hamcrest</groupId>
            <artifactId>hamcrest-core</artifactId>
        </exclusion>
    </exclusions>
</dependency>
```

## 生命周期

> **Maven 构建项目生命周期：** 一次构建过程经历了多少个事件

**Maven 对项目构建的生命周期划分为3套：**

* **clean：**清理工作。

  ![image-20230524163621301](https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-masterimage-20230524163621301.png)

* **default：**核心工作，例如编译，测试，打包，部署等。

  <img src="https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-master/image-20210726173619353.png" alt="image-20210726173619353" style="zoom:80%;" />

* **site：**产生报告，发布站点等。这套声明周期一般不会使用。

  ![image-20230524163707821](https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-masterimage-20230524163707821.png)

同一套生命周期内，执行后边的命令，前面的所有命令会自动执行。例如默认（default）生命周期如下：

<img src="https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-master/image-20210726173153576.png" alt="image-20210726173153576" style="zoom:80%;" />

执行 `install`（安装）命令时，它会先执行 `compile`命令，再执行 `test ` 命令，再执行 `package` 命令，最后执行 `install` 命令。

执行 `package` （打包）命令时，它会先执行 `compile` 命令，再执行 `test` 命令，最后执行 `package` 命令。

**default生命周期**<img src="https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-master/image-20210726173619353.png" alt="image-20210726173619353" style="zoom:80%;" />

- 执行到`test`时，会将`test`上面的命令全部执行

- **插件**：插件与生命周期的阶段绑定，在执行到对应的生命周期时执行对应的插件功能

  > maven插件官网：
  > [Maven – Available Plugins (apache.org)](https://maven.apache.org/plugins/index.html)

  - 插件是默认maven在各个生命周期上绑定的功能

  - 通过插件可以自定义其他功能

    `pom.xml`中`<bulid>`标签内（复数插件使用`<plugins>`包裹）

    ```xml
    build>
    <plugins>
        <plugin>
            <groupId>org.apache.maven.plugins</groupId>
            <artifactId>maven-source-plugin</artifactId>
            <version>2.2.1</version>
            <executions>
                <execution>
                    <goals>
                        <goal>jar</goal>
                    </goals>
                    <phase>generate-test-resources</phase>
                </execution>
            </executions>
        </plugin>
    </plugins>
    </build>
    ```

    

# 【Maven高级】

## 分模块开发与设计

**将不同的包拆分成多个模块，模块之间通过接口通讯**

- 模块中仅包含当前模块对应的功能类与配置文件
- spring核心配置根据模块功能不同而独立制作
- 当前模块所依赖的模块通过导入坐标的形式加入当前模块后才可以使用
- web.xml需要加载所有的spring核心配置文件

![image-20230524164130183](https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-masterimage-20230524164130183.png)

### ssm_pojo拆分

- 新建模块
- 拷贝原始项目中对应的相关内容到模块汇总
  - 实体类（User）
  - 配置文件（无）

### ssm_dao拆分

- 新建模块

- 拷贝原始项目中对应的相关内容到模块汇总

  - 数据接口层（UserDao）

  - 配置文件：保留与数据层相关配置文件（3个）

    - 分页插件在配置中与SqlSessionFactoryBean绑定，需要保留；虽然分页代码写在service层中

  - pom.xml：引入数据层相关坐标即可，删除springmvc相关坐标

    - spring

    - myabtis

    - spring整合mybatis

    - mysql

    - druid

    - pagehelper

    - 直接依赖ssm_pojo（对ssm_pojo模块执行install指令，将其安装到本地仓库）

      ```xml
      <!-- 导入资源文件pojo -->
      <dependency>
          <groupId>com.chongming</groupId>
          <artifactId>ssm_pojo</artifactId>
          <version>1.0-SNAPSHOT</version>
      </dependency>
      ```

### ssm_service拆分

- 新建模块
- 拷贝原始项目中对应的相关内容到模块汇总
  - 业务接口层与实现类（IService、ServiceImpl）
  - 配置文件：保留与数据层相关的配置文件（1个）
  - pom.xml：引入数据层相关坐标即可，删除spring相关坐标
    - spring
    - junit
    - spring整合junit
    - 直接依赖ssm_dao(对ssm_dao模块执行install指令，将其安装到本地仓库)
    - 间接依赖ssm_pojo（由ssm_pojo模块负责依赖关系的建立）
  - 修改service模块spring核心配置文件名，添加模块名称，格式：applicationContext-service.xml
  - 修改dao模块spring核心配置文件名，添加模块名称，格式：applicationContext-dao.xml
  - 修改单元测试引入的配置文件名称，由单个文件修改为多个文件

### ssm_control拆分

- 新建模块(使用webapp模块)

- 拷贝原始项目中对应的相关内容到模块汇总

  - 表现层与相关设置类（UserController、异常等）

  - 配置文件：保留与表现层相关的配置文件（1个）、服务器相关配置文件

  - pom.xml：引入数据层相关坐标即可，删除springmvc相关坐标

    - spring
    - springmvc
    - jackson
    - servlet
    - tomcat服务器插件
    - 直接依赖ssm_service(对ssm_service模块执行install指令，将其安装到本地仓库)
    - 间接依赖ssm_pojo、ssm_dao

  - 修改web.xml配置文件中加载spring环境的配置文件名称，使用*通配符，加载所有applicationContext-开头的配置文件

    （因为每个模块都写了自己的配置文件，需要加载所有的配置文件）

    ```xml
    <?xml version="1.0" encoding="UTF-8"?>
    <web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
             xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
             xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee
             http://xmlns.jcp.org/xml/ns/javaee/web-app_3_1.xsd"
             version="3.1">
      <context-param>
        <param-name>contextConfigLocation</param-name>
        <param-value>classpath*:applicationContext-*.xml</param-value>
      </context-param>
      ......  
    </web-app>
    ```

## 多模块构建维护

### 聚合

![image-20230524171355087](https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-masterimage-20230524171355087.png)

**作用：**聚合用于快速构建maven工程，一次性构建多个项目/模块

当有模块更新的时候，相关模块都需要收到通知，很麻烦

可以创建空模块管理所有模块，当有模块更新时编译所有模块

**制作方式：**

- 创建一个空模块，在`pom.xml`文件中定义该工程用于构建管理（此项目只提供`pom.xml`文件）：

  打包类型定义为pom：

  ```xml
  <packaging>pom</packaging>
  ```

- 定义当前模块进行构建操作时关联的其他模块名称

  ```xml
  <modules>
      <!-- 相对路径 -->
      <module>../ssm_controller</module>
      <module>../ssm_service</module>
      <module>../ssm_dao</module>
      <module>../ssm_pojo</module>
  </modules>
  ```

**注意事项：**参与聚合操作的模块最终执行顺序，与模块间的依赖关系有关，与配置顺序无关

> **模块类型：**
>
> - `pom`
>
>   使用maven进行模块划分管理，一般都会有一个父级项目，pom文件除了GAV(groupId, artifactId, version)是必须要配置的，另一个重要的属性就是packaging打包类型，**所有的父级项目的packaging都为pom**
>
>   packing默认是jar类型，如果不作配置，maven会将该项目打成jar包。作为父级项目，还有一个重要的属性，那就是modules，通过modules标签将项目的所有子项目引用进来，在build父级项目时，会根据子模块的相互依赖关系整理一个build顺序，然后依次build。
>
> - `war`
>
>   war包是一个Web应用程序
>   一个web程序进行打包便于部署的压缩包，里面包含我们web程序需要的一些东西，其中包括web.xml的配置文件，前端的页面文件，以及依赖的jar。便于我们部署工程，直接放到tomcat的webapps目录下，直接启动tomcat即可。同时，可以使用WinRAR查看war包，直接将后缀.war改成.rar。
>
> - `jar`:
>
>   JAR（Java Archive，Java 归档文件）
>   通常是开发时要引用通用类，打成jar包便于存放管理，当你使用某些功能时就需要这些jar包的支持，需要导入jar包。
>   jar包就是java的类进行编译生成的class文件打包的压缩包，包里面就是一些class文件。当我们自己使用Maven写一些java程序，进行打包生成jar包。同时在可以在其他的工程下使用，但是我们在这个工程依赖的jar包，在其他工程使用该JAR包也要导入。

### 继承

> 不只是依赖，插件也可以进行继承管理，步骤同依赖，父工程中使用`<pluginsManagement>`管理插件

![image-20230524172526194](https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-masterimage-20230524172526194.png)

**作用：**通过继承可以是现在子工程中沿用父工程中的配置

- maven中的继承与java中的继承相似

**制作方式：**

- 在父工程的pom.xml中定义依赖管理：

  用`<dependencyManagement>`标签包裹所有具体依赖

  ```xml
  <!-- 声明此处进行依赖管理 -->
  <dependencyManagement>
      <!-- 具体依赖 -->
      <dependencies>
          <!--spring -->
          <dependency>
              <groupId>org.springframework</groupId>
              <artifactId>spring-context</artifactId>
              <version>5.1.9.RELEASE</version>
          </dependency>
      <dependencies>
  <dependencyManagement>
  ```

- 在子工程的pom.xml中声明其父工程坐标与对应的pom文件位置

  `<parent>`标签包裹父工程

  ```xml
  <!-- 定义该工程的父工程 -->
  <parent>
      <groupId>com.itheima</groupId>
      <artifactId>ssm</artifactId>
      <version>1.0-SNAPSHOT</version>
      <!-- 填写父工程的pom.xml -->
      <relativePath>../ssm/pom.xml</relativePath>
  </parent>
  
  <!-- 父工程定义在pom文件最前面，下面是其他定义 -->
  <!--
  	<moudleVersion>4.0</moudleVersion>
  -->
  ```

- 在子工程中定义依赖关系，无需声明依赖版本，版本参照父工程中依赖的版本

  ```xml
  <dependencies>
      <!--spring -->
      <dependency>
          <groupId>org.springframework</groupId>
          <artifactId>spring-context</artifactId>
      </dependency>
  </dependencies>
  ```

- 工程管理：在子工程的pom.xml中：

  ```xml
  <!-- 子工程应当与父工程隶属同一组织id，可删 -->
  <goupId>com.chongming</goupId>
  <artifactId>ssm_pojo</artifactId>
  <!-- 子工程当尽量与父工程版本保持一致，可删 -->
  <version>1.0</version>
  <packaging>jar<package>
  ```

**继承的资源：**

![image-20230525094943579](https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-masterimage-20230525094943579.png)

### 异同点

**作用：**

- 聚合用于快速构建项目
- 继承用于快速配置

**相同点：**

- 聚合和继承的pom.xml文件打包方式均为pom，可以将两种关系制作成同一个pom文件
- 聚合和继承均属于设计型模块，并无实际的模块内容

**不同点：**

- 聚合是在当前模块中配置关系，聚合可以感知到参与聚合的模块有哪些
- 继承是在子模块中配置关系，父模块无法感知哪些子模块继承了自己

## 属性

> 聚合和继承让子模块配置统一，但在父版本中：
>
> 当maven中配置很复杂的时候，修改一个地方的配置可能要修改很多地方的配置，麻烦且容易出错
>
> maven中可以通过属性创建类似java中的变量，其他地方引用即可
>
> 属性通过`<propersties>`标签定义，`${}`引用
>
> ```shell
> mvn helpsystem
> ```
>
> **属性类别：**
>
> - 自定义属性
> - 内置属性
> - Setting属性
> - Java体系系统属性
> - 环境变量属性

### 自定义属性

**作用：**等同定义变量，方便统一维护

**定义格式：**

```xml
<!-- 定义自定义属性 -->
<properties>
    <spring.version>5.1.9.RELEASE</spring.version>
    <junit.version>4.12</junit.version>
</properties
```

**调用格式：**

```xml
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-context</artifactId>
    <version>${spring.version}</version>
</dependency
```

### 内置属性

**作用：**使用maven内置属性，快速配置

**调用格式：**

```java
${basedir}
${version} // 当前工程版本，引用其他模块时可以用
```

**常用属性：**

- `${basedir}`表示项目的根路径，即包含pom.xml文件的目录
- `${version}`表示项目版本
- `${project.basedir}`同${basedir}
- `${project.baseUri}`表示项目文件地址
- `${maven.build.timestamp}`表示项目构建开始时间
- `${maven.build.timestamp.format}`表示$`{maven.build.timestamp}`的展示格式，默认值为`yyyyMMdd-HHmm`

### Setting属性

**作用：**使用Maven配置文件setting.xml中的标签属性，用于动态配置

**调用格式：**

```
${settings.localRepository}
```

### Java系统属性

**作用：**读取Java系统属性

**调用格式：**

```
${user.home}
```

### 环境变量属性

**作用：**读取操作系统中的环境变量属性

**调用格式：**

```
${env.JAVA_HOME}
```

## 版本管理

**工程版本区分：**

- **SNAPSHOT（快照版本）**
  - 项目开发过程中，为方便团队成员合作，解决模块间相互依赖和时时更新的问题，开发者对每个模块进行构建的时候，输出的临时性版本称为快照版本（测试阶段版本）
  - 快照版本会随着开发的进展不断更新
- **RELEASAE（发布版本）**
  - 项目开发到进入阶段里程碑后，向团队外部发布较为稳定的版本，这种版本所对应的构建文件是稳定的，即便进行功能的后续开发，也不会改变当前发布的版本内容，这种版本称为发布版本

**工程版本号约定：**

- **约定规范：**
  - **主版本：**表示项目重大架构的变更，如：spring5相较于spring4的迭代
  - **次版本：**表示有较大的功能增加和变化，或者全面系统地修复漏洞
  - **增量版本：**表示有重大漏洞的修复
  - **里程碑版本：**表明一个版本的里程碑（版本内部）。这样的版本同下一个正式版本相比，相对来说不是很稳定，有待更多测试
- **范例：** 5.1.9.RELEASE

## 资源配置

> 资源配置多文件维护：当前项目很多配置文件，不好管理
>
> ![image-20230525102431107](https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-masterimage-20230525102431107.png)
>
> 可以在pom属性中统一配置，在其他配置文件中引用pom属性

**作用：**在任意配置文件中加载pom文件中定义的属性

**调用格式：**在需要引用pom属性的配置文件中采用`${}`使用pom属性值

```
${jdbc.url}
```

**开启配置文件加载pom属性：** `<resources>`标签

- test包下的配置文件通过`<testResources>`标签

```xml
<!-- 配置资源文件对应的信息 -->
<resources>
    <resource>
        <!-- 设定配置文件对应的位置目录，支持使用属性动态设定路径  -->
        <directory>${project.basedir}/src/main/resources</directory>
        <!-- 开启对配置文件的资源加载过滤 -->
        <filtering>true</filtering>
    </resource>
</resources>
```

## 多环境开发配置

> **多环境兼容：**
>
> ![image-20230525103558464](https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-masterimage-20230525103558464.png)
>
> 通过pom.xml文件中定义属性进行资源配置，当环境切换时需要修改属性，不方便，可以通过多环境开发配置，配置不同环境下加载不同的属性。

**多环境配置：** 在pom.xml文件`<project>`根下

```xml
<!-- 创建多环境 -->
<profiles>
    
    <!-- 定义具体的环境：生产环境 -->
    <profile>
        <!-- 定义环境对应的唯一名称 -->
        <id>pro_env</id>
        <!-- 定义环境中专用的属性值 -->
        <properties>
            <jdbc.url>jdbc:mysql://127.1.1.1:3306/ssm_db</jdbc.url>
        </properties>
        <!-- 设置默认启动(install时默认使用) -->
        <activation>
            <activeByDefault>true</activeByDefault>
        </activation>
    </profile>
    
    <!-- 定义具体的环境：开发环境 -->
    <profile>
        <id>dev_env</id>
        ……
    </profile>
    
</profiles>
```

**加载指定环境：**

- **作用：**加载指定环境配置
- **调用格式：**`mvn 指令 -p 环境定义的唯一id`
- **范例：**`mvn install -p pro_env`

## 跳过测试

> **跳过测试环节的应用场景：**
>
> - 整体模块功能未开发
> - 模块中某个功能未开发完毕
> - 单个功能更新调试导致其他功能失败
> - 快速打包
> - ......

****

**使用命令跳过测试：**

- 命令：`mvn 指令 -D skipTests`
- 注意事项：执行的指令，生命周期必须包含测试环节

****

**使用IDEA界面跳过测试：**

![image-20230525104715809](https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-masterimage-20230525104715809.png)

****

**使用配置跳过测试：**

```xml
<plugin>
    <artifactId>maven-surefire-plugin</artifactId>
    <version>2.22.1</version>
    <configuration>
        <!-- 设置跳过测试 -->
        <skipTests>true</skipTests> 
        <!-- 包含指定的测试用例（即跳过此测试类） -->
        <includes> 
            <include>**/User*Test.java</include>
        </includes>
        <!-- 排除指定的测试用例（即不跳过此测试类） -->
        <excludes>
            <exclude>**/User*TestCase.java</exclude>
        </excludes>
    </configuration>
</plugin>
```

## 私服-Nexus

> 多模块开发，不同的开发者开发不同的模块，项目公共依赖可以去maven中央仓库获取
>
> 但是项目里的需要共享的资源不能去中央仓库拿，比如：
>
> 开发者A开发dao和pojo，它的dao使用自己的pojo，没有问题
>
> 但是开发者B开发service需要开发者A的dao，即开发者A和开发者B在项目中需要进行小范围的资源共享，可以通过搭建私服实现
>
> ![image-20230525110016962](https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-masterimage-20230525110016962.png)
>
> 

### Nexus

- Nexus是Sonatype公司的一款maven私服产品
- 下载地址：https://help.sonatype.com/repomanager3/download
