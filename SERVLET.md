## SERVLET

1Servlet的实现

```java
//servlet实现  1新建包并新建普通class类   2实现servlet规范 继承servlet类  3 重写方法service  接受请求  响应结果  
// 4service在servlet被访问时自动调用  在servlet添加注解 设置servlet的访问路径   浏览器通过访问该路径从而访问到servlet
//住  service由服务器自动调用（当servlet被访问时）  2.设置servlet要加/  3.在同一项目下servlet对外访问路径唯一
//4. 访问servlet的格式如下  http://localhost:端口/项目路径/资源路径？参数
//项目路径在tomcat中deployment设置  资源路径通过@webservlet设置
```

2.servlet的工作流程

通过请求头知道浏览器访问的是哪一个主机



通过请求行获取访问的是哪一个web应用

再通过请求行中的请求路径知道访问的是哪一个资源

通过资源路径配置匹配真实的路径

服务器会创建servlet对象（如果是第一次访问时，创建servlet实例，并调用init方法进行初始化操作）

调用service方法（req，res）方法来处理请求和响应操作

调用service完毕后返回服务器  由服务器将res缓冲区的数据取出，以http的格式发送给浏览器

#### servlet的生命周期

实例和初始化时机：请求达到容器时，容器查找该servlet是否存在 不存在则创建实例并进行初始化  init（）

就绪/调用/服务阶段：有请求大到容器时，容器会调用service方法，处理请求可以在生命周期多次被调用：Httpservlet的service（）方法会依据请求方式来调用doGet或doPost方法，但是，这两个do方法默认情况下会抛出异常，需要子类去override//真正上一般重写service方法

销毁时机

关闭服务器时  destroy（）

#### 3.2httpservletRequest对象

方法

| getRequestURL（）  | 获取客户端发出请求时的完整URL |
| ------------------ | ----------------------------- |
| getRequestURI（）  | 获取请求中资源名称部分        |
| getQueryString（） | 获取请求中参数部分            |
| getMethod（）      | 获取客户端请求方式            |
| getProtocol（）    | 获取http版本号                |
| getComtextPath（） | 获取webapp名字                |

获取参数的方法

getParameter（）  获取指定名称的参数

getParameterValues（String name）  获取指定名称参数的所有值

#### 请求乱码处理：

```java
req.setCharacterEncoding("UTF-8")
```



### 3.3请求转发（用于点击登入后跳转页面）

特点：地址栏不发生改变   服务端行为 用服务型端来控制

请求转发是一种服务器的行为，当客户端请求达到后，服务器进行转发，此时会将请求对象进行保存，地址栏中的url地址不会变，得到响应后，服务器再将响应发给客户端，从始至终只有一个请求发出

#### 

格式  req.getRequestDispatcher(“路径”).forward（req.resp）;路径为目标跳转路径

### 3.4request作用域

通过该对象可以在一个请求中传递数据，作用范围：再一次请求中有效，即服务器跳转有效。 req.getRequestDispatcher(“路径”).forward（req.resp）;

```java
//设置对象内容
request.setAttribute(String name,String value);
//获取域对象内容
request.getAttribute(String name);
//删除域对象内容
request.removeAttribute(String name);
```

### HttpsServletResponse对象

4.1响应数据  getWriter（) 获取字符流 ,只可以响应字符                  getOutputStream() 获取字节流，可以响应一切数据   两种流不可以同时使用

若想返回含标签形式的内容则可以设置响应类型：resp.setHeader("content-type","text/html");



```java
//字符流
PrintWriter wruter=resp.getWriter();
writer.write("hello");
writer.close();
//字节流
ServletOutputStream out=resp.getOutputStream();
out.write("hello",getBytes(字符类型：如UTF-8))；
    out.close
```



### 重定向

```java
/*
* 重定向
*   服务端指导客户端行为的跳转方式
* 浏览器地址蓝发生改变
* 客户端跳转
* 有两次请求
* request作用域不共享
*
* */
      resp.sendRedirect("index.jsp");//跳转至...路径
```



### Cookie对象

1.cookie创建和发送

通过new Cookie（“key”，“value”）；来创建一个Cookie对象，想要将Cookie随响应发送到客户端，需要先添加到respone对象中，response.addCookie（cookie）；此时cookie对象则随着响应发送至了客户端。浏览器上可以看见。

```java
//        创建cookie对象
        Cookie COOKIE=new Cookie("uname","zhangshan");
//        发送cookie对象
        resp.addCookie(COOKIE);
```

2.Cookie的获取

req.getCookies（）；拿到全部的cookie，返回的时cookie数组



3.cookie设置到期时间：xxx.setMaxAge（）

正整数：cookie会存活指定秒数

负整数：表示cookie只在浏览器中存活，浏览器关闭则失效

0：表示即刻删除.删除cookie对象

默认值是-1

#### cookie注意事项

![Screenshot_20220320_131201](C:\Users\1678767988\Desktop\SERVLET.assets\Screenshot_20220320_131201.jpg)



### cookie路径问题



### ![Screenshot_20220320_131440](C:\Users\1678767988\Desktop\SERVLET.assets\Screenshot_20220320_131440.jpg)



### 6.HttpSession对象





![11980516477537872](C:\Users\1678767988\Desktop\SERVLET.assets\11980516477537872.png)



session表示一次会话，会话中可以有多次请求

获取session对象：request.getSession（）：当session存在则获取  不存在则表示创建session对象

```java
//获取会话
HttpSession session= req.getSession();
//获取会话标识符
System.out.println("session.getId()");
//访问开始时间
System.out.println("session.getCreationTime()");
//会话结束时间
System.out.println("session.getLastAccessedTime()");
//是否是新的会话
System.out.println("session.isNew()");
```

Session作用域：用法和request作用域一样  只是范围更大  在一次会话中生效



### Session对象的销毁

默认时间30分钟![9249116477552112](C:\Users\1678767988\Desktop\SERVLET.assets\9249116477552112.png)

### ServletContext对象

### ![16770016478556942](C:\Users\1678767988\Desktop\SERVLET.assets\16770016478556942.png)

常用方法：获取项目在服务器的真实路径:     xxx.getRealPath();

获取当前服务器版本信息：xxx.getServerInfo（）；



### 文件上传和下载

-------------------------------------------------------------------------------------前台上传------------------------------------------------------------------------------

![8705016478576402](C:\Users\1678767988\Desktop\SERVLET.assets\8705016478576402.png)

....每个表单元素需要设置name属性值

提交路径为处理上传文件的servlet对外访问路径

```java
package com.example.serclet;

import javax.servlet.ServletException;
import javax.servlet.annotation.MultipartConfig;
import javax.servlet.annotation.WebServlet;
import javax.servlet.http.HttpServlet;
import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import javax.servlet.http.Part;
import java.io.File;
import java.io.IOException;

@WebServlet("/upload")
@MultipartConfig
public class upload extends HttpServlet {



    @Override
    protected void service(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {

        //设置编码格式
        req.setCharacterEncoding("UTF-8");
        //获取普通文本框（通过getParameter（name）方法来获取）
        String uname = req.getParameter("uname");
        System.out.println("用户名" + uname);
        //得到part对象
        Part part = req.getPart("myfile");
        //得到上传文件的文件名
        String fileName = part.getSubmittedFileName();
        //设置上传文件的的存放路径
        String uploadPath = "C:\\Users\\1678767988\\IdeaProjects\\sercletupload\\src\\main\\webapp\\upload";
        System.out.println(uploadPath);
        //上传文件
        part.write(uploadPath+ File.separator + fileName);
        super.service(req, resp);
    }
}
```

-------------------------------------------------------------------------------------------后台上传---------------------------------------------------------------------------------------------------------------------

1.需要加@MultipartConfig注解

2.Servlet将multipart/form-data的POST请求封装成part，通过Part对上传文件进行操作

```
Part part=request.getPart(name);
name代表的是元素（文件域的name属性值	）
```

​           

## 过滤器

1，创建普通java类

2.实现filter接口    implement filter

```java
@WebFilter("/ser01")//拦截ser01
public class filter implements Filter {
    @Override
    public void init(FilterConfig filterConfig) throws ServletException {
        Filter.super.init(filterConfig);
    }
/*过滤方法
*  @parade， servletRequest
* @param    servletResponse
* @param Filterchain
*
* */
    @Override
    public void doFilter(ServletRequest servletRequest, ServletResponse servletResponse, FilterChain filterChain) throws IOException, ServletException {

        //对请求进行预处理

        System.out.println("ser01已经拦截");
        //放行资源   被放行资源就可以正常传达  对资源进行后处理
        //例如处理请求乱码问题（基于HTTP请求）
        HttpServletRequest request=(HttpServletRequest) servletRequest;
        //设置请求的编码格式
        request.setCharacterEncoding("UTF-8");

        filterChain.doFilter(servletRequest,servletResponse);
    }

    @Override
    public void destroy() {
        Filter.super.destroy();
    }
}
```

