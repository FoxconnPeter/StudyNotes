### Java Web

#### 软件的结构
> C/S Client-Server 客户的-服务器端

- 典型应用QQ
- 特点：必须下载特定的客户端程序。
- 服务器端的升级，客户端升级
- 
#### B/S（Broswer-Server 浏览器与服务器端）

典型应用：网站
特点：
 - 不需要客户端（只需浏览器）
 - 服务器端升级，客户端不用升级
 
JavaWeb的程序就是B/S

> 服务器

web服务器软件作用：把本地的资源共享给外部访问。

> 网络通讯的基础
  
  通过ip和端口发送二进制数据
  
> 常见的市面上web服务软件

WebLogic:BEA
WebSphere :IBM
JBoss:RedHat
Tomcat:Apache   免费

#### 静态资源和动态资源的区别
静态资源：当用户多次访问这个资源，资源的源代码永远不会改变
动态:与之相反

> Servlet:用java语言来编写动态资源的开发技术

- 特点 ： 普通的java类，继承HttpServlet类
- Servlet类只有服务器调用。