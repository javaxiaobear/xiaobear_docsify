# 1、JavaWeb的概念

> JavaWeb 是指，所有通过Java 语言编写可以通过浏览器访问的程序的总称，叫JavaWeb。
> JavaWeb 是基于请求和响应来开发的。

**什么是请求?**

> 请求是指客户端给服务器发送数 据，叫请求Request。

**什么是响应?**

> 响应是指服务器给客户端回传数据，叫响应Response。

请求和响应是成对出现的，有请求就有响应。

## 1、Web 资源的分类

- 静态资源： html、css、js、txt、mp4 视频, jpg 图片
- 动态资源： jsp 页面、Servlet 程序

## 2、常用的Web 服务器

- Tomcat：由Apache 组织提供的一种Web 服务器，提供对jsp 和Servlet 的支持。它是一种轻量级的**javaWeb 容器**（服务器），也是当前应用最广的JavaWeb 服务器（免费）。

- Jboss：是一个遵从JavaEE 规范的、开放源代码的、纯Java 的EJB 服务器，它支持所有的JavaEE 规范（免

	费）。

- GlassFish： 由Oracle 公司开发的一款JavaWeb 服务器，是一款强健的商业服务器，达到产品级质量（应用很

	少）。

- Resin：是CAUCHO 公司的产品，是一个非常流行的服务器，对servlet 和JSP 提供了良好的支持，

	性能也比较优良，resin 自身采用JAVA 语言开发（收费，应用比较多）。

- WebLogic：是Oracle 公司的产品，是目前应用最广泛的Web 服务器，支持JavaEE 规范，而且不断的完善以适应新的开发要求，适合大型项目（收费，用的不多，适合大公司）。

	

### 1、Tomcat 服务器和Servlet 版本的对应关系

| Tomcat版本 | Servlet/Jsp版本 | JavaEE版本 | 运行环境 |
| ---------- | --------------- | ---------- | -------- |
| 7.0        | 3.0 / 2.2       | 6.0        | JDK6.0   |
| 8.0        | 3.1/ 2.3        | 7.0        | JDK7.0   |

Servlet 程序从**2.5 版本**是现在世面使用最多的版本（**xml 配置**），**Servlet3.0 之后。就是注解版本的Servlet 使用**。

# 2、Servlet

## 1、什么是Servlet？

> Servlet 是JavaEE 规范之一。规范就是接口
> Servlet 就JavaWeb 三大组件之一。三大组件分别是：Servlet 程序、Filter 过滤器、Listener 监听器。
> Servlet 是运行在服务器上的一个java 小程序，它可以接收客户端发送过来的请求，并响应数据给客户端

## 2、手动实现Servlet 程序

1、编写一个类去实现Servlet 接口

2、实现service 方法，处理请求，并响应数据

3、到web.xml 中去配置servlet 程序的访问地址

Servlet 程序的示例代码：

```java
public class HelloServlet implements Servlet {
    /**
* service 方法是专门用来处理请求和响应的
*/
    @Override
    public void service(ServletRequest servletRequest, ServletResponse servletResponse) throws ServletException, IOException {
        System.out.println("Hello Servlet 被访问了");
    }
}
```

```xml
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee
http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd"
version="4.0">
<!-- servlet 标签给Tomcat 配置Servlet 程序-->
<servlet>
<!--servlet-name 标签Servlet 程序起一个别名（一般是类名） -->
<servlet-name>HelloServlet</servlet-name>
<!--servlet-class 是Servlet 程序的全类名-->
<servlet-class>com.atguigu.servlet.HelloServlet</servlet-class>
</servlet>
<!--servlet-mapping 标签给servlet 程序配置访问地址-->
<servlet-mapping>
<!--servlet-name 标签的作用是告诉服务器，我当前配置的地址给哪个Servlet 程序使用-->
<servlet-name>HelloServlet</servlet-name>
<!--url-pattern 标签配置访问地址<br/>
/ 斜杠在服务器解析的时候，表示地址为：http://ip:port/工程路径<br/>
/hello 表示地址为：http://ip:port/工程路径/hello <br/>
-->
<url-pattern>/hello</url-pattern>
</servlet-mapping>
</web-app>
```

**继承HttpServlet 实现Servlet 程序**

一般在实际项目开发中，都是使用继承HttpServlet 类的方式去实现Servlet 程序。

1、编写一个类去继承HttpServlet 类

2、根据业务需要重写doGet 或doPost 方法

3、到web.xml 中的配置Servlet 程序的访问地址

Servlet 类的代码：

```java
public class HelloServlet extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        System.out.println("HelloServlet 调用doGet()被执行了！！！");
    }

    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        System.out.println("HelloServlet 调用doPost()被执行了！！！");
    }
}
```

web.xml 中的配置：

```xml
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd"
         version="4.0">
    <!-- servlet 标签给Tomcat 配置Servlet 程序-->
    <servlet>
        <!--servlet-name 标签Servlet 程序起一个别名（一般是类名） -->
        <servlet-name>HelloServlet</servlet-name>
        <!--servlet-class 是Servlet 程序的全类名-->
        <servlet-class>com.xiaobear.HelloServlet</servlet-class>
    </servlet>
    <!--servlet-mapping 标签给servlet 程序配置访问地址-->
    <servlet-mapping>
        <!--servlet-name 标签的作用是告诉服务器，我当前配置的地址给哪个Servlet 程序使用-->
        <servlet-name>HelloServlet</servlet-name>
        <!--url-pattern 标签配置访问地址<br/>
        / 斜杠在服务器解析的时候，表示地址为：http://ip:port/工程路径<br/>
        /hello 表示地址为：http://ip:port/工程路径/hello <br/>
        -->
        <url-pattern>/hello</url-pattern>
    </servlet-mapping>
</web-app>
```

## 3、Servlet 的生命周期

- 执行Servlet 构造器方法
- 执行init 初始化方法  （第一、二步，是在第一次访问的时候创建Servlet 程序会调用）
- 执行service 方法（第三步，每次访问都会调用）
- 执行destroy 销毁方法（第四步，在web 工程停止的时候调用）

## 4、Servlet继承体系

![](https://raw.githubusercontent.com/yhx1001/PicGo/img/18141219607756.png)

### 1、ServletConfig 类

```java
public interface ServletConfig {
    String getServletName();  //可以获取Servlet 程序的别名servlet-name 的值

    ServletContext getServletContext();//获取ServletContext 对象

    String getInitParameter(String var1);//获取初始化参数init-param

    Enumeration<String> getInitParameterNames();
}
```

ServletConfig 类从类名上来看，就知道是Servlet 程序的配置信息类。

Servlet 程序和ServletConfig 对象都是由Tomcat 负责创建，我们负责使用。

Servlet 程序默认是第一次访问的时候创建，ServletConfig 是每个Servlet 程序创建时，就创建一个对应的

ServletConfig 对象。

### 2、ServletConfig 类的三大作用

- 可以获取Servlet 程序的别名servlet-name 的值
- 获取ServletContext 对象
- 获取初始化参数init-param

web.xml

```xml
<!-- servlet 标签给Tomcat 配置Servlet 程序-->
<servlet>
    <!--servlet-name 标签Servlet 程序起一个别名（一般是类名） -->
    <servlet-name>HelloServlet</servlet-name>
    <!--servlet-class 是Servlet 程序的全类名-->
    <servlet-class>com.atguigu.servlet.HelloServlet</servlet-class>
    <!--init-param 是初始化参数-->
    <init-param>
        <!--是参数名-->
        <param-name>username</param-name>
        <!--是参数值-->
        <param-value>root</param-value>
    </init-param>
    <!--init-param 是初始化参数-->
    <init-param>
        <!--是参数名-->
        <param-name>url</param-name>
        <!--是参数值-->
        <param-value>jdbc:mysql://localhost:3306/test</param-value>
    </init-param>
</servlet>
<!--servlet-mapping 标签给servlet 程序配置访问地址-->
<servlet-mapping>
    <!--servlet-name 标签的作用是告诉服务器，我当前配置的地址给哪个Servlet 程序使用-->
    <servlet-name>HelloServlet</servlet-name>
    <!--
url-pattern 标签配置访问地址<br/>
/ 斜杠在服务器解析的时候，表示地址为：http://ip:port/工程路径<br/>
/hello 表示地址为：http://ip:port/工程路径/hello <br/>
-->
    <url-pattern>/hello</url-pattern>
</servlet-mapping>
```

Servlet 中的代码

```java
@Override
    public void init(ServletConfig config) throws ServletException {
        System.out.println("init 初始化方法！！！");
        //可以获取Servlet 程序的别名servlet-name 的值
        System.out.println("HelloServlet 程序的别名是:" + config.getServletName());
        //获取初始化参数init-param
        System.out.println("初始化参数username 的值是;" + config.getInitParameter("username"));
        System.out.println("初始化参数url 的值是;" + config.getInitParameter("url"));
        //获取ServletContext 对象
        System.out.println(config.getServletContext());
        super.init(config);    }
```

### 3、ServletContext 类

1、ServletContext 是一个接口，它表示Servlet 上下文对象

2、一个web 工程，只有一个ServletContext 对象实例。

3、ServletContext 对象是一个域对象。

4、ServletContext 是在web 工程部署启动的时候创建。在web 工程停止的时候销毁。

什么是域对象?域对象，是可以像Map 一样存取数据的对象，叫域对象。这里的域指的是存取数据的操作范围，整个web 工程。

|        | 存数据         | 取数据         | 删除数据          |
| ------ | -------------- | -------------- | ----------------- |
| Map    | put()          | get()          | remove()          |
| 域对象 | setAttribute() | getAttribute() | removeAttribute() |

### 4、ServletContext 类的四个作用

1、获取web.xml 中配置的上下文参数context-param

2、获取当前的工程路径，格式: /工程路径

3、获取工程部署后在服务器硬盘上的绝对路径

4、像Map 一样存取数据

ServletContext的代码

```java
 @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        System.out.println("HelloServlet 调用doGet()被执行了！！！");
        // 1、获取web.xml 中配置的上下文参数context-param
        ServletContext context = getServletConfig().getServletContext();
        String username = context.getInitParameter("username");
        System.out.println("context-param 参数username 的值是:" + username);
        System.out.println("context-param 参数password 的值是:" +
                context.getInitParameter("password"));
		// 2、获取当前的工程路径，格式: /工程路径
        System.out.println( "当前工程路径:" + context.getContextPath() );
		// 3、获取工程部署后在服务器硬盘上的绝对路径
/**
 * / 斜杠被服务器解析地址为:http://ip:port/工程名/ 映射到IDEA 代码的web 目录<br/>
 */
        System.out.println("工程部署的路径是:" + context.getRealPath("/"));
        System.out.println("工程下css 目录的绝对路径是:" + context.getRealPath("/css"));
        System.out.println("工程下imgs 目录1.jpg 的绝对路径是:" + context.getRealPath("/imgs/1.jpg"));
    }
```

```xml
<!--context-param 是上下文参数(它属于整个web 工程)-->
<context-param>
    <param-name>username</param-name>
    <param-value>context</param-value>
</context-param>
<!--context-param 是上下文参数(它属于整个web 工程)-->
<context-param>
    <param-name>password</param-name>
    <param-value>root</param-value>
</context-param>
```

ContextServlet1代码

```java
public class ContextServlet1 extends HttpServlet {
    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws
        ServletException, IOException {
        // 获取ServletContext 对象
        ServletContext context = getServletContext();
        System.out.println(context);
        System.out.println("保存之前: Context1 获取key1 的值是:"+ context.getAttribute("key1"));
        context.setAttribute("key1", "value1");
        System.out.println("Context1 中获取域数据key1 的值是:"+ context.getAttribute("key1"));
    }
}
		//ContextServlet2
protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException,
IOException {
    ServletContext context = getServletContext();
    System.out.println(context);
    System.out.println("Context2 中获取域数据key1 的值是:"+ context.getAttribute("key1"));
}
```

# 3、HTTP协议

什么是协议 ? 协议是指双方，或多方，相互约定好，大家都需要遵守的规则，叫协议。

> 客户端和服务器之间通信时，发送的数据，需要遵守的规则，叫HTTP 协议。HTTP 协议中的数据又叫报文

## 1、请求的HTTP 协议格式

请求分为**GET** 请求，和**POST** 请求

哪些是GET 请求，哪些是POST 请求

GET 请求有哪些：

> 1、form 标签method=get
> 2、a 标签
> 3、link 标签引入css
> 4、Script 标签引入js 文件
> 5、img 标签引入图片
> 6、iframe 引入html 页面
> 7、在浏览器地址栏中输入地址后敲回车

POST 请求有哪些：

> form 标签method=post

## 2、常用的响应码说明

- 200 表示请求成功
- 302 表示请求重定向（明天讲）
- 404 表示请求服务器已经收到了，但是你要的数据不存在（请求地址错误）
- 500 表示服务器已经收到请求，但是服务器内部错误（代码错误）

# 4、HttpServlet

## 1、HttpServletRequest

### 1、HttpServletRequest 类有什么作用。

> 每次只要有请求进入Tomcat 服务器，Tomcat 服务器就会把请求过来的HTTP 协议信息解析好封装到Request 对象中。然后传递到service 方法（doGet 和doPost）中给我们使用。我们可以通过HttpServletRequest 对象，获取到所有请求的信息。

### 2、HttpServletRequest 类的常用方法

- getRequestURI() 获取请求的资源路径
- getRequestURL() 获取请求的统一资源定位符（绝对路径）
- getRemoteHost() 获取客户端的ip 地址
- getHeader() 获取请求头
- getParameter() 获取请求的参数
- getParameterValues() 获取请求的参数（多个值的时候使用）
- getMethod() 获取请求的方式GET 或POST
- setAttribute(key, value); 设置域数据
- getAttribute(key); 获取域数据
- getRequestDispatcher() 获取请求转发对象

```java
public class RequestApiServlet extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        System.out.println("获取请求的路径=>"+ req.getRequestURI());
        System.out.println("获取统一资源定位符=>"+ req.getRequestURI());
        System.out.println("获取ip=>"+req.getRemoteHost());
        System.out.println("获取请求头=>"+req.getHeader("User-Agent"));
        System.out.println("请求方式=>"+req.getMethod());
    }
}

```



```html
<!--index.html-->
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
<form action="http://localhost:8080/JavaWeb_02_war_exploded/parameterServlet" method="get">
    用户名：<input type="text" name="username"><br/>
    密码：<input type="password" name="password"><br/>
    兴趣爱好：<input type="checkbox" name="hobby" value="cpp">C++
    <input type="checkbox" name="hobby" value="java">Java
    <input type="checkbox" name="hobby" value="js">JavaScript<br/>
    <input type="submit">
</form>
</body>
</html>
```

```java
public class ParameterServlet extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        String username = req.getParameter("username");
        String password = req.getParameter("password");
        String[]  hobby = req.getParameterValues("hobby");
        System.out.println("用户名："+username);
        System.out.println("密码："+password);
        System.out.println("兴趣爱好："+ Arrays.asList(hobby));
    }
}
```



### 3、doGet 请求的中文乱码解决：

```java
// 获取请求参数
String username = req.getParameter("username");
//1 先以iso8859-1 进行编码
//2 再以utf-8 进行解码
username = new String(username.getBytes("iso-8859-1"), "UTF-8");
```

### 4、POST 请求的中文乱码

```java
@Override
protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException,
IOException {
    // 设置请求体的字符集为UTF-8，从而解决post 请求的中文乱码问题
    req.setCharacterEncoding("UTF-8");
    System.out.println("-------------doPost------------");
    // 获取请求参数
    String username = req.getParameter("username");
    String password = req.getParameter("password");
    String[] hobby = req.getParameterValues("hobby");
    System.out.println("用户名：" + username);
    System.out.println("密码：" + password);
    System.out.println("兴趣爱好：" + Arrays.asList(hobby));
}
```

### 5、Web路径

- 相对路径
	- .  表示当前目录
	- ..  表示上一级目录
	- 资源名 表示当前目录/资源名
- 绝对路径
	- http://ip:port/工程路径/资源路径

### 6、/ 斜杠的不同意义

在web 中**/ 斜杠是一种绝对路径**。

- / 斜杠如果被浏览器解析，得到的地址是：http://ip:port/

	```html
	<a href="/">斜杠</a>
	```

- / 斜杠如果被服务器解析，得到的地址是：http://ip:port/工程路径

	```xml
	<url-pattern>/servlet1</url-pattern>
	```

	```java
	servletContext.getRealPath(“/”);
	request.getRequestDispatcher(“/”);
	```

- 特殊情况：` response.sendRediect(“/”); `把斜杠发送给浏览器解析。得到http://ip:port/

## 2、HttpServletResponse

### 1、HttpServletResponse 类的作用

> HttpServletResponse 类和HttpServletRequest 类一样。每次请求进来，Tomcat 服务器都会创建一个Response 对象传递给Servlet 程序去使用。HttpServletRequest 表示请求过来的信息，HttpServletResponse 表示所有响应的信息，我们如果需要设置返回给客户端的信息，都可以通过HttpServletResponse 对象来进行设置

### 2、相关的输出流

字节流              getOutputStream();                              常用于下载（传递二进制数据）
字符流              getWriter();                                            常用于回传字符串（常用）

**两个流同时只能使用一个。**

### 3、往客户端回传数据

```java
public class ReponseIOServlet extends HttpServlet {
    @Override
    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
    }
    @Override
    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        PrintWriter writer = response.getWriter();
        writer.write("xiaobear,you are nice!!!");
    }
}
```

### 4、响应的乱码解决

```java
/*-----------------------------------方式一（不推荐）--------------------------------------*/
// 设置服务器字符集为UTF-8
resp.setCharacterEncoding("UTF-8");
// 通过响应头，设置浏览器也使用UTF-8 字符集
resp.setHeader("Content-Type", "text/html; charset=UTF-8");
/*-----------------------------------方式二（推荐）--------------------------------------*/
// 它会同时设置服务器和客户端都使用UTF-8 字符集，还设置了响应头
// 此方法一定要在获取流对象之前调用才有效
resp.setContentType("text/html; charset=UTF-8");
PrintWriter writer = response.getWriter();
writer.write("鄢汉雄最棒！！！");
```

### 5、请求重定向

> 请求重定向，是指客户端给服务器发请求，然后服务器告诉客户端说。我给你一些地址。你去新地址访问。叫请求重定向（因为之前的地址可能已经被废弃）。

![](https://raw.githubusercontent.com/yhx1001/PicGo/img/1710080511514599.png)

- 请求重定向方式一：

	```java
	// 设置响应状态码302 ，表示重定向，（已搬迁）
	resp.setStatus(302);
	// 设置响应头，说明新的地址在哪里
	resp.setHeader("Location", "http://localhost:8080");
	```

- 请求重定向方式二（推荐）：

	```java
	resp.sendRedirect("http://localhost:8080");
	```

# 5、jsp

> jsp 的全换是java server pages。Java 的服务器页面。
> jsp 的主要作用是代替Servlet 程序回传html 页面的数据。
> 因为Servlet 程序回传html 页面数据是一件非常繁锁的事情。开发成本和维护成本都极高。

Servlet 回传html 页面数据的代码：

```java
public class JspDemo extends HttpServlet {
    @Override
    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {

    }
    @Override
    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        response.setContentType("text/html;charset=UTF-8");
        PrintWriter writer = response.getWriter();
        writer.write("<!DOCTYPE html>\r\n");
        writer.write(" <html lang=\"en\">\r\n");
        writer.write(" <head>\r\n");
        writer.write(" <meta charset=\"UTF-8\">\r\n");
        writer.write(" <title>Title</title>\r\n");
        writer.write(" </head>\r\n");
        writer.write(" <body>\r\n");
        writer.write(" 这是html 页面数据\r\n");
        writer.write(" </body>\r\n");
        writer.write("</html>\r\n");
        writer.write("\r\n");
    }
}
```

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
这是HTML页面数据
</body>
</html>
```

## 1、jsp的本质

> jsp 页面本质上是一个Servlet 程序。

## 2、jsp 语法

### 1、jsp 头部的page 指令

>  jsp 的page 指令可以修改jsp 页面中一些重要的属性，或者行为。

```jsp
<%@ page contentType="text/html;charset=UTF-8"
         import="java.applet.Applet"
         autoFlush="true"
         buffer="8kb"
         errorPage="index.jsp"
         isErrorPage="true"
         session="true"
         extends="java.io"
         language="java" %>
<%--
    language     属性表示jsp 翻译后是什么语言文件。暂时只支持java。
    contentType  属性表示jsp 返回的数据类型是什么。也是源码中response.setContentType()参数值
    pageEncoding 属性表示当前jsp 页面文件本身的字符集。
    import       属性跟java 源代码中一样。用于导包，导类。
    ========================两个属性是给out 输出流使用=============================
    autoFlush    属性设置当out 输出流缓冲区满了之后，是否自动刷新缓冲区。默认值是true。
    buffer       属性设置out 缓冲区的大小。默认是8kb
    ========================两个属性是给out 输出流使用=============================
    errorPage    属性设置当jsp 页面运行时出错，自动跳转去的错误页面路径。
    isErrorPage  属性设置当前jsp 页面是否是错误信息页面。默认是false。如果是true 可以获取异常信息。
    session      属性设置访问当前jsp 页面，是否会创建HttpSession 对象。默认是true。
    extends      属性设置jsp 翻译出来的java 类默认继承谁。
    --%>
```

### 2、jsp常用脚本

- **声明脚本**

	```jsp
	<%--声明脚本的格式是：--%> <%! 声明java 代码%>
	```

	==作用：可以给jsp 翻译出来的java 类定义属性和方法甚至是静态代码块。内部类等。==

	```jsp
	<%@ page contentType="text/html;charset=UTF-8" language="java" %>
	<html>
	<head>
	    <title>Title</title>
	</head>
	<body>
	<%--1、声明类属性--%>
	<%!
	    private Integer id;
	    private String name;
	    private static Map<String,Object> map;
	%>
	<%--2、声明static 静态代码块--%>
	<%!
	    static {
	        map = new HashMap<String,Object>();
	        map.put("key1","value1");
	        map.put("key2","value2");
	        map.put("key3","value3");
	    }
	%>
	<%--3、声明类方法--%>
	<%!
	    public int sb(){
	        return 12;
	    }
	%>
	<%--4、声明内部类--%>
	<%!
	    public static class Yhx{
	        private Integer id = 10;
	        private String abc = "abc";
	    }
	%>
	</body>
	</html>
	```

- **表达式脚本（常用）**

>表达式脚本的格式是：<%=表达式%>
>表达式脚本的作用是：jsp 页面上输出数据。

- ```jsp
	<%--1. 输出整型--%>
	<%=12%>
	<%--2. 输出浮点型--%>
	<%=12.12%>
	<%--3. 输出字符串--%>
	<%="你是最棒的！"%>
	<%--4. 输出对象--%>
	<%=request.getParameter("username")%>
	```

	- 表达式脚本的特点：
		- 所有的表达式脚本都会被翻译到`jspService() `方法中
		- 表达式脚本都会被翻译成为`out.print()`输出到页面上
		- 由于表达式脚本翻译的内容都在`jspService() `方法中,所以`_jspService()`方法中的对象都可以直接使用
		- 表达式脚本中的表达式不能以分号结束*。

- **代码脚本**

	```jsp
	<% java 语句 %>
	```

	==代码脚本的作用是：可以在jsp 页面中，编写我们自己需要的功能（写的是java 语句）。==

- ```jsp
	<%--代码脚本----if 语句--%>
	<%
	    int i = 18;
	    if (i == 10){
	        System.out.println("你很帅！！！");
	    }else{
	        System.out.println("你是真的帅！！！");
	    }
	%>
	<%--2. 代码脚本----for 循环语句--%>
	<%
	    for (int j = 0; j < 100; j++) {
	        System.out.println(j);
	    }
	%>
	<%--3. 翻译后java 文件中_jspService 方法内的代码都可以写--%>
	<%
	    String username = request.getParameter("username");
	    System.out.println(username);
	%>
	```

	- 代码脚本的特点是：
		- 代码脚本翻译之后都在`_jspService `方法中
		- 代码脚本由于翻译到`_jspService()`方法中，所以在`_jspService()`方法中的现有对象都可以直接使用。
		- 还可以由多个代码脚本块组合完成一个完整的java 语句。
		- 代码脚本还可以和表达式脚本一起组合使用，在jsp 页面上输出数据

### 3、jsp 中的三种注释

- html 注释

	> <!-- 这是html 注释-->
	> html 注释会被翻译到java 源代码中。在_jspService 方法里，以out.writer 输出到客户端。

- java 注释

	> <%
	> // 单行java 注释
	> /* 多行java 注释*/
	> %>
	> java 注释会被翻译到java 源代码中。

- jsp 注释

	> <%-- 这是jsp 注释--%>
	> jsp 注释可以注掉，jsp 页面中所有代码。

### 4、jsp 九大内置对象

==jsp 中的内置对象，是指Tomcat 在翻译jsp 页面成为Servlet 源代码后，内部提供的九大对象，叫内置对象。==

![image-20200423145129906](https://raw.githubusercontent.com/yhx1001/PicGo/img/image-20200423145129906.png)

### 5、jsp的四大域对象

> 域对象是可以像Map 一样存取数据的对象

- **pageContext (PageContextImpl 类)**   当前jsp 页面范围内有效
- **request (HttpServletRequest 类)**       一次请求内有效
- **session (HttpSession 类)**                   一个会话范围内有效（打开浏览器访问服务器，直到关闭浏览器）
- **application (ServletContext 类)**         整个web 工程范围内都有效（只要web 工程不停止，数据都在）

优先顺序：==**pageContext ----> request ----> session ----> application**==

```jsp
<%----------------------------------------socre.jsp--------------------------------------%>
<body>
<h2>score.jsp页面</h2>
<%
    pageContext.setAttribute("key","pageContext");
    request.setAttribute("key","request");
    session.setAttribute("key","session");
    application.setAttribute("key","application");
%>
pageContext域是否有值：<%=pageContext.getAttribute("key") %><br>
request域是否有值：<%=request.getAttribute("key") %><br>
session域是否有值：<%=session.getAttribute("key") %><br>
application域是否有值：<%=application.getAttribute("key") %><br>
<%
    request.getRequestDispatcher("/score2.jsp").forward(request,response);
%>
</body>
</html>
<%--------------------------------------socre2.jsp--------------------------------------%>
<body>
<h2>score2.jsp页面</h2>
pageContext域是否有值：<%=pageContext.getAttribute("key") %><br>
request域是否有值：<%=request.getAttribute("key") %><br>
session域是否有值：<%=session.getAttribute("key") %><br>
application域是否有值：<%=application.getAttribute("key") %><br>
<%
    request.getRequestDispatcher("/score2.jsp").forward(request,response);
%>
</body>
</html>
```

### 6、out 输出和response.getWriter 输出

response 中表示响应，我们经常用于设置返回给客户端的内容（输出）。out 也是给用户做输出使用的。

out.write() 输出字符串没有问题

out.print() 输出任意数据都没有问题（都转换成为字符串后调用的write 输出）

==**在jsp 页面中，可以统一使用out.print()来进行输出**==

## 3、jsp 的常用标签

### 1、jsp 静态包含

> ```jsp
> <%--
>  <%@ include file=""%> 就是静态包含
> file 属性指定你要包含的jsp 页面的路径
> 地址中第一个斜杠/ 表示为http://ip:port/工程路径/ 映射到代码的web 目录
> 静态包含的特点：
> 1、静态包含不会翻译被包含的jsp 页面。
> 2、静态包含其实是把被包含的jsp 页面的代码拷贝到包含的位置执行输出。
> --%>
> <%@ include file="/include/footer.jsp"%>
> ```

### 2、jsp动态包含

```jsp
<%--
    <jsp:include page=""></jsp:include> 这是动态包含
        page 属性是指定你要包含的jsp 页面的路径
        动态包含也可以像静态包含一样。把被包含的内容执行输出到包含位置
        动态包含的特点：
        1、动态包含会把包含的jsp 页面也翻译成为java 代码
        2、动态包含底层代码使用如下代码去调用被包含的jsp 页面执行输出。
        JspRuntimeLibrary.include(request, response, "/include/footer.jsp", out, false);
		3、动态包含，还可以传递参数
    --%>
<jsp:include page="/include/footer.jsp">
<jsp:param name="username" value="bbj"/>
<jsp:param name="password" value="root"/>
</jsp:include>
```

### 3、jsp 标签-转发

```jsp
<%--
    <jsp:forward page=""></jsp:forward> 是请求转发标签，它的功能就是请求转发
        page 属性设置请求转发的路径
        --%>
<jsp:forward page="/scope2.jsp"></jsp:forward>
```

### 4、jsp练习

- 9*9乘法表

	```jsp
	<%@ page contentType="text/html;charset=UTF-8" language="java" %>
	<html>
	    <head>
	        <title>输出99乘法表</title>
	    </head>
	    <body>
	        <h1 align="center">乘法表</h1>
	        <table align="center">
	            <%
	            for (int i = 0; i <= 9; i++) {  %>
	            <tr>
	                <%  for (int j = 1; j <= i; j++) {
	    %>
	                <td><%=j + "x" + i + "=" +(i*j) %></td>
	                <%
	                }
	                %>
	            </tr>
	            <% }
	            %>
	        </table>
	    </body>
	</html>
	```

- jsp 输出一个表格，里面有10 个学生信息。

	```jsp
	<%@ page contentType="text/html;charset=UTF-8" language="java" %>
	<html>
	<head>
	    <title>Title</title>
	    <style>
	        table{
	            border: 1px blue solid;
	            width: 600px;
	            border-collapse: collapse;
	        }
	        td,th{
	            border: 1px blue solid;
	        }
	    </style>
	</head>
	<body>
	<%--jsp 输出一个表格，里面有10 个学生信息。--%>
	<%
	    List<Student> studentList = new ArrayList<Student>();
	    for (int i = 0; i < 10; i++) {
	        int t = i + 1;
	        studentList.add(new Student("name"+t,t,18+t,"phone"+t));
	    }
	%>
	<% for (Student student : studentList) { %>
	<table>
	    <tr>
	        <td>编号</td>
	        <td>姓名</td>
	        <td>年龄</td>
	        <td>电话</td>
	        <td>操作</td>
	    </tr>
	<tr>
	    <td><%=student.getId()%></td>
	    <td><%=student.getName()%></td>
	    <td><%=student.getAge()%></td>
	    <td><%=student.getPhone()%></td>
	    <td>删除、修改</td>
	</tr>
	   <% } %>
	</table>
	</body>
	</html>
	```

# 6、Listener 监听器

> 1、Listener 监听器它是JavaWeb 的三大组件之一。JavaWeb 的三大组件分别是：Servlet 程序、Filter 过滤器、Listener 监听器。
> 2、Listener 它是JavaEE 的规范，就是接口
> 3、监听器的作用是，监听某种事物的变化。然后通过回调函数，反馈给客户（程序）去做一些相应的处理。

### ServletContextListener 监听器

==ServletContextListener 它可以监听ServletContext 对象的创建和销毁。==
**ServletContext 对象在web 工程启动的时候创建，在web 工程停止的时候销毁。**监听到创建和销毁之后都会分别调用ServletContextListener 监听器的方法反馈。

```java
public interface ServletContextListener extends EventListener {
    
    default void contextInitialized(ServletContextEvent sce) {
        //在ServletContext 对象创建之后马上调用，做初始化
    }

    default void contextDestroyed(ServletContextEvent sce) {
        //在ServletContext 对象销毁之后调用
    }
}
```

如何使用ServletContextListener 监听器监听ServletContext 对象

1. 编写一个类去实现ServletContextListener
2. 实现其两个回调方法
3. 到web.xml 中去配置监听器

```java
public class MyServletContextListener implements ServletContextListener {
    public void contextInitialized(ServletContextEvent sce) {
        System.out.println("ServletContext被创建了！！！");
    }
    public void contextDestroyed(ServletContextEvent sce) {
        System.out.println("ServletContext被销毁了！！！");
    }
}
```

```xml
<!--配置listener-->
<listener>
    <listener-class>com.xiaobear.listener.MyServletContextListener</listener-class>
</listener>
```

# 7、EL表达式 & JSTL 标签库

## 1、EL 表达式

### 1、什么是EL 表达式，EL 表达式的作用?

> EL 表达式的全称是：Expression Language。是表达式语言。
>
> EL 表达式的什么作用：EL 表达式主要是代替jsp 页面中的表达式脚本在jsp 页面中进行数据的输出。
> 因为EL 表达式在输出数据的时候，要比jsp 的表达式脚本要简洁很多。

```jsp
<%
request.setAttribute("key","鄢汉雄");
%>
表达式脚本输出的值是：<%=request.getAttribute("key")%></br>
EL表达式输出的值是：${key}
```

==EL 表达式的格式是：${表达式}==

EL 表达式在输出null 值的时候，输出的是空串。

jsp 表达式脚本输出null 值的时候，输出的是null 字符串

### 2、EL 表达式搜索域数据的顺序

EL 表达式主要是在jsp 页面中输出数据。主要是**输出域对象中的数据**。

当四个域中都有相同的key 的数据的时候，EL 表达式会按照四个域的从小到大的顺序去进行搜索，找到就输出。

==**pageContext ----> request ----> session ----> application**==

```jsp
<%
    request.setAttribute("key","request");
    session.setAttribute("key","session");
    application.setAttribute("key","application");
    pageContext.setAttribute("key","pageContext");
%>
<%----%>
表达式脚本输出的值是：<%=request.getAttribute("key")%></br>
EL表达式输出的值是：${key}
```

### 3、EL 表达式输出Bean 的普通属性，数组属性。List 集合属性，map 集合属性

```java
@Data
@AllArgsConstructor
@NoArgsConstructor
public class Person {
    private Integer id;
    private String[] phone;
    private List<String> city;
    private Map<String,Object> map;
}
```

```jsp
<%
    Person person = new Person();
    person.setId(12);
    person.setPhone(new String[]{"18391242239","1234523121"});
    ArrayList<String> objects = new ArrayList<>();
    objects.add("湖南");
    objects.add("长沙");
    objects.add("上海");
    person.setCity(objects);
    Map<String, Object> map = new HashMap<>();
    map.put("Key1","value1");
    map.put("Key2","value2");
    map.put("Key3","value3");
    person.setMap(map);
    pageContext.setAttribute("p", person);
%>

输出person:${p}</br>
输出person id的属性：${p.id}
输出Person 的pnones 数组属性值：${p.phones[2]} <br>
输出Person 的cities 集合中的元素值：${p.city} <br>
输出Person 的List 集合中个别元素值：${p.city[2]} <br>
输出Person 的Map 集合: ${p.map} <br>
输出Person 的Map 集合中某个key 的值: ${p.map.key3} <br>
```

### 4、EL 表达式——运算

> 语法：${ 运算表达式}

#### 1、关系运算

| 关系运算符 | 说明     | 范例                               | 结果           |
| ---------- | -------- | ---------------------------------- | -------------- |
| == 或eq    | 等于     | ${1001 == 1001} or ${1001 eq 1001} | true or true   |
| != 或ne    | 不等于   | ${1001 != 1001} or ${1001 ne 1001} | false or false |
| <或lt      | 小于     | ${1001 < 1004} or ${1001 lt 1001}  | true or false  |
| > 或者gt   | 大于     | ${1001 > 1004} or ${1001 gt 1001}  | false or false |
| <= 或le    | 小于等于 | ${1001 <= 1004} or ${1001 le 1001} | true or true   |
| >= 或ge    | 大于等于 | ${1001 >= 1004} or ${1001 ge 1001} | false or true  |

#### 2、逻辑运算

| 逻辑运算符 | 说明     | 范例                                               | 结果           |
| ---------- | -------- | -------------------------------------------------- | -------------- |
| && 或and   | 与运算   | ${14 == 10 && 10 < 14} or ${14 == 10 and 10 < 14}  | false or false |
| \|\|或or   | 或运算   | ${14 == 10 \|\| 10 < 14} or ${14 == 10 or 10 < 14} | true or true   |
| ! 或not    | 取反运算 | ${!false} or ${not true}                           | true or false  |

#### 3、算数运算

| 逻辑运算符 | 说明 | 范例                       | 结果         |
| ---------- | ---- | -------------------------- | ------------ |
| +          | 加   | ${10 + 14}                 | 24           |
| -          | 减   | ${520 - 250}               | 270          |
| *          | 乘   | ${25 * 25}                 | 625          |
| /或div     | 除   | ${99 / 3} or ${99 div 3}   | 33.0 or 33.0 |
| %或mod     | 取余 | ${100 % 3} or ${100 mod 3} | 1 or 1       |

#### 4、empty运算

> empty 运算可以判断一个数据是否为空，如果为空，则输出true,不为空输出false

**为空的情况：**

- 值为null 值的时候，为空
- 值为空串的时候，为空
- 值是Object 类型数组，长度为零的时候
- list 集合，元素个数为零
- map 集合，元素个数为零

```jsp
<%
//值为null 值的时候，为空
    request.setAttribute("emptyNull",null);
//值为空串的时候，为空
request.setAttribute("emptyStr","");
// 值是Object 类型数组，长度为零的时候
request.setAttribute("emptyArr",new Object[]{});
//list 集合，元素个数为零
request.setAttribute("emptyList",new ArrayList<>());
//map 集合，元素个数为零
    request.setAttribute("emptyMap",new HashMap<>());
%>
${ empty emptyNull } <br/>
${ empty emptyStr } <br/>
${ empty emptyArr } <br/>
${ empty emptyList } <br/>
${ empty emptyMap } <br/>
```

#### 5、三元运算

> 表达式1？表达式2：表达式3

```jsp
${100>120?"Xiaobear很帅！！":"非常帅！！！"}
```

#### 6、.点运算和[] 中括号运算符

> .点运算，可以输出Bean 对象中某个属性的值。
> []中括号运算，可以输出有序集合中某个元素的值。
> 并且[]中括号运算，还可以输出map 集合中key 里含有特殊字符的key 的值。

```jsp
<%
    HashMap<String, Object> map = new HashMap<String,Object>();
    map.put("a.b.c","abc");
    map.put("a-b-c","a-b-c");
    request.setAttribute("map",map);
%>

${map['a.b.c']}
${map['a-b-c']}
```

### 5、EL 表达式的11 个隐含对象

| 变量             | 类型                 | 作用                                                  |
| ---------------- | -------------------- | ----------------------------------------------------- |
| pageContext      | pageContextImpl      | 它可以获取jsp 中的九大内置对象                        |
| pageScope        | Map<String,Object>   | 它可以获取pageContext 域中的数据                      |
| requestScope     | Map<String,Object>   | 它可以获取Request 域中的数据                          |
| sessionScope     | Map<String,Object>   | 它可以获取Session 域中的数据                          |
| applicationScope | Map<String,Object>   | 它可以获取ServletContext 域中的数据                   |
| param            | Map<String,String>   | 它可以获取请求参数的值                                |
| paramValues      | Map<String,String[]> | 它也可以获取请求参数的值，获取多个值的时候使用。      |
| header           | Map<String,String>   | 它可以获取请求头的信息                                |
| headerValues     | Map<String,String[]> | 它可以获取请求头的信息，它可以获取多个值的情况        |
| cookie           | Map<String,Cookie>   | 它可以获取当前请求的Cookie 信息                       |
| initParam        | Map<String,String>   | 它可以获取在web.xml 中配置的<context-param>上下文参数 |

#### 1、EL 获取四个特定域中的属性

> pageScope ====== pageContext 域
> requestScope ====== Request 域
> sessionScope ====== Session 域
> applicationScope ====== ServletContext 域

```jsp
<%
    pageContext.setAttribute("key1","value1");
    pageContext.setAttribute("key2","value2");
    request.setAttribute("key2","value2");
    session.setAttribute("key2","value2");
    application.setAttribute("key2","value2");
%>
${applicationScope.key2}
```

#### 2、pageContext 对象的使用

1. 协议：

2. 服务器ip：
3. 服务器端口：
4. 获取工程路径：
5. 获取请求方法：
6. 获取客户端ip 地址：
7. 获取会话的id 编号：

```jsp
<%
    request.setAttribute("req",request);
%>
<%=request.getScheme() %> <br>
1.协议： ${ req.scheme }<br>
2.服务器ip：${ pageContext.request.serverName }<br>
3.服务器端口：${ pageContext.request.serverPort }<br>
4.获取工程路径：${ pageContext.request.contextPath }<br>
5.获取请求方法：${ pageContext.request.method }<br>
6.获取客户端ip 地址：${ pageContext.request.remoteHost }<br>
7.获取会话的id 编号：${ pageContext.session.id }<br>
```

## 2、JSTL 标签库

> JSTL 标签库全称是指JSP Standard Tag Library JSP 标准标签库。是一个不断完善的开放源代码的JSP 标签库。
> EL 表达式主要是为了替换jsp 中的表达式脚本，而标签库则是为了替换代码脚本。这样使得整个jsp 页面变得更佳简洁。

**JSTL 标签库的使用步骤：**

- 先导入jstl 标签库的jar 包。[http://archive.apache.org/dist/jakarta/taglibs/standard/binaries/](http://archive.apache.org/dist/jakarta/taglibs/standard/binaries/)

- 使用taglib 指令引入标签库

	```jsp
	<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>
	```


### 1、core 核心库使用

#### 1、<c:set />（使用很少）

> set 标签可以往域中保存数据

```jsp
保存之前：${requestScope.abc}
<c:set scope="session" var="abc" value="abcValue"></c:set>
保存之后：${sessionScope.abc}
```

#### 2、<c:if />

> if 标签用来做if 判断。不支持if-else

```jsp
<%--
ii.<c:if />
if 标签用来做if 判断。
test 属性表示判断的条件（使用EL 表达式输出）
--%>
<c:if test="${ 12 == 12 }">
    <h1>12 等于12</h1>
</c:if>
<c:if test="${ 12 != 12 }">
    <h1>12 不等于12</h1>
</c:if>
```

#### 3、<c:choose> <c:when> <c:otherwise>标签

> 多路判断。跟switch ... case .... default 非常接近

```jsp
<%--<c:choose> <c:when> <c:otherwise>标签
    1、标签里不能使用html 注释，要使用jsp 注释
	2、when 标签的父标签一定要是choose 标签
    --%>
<%
    request.setAttribute("height",175);
%>
<c:choose>
    <c:when test="${requestScope.height} > 180">
        <h2>是巨人</h2>
    </c:when>
    <c:when test="${requestScope.height} < 170">
        <h2>还可以</h2>
    </c:when>
    <c:when test="${requestScope.height} > 185">
        <h2>超巨人</h2>
    </c:when><c:when test="${requestScope.height} > 180">
    <h2>可以</h2>
</c:when>
<c:otherwise>
    qita
</c:otherwise>
</c:choose>
```

#### 4、<c:forEach />

> 遍历输出使用。

##### 1. 遍历1 到10

```jsp
<%--1.遍历1 到10，输出
begin 属性设置开始的索引
end 属性设置结束的索引
var 属性表示循环的变量(也是当前正在遍历到的数据)
for (int i = 1; i < 10; i++)
--%>
<c:forEach begin="1" end="10" var="i">
    ${i}
</c:forEach>
```

##### 2、遍历Object 数组

```jsp
<%-- 2.遍历Object 数组
for (Object item: arr)
items 表示遍历的数据源（遍历的集合）
var 表示当前遍历到的数据
--%>
<%
    request.setAttribute("hello",new String[]{"123","1234","124"});
%>
<c:forEach items="${requestScope.hello}" var="item">
    ${item}
</c:forEach>
```

##### 3、遍历Map 集合

```jsp
<%
Map<String,Object> map = new HashMap<String, Object>();
map.put("key1", "value1");
map.put("key2", "value2");
map.put("key3", "value3");
// for ( Map.Entry<String,Object> entry : map.entrySet()) {
// }
request.setAttribute("map", map);
%>
<c:forEach items="${ requestScope.map }" var="entry">
<h1>${entry.key} = ${entry.value}</h1>
</c:forEach>
```

##### 4、遍历List 集合

```java
@Data
@AllArgsConstructor
@NoArgsConstructor
public class Student {
    private Integer id;
    private String username;
    private String password;
    private Integer age;
    private String phone;
}
```

```jsp
<%
    ArrayList<Student> objects = new ArrayList<Student>();
    for (int i = 0; i <= 10; i++) {
        objects.add(new Student(1,"username"+i,"pass"+i,18+i,"phone"+i));
    }
    request.setAttribute("stu",objects);
%>
<table>
    <tr>
        <th>编号</th>
        <th>用户名</th>
        <th>密码</th>
        <th>年龄</th>
        <th>电话</th>
        <th>操作</th>
    </tr>
    <%--
    items 表示遍历的集合
    var 表示遍历到的数据
    begin 表示遍历的开始索引值
    end 表示结束的索引值
    step 属性表示遍历的步长值
    varStatus 属性表示当前遍历到的数据的状态
    for（int i = 1; i < 10; i+=2）
    --%>
    <c:forEach begin="2" end="7" varStatus="status" items="${requestScope.stu}" var="stu">
    <tr>
        <td>${stu.id}</td>
        <td>${stu.username}</td>
        <td>${stu.password}</td>
        <td>${stu.age}</td>
        <td>${stu.phone}</td>
    </tr>
    </c:forEach>
```

# 8、文件上传与下载

## 1、文件的上传

需要的jar包：commons-fileupload-1.2.1.jar、commons-io-1.4.jar

1. 要有一个form 标签，method=post 请求
2. form 标签的encType 属性值必须为multipart/form-data 值
3. 在form 标签中使用input type=file 添加上传的文件
4. 编写服务器代码（Servlet 程序）接收，处理上传的数据。

### 1、[commons-fileupload.jar](http://commons.apache.org/)

```
ServletFileUpload 类，用于解析上传的数据。
FileItem 类，表示每一个表单项。

boolean ServletFileUpload.isMultipartContent(HttpServletRequest request);
判断当前上传的数据格式是否是多段的格式。

public List<FileItem> parseRequest(HttpServletRequest request)
解析上传的数据
    
boolean FileItem.isFormField()
判断当前这个表单项，是否是普通的表单项。还是上传的文件类型。
true 表示普通类型的表单项
false 表示上传的文件类型
    
String FileItem.getFieldName()
获取表单项的name 属性值

String FileItem.getString()
获取当前表单项的值。

String FileItem.getName();
获取上传的文件名

void FileItem.write( file );
将上传的文件写到参数file 所指向抽硬盘位置。
```

form表单

```jsp
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>upload</title>
</head>
<body>
<form action="http://localhost:8080/JavaWeb_04_war_exploded/upload" method="post" enctype="multipart/form-data">
    用户名:<input type="text" name="username"><br>
    头像:<input type="file" name="photo"><br>
    <input type="submit" value="上传">
</form>
</body>
</html>
```

Servlet，web.xml配置省略

```java
public class UploadServlet extends HttpServlet {

    @SneakyThrows
    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        /* 
        System.out.println("文件已上传");
        ServletInputStream inputStream = req.getInputStream();
        byte[] buffer = new byte[10240000];
        int read = inputStream.read(buffer);
        System.out.println(new String(buffer,0,read));
        */
        // 先判断上传的数据是否多段数据（只有是多段的数据，才是文件上传的）
        if(ServletFileUpload.isMultipartContent(req)){
            //创建FileItemFactory 工厂实现类
FileItemFactory factory = new DiskFileItemFactory();
            // 创建用于解析上传数据的工具类ServletFileUpload 类
            ServletFileUpload upload = new ServletFileUpload(factory);
            try{
                // 解析上传的数据，得到每一个表单项FileItem
                List<FileItem> list = upload.parseRequest(req);
                // 循环判断，每一个表单项，是普通类型，还是上传的文件
                for (FileItem fileItem : list) {
                    if (fileItem.isFormField()) {
                            // 普通表单项
                        System.out.println("表单项的name 属性值：" + fileItem.getFieldName());
                                    // 参数UTF-8.解决乱码问题
                        System.out.println("表单项的value 属性值：" + fileItem.getString("UTF-8"));
                    } else {
                                                    // 上传的文件
                        System.out.println("表单项的name 属性值：" + fileItem.getFieldName());
                        System.out.println("上传的文件名：" + fileItem.getName());
                        fileItem.write(new File("e:\\" + fileItem.getName()));
                    }
                }
            }catch (Exception e){
                e.printStackTrace();
            }
        }
    }
}
```

## 2、文件的下载

> 下载的常用API 说明：
> response.getOutputStream();
> servletContext.getResourceAsStream();
> servletContext.getMimeType();
> response.setContentType();

```java
public class Download extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        //获取下载的文件名
        String downFile = "t011c6cdb031350fcb6.jpg";
        //读取下载的文件内容
        ServletContext context = getServletContext();
        //获取下载的文件类型
        String mimeType = context.getMimeType("/file/"+downFile);
        System.out.println("下载的文件类型："+mimeType);
        //在回传前，通过响应头告诉客户端返回的数据类型
        resp.setContentType(mimeType);
        resp.setHeader("Content-Disposition", "attachment; fileName=t011c6cdb031350fcb6.jpg");
        //告诉客户端收到的数据类型是用于下载
        InputStream stream = context.getResourceAsStream("/file/"+downFile);
        //获取输出流
        ServletOutputStream outputStream = resp.getOutputStream();
        //读取全部数据，复制给输出流，输出给客户端
        IOUtils.copy(stream,outputStream);
        //把下载的内容回传给客户端
    }
}
```

**中文名乱码问题:**

- URLEncoder 解决IE 和谷歌浏览器的附件中文名问题

	```java
	// 把中文名进行UTF-8 编码操作。
	String str = "attachment; fileName=" + URLEncoder.encode("中文.jpg", "UTF-8");
	// 然后把编码后的字符串设置到响应头中
	response.setHeader("Content-Disposition", str);
	```

- BASE64 编解码解决火狐浏览器的附件中文名问题

	```java
	//因为火狐使用的是BASE64 的编解码方式还原响应中的汉字。所以需要使用BASE64Encoder 类进行编码操作。
	// 使用下面的格式进行BASE64 编码后
	String str = "attachment; fileName=" + "=?utf-8?B?"
	+ new BASE64Encoder().encode("中文.jpg".getBytes("utf-8")) + "?=";
	// 设置到响应头中
	response.setHeader("Content-Disposition", str);
	----------------------------------------------------------------------------------
	//通过判断请求头中User-Agent 这个请求头携带过来的浏览器信息即可判断出是什么浏览器。如下：
	String ua = request.getHeader("User-Agent");
	// 判断是否是火狐浏览器
	if (ua.contains("Firefox")) {
	// 使用下面的格式进行BASE64 编码后
	String str = "attachment; fileName=" + "=?utf-8?B?"
	+ new BASE64Encoder().encode("中文.jpg".getBytes("utf-8")) + "?=";
	// 设置到响应头中
	response.setHeader("Content-Disposition", str);
	} else {
	// 把中文名进行UTF-8 编码操作。
	String str = "attachment; fileName=" + URLEncoder.encode("中文.jpg", "UTF-8");
	// 然后把编码后的字符串设置到响应头中
	response.setHeader("Content-Disposition", str);
	}
	```

	

# 9、Cookie & Session

## 1、cookie

> 1、Cookie 翻译过来是饼干的意思。
> 2、Cookie 是服务器通知客户端保存键值对的一种技术。
> 3、客户端有了Cookie 后，每次请求都发送给服务器。
> 4、每个Cookie 的大小不能超过4kb

准备页面cookie.html

```html
<!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
<html>
<head>
<meta http-equiv="pragma" content="no-cache" />
<meta http-equiv="cache-control" content="no-cache" />
<meta http-equiv="Expires" content="0" />
<meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
<title>Cookie</title>
	<base href="http://localhost:8080/JavaWeb_05_Cookie_Session/">
<style type="text/css">

	ul li {
		list-style: none;
	}
	
</style>
</head>
<body>
	<iframe name="target" width="500" height="500" style="float: left;"></iframe>
	<div style="float: left;">
		<ul>
			<li><a href="" target="target">Cookie的创建</a></li>
			<li><a href="" target="target">Cookie的获取</a></li>
			<li><a href="" target="target">Cookie值的修改</a></li>
			<li>Cookie的存活周期</li>
			<li>
				<ul>
					<li><a href="" target="target">Cookie的默认存活时间（会话）</a></li>
					<li><a href="" target="target">Cookie立即删除</a></li>
					<li><a href="" target="target">Cookie存活3600秒（1小时）</a></li>
				</ul>
			</li>
			<li><a href="" target="target">Cookie的路径设置</a></li>
			<li><a href="" target="target">Cookie的用户免登录练习</a></li>
		</ul>
	</div>
</body>
</html>
```

BaseServlet

```java
public abstract class BaseServlet extends HttpServlet {

    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        doPost(req, resp);
    }
    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        // 解决post请求中文乱码问题
        // 一定要在获取请求参数之前调用才有效
        req.setCharacterEncoding("UTF-8");
        // 解决响应中文乱码问题
        resp.setContentType("text/html; charset=UTF-8");

        String action = req.getParameter("action");
        try {
            // 获取action业务鉴别字符串，获取相应的业务 方法反射对象
            Method method = this.getClass().getDeclaredMethod(action, HttpServletRequest.class, HttpServletResponse.class);
            //  System.out.println(method);
            // 调用目标业务 方法
            method.invoke(this, req, resp);
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}
```

### 1、Cookie的创建

Servlet程序

```java
public class CookieServlet extends BaseServlet{

    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
         // 创建Cookie 对象
        Cookie cookie = new Cookie("key4", "value4");
        // 通知客户端保存Cookie
        resp.addCookie(cookie);
        resp.getWriter().write("Cookie 创建成功");
    }
}
```

### 2、服务器如何获取Cookie

```java
/*Cookie的获取*/
    protected void getCookie(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        Cookie[] cookies = req.getCookies();
        for (Cookie cookie : cookies) {
            //返回Cookie的名称
            resp.getWriter().write("Cookie["+cookie.getName()+"="+cookie.getValue()+"]");
        }
    }
```

### 3、Cookie 值的修改

> 方案一：
>
> - 先创建一个要修改的同名（指的就是key）的Cookie 对象
>
> - 在构造器，同时赋于新的Cookie 值。
>
> - 调用response.addCookie( Cookie );
>
> 	```java
> 	   /*
> 	    *Cookie值修改
> 	    */
> 	    protected void updateCookie(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
> 	//        - 先创建一个要修改的同名（指的就是key）的Cookie 对象
> 	        Cookie cookie = new Cookie("key4","newValue4");
> 	//        - 在构造器，同时赋于新的Cookie 值。
> 	        resp.addCookie(cookie);
> 	//        - 调用response.addCookie( Cookie );
> 	        resp.getWriter().write("Cookie修改成功");
> 	    }
> 	```

> 方案二：
>
> - 先查找到需要修改的Cookie 对象
>
> - 调用setValue()方法赋于新的Cookie 值。
>
> - 调用response.addCookie()通知客户端保存修改
>
> 	```java
> 	public class CookieUtils {
> 	    public static Cookie findCookie(String name , Cookie[] cookies){
> 	        if (name == null || cookies == null || cookies.length == 0) {
> 	            return null;
> 	        }
> 	        for (Cookie cookie : cookies) {
> 	            if (name.equals(cookie.getName())) {
> 	                return cookie;
> 	            }
> 	        }
> 	        return null;
> 	    }
> 	}
> 	```
>
> 	```java
> 	Cookie cookie1 = CookieUtils.findCookie("key5", req.getCookies());
> 	if (cookie1 != null) {
> 	    cookie1.setValue("newValue5");
> 	}
> 	resp.addCookie(cookie1);
> 	resp.getWriter().write("Cookie修改成功");
> 	```

### 4、Cookie 生命控制

> Cookie 的生命控制指的是如何管理Cookie 什么时候被销毁（删除）
>
> setMaxAge()
>
> - 正数，表示在指定的秒数后过期
> - 负数，表示浏览器一关，Cookie 就会被删除（默认值是-1）
> - 零，表示马上删除Cookie

```java

    protected void defaultCookie(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        Cookie cookie = new Cookie("default","default");
        cookie.setMaxAge(-1);
        //cookie.setMaxAge(60 * 60); // 设置Cookie 一小时之后被删除。无效
        resp.addCookie(cookie);
    }
    protected void deleteNow(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        // 先找到你要删除的Cookie 对象
        Cookie cookie = CookieUtils.findCookie("key4", req.getCookies());
        if (cookie != null) {
            // 调用setMaxAge(0);
            cookie.setMaxAge(0); // 表示马上删除，都不需要等待浏览器关闭
            // 调用response.addCookie(cookie);
            resp.addCookie(cookie);
            resp.getWriter().write("key4 的Cookie 已经被删除");
        }
    }
```

### 5、Cookie 有效路径Path 的设置

> Cookie 的path 属性可以有效的过滤哪些Cookie 可以发送给服务器。哪些不发。
> path 属性是通过请求的地址来进行有效的过滤。

```java
protected void pathCookie(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
    Cookie cookie = new Cookie("path","path");
    cookie.setPath(req.getContextPath()+"/abc");
    resp.addCookie(cookie);
    resp.getWriter().write("创建了一个带有path的Cookie");
}
```

### 6、练习--免密登录

```jsp
<%--login.jsp--%>
<form action="http://localhost:8080/JavaWeb_05_Cookie_Session/loginServlet" method="get">
    用户名：<input type="text" name="username" value="${cookie.username.value}"><br>
    密码：<input type="password" name="password" ><br>
    <input type="submit" value="登录">
</form>
```

```java
//LoginServlet
public class LoginServlet extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        String username = req.getParameter("username");
        String password = req.getParameter("password");
        if ("xiaobear".equals(username) && "123456".equals(password)) {
            //登录成功
            Cookie cookie = new Cookie("username", username);
            cookie.setMaxAge(60 * 60 * 24 * 7);
            //当前Cookie 一周内有效
            resp.addCookie(cookie);
            System.out.println("登录成功");
        } else {
            // 登录失败
            System.out.println("登录失败");
        }
    }
}
```

## 2、Session

> 1、Session 就一个接口（HttpSession）。
> 2、Session 就是会话。它是用来维护一个客户端和服务器之间关联的一种技术。
> 3、每个客户端都有自己的一个Session 会话。
> 4、Session 会话中，我们经常用来保存用户登录之后的信息。

### 1、创建Session 和获取(id 号,是否为新)

session.html页面

```html
<!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
<html>
<head>
<meta http-equiv="pragma" content="no-cache" />
<meta http-equiv="cache-control" content="no-cache" />
<meta http-equiv="Expires" content="0" />
<meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
<title>Session</title>
	<base href="http://localhost:8080/JavaWeb_05_Cookie_Session/">
<style type="text/css">
base
	ul li {
		list-style: none;
	}
	
</style>
</head>
<body>
	<iframe name="target" width="500" height="500" style="float: left;"></iframe>
	<div style="float: left;">
		<ul>
			<li><a href="sessionServlet?action=creatOrGetSession" target="target">Session的创建和获取（id号、是否为新创建）</a></li>
			<li><a href="" target="target">Session域数据的存储</a></li>
			<li><a href="" target="target">Session域数据的获取</a></li>			
			<li>Session的存活</li>
			<li>
				<ul>
					<li><a href="" target="target">Session的默认超时及配置</a></li>
					<li><a href="" target="target">Session3秒超时销毁</a></li>
					<li><a href="" target="target">Session马上销毁</a></li>
				</ul>
			</li>
			<li><a href="" target="target">浏览器和Session绑定的原理</a></li>
		</ul>
	</div>
</body>
</html>
```

```
如何创建和获取Session。它们的API 是一样的。
request.getSession()
第一次调用是：创建Session 会话
之后调用都是：获取前面创建好的Session 会话对象。

isNew(); 判断到底是不是刚创建出来的（新的）
true 表示刚创建
false 表示获取之前创建

每个会话都有一个身份证号。也就是ID 值。而且这个ID 是唯一的。
getId() 得到Session 的会话id 值。
```

```java
protected void creatOrGetSession(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
    HttpSession session = req.getSession();
    boolean aNew = session.isNew();
    String id = session.getId();
    resp.getWriter().write("Session的ID:"+id+" ;");
    resp.getWriter().write("Session是否是新创建："+aNew+" ;");
}
```

### 2、Session 域数据的存取

```java
protected void getSession(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
    Object attribute = req.getSession().getAttribute("key1");
    resp.getWriter().write("从Session 中获取出key1 的数据是：" + attribute);
}
protected void setSession(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
    req.getSession().setAttribute("key1", "value1");
    resp.getWriter().write("已经往Session 中保存了数据");
}
```

### 3、Session 生命周期控制

```
public void setMaxInactiveInterval(int interval) 设置Session 的超时时间（以秒为单位），超过指定的时长，Session
就会被销毁。
值为正数的时候，设定Session 的超时时长。
负数表示永不超时（极少使用）

public int getMaxInactiveInterval()获取Session 的超时时间
public void invalidate() 让当前Session 会话马上超时无效。

Session 默认的超时时长是多少！
Session 默认的超时时间长为30 分钟。

因为在Tomcat 服务器的配置文件web.xml 中默认有以下的配置，它就表示配置了当前Tomcat 服务器下所有的Session
超时配置默认时长为：30 分钟。
<session-config>
<session-timeout>30</session-timeout>
</session-config>

如果你想只修改个别Session 的超时时长。就可以使用上面的API。setMaxInactiveInterval(int interval)来进行单独的设置。
session.setMaxInactiveInterval(int interval)单独设置超时时长。
```

```java
protected void setTime(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
    HttpSession session = req.getSession();
    session.setMaxInactiveInterval(10);
    resp.getWriter().write("10s后超时！！");
}
```

```java
protected void deleteNow(HttpServletRequest req, HttpServletResponse resp) throws ServletException,IOException {
    // 先获取Session 对象
    HttpSession session = req.getSession();
    // 让Session 会话马上超时
    session.invalidate();
    resp.getWriter().write("Session 已经设置为超时（无效）");
}
```

### 4、浏览器和Session

> Session 技术，底层其实是基于Cookie 技术来实现的。

![image-20200502232022644](https://raw.githubusercontent.com/yhx1001/PicGo/img/image-20200502232022644.png)

# 10、Filter 过滤器

> Filter 过滤器它的作用是：**拦截请求**，过滤响应。

拦截请求常见的应用场景有：

- 权限检查
- 日记操作
- 事务管理
- ......

## 1、案例：用户登录

```jsp、
<form action="http://localhost:8080/JavaWeb_06_war_exploded/loginServlet" method="get">
用户名：<input type="text" name="username" ><br>
密码：<input type="password" name="password" ><br>
<input type="submit" value="登录">
</form>
```

```java
//LoginServlet
public class LoginServlet extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        String username = req.getParameter("username");
        String password = req.getParameter("password");
        if ("xiaobear".equals(username) && "123456".equals(password)) {
            //登录成功
            Cookie cookie = new Cookie("username", username);
            cookie.setMaxAge(60 * 60 * 24 * 7);
            //当前Cookie 一周内有效
            resp.addCookie(cookie);
            System.out.println("登录成功");
        } else {
            // 登录失败
            System.out.println("登录失败");
        }
    }
}
```

```java
//AdminFilter
public class AdminFilter implements Filter {

    public void doFilter(ServletRequest servletRequest, ServletResponse servletResponse, FilterChain filterChain) throws IOException, ServletException {
        HttpServletRequest request = (HttpServletRequest) servletRequest;
        HttpSession session = request.getSession();
        Object user = session.getAttribute("user");
        if (user == null) {
            servletRequest.getRequestDispatcher("/login.jsp").forward(servletRequest,servletResponse);
        }else {
            //让程序继续访问目标资源
            filterChain.doFilter(servletRequest,servletResponse);
        }
    }
}
```

```xml
<!--Web.xml-->
<filter>
    <filter-name>AdminFilter</filter-name>
    <filter-class>com.xiaobear.filter.AdminFilter</filter-class>
</filter>
<filter-mapping>
    <!--filter-name 表示当前的拦截路径给哪个filter 使用-->
    <filter-name>AdminFilter</filter-name>
    <!--url-pattern 配置拦截路径
        / 表示请求地址为：http://ip:port/工程路径/ 映射到IDEA 的web 目录
        /admin/* 表示请求地址为：http://ip:port/工程路径/admin/*
        -->
    <url-pattern>/admin/*</url-pattern>
</filter-mapping>

<servlet>
    <servlet-name>LoginServlet</servlet-name>
    <servlet-class>com.xiaobear.servlet.LoginServlet</servlet-class>
</servlet>
<servlet-mapping>
    <servlet-name>LoginServlet</servlet-name>
    <url-pattern>/loginServlet</url-pattern>
</servlet-mapping>
```

## 2、Filter生命周期

```
Filter 的生命周期包含几个方法
1、构造器方法

2、init 初始化方法

第1，2 步，在web 工程启动的时候执行（Filter 已经创建）

3、doFilter 过滤方法
第3 步，每次拦截到请求，就会执行

4、destroy 销毁
第4 步，停止web 工程的时候，就会执行（停止web 工程，也会销毁Filter 过滤器）
```

```java
public class AdminFilter implements Filter {

    public AdminFilter() {
        System.out.println("1、构造器方法");
    }

    public void init(FilterConfig filterConfig) throws ServletException {
        System.out.println("2、初始化方法");
    }

    public void doFilter(ServletRequest servletRequest, ServletResponse servletResponse, FilterChain filterChain) throws IOException, ServletException {
        System.out.println("3、doFilter过滤方法");
        HttpServletRequest request = (HttpServletRequest) servletRequest;
        HttpSession session = request.getSession();
        Object user = session.getAttribute("user");
        if (user == null) {
            servletRequest.getRequestDispatcher("/login.jsp").forward(servletRequest,servletResponse);
        }else {
            //让程序继续访问目标资源
            filterChain.doFilter(servletRequest,servletResponse);
        }
    }
    public void destroy() {
        System.out.println("4、destory方法");
    }
}
```

## 3、FilterConfig 类

==Tomcat 每次创建Filter 的时候，也会同时创建一个FilterConfig 类，这里包含了Filter 配置文件的配置信息。==

FilterConfig 类的作用：**获取filter 过滤器的配置内容**

- 获取Filter 的名称filter-name 的内容
- 获取在Filter 中配置的init-param 初始化参数
- 获取ServletContext 对象

```java
public void init(FilterConfig filterConfig) throws ServletException {
    System.out.println("2、初始化方法");
    System.out.println("filter-name的值是："+filterConfig.getFilterName());
    System.out.println("init-param中username的值		是："+filterConfig.getInitParameter("username"));
    System.out.println(filterConfig.getServletContext());
}
```

## 4、FilterChain过滤器链

![](https://raw.githubusercontent.com/yhx1001/PicGo/img/2020-05-06-21-23-04.png)

## 5、Filter拦截路径

- 精准匹配

	```xml
	<url-pattern>/target.jsp</url-pattern>
	以上配置的路径，表示请求地址必须为：http://ip:port/工程路径/target.jsp
	```

- 目录匹配

	```xml
	<url-pattern>/admin/*</url-pattern>
	以上配置的路径，表示请求地址必须为：http://ip:port/工程路径/admin/*
	```

- 后缀名匹配

	```xml
	<url-pattern>*.html</url-pattern>
	以上配置的路径，表示请求地址必须以.html 结尾才会拦截到
	<url-pattern>*.do</url-pattern>
	以上配置的路径，表示请求地址必须以.do 结尾才会拦截到
	<url-pattern>*.action</url-pattern>
	以上配置的路径，表示请求地址必须以.action 结尾才会拦截到
	
	Filter 过滤器它只关心请求的地址是否匹配，不关心请求的资源是否存在！！！
	```

# 11、JSON & Ajax & i18n

## 1、JSON

> JSON (JavaScript Object Notation) 是一种**轻量级的数据交换格式**。易于人阅读和编写。同时也易于机器解析和生成。JSON采用完全独立于语言的文本格式，而且很多语言都提供了对json 的支持（包括C, C++, C#, Java, JavaScript, Perl, Python等）。这样就使得JSON 成为理想的数据交换格式。
>
> - 轻量级指的是跟xml 做比较
> - 数据交换指的是客户端和服务器之间业务数据的传递格式

### 1、json 的定义

json 是由键值对组成，并且由花括号（大括号）包围。每个键由引号引起来，键和值之间使用冒号进行分隔，多

组键值对之间进行逗号进行分隔。

```html
// json的定义
var jsonObj = {
"key1":12,
"key2":"yhx",
"key3":true,
"key4":[11,"arr",false],
"key5":{
"key6":1001,
"key7":"102"
},
"key8":[{
"key9":1001,
"key10":"102"
},
{
"key9":1001,
"key10":"102"
}]
}
```

### 2、json的访问

> json就是一个对象
>
> json 中的key 我们可以理解为是对象中的一个属性
>
> json 中的key 访问就跟访问对象的属性一样： json 对象.key

```html
// json的访问
			alert(typeof(jsonObj));
			alert(jsonObj.key1);
			alert(jsonObj.key2);
			alert(jsonObj.key3);
			alert(jsonObj.key4);
			//遍历数组
			for (var i = 0; i < jsonObj.key4.length ; i++) {
				alert(jsonObj.key4[i]);
			}
			alert(jsonObj.key5.key6);
			alert(jsonObj.key5.key7);
			alert( jsonObj.key8 );// 得到json 数组
			// 取出来每一个元素都是json 对象
			var jsonItem = jsonObj.key6[0];
			 alert( jsonItem.key6_1_1 );
			alert( jsonItem.key9 ); 
```

### 3、json的常用方法

> json 的存在有两种形式。
>
> - 对象的形式存在，我们叫它json 对象，一般我们要操作json 中的数据的时候，需要json 对象的格式。
> - 字符串的形式存在，我们叫它json 字符串，一般我们要在客户端和服务器之间进行数据交换的时候，使用json 字符串。
> - JSON.stringify()   把json 对象转换成为json 字符串
> - JSON.parse()       把json 字符串转换成为json 对象

```java
// json对象转字符串 像Java中的toString
var Stringobj = JSON.stringify(jsonObj);
alert(Stringobj);
// json字符串转json对象
var jsonobj2 = JSON.parse(jsonObj);
alert(jsonobj2.key1);
alert(jsonobj2.key4);
```

### 4、JSON 在java 中的使用

#### 1、JavaBean 与 json的互转

```java
@Test
public void test1(){
    Person person = new Person();
    Gson gson = new Gson();
    // toJson 方法可以把java 对象转换成为json 字符串
    String json = gson.toJson(person);
    System.out.println(json);
    // fromJson 把json 字符串转换回Java 对象
    // 第一个参数是json 字符串
    // 第二个参数是转换回去的Java 对象类型
    Person person1 = gson.fromJson(json, Person.class);
    System.out.println(person1);
}
```

#### 2、List 和json 的互转

```java
@Test
public void test2(){
    List<Person> list = new ArrayList<Person>();
    list.add(new Person(1,"yhx"));
    list.add(new Person(2,"lwh"));
    Gson gson = new Gson();
    String json = gson.toJson(list);
    System.out.println(json);
}
```

#### 3、Map和json的互转

```java
@Test
public void test3(){
    Map<Integer, Person> map = new HashMap<Integer, Person>();
    map.put(1,new Person(3,"333"));
    map.put(2,new Person(4,"444"));
    Gson gson = new Gson();
    // 把map 集合转换成为json 字符串
    String s = gson.toJson(map);
    System.out.println(s);
}
```

## 2、Ajax

> AJAX 即==**“Asynchronous Javascript And XML”（异步JavaScript 和XML**==），是指一种创建交互式网页应用的网页开发技术。
>
> - **Ajax 是一种浏览器通过js 异步发起请求，局部更新页面的技术。**
> - **Ajax 请求的局部更新，浏览器地址栏不会发生变化**
> - **局部更新不会舍弃原来页面的内容**

### 1、原生Ajax请求

```html
<!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
<html>
	<head>
		<meta http-equiv="pragma" content="no-cache" />
		<meta http-equiv="cache-control" content="no-cache" />
		<meta http-equiv="Expires" content="0" />
		<meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
		<title>Insert title here</title>
		<script type="text/javascript">
			function ajaxRequest() {
// 				1、我们首先要创建XMLHttpRequest
				var request = new XMLHttpRequest();
// 				2、调用open方法设置请求参数
				request.open("GET","http://localhost:8080/JavaWeb_07_war_exploded/ajaxServlet?action=javaScriptAjax",true)
//
 				request.onreadystatechange = function () {
				if(request.readyState == 4 && request.status == 200){
					alert(request.responseText);
				}
			}
// 				3、调用send方法发送请求
				request.send();
// 				4、在send方法前绑定onreadystatechange事件，处理请求完成后的操作。

			}
		</script>
	</head>
	<body>	
		<button onclick="ajaxRequest()">ajax request</button>
		<div id="div01">
		</div>
	</body>
</html>
```

BaseServlet代码：

```java
public abstract class BaseServlet extends HttpServlet {

    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        doPost(req, resp);
    }

    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        // 解决post请求中文乱码问题
        // 一定要在获取请求参数之前调用才有效
        req.setCharacterEncoding("UTF-8");
        // 解决响应中文乱码问题
        resp.setContentType("text/html; charset=UTF-8");

        String action = req.getParameter("action");
        try {
            // 获取action业务鉴别字符串，获取相应的业务 方法反射对象
            Method method = this.getClass().getDeclaredMethod(action, HttpServletRequest.class, HttpServletResponse.class);
            //  System.out.println(method);
            // 调用目标业务 方法
            method.invoke(this, req, resp);
        } catch (Exception e) {
            e.printStackTrace();
        }
    }

}

```

AjaxServlet代码：

```java
public class AjaxServelt extends BaseServlet{

    protected void javaScriptAjax(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        System.out.println("Ajax请求过来了！！！");
    }
}
```

### 2、jQuery 中的AJAX 请求

Jquery_Ajax_request.html页面

```html
<!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
<html>
	<head>
		<meta http-equiv="pragma" content="no-cache" />
		<meta http-equiv="cache-control" content="no-cache" />
		<meta http-equiv="Expires" content="0" />
		<meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
		<title>Insert title here</title>
		<script type="text/javascript" src="script/jquery-1.7.2.js"></script>
		<script type="text/javascript">
			$(function(){
				// ajax请求
				$("#ajaxBtn").click(function(){

					$.ajax({
						url:"http://localhost:8080/JavaWeb_07_war_exploded/ajaxServlet" ,
						data:"action=jQueryAjax" ,
						type:"GET" ,
						success: function (data) {
							alert("服务器返回的类型是："+data)
						},
						dataType:"json",
					});

					})
					alert("ajax btn");

				});

				// ajax--get请求
				$("#getBtn").click(function(){

					alert(" get btn ");
					
				});
				
				// ajax--post请求
				$("#postBtn").click(function(){
					// post请求
					alert("post btn");
					
				});

				// ajax--getJson请求
				$("#getJSONBtn").click(function(){
					// 调用
					alert("getJSON btn");

				});

				// ajax请求
				$("#submit").click(function(){
					// 把参数序列化
					alert("serialize()");
				});
				
			});
		</script>
	</head>
	<body>
		<div>
			<button id="ajaxBtn">$.ajax请求</button>
			<button id="getBtn">$.get请求</button>
			<button id="postBtn">$.post请求</button>
			<button id="getJSONBtn">$.getJSON请求</button>
		</div>
		<br/><br/>
		<form id="form01" >
			用户名：<input name="username" type="text" /><br/>
			密码：<input name="password" type="password" /><br/>
			下拉单选：<select name="single">
			  	<option value="Single">Single</option>
			  	<option value="Single2">Single2</option>
			</select><br/>
		  	下拉多选：
		  	<select name="multiple" multiple="multiple">
		    	<option selected="selected" value="Multiple">Multiple</option>
		    	<option value="Multiple2">Multiple2</option>
		    	<option selected="selected" value="Multiple3">Multiple3</option>
		  	</select><br/>
		  	复选：
		 	<input type="checkbox" name="check" value="check1"/> check1
		 	<input type="checkbox" name="check" value="check2" checked="checked"/> check2<br/>
		 	单选：
		 	<input type="radio" name="radio" value="radio1" checked="checked"/> radio1
		 	<input type="radio" name="radio" value="radio2"/> radio2<br/>
		</form>			
		<button id="submit">提交--serialize()</button>
	</body>
</html>
```

>$.ajax 方法
>
>- url 表示请求的地址
>- type 表示请求的类型GET 或POST 请求
>- data 表示发送给服务器的数据
>- 格式有两种：
>	- name=value&name=value
>	- {key:value}
>		success 请求成功，响应的回调函数
>		dataType 响应的数据类型
>		常用的数据类型有：
>
>- text 表示纯文本
>- xml 表示xml 数据
>- json 表示json 对象

```javascript
$("#ajaxBtn").click(function(){
$.ajax({
url:"http://localhost:8080/16_json_ajax_i18n/ajaxServlet",
// data:"action=jQueryAjax",
data:{action:"jQueryAjax"},
type:"GET",
success:function (data) {
// alert("服务器返回的数据是：" + data);
// var jsonObj = JSON.parse(data);
$("#msg").html("编号：" + data.id + " , 姓名：" + data.name);
},
dataType : "json"
});
});
```

> $.get 方法和$.post 方法
> url                        请求的url 地址
> data                     发送的数据
> callback               成功的回调函数
> type                     返回的数据类型

```javascript
$("#getBtn").click(function(){
$.get("http://localhost:8080/16_json_ajax_i18n/ajaxServlet","action=jQueryGet",function (data) {
$("#msg").html(" get 编号：" + data.id + " , 姓名：" + data.name);
},"json");
});
// ajax--post 请求
$("#postBtn").click(function(){
$.post("http://localhost:8080/16_json_ajax_i18n/ajaxServlet","action=jQueryPost",function (data)
{
$("#msg").html(" post 编号：" + data.id + " , 姓名：" + data.name);
},"json");
});
```

> $.getJSON 方法
> url                       请求的url 地址
> data                    发送给服务器的数据
> callback               成功的回调函数

```javascript
// ajax--getJson 请求
$("#getJSONBtn").click(function(){
$.getJSON("http://localhost:8080/16_json_ajax_i18n/ajaxServlet","action=jQueryGetJSON",function
(data) {
$("#msg").html(" getJSON 编号：" + data.id + " , 姓名：" + data.name);
});
});
```

> 表单序列化serialize()
> serialize()        可以把表单中所有表单项的内容都获取到，并以name=value&name=value 的形式进行拼接。

```javascript
// ajax 请求
$("#submit").click(function(){
// 把参数序列化
$.getJSON("http://localhost:8080/16_json_ajax_i18n/ajaxServlet","action=jQuerySerialize&" +
$("#form01").serialize(),function (data) {
$("#msg").html(" Serialize 编号：" + data.id + " , 姓名：" + data.name);
});
});
```

## 3、i18n国际化

> - 国际化（Internationalization）指的是同一个网站可以支持多种不同的语言，以方便不同国家，不同语种的用户访问。
> - 关于国际化我们想到的最简单的方案就是为不同的国家创建不同的网站，比如苹果公司，他的英文官网是：http://www.apple.com 而中国官网是http://www.apple.com/cn
> - 苹果公司这种方案并不适合全部公司，而我们希望相同的一个网站，而不同人访问的时候可以根据用户所在的区域显示不同的语言文字，而网站的布局样式等不发生改变。
> - 于是就有了我们说的国际化，国际化总的来说就是同一个网站不同国家的人来访问可以显示出不同的语言。但实际上这种需求并不强烈，一般真的有国际化需求的公司，主流采用的依然是苹果公司的那种方案，为不同的国家创建不同的页面。所以国际化的内容我们了解一下即可。
> -  国际化的英文Internationalization，但是由于拼写过长，老外想了一个简单的写法叫做I18N，代表的是Internationalization这个单词，以I 开头，以N 结尾，而中间是18 个字母，所以简写为I18N。以后我们说I18N 和国际化是一个意思。

### 1、i18n相关要素

- 国际化包名命名规则：baseName_locale.properties (locale表示不同的时区、位置、语言)
- 中文的配置文件名：i18n_zh_CN.properties
- 英文的配置文件名：i18n_en_US.properties

### 2、国际化资源properties 测试

i18n_en_US.properties 英文

```properties
username=username
password=password
sex=sex
age=age
regist=regist
boy=boy
email=email
girl=girl
reset=reset
submit=submit
```

i18n_zh_CN.properties 中文

```properties
username=用户名
password=密码
sex=性别
age=年龄
regist=注册
boy=男
girl=女
email=邮箱
reset=重置
submit=提交
```

测试代码：

```java
public class I18nTest {

    @Test
    public void LocaleTest(){
        Locale locale = Locale.getDefault();
        System.out.println(locale);
        //遍历不同国家的locale
        for (Locale availableLocale : Locale.getAvailableLocales()) {
            System.out.println(availableLocale);
        }
        // 获取中文，中文的常量的Locale 对象
        System.out.println(Locale.CHINA);
        // 获取英文，英文的常量的Locale 对象
        System.out.println(Locale.US);
    }
    @Test
    public void testI18n(){
        // 得到我们需要的Locale 对象
        Locale locale = Locale.CHINA;
        // 通过指定的basename 和Locale 对象，读取相应的配置文件
        ResourceBundle resourceBundle = ResourceBundle.getBundle("i18n", locale);
        System.out.println("username: "+resourceBundle.getString("username"));
        System.out.println("password: "+resourceBundle.getString("password"));
        System.out.println("girl: "+resourceBundle.getString("girl"));
        System.out.println("boy: "+resourceBundle.getString("boy"));
    }
}
```

### 3、请求头国际化页面

```html
<%@ page import="java.util.Locale" %>
<%@ page import="java.util.ResourceBundle" %>
<%@ page language="java" contentType="text/html; charset=UTF-8"
		 pageEncoding="UTF-8"%>
<!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
<html>
<head>
<meta http-equiv="pragma" content="no-cache" />
<meta http-equiv="cache-control" content="no-cache" />
<meta http-equiv="Expires" content="0" />
<meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
<title>Insert title here</title>
</head>
<body>
<%
	Locale locale = request.getLocale();
	System.out.println(locale);
	ResourceBundle i18n = ResourceBundle.getBundle("i18n", locale);
%>
	<a href="">中文</a>|
	<a href="">english</a>
	<center>
		<h1><%=i18n.getString("regist")%></h1>
		<table>
		<form>
			<tr>
				<td><%=i18n.getString("username")%></td>
				<td><input name="username" type="text" /></td>
			</tr>
			<tr>
				<td><%=i18n.getString("password")%></td>
				<td><input type="password" /></td>
			</tr>
			<tr>
				<td><%=i18n.getString("sex")%></td>
				<td><input type="radio" /><%=i18n.getString("boy")%><input type="radio" /><%=i18n.getString("girl")%></td>
			</tr>
			<tr>
				<td><%=i18n.getString("email")%></td>
				<td><input type="text" /></td>
			</tr>
			<tr>
				<td colspan="2" align="center">
				<input type="reset" value="<%=i18n.getString("reset")%>" />&nbsp;&nbsp;
				<input type="submit" value="<%=i18n.getString("submit")%>" /></td>
			</tr>
			</form>
		</table>
		<br /> <br /> <br /> <br />
	</center>
	国际化测试：
	<br /> 1、访问页面，通过浏览器设置，请求头信息确定国际化语言。
	<br /> 2、通过左上角，手动切换语言
</body>
</html>
```

### 4、通过显示的选择语言类型进行国际化

```html
<%@ page import="java.util.Locale" %>
<%@ page import="java.util.ResourceBundle" %>
<%@ page language="java" contentType="text/html; charset=UTF-8"
		 pageEncoding="UTF-8"%>
<!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
<html>
<head>
<meta http-equiv="pragma" content="no-cache" />
<meta http-equiv="cache-control" content="no-cache" />
<meta http-equiv="Expires" content="0" />
<meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
<title>Insert title here</title>
</head>
<body>
<%

	Locale locale = null;
	String country = request.getParameter("country");
	if ("cn".equals(country)){
		locale = Locale.CHINA;
	}else if("usa".equals(country)){
		locale = Locale.US;
	}else {
		locale = Locale.getDefault();
	}
	System.out.println(locale);
	ResourceBundle i18n = ResourceBundle.getBundle("i18n", locale);
%>
	<a href="i18n.jsp?country=cn">中文</a>|
	<a href="i18n.jsp?country=usa">english</a>
	<center>
		<h1><%=i18n.getString("regist")%></h1>
		<table>
		<form>
			<tr>
				<td><%=i18n.getString("username")%></td>
				<td><input name="username" type="text" /></td>
			</tr>
			<tr>
				<td><%=i18n.getString("password")%></td>
				<td><input type="password" /></td>
			</tr>
			<tr>
				<td><%=i18n.getString("sex")%></td>
				<td><input type="radio" /><%=i18n.getString("boy")%><input type="radio" /><%=i18n.getString("girl")%></td>
			</tr>
			<tr>
				<td><%=i18n.getString("email")%></td>
				<td><input type="text" /></td>
			</tr>
			<tr>
				<td colspan="2" align="center">
				<input type="reset" value="<%=i18n.getString("reset")%>" />&nbsp;&nbsp;
				<input type="submit" value="<%=i18n.getString("submit")%>" /></td>
			</tr>
			</form>
		</table>
		<br /> <br /> <br /> <br />
	</center>
	国际化测试：
	<br /> 1、访问页面，通过浏览器设置，请求头信息确定国际化语言。
	<br /> 2、通过左上角，手动切换语言
</body>
</html>
```

### 5、JSTL 标签库实现国际化

```jsp
<%--1 使用标签设置Locale 信息--%>
<fmt:setLocale value="" />
<%--2 使用标签设置baseName--%>
<fmt:setBundle basename=""/>
<%--3 输出指定key 的国际化信息--%>
<fmt:message key="" />
```

```html
<%@ taglib prefix="fmt" uri="http://java.sun.com/jstl/fmt" %>
<%@ page import="java.util.Locale" %>
<%@ page language="java" contentType="text/html; charset=UTF-8"
		 pageEncoding="UTF-8"%>
<!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
<html>
<head>
<meta http-equiv="pragma" content="no-cache" />
<meta http-equiv="cache-control" content="no-cache" />
<meta http-equiv="Expires" content="0" />
<meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
<title>Insert title here</title>
</head>
<body>
<%--1 使用标签设置Locale 信息--%>
<fmt:setLocale value="${param.locale}" />
<%--2 使用标签设置baseName--%>
<fmt:setBundle basename="i18n"/>

	<a href="i18n_fmt.jsp?locale=zh_CN">中文</a>|
	<a href="i18n_fmt.jsp?locale=en_US">english</a>
	<center>
		<h1><fmt:message key="regist" /></h1>
		<table>
		<form>
			<tr>
				<td><fmt:message key="username" /></td>
				<td><input name="username" type="text" /></td>
			</tr>
			<tr>
				<td><fmt:message key="password" /></td>
				<td><input type="password" /></td>
			</tr>
			<tr>
				<td><fmt:message key="sex" /></td>
				<td><input type="radio" /><fmt:message key="boy" />
					<input type="radio" /><fmt:message key="girl" /></td>
			</tr>
			<tr>
				<td><fmt:message key="email" /></td>
				<td><input type="text" /></td>
			</tr>
			<tr>
				<td colspan="2" align="center">
				<input type="reset" value="<fmt:message key="reset" />" />&nbsp;&nbsp;
				<input type="submit" value="<fmt:message key="submit" />" /></td>
			</tr>
			</form>
		</table>
		<br /> <br /> <br /> <br />
	</center>
</body>
</html>
```

错误总结：

```
Exception

org.apache.jasper.JasperException: /i18n_fmt.jsp (行.: [16], 列: [0]) According to TLD or attribute directive in tag file, attribute [value] does not accept any expressions
	org.apache.jasper.compiler.DefaultErrorHandler.jspError(DefaultErrorHandler.java:42)
	org.apache.jasper.compiler.ErrorDispatcher.dispatch(ErrorDispatcher.java:292)
	org.apache.jasper.compiler.ErrorDispatcher.jspError(ErrorDispatcher.java:115)
	org.apache.jasper.compiler.Validator$ValidateVisitor.checkXmlAttributes(Validator.java:1250)
	org.apache.jasper.compiler.Validator$ValidateVisitor.visit(Validator.java:888)
	org.apache.jasper.compiler.Node$CustomTag.accept(Node.java:1544)
	org.apache.jasper.compiler.Node$Nodes.visit(Node.java:2389)
	org.apache.jasper.compiler.Node$Visitor.visitBody(Node.java:2441)
	org.apache.jasper.compiler.Node$Visitor.visit(Node.java:2447)
	org.apache.jasper.compiler.Node$Root.accept(Node.java:470)
	org.apache.jasper.compiler.Node$Nodes.visit(Node.java:2389)
	org.apache.jasper.compiler.Validator.validateExDirectives(Validator.java:1857)
	org.apache.jasper.compiler.Compiler.generateJava(Compiler.java:224)
	org.apache.jasper.compiler.Compiler.compile(Compiler.java:386)
	org.apache.jasper.compiler.Compiler.compile(Compiler.java:362)
	org.apache.jasper.compiler.Compiler.compile(Compiler.java:346)
	org.apache.jasper.JspCompilationContext.compile(JspCompilationContext.java:605)
	org.apache.jasper.servlet.JspServletWrapper.service(JspServletWrapper.java:400)
	org.apache.jasper.servlet.JspServlet.serviceJspFile(JspServlet.java:385)
	org.apache.jasper.servlet.JspServlet.service(JspServlet.java:329)
	javax.servlet.http.HttpServlet.service(HttpServlet.java:741)
	org.apache.tomcat.websocket.server.WsFilter.doFilter(WsFilter.java:53)
```

原因：版本不支持该标签

解决：

```html
<%@ taglib prefix="fmt" uri="http://java.sun.com/jstl/fmt" %>
    改为
 <%@ taglib prefix="fmt" uri="http://java.sun.com/jsp/jstl/fmt" %>
```

