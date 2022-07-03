# 【Web概述】

> **B/S 架构：** Browser/Server，浏览器/服务器 架构模式，它的特点是，客户端只需要浏览器，应用程序的逻辑和数据都存储在服务器端。浏览器只需要请求服务器，获取Web资源，服务器把Web资源发送给浏览器即可。
>
> **静态资源：** 
>
> * 静态资源主要包含HTML、CSS、JavaScript、图片等，主要负责页面的展示。
> * 前端网页制作`三剑客`(HTML+CSS+JavaScript)，使用这些技术就可以制作出效果比较丰富的网页，将来展现给用户。但是由于做出来的这些内容都是静态的，这就会导致所有的人看到的内容将是一模一样。
>
> **动态资源：**
>
> * 动态资源主要包含Servlet、JSP等，主要用来负责逻辑处理。
> * 动态资源处理完逻辑后会把得到的结果交给静态资源来进行展示，动态资源和静态资源要结合一起使用。
>
> **数据库：**
>
> * 数据库主要负责存储数据。
> * 整个Web的访问过程就如下图所示:
>   * 浏览器发送一个请求到服务端，去请求所需要的相关资源;
>   * 资源分为动态资源和静态资源，动态资源可以是使用Java代码按照Servlet和JSP的规范编写的内容;
>   * 在Java代码可以进行业务处理也可以从数据库中读取数据;
>   * 拿到数据后，把数据交给HTML页面进行展示,再结合CSS和JavaScript使展示效果更好;
>   * 服务端将静态资源响应给浏览器;
>   * 浏览器将这些资源进行解析;
>   * 解析后将效果展示在浏览器，用户就可以看到最终的结果。
>
> **HTTP协议：** 主要定义通信规则
>
> **Web服务器：** 
>
> * Web服务器:负责解析 HTTP 协议，解析请求数据，并发送响应数据
> * 浏览器按照HTTP协议发送请求和数据，后台就需要一个Web服务器软件来根据HTTP协议解析请求和数据，然后把处理结果再按照HTTP协议发送给浏览器
> * Web服务器软件有很多，最为常用的是Tomcat服务器

# 【HTTP】

## 概述

> HyperText Transfer Protocol，超文本传输协议，规定了浏览器和服务器之间数据传输的规则
>
> 数据传输的规则指的是请求数据和响应数据需要按照指定的格式进行传输。
>
> 如果想知道具体的格式，可以打开浏览器，点击`F12`打开开发者工具，点击`Network(网络)`>来查看某一次请求的请求数据和响应数据具体的格式内容，如下图所示:
>
> ![1627046235092](https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-master/1627046235092.png)
>
> 注意:在浏览器中如果看不到上述内容，需要清除浏览器的浏览数据。chrome浏览器可以使用ctrl+shift+Del进行清除。

**HTTP协议特点：**

* **基于TCP协议: 面向连接，安全**

  TCP是一种面向连接的(建立连接之前是需要经过三次握手)、可靠的、基于字节流的传输层通信协议，在数据传输方面更安全。

* **基于请求-响应模型的:一次请求对应一次响应**

  请求和响应是一一对应关系

* **HTTP协议是无状态协议:** 对于事物处理没有记忆能力。每次请求-响应都是独立的

  无状态指的是客户端发送HTTP请求给服务端之后，服务端根据请求响应数据，响应完后，不会记录任何信息。这种特性有优点也有缺点，

  * 缺点:多次请求间不能共享数据
  * 优点:速度快

  请求之间无法共享数据会引发的问题，如:

  * 京东购物，`加入购物车`和`去购物车结算`是两次请求，
  * HTTP协议的无状态特性，加入购物车请求响应结束后，并未记录加入购物车是何商品
  * 发起去购物车结算的请求后，因为无法获取哪些商品加入了购物车，会导致此次请求无法正确展示数据
  * 解决方案：`会话技术(Cookie、Session)`


## 请求数据格式

> **请求数据总共分为三部分内容，分别是请求行、请求头、请求体**

![1627050004221](https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-master/1627050004221.png)

* **请求行:** HTTP请求中的第一行数据，请求行包含三块内容，分别是 `GET[请求方式] /[请求URL路径] HTTP/1.1[HTTP协议及版本]`

  请求方式有七种,最常用的是GET和POST

* **请求头:** 第二行开始，格式为key: value形式

  请求头中会包含若干个属性，常见的HTTP请求头有:

  |       属性        |                             说明                             |
  | :---------------: | :----------------------------------------------------------: |
  |      `Host`       |                       表示请求的主机名                       |
  |   `User-Agent`    | 浏览器版本,例如Chrome浏览器的标识类似`Mozilla/5.0 ...Chrome/79`，IE浏览器的标识类似`Mozilla/5.0 (Windows NT ...)like Gecko` |
  |     `Accept`      | 表示浏览器能接收的资源类型，如`text/*`，`image/*`或者`/*`(表示所有） |
  | `Accept-Language` |    表示浏览器偏好的语言，服务器可以据此返回不同语言的网页    |
  | `Accept-Encoding` |     表示浏览器可以支持的压缩类型，例如`gzip`,`deflate`等     |

- **请求体:** POST请求的最后一部分，存储请求参数

  下图红线框的内容就是请求体的内容，请求体和请求头之间是有一个空行隔开。此时浏览器发送的是POST请求

  * GET请求请求参数在请求行中，没有请求体，POST请求请求参数在请求体中
  * GET请求请求参数大小有限制，POST没有

![1627050930378](https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-master/1627050930378.png)

## 响应数据格式

> **响应数据总共分为三部分内容，分别是响应行、响应头、响应体**

![1627053710214](https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-master/1627053710214.png)

* **响应行:** 响应数据的第一行,响应行包含三块内容，分别是`HTTP/1.1[HTTP协议及版本] 200[响应状态码] ok[状态码的描述]`

* **响应头:** 第二行开始，格式为`key:value`形式

  响应头中会包含若干个属性，常见的HTTP响应头有:

  |        属性        |                             说明                             |
  | :----------------: | :----------------------------------------------------------: |
  |   `Content-Type`   |      表示该响应内容的类型，例如`text/html，image/jpeg`       |
  |  `Content-Length`  |                表示该响应内容的长度（字节数）                |
  | `Content-Encoding` |                表示该响应压缩算法，例如`gzip`                |
  |  `Cache-Control`   | 指示客户端应如何缓存，例如`max-age=300`表示可以最多缓存300秒 |

- **响应体:**  存放响应数据，和响应头之间有空行隔开

  上图中`<html>...</html>`这部分内容就是响应体

## 响应状态码

### 状态码大类

| 状态码分类 | 说明                                                         |
| ---------- | ------------------------------------------------------------ |
| 1xx        | **响应中**——临时状态码，表示请求已经接受，告诉客户端应该继续请求或者如果它已经完成则忽略它 |
| 2xx        | **成功**——表示请求已经被成功接收，处理已完成                 |
| 3xx        | **重定向**——重定向到其它地方：它让客户端再发起一个请求以完成整个处理。 |
| 4xx        | **客户端错误**——处理发生错误，责任在客户端，如：客户端的请求一个不存在的资源，客户端未被授权，禁止访问等 |
| 5xx        | **服务器端错误**——处理发生错误，责任在服务端，如：服务端抛出异常，路由出错，HTTP版本不支持等 |

> [状态 | Status - HTTP 中文开发手册 - 开发者手册 - 云+社区 - 腾讯云 (tencent.com)](https://cloud.tencent.com/developer/chapter/13553)

### 常见的响应状态码

| 状态码 | 英文描述                               | 解释                                                         |
| ------ | -------------------------------------- | ------------------------------------------------------------ |
| 200    | **`OK`**                               | 客户端请求成功，即处理成功                                   |
| 302    | **`Found`**                            | 重定向，指示所请求的资源已移动到由响应头给定的 URL，浏览器会自动重新访问到这个页面 |
| 304    | **`Not Modified`**                     | 隐式重定向，告诉客户端，你请求的资源至上次取得后，服务端并未更改，直接用本地缓存。 |
| 400    | **`Bad Request`**                      | 客户端请求有语法错误，不能被服务器所理解                     |
| 403    | **`Forbidden`**                        | 服务器收到请求，但是拒绝提供服务，比如：没有权限访问相关资源 |
| 404    | **`Not Found`**                        | 请求资源不存在，一般是URL输入有误，或者网站资源被删除了      |
| 428    | **`Precondition Required`**            | 服务器要求有条件的请求，告诉客户端要想访问该资源，必须携带特定的请求头 |
| 429    | **`Too Many Requests`**                | 太多请求，可以限制客户端请求某个资源的数量，配合 `Retry-After`(多长时间后可以请求)响应头一起使用 |
| 431    | **` Request Header Fields Too Large`** | 请求头太大，服务器不愿意处理请求，因为它的头部字段太大。请求可以在减少请求头域的大小后重新提交。 |
| 405    | **`Method Not Allowed`**               | 请求方式有误，比如应该用GET请求方式的资源，用了POST          |
| 500    | **`Internal Server Error`**            | 服务器发生不可预期的错误，服务器出异常了                     |
| 503    | **`Service Unavailable`**              | 服务器尚未准备好处理请求，服务器刚刚启动，还未初始化好       |
| 511    | **`Network Authentication Required`**  | 客户端需要进行身份验证才能获得网络访问权限                   |

# 【Tomcat】

## 简介

> Web服务器是一个应用程序(软件)，对HTTP协议的操作进行封装，使得程序员不必直接对协议进行操作，让Web开发更加便捷。主要功能是"提供网上信息浏览服务"。
>
> 将来把Web项目部署到Web服务器软件中，当Web服务器软件启动后，部署在Web服务器软件中的页面就可以直接通过浏览器来访问了。
>
> **Web服务器软件使用步骤**
>
> * 准备静态资源
> * 下载安装Web服务器软件
> * 将静态资源部署到Web服务器上
> * 启动Web服务器使用浏览器访问对应的资源
>
> **Tomcat**
>
> * Tomcat是Apache软件基金会一个核心项目，是一个开源免费的轻量级Web服务器，支持Servlet/JSP少量JavaEE规范。
>
> * JavaEE规范：
>
>   Java Enterprise Edition,Java企业版，指Java企业级开发的技术规范总和。
>
>   包含13项技术规范:JDBC、JNDI、EJB、RMI、JSP、Servlet、XML、JMS、Java IDL、JTS、JTA、JavaMail、JAF。
>
> * 因为Tomcat支持Servlet/JSP规范，所以Tomcat也被称为Web容器、Servlet容器。Servlet需要依赖Tomcat才能运行。
>
> * Tomcat的官网: [Apache Tomcat® - Welcome!](https://tomcat.apache.org/)
>
>   从官网上可以下载对应的版本进行使用。

## 基本使用

> Tomcat版本：9.0

### 下载与安装

- 官网下载：[Apache Tomcat® - Welcome!](https://tomcat.apache.org/)

![1627178001030](https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-master/1627178001030.png)

- 直接解压(注意路径不要有路径和空格)，目录结构如下：

  ![1627178815892](https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-master/1627178815892.png)

  - `bin`目录下有两类文件，一种是以`.bat`结尾的，是Windows系统的可执行文件，一种是以`.sh`结尾的，是Linux系统的可执行文件。

  - `webapps` 就是以后项目部署的目录

### 启动

- 双击：`bin\startup.bat`
- 双击：`bin\Tomcat9.exe`

启动后，通过浏览器访问`http://localhost:8080`能看到Apache Tomcat的内容就说明Tomcat已经启动成功。

![image-20220429094332060](https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-master/image-20220429094332060.png)

> 注意: 启动的过程中，控制台有中文乱码，需要修改`conf/logging.prooperties`
>
> ![1627199827589](https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-master/1627199827589.png)

### 关闭

- 关掉运行窗口（不推荐）
- 双击`bin\shutdown.bat`

### 配置

- 修改启动端口号：`conf/server.xml`

![image-20220429100746432](https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-master/image-20220429100746432.png)

> 注: HTTP协议默认端口号为80，如果将Tomcat端口号改为80，则将来访问Tomcat时，将不用输入端口号。
>
> **启动时可能出现的错误**
>
> * Tomcat的端口号取值范围是0-65535之间任意未被占用的端口，如果设置的端口号被占用，启动的时候就会出现错误：`Address already in use: bind`
> * Tomcat启动的时候，启动窗口一闪而过: 需要检查JAVA_HOME环境变量是否正确配置
>
> ![1627201248802](https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-master/1627201248802.png)

### 部署

- Tomcat部署项目： 将项目放置到webapps目录下，即部署完成

  一般JavaWeb都会打包成war包，然后将war包放到`webapps`目录下，Tomcat会自动解压缩war文件

- 启动Tomcat访问地址即可

## Web项目

### web项目结构

> web项目的结构分为：开发中的项目和开发完可以部署的Web项目

**Maven Web项目结构：开发中的项目：**

![1627202865978](https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-master/1627202865978.png)

**开发完成部署的Web项目：**

![1627202903750](https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-master/1627202903750.png)

* 开发项目通过执行Maven打包命令`package`,可以获取到部署的Web项目目录
* 编译后的Java字节码文件和resources的资源文件，会被放到WEB-INF下的classes目录下
* pom.xml中依赖坐标对应的jar包，会被放入WEB-INF下的lib目录下

### 创建web项目

> 使用Maven来创建Web项目

**步骤：**

- 创建Maven项目，选择骨架：

![image-20220429102538990](https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-master/image-20220429102538990.png)

- 输入项目信息：

![](https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-master/20220429102701.png)

- 确认Maven信息：

![image-20220429102756083](https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-master/image-20220429102756083.png)

- 删除pom.xml中的多余内容，只保留以下内容：

​	  `<package>`处要打成war包

![image-20220429103016436](https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-master/image-20220429103016436.png)

- 补齐Maven Web项目缺失的目录结构：默认是没有java和resources目录，需要手动补齐

![image-20220429103534649](https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-master/image-20220429103534649.png)

## IDEA集成

> 将本地安装好的Tomcat集成到IDEA中，完成项目部署

**打开添加本地Tomcat面板：**

![image-20220429103827511](https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-master/image-20220429103827511.png)

**指定本地Tomcat的具体路径：**

![image-20220429104048508](https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-master/image-20220429104048508.png)

**部署项目：**

![image-20220429104257604](https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-master/image-20220429104257604.png)

**修改项目启动路径：**

![image-20220429105122740](https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-master/image-20220429105122740.png)

**修改项目启动时默认访问的浏览器和路径**

![image-20220429104705878](https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-master/image-20220429104705878.png)

# 【Servlet】

