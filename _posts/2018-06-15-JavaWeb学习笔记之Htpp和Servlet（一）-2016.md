---
layout:     post                    # 使用的布局（不需要改）
title:      JavaWeb学习笔记之Http和Servlet（一）               # 标题
subtitle:   Servlet                      #副标题
date:       2018-6-15              # 时间
author:     Dxhua                      # 作者
header-img: img/world-new.jpg    #这篇文章标题背景图片
catalog: true                       # 是否归档
tags:                               #标签
    - JavaWeb
    - http
    - Servlet
---
# JavaWeb学习笔记之Http和Servlet（一）  
1.http:超文本传输协议
包括请求行、若干请求头、请求体
- 请求行包括了请求方式、请求资源名称、HTTP版本
- 请求行格式 ：请求方式 资源路径 协议/版本
常见请求方式 POST、GET、DELETE、PUT
GET请求：向特定服务器发送查询请求，一般用于查询数据和资源请求 ，get请求查询的参数可以在浏览器的地址栏中显示，请求的数据会附加在URL之后，以？分割URL和传输数据，多个资源以&连接并且没有请求体
POST请求：向服务器提交数据，一般用在客户端将本地数据或资源提交给服务器，例如注册用户，将用户信息提交给服务器post请求会把请求的数据放在HTTP的请求体中
> 所以get请求一般用于查询数据，post请求一般用于提交数据操作，两者的区别就是在实际开发中get请求会受到URL的限制，而post由于没有将请求参数放在URL中，理论上不受URL的限制
DELETE用于删除数据的时候，PUT用于更新数据的时候
- 请求头 用于客户端请求哪台主机，以及客户端的一些环境信息等，请求头一键值对（key=value）的方式传递数据
- 请求体 代表浏览器在post请求的方式中传递给服务器的参数，请求体中每一个数据都使用键值对的（key=value)的形式，多个参数以&来接  注:服务器在接收到请求体后需要单独解析
- HTTP响应 代表服务器向客户端回送的数据，包括一个响应行和若干响应头以及响应体
响应行包含了HTTP协议的版本，以及描述服务器对请求的处理  格式：协议\版本号 状态码 状态码描述
响应头 用于描述服务器的基本信息以及数据的描述，服务器通过这些信息可以告诉客户端怎么处理它回送的数据
响应体 代表服务器向客户端回送的正文

2.servlet
- 创建一个类继承HttpServlet,重写doGet()和doPost()方法
- 通过HttpServletRequest来获取请求行和请求头
```
@WebServlet(name = "TestServlet",urlPatterns = "/test")
public class TestServlet extends HttpServlet {
    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {


    }

    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        System.out.println("TestServlet收到GET请求");
            //获取请求行内容
        System.out.println("请求方式:"+request.getMethod());
        System.out.println("URL:"+request.getRequestURI());
        System.out.println("发出请求的客户端地址："+request.getRemoteAddr());
        System.out.println("服务点接收请求的IP地址："+request.getLocalAddr());
        System.out.println("访问客户端的端口号："+request.getRemotePort());
        System.out.println("web应用路径："+request.getContextPath());
        System.out.println("协议版本："+request.getProtocol());
          //获取请求头内容
        Enumeration<String> headername = request.getHeaderNames();
        while (headername.hasMoreElements()){
          String element =  headername.nextElement();
            System.out.println(element+":"+request.getHeader(element));

        }
    }
}
```
结果如下
![请求行和请求头打印结果](http://ovt2nfhfc.bkt.clouddn.com/%E8%8E%B7%E5%8F%96%E5%88%B0%E7%9A%84%E8%AF%B7%E6%B1%82%E8%A1%8C%E5%92%8C%E8%AF%B7%E6%B1%82%E5%A4%B4%E6%89%93%E5%8D%B0%E7%BB%93%E6%9E%9C.png)
- 获取请求参数
getParamter(String)用于获取指定名称的参数值，如果请求中没有包含指定的参数值，则返回null,如果指定的参数值没有给定值，返回空串""，包含等多个参数值值，则返回第一个值
getParamterName():该方法用于返回一个包含请求消息中所有的参数名的Enumbernation
getParamerMap()该方法用于将请求中所有的参数和值植转入一个Map对象返回

3. HttpServlerResponse响应对象
- 发送响应行 setStatus(int status)用于设置HTTP响应消息的状态码
sendError(int code)该方法用于发送错误信息的状态码，例如404找不到访问资源，他还有一种重载的形式sendError(int code,String errorMessage)errorMessage可以以文本的形式显示在客户端浏览器
- 发送响应头 addHeader(String name,String value)该方法用来设置HTTP协议的响应头字段，其中name是响应头字段名，value是响应字段的值
setHeader(String name,String value)该方法与addHeader()相同 区别是addHeader()可以重复添加同名的响应头，而setHeader()会覆盖原来的值
addIntHeader(String name,int value)、setIntHeader(String name ,int value)这两个方法将响应字段为int类型的添加到响应头中，两种方法的区别跟上面一样
setContentLength():该方法用于设置HTTP响应消息的内容大小，单位是字节
setContentType():该方法用于设置servlet输出内容的类型，也就是HTTP协议中的Content-Type响应头
- 发送响应体消息  由于在HTTP响应消息中，大量的数据都是通过响应体传递的，因此servletResponse遵循以IO流传递大数据的设计理念，定义了两个与输出流有关的方法
getOutputStream()方法：该方法获取的字节流输出对象为ServletOutputStream类型，它是OutputStream的子类，因此可以直接输出字节数组中的二进制数据
getWrite()方法：该方法获得的字符输出流是PrintWrite类型由于它可以直接输出文本类型，因此要输出网页文档，可以采用该方法

4. servlet的生命周期指的是servlet从创建到销毁的过程
- servlet的接口 javax.servlet.Servler
init(ServletConfig)方法，初始化方法
service(ServletRequest ,ServletResponse)方法，每次访问都会调用来处理请求
destroy()方法，销毁Servlet方法
HttpServlet接口 javax.servlet.http
继承servlet接口，并重新实现service方法，根据不同请求方式调用不同的处理方法
service(HttpServletRequest,HttpServletResponse)方法，获取请求方式，分别调用doGet()或者doPost()方法
![servlet生命周期](http://ovt2nfhfc.bkt.clouddn.com/Servlet%E7%94%9F%E5%91%BD%E5%91%A8%E6%9C%9F.png)
>注意：Servlet是单例的，即无论请求多少次servlet，最多只有一个servlet单例，如果多个客户端同时访问Servlet的时候，服务器会启动多个线程分别执行Servlet的service（）方法
如果我们每次访问都创建一个实例，就会占用计算机资源

5. ServletConfig对象，是它所对Servlet对象的相关配置
特点：每一个Servlet对象都有一个ServletConfig对象和它对应  ，ServletConfig对象在多个Servlet对象之间不是共享的
常见的ServletConfig对象的方法：
- getInitParameter(String name),返回一个初始变量的值
- getInitParameterName()返回servlet初始化参数的所有名称
- getServletContext()获得ServletContext对象
- getServletName()获得Servlet的name配置值

6. SevrvlentContext对象定义：ServletContext即Servlet上下文对象，该对象表示当前web应用环境对象
 获取ServletContext对象：  用来获取全局变量
 > 通过ServletConfig的getServletContext()方法可以得到ServletContext对象
 > HttpServlet中直接通过this.getServletContext()获取，
 通过ServletContext对象来实现多个Servlet对象之间共享参数，首先在一个Servlet中获取到ServletContext对象，然后通过setAttribute()方法将参数放置到ServletContext对象中，在
 其他servlet对象中，先说去到ServletContext对象，在通过getAttribute()方法来获取到共享的参数。    

利用ServletContext对象来读取项目的资源文件    
首先通过this.getServletContext()方法获取到ServletContext对象，在通过getResourceAsStream("文件路径")读取一个文件的路径，返回一个文件流，具体使用如下：  
```
InputStream resourceAsStream = this.getServletContext().getResourceAsStream("/db.propetties");
```  
然后读取配置文件，固定方法可以记下来  
```
Properties properties = new Properties();
       properties.load(resourceAsStream);//把拿到的输入流放进去
       String name = properties.getProperty("name");//拿到配置文件中固定的值
       String passWord = properties.getProperty("passWord");
       String url = properties.getProperty("url");
       System.out.println("name:"+name);
       System.out.println("passWord:"+passWord);
       System.out.println("url:"+url);
```
7. Servlet之间的跳转  
Servlet之间可以实现跳转，从一个Servlet跳转到另一个Servlet，Servlet的跳转技术可以很方便的把一个业务模块分开，  
比如使用一个Servlet来提交用户数据，使用另一个Servlet来读取数据库，最后跳转到另一个Servlet来显示处理结果，这个也就是MVC模式。
> MVC:（mode,view,controller）用一种业务逻辑、数据、界面显示分离的方法构建代码，将业务逻辑聚集到一个部件里面，在改进和个性化定制界面的同时，
不需要重新编写业务逻辑。MVC被独特的发展起来用于映射传统的输入、处理和输出功能在一个业务逻辑的图形化用户界面中  
- 跳转方式一：转发Forward,在Servlet中如果当前的web资源不想处理请求时，可以通过forward方将当前请求传递给其他的web资源处理，这种方式成为请求转发  
转发流程图：  
![转发流程图](http://pdg3d7gpb.bkt.clouddn.com/%E8%BD%AC%E5%8F%91%E6%B5%81%E7%A8%8B%E5%9B%BE.png)  
在上方的流程图中可以看出浏览器访问的是firstServlet，然而firstServlet没有对请求进行处理，而是通过forward方法将请求转发给secondServlet来处理，secondServlet对请求进行了处理，并返回了一个Response对象给浏览器  
请求转发的相关方法：  
RequestDispatcher对象，可以通过requset.getRequsetDispatcher()方法获取到RequestDispatcher对象，然后调用这个对象的forward方法就可以实现请求转发
```
  request.getRequestDispatcher("/loginError.jsp").forward(request,response);//getRequestDispather()中的参数为资源路径
```  
转发过程中携带数据 ：  
在转发过程中传递参数不易使用ServletContext对象来传递，因为ServletContext对象是一个共享域对象，当产生不同的错误时，会把前面的错误给覆盖掉，所以这里采用request。  
request本身也是一个域对象，request可以携带数据传递给其他web资源  
setAttribute()方法 ：  
getAttribute()方法：  
removeAttribute()方法：  
getAttributeName()方法：  
通过request对象的setAttribute()方法  
- 方式二：重定向:重定向是根据服务器返回的状态码来实现的。客户端浏览器在请求服务器的时候，服务端会返回一个状态码。  
服务器通过HttpServletResponse的setStatus(int status)方法来设置状态码，如果服务器返回返回的状态码是301或302，则浏览器就会到新的网址重新请求资源，服务器的响应中会带着这个新资源的地址。  
请求重定向的相关代码
```
//设置状态码为302，SC_MOVED_TEMPORAILY是302的静态常量  
resonse.setStatus(HttpServletResponse.SC_MOVED_TEMPORAILY);
//在请求头中携带新资源的地址
response.setHeader("Localtion","http://www.baidu.com");//在请求重定向的时候，地址需要加入项目名，在访问其他网站地址时，只需要填入其他地址就可以了
```   
重定向的流程如下图所示：  
![重定向流程](http://pdg3d7gpb.bkt.clouddn.com/%E9%87%8D%E5%AE%9A%E5%90%91%E6%B5%81%E7%A8%8B.png)    
为了使用方便HttpServletResponse中将setStatus和setHeader这两个方法合并在一起叫做sendRedirect(String Localtion)  
请求转发和重定向的区别:  
(1)重定向浏览器的地址栏会发生改变，而请求转发不会，因为请求转发是在服务器内部完成的，浏览器不知道进行了转发，而重定向是ServletA告诉了浏览器我不能处理，并返回了一个新的地址，有浏览器重新去访问这个新的地址，所以地址栏是会改变的    
(2)重定向是两次请求两次响应，请求转发是一次请求一次响应  
(3)重定向的路径需要加工程名，请求转发路径不需要加工程名  
(4)重定向可以跳转到任何网页，而请求转发只能在服务器内部进行  
![请求转发和重定向](http://pdg3d7gpb.bkt.clouddn.com/%E8%AF%B7%E6%B1%82%E8%BD%AC%E5%8F%91%E5%92%8C%E9%87%8D%E5%AE%9A%E5%90%91.png)   
实现Servlet的的自动刷新
```
//        response.setContentType("text/html;charset=utf-8");
//        response.setHeader("refresh","3;url='/hello/home.html'");
//        response.getWriter().print("3秒后自动刷新");//在实际的项目中自动跳转或者刷新一般不写在Servlet中，一般在jsp中实现
      String message = "<meta http-equiv='refresh' content='3;url=/hello/home.html'>3秒后会自动跳转到首页，如果没有跳转，请点击<a href = '/hello/home.html'>跳转链接<a>";
      //meta标签可以模仿一个http的响应头
       request.setAttribute("message",message);
        request.getRequestDispatcher("/index.jsp").forward(request,response);
```   
8. Servlet的线程安全  
当多个客户端并发访问同一个Servlet的时候，web服务器会为每一个客户端浏览器的访问创建一个线程，并在这个线程上调用的Servlet的service方法，
因此service方法内部访问了同一个共享资源的话，就可能引发线程安全问题，如果访问的是局部变量，就不会出现线程安全问题。  
解决方法：
(1)使用同步代码块，就是使用java中的synchronized()方法，该方法在对同一个变量进行操作的时候，会把后面的线程阻塞掉，只有当第一个线程结束后，第二个线程才能进来，因此在线程较多的时候会等待很长时间(不推荐)。
(2)实现singleThreadModle接口，(每次调用Servlet都会创建一个实例，占用计算机资源)
(3)尽量不要在Servlet实例中使用共享变量  (最好的方法，推荐使用)
