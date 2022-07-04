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

## 简介

> * Servlet是JavaWeb最为核心的内容，它是Java提供的一门动态web资源开发技术。
> * 使用Servlet就可以实现，根据不同的登录用户在页面上动态显示不同内容。
> * Servlet是JavaEE规范之一，其实就是一个接口，将来需要定义Servlet类实现Servlet接口，并由web服务器运行Servlet

## 快速入门

**1.创建Web项目web-demo，导入servlet依赖**

```xml
<dependency>
    <groupId>javax.servlet</groupId>
    <artifactId>javax.servlet-api</artifactId>
    <version>3.1.0</version>
    <!--
      provided指的是在编译和测试过程中有效,最后生成的war包时不会加入
       因为Tomcat的lib目录中已经有servlet-api这个jar包，如果在生成war包的时候生效就会和Tomcat中的jar包冲突，导致报错
    -->
    <scope>provided</scope>
</dependency>
```

**2.定义一个类，实现servlet接口并重写接口，并在service方法中输入一句话**

```java
public class ServletDemo1 implements Servlet {
    @Override
    public void init(ServletConfig config) throws ServletException {
}

    // 这个方法用于获取servlet配置对象
    @Override
    public ServletConfig getServletConfig() {
        return null;
    }

    @Override
    public void service(ServletRequest req, ServletResponse res) throws ServletException, IOException {
        System.out.println("Hello World!");
    }

    // 这个方法用于返回servlet的一些信息
    @Override
    public String getServletInfo() {
        return null;
    }

    @Override
    public void destroy() {

    }
}
```

**3.在类上使用@WebServlet注解，配置该Servlet的访问路径**

```java
@WebServlet("/demo1")
```

**4.启动Tomcat，在浏览器中输入URL地址访问该Servlet**

```
http://localhost:8080/web-demo/demo1
```

控制台中输出了“Hello World！”，即访问成功

## 执行流程

![image-20220704091853668](https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-masterimages-masterimage-20220704091853668.png)

* 浏览器发出`http://localhost:8080/web-demo/demo1`请求，从请求中可以解析出三部分内容，分别是`localhost:8080`、`web-demo`、`demo1`
  * 根据`localhost:8080`可以找到要访问的Tomcat Web服务器
  * 根据`web-demo`可以找到部署在Tomcat服务器上的web-demo项目
  * 根据`demo1`可以找到要访问的是项目中的哪个Servlet类，根据@WebServlet后面的值进行匹配
* 找到ServletDemo1这个类后，Tomcat Web服务器就会为ServletDemo1这个类创建一个对象，然后调用对象中的service方法
  * ServletDemo1实现了Servlet接口，所以类中必然会重写service方法供Tomcat Web服务器进行调用
  * service方法中有ServletRequest和ServletResponse两个参数，ServletRequest封装的是请求数据，ServletResponse封装的是响应数据，可以通过这两个参数实现前后端的数据交互

## 生命周期

**生命周期: 对象的生命周期指一个对象从被创建到被销毁的整个过程。**

Servlet运行在Servlet容器(web服务器)中，其生命周期由容器来管理，分为4个阶段：

1. **加载和实例化:** 默认情况下，当Servlet第一次被访问时，由容器创建Servlet对象。

   ```java
   // 默认情况，Servlet会在第一次访问被容器创建，但是如果创建Servlet比较耗时的话，那么第一个访问的人等待的时间就比较长，用户的体验就比较差，所以可以把Servlet的创建放到服务器启动的时候来创建
   
   @WebServlet(urlPatterns = "/demo1",loadOnStartup = 1)
   loadOnstartup的取值有两类情况：
   	（1）负整数:第一次访问时创建Servlet对象
   	（2）0或正整数:服务器启动时创建Servlet对象，数字越小优先级越高
   ```

2. **初始化:** 在Servlet实例化之后，容器将调用Servlet的init()方法初始化这个对象，完成一些如加载配置文件、创建连接等初始化的工作。该方法只调用一次。

3. **请求处理:** 每次请求Servlet时，Servlet容器都会调用Servlet的service()方法对请求进行处理。

4. **服务终止:** 当需要释放内存或者容器关闭时，容器就会调用Servlet实例的destroy()方法完成资源的释放。在destroy()方法调用之后，容器会释放这个Servlet实例，该实例随后会被Java的垃圾收集器所回收。

**注意：**

- init方法在Servlet对象被创建的时候执行，只执行1次
- service方法在Servlet被访问的时候调用，每访问1次就调用1次
- destroy方法在Servlet对象被销毁的时候调用，只执行1次

## 体系结构

![1627240593506](https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-master1627240593506.png)B/S架构的web项目，都是针对HTTP协议，所以自定义Servlet应继承HttpServlet

**案例：**

```java
@WebServlet("/demo4")
public class ServletDemo4 extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        //TODO GET 请求方式处理逻辑
        System.out.println("get...");
    }
    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        //TODO Post 请求方式处理逻辑
        System.out.println("post...");
    }
}
```

**HttpServlet的service()方法通过获取请求方式来调用不同的请求方法：**

```java
protected void service(HttpServletRequest req, HttpServletResponse resp)
    throws ServletException, IOException
{
    String method = req.getMethod();
    if (method.equals(METHOD_GET)) {
        long lastModified = getLastModified(req);
        if (lastModified == -1) {
            // servlet doesn't support if-modified-since, no reason
            // to go through further expensive logic
            doGet(req, resp);
        } else {
            long ifModifiedSince = req.getDateHeader(HEADER_IFMODSINCE);
            if (ifModifiedSince < lastModified) {
                // If the servlet mod time is later, call doGet()
                // Round down to the nearest second for a proper compare
                // A ifModifiedSince of -1 will always be less
                maybeSetLastModified(resp, lastModified);
                doGet(req, resp);
            } else {
                resp.setStatus(HttpServletResponse.SC_NOT_MODIFIED);
            }
        }
    } else if (method.equals(METHOD_HEAD)) {
        long lastModified = getLastModified(req);
        maybeSetLastModified(resp, lastModified);
        doHead(req, resp);
    } else if (method.equals(METHOD_POST)) {
        doPost(req, resp);
        
    } else if (method.equals(METHOD_PUT)) {
        doPut(req, resp);
        
    } else if (method.equals(METHOD_DELETE)) {
        doDelete(req, resp);
        
    } else if (method.equals(METHOD_OPTIONS)) {
        doOptions(req,resp);
        
    } else if (method.equals(METHOD_TRACE)) {
        doTrace(req,resp);
        
    } else {
        //
        // Note that this means NO servlet supports whatever
        // method was requested, anywhere on this server.
        //
        String errMsg = lStrings.getString("http.method_not_implemented");
        Object[] errArgs = new Object[1];
        errArgs[0] = method;
        errMsg = MessageFormat.format(errMsg, errArgs);
        
        resp.sendError(HttpServletResponse.SC_NOT_IMPLEMENTED, errMsg);
    }
}
```

## url配置

Servlet类编写好后，要想被访问到，就需要配置其访问路径：

一个Servlet,可以配置多个urlPattern

```java
@WebServlet(urlPatterns = {"/demo7","/demo8"})
```

配置后两个路径都能访问到一个servlet

**urlPattern配置规则：**

- **精准匹配**

  ```java
  @WebServlet(urlPatterns = "/user/select")
  ```

- **目录匹配**

  ```java
  @WebServlet(urlPatterns = "/user/*")
  ```

  访问user目录下的所有路径都会访问到一个servlet

  精准匹配优先级高于目录匹配

- **扩展名匹配**

  ```java
  @WebServlet(urlPatterns = "*.do")
  ```

  访问路径为：`http://localhost:8080/web-demo/任意.do`

  非扩展名匹配的时候路径配置必须有`/`，即`/路径`

  使用扩展名匹配时不能添加`/`，即不能写成`/*.do`

- **任意匹配**

  ```java
  @WebServlet(urlPatterns = "/")
  @WebServlet(urlPatterns = "/*")
  ```

  访问路径为：`http://localhost:8080/web-demo/任意`

  - `/`：覆盖DefaultServlet，当其他的url都匹配不上时会访问这个servlet
  - `/*`：匹配任意访问路径
  - `/*`优先级高于`/`
  - DefaultServlet是用来处理静态资源的，如果配置了`/`会把默认的覆盖掉，请求静态资源时就不会走默认而是自定义的Servlet类，最终导致静态资源不能访问

**配置的优先级为：**

精确匹配 > 目录匹配 > 扩展名匹配 > `/*` > `/`

## xml配置

> Servlet从3.0版本后开始支持注解配置，3.0版本前只支持XML配置文件的配置方法

**XML配置步骤：**

- **编写Servlet类：**

  ```java
  public class ServletDemo3 extends HttpServlet {
      @Override
      protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
          super.doGet(req, resp);
      }
  
      @Override
      protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
          super.doPost(req, resp);
      }
  }
  ```

- **在web.xml中配置该Servlet**

  ```xml
  <web-app>
    <display-name>Archetype Created Web Application</display-name>
    
    <!-- Servlet 全类名 -->
    <servlet>
      <!-- servlet的名称，名字任意 -->
      <servlet-name>ServletDemo3</servlet-name>
      <!--servlet的类全名-->
      <servlet-class>servlet.ServletDemo3</servlet-class>
    </servlet>
  
    <!-- Servlet 访问路径 -->
    <servlet-mapping>
      <!-- servlet的名称，要和上面的名称一致 -->
      <servlet-name>ServletDemo3</servlet-name>
      <!-- servlet的访问路径-->
      <url-pattern>/demo13</url-pattern>
    </servlet-mapping>
    
  </web-app>
  ```

# 【Request&Response】

## 简介

> ![1628857632899](https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-master1628857632899.png)
>
> Request是请求对象，Response是响应对象。
>
> ```java
> @Override
> // doGet方法的参数就是request和response对象
> protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
>     super.doGet(req, resp);
> }
> ```
>
> * **request:** 获取请求数据
>   * 浏览器会发送HTTP请求到后台服务器[Tomcat]
>   * HTTP的请求中会包含很多请求数据[请求行+请求头+请求体]
>   * 后台服务器[Tomcat]会对HTTP请求中的数据进行解析并把解析结果存入到一个对象中
>   * 所存入的对象即为request对象，所以可以从request对象中获取请求的相关参数
>   * 获取到数据后就可以继续后续的业务，比如获取用户名和密码就可以实现登录操作的相关业务
> * **response:** 设置响应数据
>   * 业务处理完后，后台就需要给前端返回业务处理的结果即响应数据
>   * 把响应数据封装到response对象中
>   * 后台服务器[Tomcat]会解析response对象,按照[响应行+响应头+响应体]格式拼接结果
>   * 浏览器最终解析结果，把内容展示在浏览器给用户浏览

**使用案例：**

```java
@Override
protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
    //使用request对象 获取请求数据(url?name=zhangsan)
    String name = req.getParameter("name");
    //使用response对象 设置响应数据
    resp.setHeader("content-type","text/html;charset=utf-8");
    resp.getWriter().write("<h1>"+name+",欢迎您！</h1>");
}
```

**效果：**

![image-20220704100738941](https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-masterimage-20220704100738941.png)

## Request对象

### 继承体系

![1628740441008](https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-master1628740441008.png)

### 获取请求数据

HTTP请求数据总共分为三部分内容，分别是请求行、请求头、请求体

- **获取请求行数据：**

  ```java
  // 1.获取请求方式
  String getMethod();
  // 2.获取虚拟目录(项目访问路径): /web-demo
  String getContextPath();
  // 3.获取URL(统一资源定位符): "http://localhost:8080/web-demo/demo3"
  StringBuffer getRequestURL();
  // 4.获取URI(统一资源标识符): "/web-demo/demo3"
  String getRequestURI();
  // 5.获取请求参数(GET方式): "username=zhangsan&password=123"
  String getQueryString();
  ```

- **获取请求头数据：**

  请求头的数据格式为`key:value`

  ![1628768652535](https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-master1628768652535.png)

  ```java
  // 根据请求头名称获取对应值的方法为:
  String getHeader(String name);
  ```

- **获取请求体数据：**

  浏览器在发送GET请求的时候是没有请求体的，需要把请求方式变更为POST，请求体中的数据格式如下:

  ![1628768665185](https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-master1628768665185.png)

  对于请求体中的数据，Request对象提供了如下两种方式来获取其中的数据，分别是:

  - **获取字节输入流:** 如果前端发送的是字节数据，比如传递的是文件数据，则使用该方法

    ```java
    ServletInputStream getInputStream();
    ```

  - **获取字符输入流:** 如果前端发送的是纯文本数据，则使用该方法

    ```java
    BufferedReader getReader();
    ```

- **获取请求参数的通用方式：**

  对于GET方法和POST方法，获取请求参数的方法不一样（前者在请求行中获取，后者在请求体中获取），request针对这些方法进行了封装：

  - **获取所有参数Map集合**

    ```java
    Map<String,String[]> getParameterMap();
    ```

  - **根据名称获取参数值(数组)**

    ```java
    String[] getParameterValues(String name);
    ```

  - **根据名称获取参数值(单个值)**

    ```java
    String getParameter(String name);
    ```

### IDEA配置模板

Servlet具有固定的格式，可以使用IDEA制作一个固定的模板，方便更高效地创建Servlet

1. 根据需求修改模板内容

   ![image-20220704103001277](https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-masterimage-20220704103001277.png)

2. 右键菜单中选中servlet模板

   ![image-20220704103729460](https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-masterimage-20220704103729460.png)

3. 如果右键菜单中没有servlet选项![image-20220704103646955](https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-masterimage-20220704103646955.png)

### 请求参数中文乱码

**Post请求中文乱码：**

> * Post请求出现中文乱码的原因：
>   * POST的请求参数是通过request的getReader()来获取流中的数据
>   * TOMCAT在获取流的时候采用的编码是ISO-8859-1
>   * ISO-8859-1编码是不支持中文的，所以会出现乱码
> * 解决方案：
>   1. 页面设置的编码格式为UTF-8
>   2. 把tomcat在获取流数据之前的编码设置为UTF-8
>   3. 通过request.setCharacterEncoding("UTF-8")设置编码,UTF-8也可以写成小写

**解决代码：**

```java
@Override
protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
    //1. 解决乱码: POST getReader()
    //设置字符输入流的编码，设置的字符集要和页面保持一致
    request.setCharacterEncoding("UTF-8");
    //2. 获取username
    String username = request.getParameter("username");
    System.out.println(username);
}    
```

**Get请求中文乱码：**

**新版tomcat(8以上版本)已经解决了Get请求中文乱码**

> * Get请求出现中文乱码的原因：
>   * 浏览器通过HTTP协议发送请求和数据给后台服务器（Tomcat)
>   * 浏览器在发送HTTP的过程中会对中文数据进行URL编码
>   * 在进行URL编码的时候会采用页面`<meta>`标签指定的UTF-8的方式进行编码，`张三`编码后的结果为`%E5%BC%A0%E4%B8%89`
>   * 后台服务器(Tomcat)接收到`%E5%BC%A0%E4%B8%89`后会默认按照`ISO-8859-1`进行URL解码
>   * 由于前后编码与解码采用的格式不一样，就会导致后台获取到的数据为乱码。
> * URL编码：
>   1. 将字符串按照编码方式转为二进制
>   2. 每个字节转为2个16进制数并在前边加上%
> * 解决方案：
>   1. 按照ISO-8859-1编码获取乱码`å¼ ä¸`对应的字节数组
>   2. 按照UTF-8编码获取字节数组对应的字符串

**解决代码：**

```java
String username = "张三";

// URL编码
String encode = URLEncoder.encode(username, "utf-8");
// URL解码
String decode = URLDecoder.decode(encode, "ISO-8859-1");

// 以下是解决乱码数据的步骤
new String(username.getBytes("ISO-8859-1"), "utf-8");
```

### 请求转发

**转发(forward):** 一种在服务器内部的资源跳转方式。

![1628851404283](https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-master1628851404283.png)

1. 浏览器发送请求给服务器，服务器中对应的资源A接收到请求。
2. 资源A处理完请求后将请求发给资源B。
3. 资源B处理完后将结果响应给浏览器，这个过程就叫请求转发

**实现方式：**

```java
req.getRequestDispatcher("资源B路径").forward(req,resp);
```

**如何在转发资源间共享数据：使用Request对象**

Request对象提供了三个方法：

- 存储数据到request域中

  ```java
  void setAttribute(String name, Object o);
  ```

- 根据key获取值

  ```java
  Object getAttribute(String name);
  ```

- 根据key删除该键值对

  ```java
  void removeAttribute(String name);
  ```

**请求转发的特点：**

- 浏览器地址栏路径不发生变化
- 只能转发到当前服务器的内部资源，不能从一台服务器转发访问另一台服务器
- 一次请求，可以在转发资源间使用request对象共享数据

## Response对象

### 继承体系

![1628857761317](https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-master1628857761317.png)

### 设置响应数据

HTTP响应数据总共分为三部分内容，分别是响应行、响应头、响应体

- **设置响应行**

  ![1628858926498](https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-master1628858926498.png)

  对于响应头，比较常用的就是设置响应状态码：

  ```java
  void setStatus(int sc);
  ```

- **设置响应头**

  ![1628859051368](https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-master1628859051368.png)

  ```java
  void setHeader(String name, String value);
  ```

- **设置响应体**

  ![1628859268095](https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-master1628859268095.png)

  对于响应体，是通过字符、字节输出流的方式往浏览器写入：

  - 字符输出流

    ```java
    PrintWriter writer = response.getWriter();
    
    //content-type，告诉浏览器返回的数据类型是HTML类型数据，这样浏览器才会解析HTML标签
    response.setHeader("content-type","text/html");
    
    writer.write("<h1>aaa</h1>");
    ```

  - 字节输出流

    ```java
    // 1. 读取文件
    FileInputStream fis = new FileInputStream("d://a.jpg");
    // 2. 获取response字节输出流
    ServletOutputStream os = response.getOutputStream();
    // 3. 完成流的copy
    byte[] buff = new byte[1024];
    int len = 0;
    while ((len = fis.read(buff))!= -1){
        os.write(buff,0,len);
    }
    fis.close();
    ```

  **注意:** 一次请求响应结束后，response对象就会被销毁掉，所以不要手动关闭流。

### 请求重定向

**Response重定向(redirect):** 一种资源跳转方式

![1628859860279](https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-master1628859860279.png)

1. 浏览器发送请求给服务器，服务器中对应的资源A接收到请求。
2. 资源A现在无法处理该请求，就会给浏览器响应一个302的状态码+location的一个访问资源B的路径。
3. 浏览器接收到响应状态码为302就会重新发送请求到location对应的访问地址去访问资源B。
4. 资源B接收到请求后进行处理并最终给浏览器响应结果，这个过程就叫重定向。

**实现方式：**

```java
resp.setStatus(302);
resp.setHeader("location","资源B的访问路径");

// 简化编写方式(resp1重定向到resp2)
resposne.sendRedirect("/web-demo/resp2")
```

**请求重定向的特点：**

- 浏览器地址栏路径发生变化
- 可以重定向到任何位置的资源（服务内外部均可）
- 两次请求，不能在多个资源间使用request对象共享数据

**转发和重定向的区别：**

![1628862170296](https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-master1628862170296.png)

**转发和重定向的路径问题：**

![1628862652700](https://chongming-images.oss-cn-hangzhou.aliyuncs.com/images-master1628862652700.png)

* **浏览器使用:需要加虚拟目录(项目访问路径)。**

  对于重定向来说，路径最终是由浏览器来发送请求，就需要添加虚拟目录。

* **服务端使用:不需要加虚拟目录。**

  对于转发来说，因为是在服务端进行的，所以不需要加虚拟目录。



