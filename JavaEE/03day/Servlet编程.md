#### 学习大纲
- 1. servlet概念及相关接口简介
- 2. servet 执行过程
- 3. servlet路径映射
- 4. 缺省servlet          --应用
- 5. servlet生命周期(重点)   --理解（重点）
- 6. Servlet自动加载 
- 7. Servlet线程安全 
- 8. servletConfig对象 主要用于加载servlet的初始化参数，在一个web应用可以存在多个ServletConfig 对象
- 9. Servlet相关接口详解
- 10. ServletContext对象     --知识点
- 

#### 如何开发一个Servlet

> 1.1步骤
- 编写java类,继承HttpServlet类
- 重写doGet和doPost方法
- Servlet程序交给tomcat服务器运行
-  servlet程序的class码拷贝到WEB-INF/classes目录
在web.xm文件中进行配置

http://http协议
- locallhost:到本地hosts文件中查找是否存在该域名的IP地址：127.0.0.1
- 808：找到tomcat服务器

> servlet缺省路径
- servlet的缺省路径是在tomcat服务器内部的一个路径。该路径对应的是一个DefaultServlet。这个缺省的Servlet的作用是用于解析web应用的静态资源文件。

> 问题URL 是如何读取文件
 - 到当前应用下的web.xm文件查找是否配。
 - 如果没有匹配，则交给tomcat的内置的default处理
 
 
 #### Servlet的生命周期（重点）
 > 引入：
 - Servlet的生命周期：servlet类对象什么时候创建，调用什么方法，什么时候销毁。
 servlet程序的生命周期由tomcat服务器控制的。
 #####  Servlet重要的生命周期
 > 构造方法：
 - 创建servelt对象的时候调用。第一次访问servlet的时候创建servlet对象。只调用1次。证明servlet对象在tomcat是单实例的
 > init方法：
 - 创建完servlet对象的时候调用。只调用一次
 > service方法：
 - 每次发出请求时调用。调用N次
 >  destroy方法：
 - 销毁servlet对象的时候调用。停止服务器或者重新部署web应用时销毁servlet对象。只调用1次
 
 ##### 伪代码演示Servlet的生命周期
 
> 通过映射找到servlet-class内容
- 字符串：gz........FirstServlet
> 通过反射构造FirstServlet对象
 - 得到字节对象：Class clazz = class.forName(gz.....FirstServlet");
 - 调用无参数的构造方法来构造对象
 clazz.newInstance();servlet的构造方法被调用
 > 创建ServletConfig对象，通过反射调用init方法
 - 得到方法对象
 Method m = clazz.getDeclareMethod("init",ServletConfig.class);
 - d调用方法
 m.invoke(obj,config);  init 方法被调用
 
 > 创建request,response对象，通过反射调用service方法
  - 得到方法对象
   clazz.getDeclareMethod("service",HttpServletRequest.class,HttpServletResponse.class)
   > 调用方法
   - m.invoke(obj,request,response);servlet的service方法被调用
   
   
   
   > 当tomcat服务器停止或web应用重新部署，通过反射调用destroy方法
   - 得到方法对象
   Method m=clazz.getDeclareMethod("destroy",null);
   - 调用方法
    m.invoke(obj,null) servlet的destroy方法被调用
    
 #####  Sevlet时序图 
 
[Servlet时序图地址](http://www.jianshu.com/p/d19613eb8a64)
 
 
##### Sevlet的自动加载

默认情况下，第一次访问servlet的时候创建servivce对象。如果servlet的构造方法或init方法中执行了比较多的逻辑代码，那么会导致用户第一次访问servlet的时候会比较慢

> 改变servlet创建对象的时间
- 提前到加载web应用的时候
- 在servlet的配置中，加上一个<load-on-startup>即可！！
注意：整数值越大，创建的优先级越低！！！

#### init方法
- 有参数的init方法 是servlet的生命周期方法，一定会被tomcat服务器调用
- 无参数的init方法，该方法是servlet的编写的初始化代码的方法
- 

##### servlet的多线程并发问题
>Servlet在tomcat的运行机制：单实例多线程
- 如果多个线程同时访问了servlet对象的共享数据（成员变量）可能会引发线程安全问题
- servlet的多线程并发问题
- 因为servlet是多线程的，所以当多个servlet的线程同时访问了servlet的共享数据吗，如果成员变量，可能会引发线程安全问题。
- 解决办法;
-  把使用到共享数据的代码块进行同步（使用synchronized关键字进行同步）
-  建议在servlet类中尽量不要使用成员变量。如果确实要使用成员，必须同步。而且尽量缩小同步代码块的方位（哪里使用了成员变量，就同步哪里！！） 以避免因为同步而导致并发效率降低。
##### Servlet学习
- HttpServletRequest：请求对象:获取请求信息
- HttpServletResponse：响应对象：设置响应对象
- ServletConfig对象： servlet配置对象
- ServletContext对象：servlet的上下文对象。


####ServletConfig对象
> 作用：主要是用于加载servlet的初始化参数。
> 对象创建和得到
- 创建时机：在创建完servlet对象之后，在调用Init方法之前创建。
- 得到对象：直接从有参数的init方法中得到！

> servlet的初始化参数



```
<init-param>
             <param-name>path</param-name>
             <param-value>e:/a.txt</param-value>
 </init-param>
```
> ServletConfig的API
- getInitParameter(String name) 根据参数名获取参数值。
- getInitParameterNames() 获取所有参数
- ServletContext getServletContext() 得到servlet上下文对象
- getServletName() 得到servlet的名称。

#### SeverletContxt

##### ServletContxt核心API
- getContextpath() 得到当前web应用的路径
- getAttribute()
- getInitParameter 得到web应用的初始化参数
- getInitParameterName()
- setAttribute() 域对象有关方法
- getRequestDispatcher 转发 类似重定向

- getRealPath 得到web应用资源文件
- getResourceAsStream
- removeAttribute

> 域对象有关的方法
- 域对象：作用是用于保存数据，获取数据。可以再不同的动态资源之间共享数据

- 方案1 ：可以通过传递参数的形式,共享数据。局限：智能传递字符串类型。
- 方案2：可以使用域对象共享数据，好处：可以共享任何类型的数据！！！
- ServletContext就是一个域对象

> 保存数据：
-  setAttribute
>  获取数据
- getAttribute
> 删除数据
- removeAttribute

##### ServletContext
- 域对象：作用整个web应用中有效



##### ServletContext
- 生命周期：当Web应用被加载进容器时创建代表整个web应用的ServletContext对象，当服务器关闭或Web应用被移除时，ServletContext对象跟着销毁。
- 作用范围：整个Web应用
> 作用：
- 在不同的Servlet之间转发
- 读取资源文件

##### HttpSession
- 在服务器中，为浏览器创建独一无二的内存空间，在其中保存会话相关信息。
- 
> 生命周期：

- 在第一次调用request.getSession()方法时，服务器会检查是否已经有对应的session,如果没有就在内存中创建一个session.当一段时间内Session没有被使用（默认为30分钟），则服务器会销毁该session.如果服务器非正常关闭，没有到期的session也会跟着销毁。
如果调用session提供的invalidate(),可以立即销毁session.

- 注意：服务器正常关闭，在启动，Session对象会进行钝化和活化操作。同时如果服务器钝化的时间在session默认销毁时间之内。则活化后session还是存在的。否则Session不存在。如果JavaBean数据在session钝化时，没有实现Serializable则当Session活化时，会消失。
- 作用范围：一次会话

##### HttpServletRequest 域对象
- 生命周期：在Service方法调用前由服务器创建，传入Service方法。整个请求结束，Request生命结束。

-作用范围:整个请求链（请求转发也存在）。
- 作用：在整个请求链中共享数据。最常用到：在Servlet中处理好的数据交给JSP显示，此时参数就可以放置在Request域中带过去。

##### PageContext域对象

- 生命周期：当对JSP的请求时开始，当响应结束时销毁。
- 作用范围：整个JSP页面。是四大作用域中最小的一个。

> 作用

- 获取其他八大隐式对象，可以认为是一个入口对象。
- 获取其所有域中的数据
- pageContext 操作所有域中属性的方法
- 跳转到其他资源 其身上提供了forward和include方法，简化重定向和转发的操作.


> 四大域和九大隐式对象
- 四大域：pageContext、servletrequest、httpsession、servletcontext.
- 九大隐式对象：
request、response、session、application、config、page、pageContext、out、exception


##### 转发
> 效果：跳转页面
Request.getRequestDispatcher.forward

##### 重定向
- 可以跳转到web应用内，或其他web应用。甚至是其他外部域名。
- 地址栏会改变，变成重定向到的的地址。
- 不能再重定向过程，把数据保存到request中。

##### 转发
- 地址栏不会改变
- 转发智能转发到当前web应用内的资源
- 可以再转发过程中，可以把数据保存到request域对象中。


> 结论：如果要使用request域对象进行数据共享，只能用转发！

##### 总结
> Servlet编程：
- Servlet生命周期（重点）






