#### HTTP协议：
- HTTP协议： 对浏览器客户端和服务器端之间数据传输的格式规范。
- HTTP请求：浏览器>服务器
- 格式：请求方式GET/POST,请求资源URL，HTTP协议版本1.1
- 请求头：键值对形式存在.host user-agent refere
- 实体内容（Post提交的参数）
- HttpServletRequest 对象：请求对象。获取请求信息.
- request.getMethod() request.getRequestURI/getRequestURL() request.getProtocol();
- 请求头： request.getHeader("name")  request.getHeaderNames()
- 实体内容：request.getInputStream();
> 获取参数数据:
- request.getParameter("name") 只能获取一个值的参数
- request.getParameterValues("name");多个值的参数
- request.getParameterNames() 所有参数

> HTTP响应 ：服务器->浏览器端
##### 格式：响应行
- 响应行 常用状态么
- 响应头 location 结合302状态码完成请求重定向功能、refresh (定时刷新)、content-type、content-disiposition(以下载的方式打开)

- HttpServletResponse 对象：响应对象。设置响应信息。
响应行：response.setStatus();
响应头：response.setHeader("name","value")
实体内容:
(PrintWriter) response.getWriter().writer();字符内容
(OutputStream) response.getOutputStream().writer;字节内容.
