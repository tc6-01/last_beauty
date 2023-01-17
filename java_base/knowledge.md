# java基础知识记录

## java web

### servlet接口

TomCat服务器创建后通过访问对应的URL地址，实现Servlet接口

#### 生命周期

加载servlet类---》》》构造器创建Servlet对象---》》》init初始化Servlet对象---》》》访问URL时调用service方法---》》》销毁servlet方法

#### 部署

通过web.xml添加`<servlet>`标签来进行添加对应的服务类，另外可以添加`servlet-mapping`标签来实现URL映射。

```xml
	<servlet>
        <servlet-name>helloResponse</servlet-name>
        <servlet-class>com.BeerAn.response.helloResponse</servlet-class>
    </servlet>
    <servlet-mapping>
        <servlet-name>helloResponse</servlet-name>
        <url-pattern>/response00</url-pattern>
    </servlet-mapping>
```

#### HttpServletRequest

##### 请求转发

通过一次访问将数据转发给两个资源。类似于流水线业务办理。

浏览器通过访问第一个资源的URL，通过HTTP协议将数据进行传输，第一个资源加载完成后，通过response对象设置域对象，增加类似于签名。然后通过`forward`方法将response和request对象传给第二个资源的URL，然后先获取对象，然后通过检查第一个资源设置的域对象值，进行自己的逻辑。

> base标签：
>
> 因为可以通过转发访问新页面，但是在返回过程中会因为相对路径的参照路径而发生错误，因此可以设置base标签来固定相对路径的参照路径，来使得返回也可以正常进行。

#### HttpServletResponse 

##### 两个响应流

在Response对象中，有两个输出流，一个是字节流`getWriter`,另外一个是字符流`getOutputStream`,这两个流的效果一致，但是不能重复用。

- 向客户端发送数据
- 发送中文数据乱码问题
  - 设置服务器编码为`UTF-8`
  - 设置HTTP响应头确定浏览器使用的编码为`UTF-8`
  - 或者直接设置服务器和响应头的编码方式为`UTF-8`
- 请求重定向
  - 与请求转发较为不同，需要改变`Status & Header`
  - 不能重定向为内部资源，可重定向为外部资源
  - 会改变访问地址
  - 等价于浏览器两次访问服务器
  - 简便写法为`setResender`,默认设置302头，只需要设置location即可

### jsp（Java Service Pages）

访问地址与html页面一致

在第一次访问时，会被Tomcat服务器翻译成字节码，并被编译成对应的类。这个被翻译的类汇**间接继承(HttpJspBase直接继承Servlet类)**Servlet类，jsp就是通过封装将写好的页面（jsp）进行模拟生成动态html（翻译后的类中写的也是用字符流写入html）

#### 几类属性

1. page===》》》页面属性`<%page%>`
2. 声明===》》》Java Code `<%!  %>`极少使用
3. 表达式===》》》输出内容`<%= %>`
4. 代码===》》》Java Statements `<% JAVA CODE%>`控制台输出内容，提供HttpServeletRespose &HttpServeletRequest  

几类属性可放在一起组成脚本

#### 注释类型

1. html注释  `<!-->`
2. java注释`<% // 单行注释%>`
3. jsp注释 `<%-- --%>`可以是所有jsp代码失效

#### 九大 内置对象 

#### 四大域对象

存取数据对象，数据存取范围不同 

注意：out与response.getWriter().write的区别

out.print与out.write的区别

#### jsp的几个常用标签

- 静态包含--->>>网页底部的信息（不变的内容）
- 动态包含--->>>通过调用代码实现jsp页面执行输出，另外还可以调用参数
- 标签-转发--->>>简化servlet程序的转发需求

服务端获取请求参数并使用jsp获取sql信息 ，将获取的数据存入request对象中，通过转发发送至新的jsp中返回页面对象

#### Listener监听器

JavaWeb的三大组件之一：Servlet程序，Filter过滤器，Listener监听器

监听某种事物的变化，通过回调函数来调用对应逻辑

- ServletContext Listener监听器

#### EL（Expression Language）表达式

 EL表达式代替JSP中的表达式脚本，语法更简洁，对于用户更友好

> 语法形式 `${ }`

jsp主要输出域对象的值，输出域顺序为从小到大 

EL表达式底层并未直接获取对象的属性值，而是调用其get方法

- 运算（逻辑运算，算术运算，empty运算，三元运算，点运算---Bean对象中的属性值，中括号运算---属性胡有序集合中某个元素的值或者map中某个包含特殊字符的key的属性）
- 11个隐含对象
  - 通过隐含对象获取四个域中的属性值---->>>>解决获取不同域中的同一key的属性
  - pageContext获取jsp的九大内置对象，并通过点运算直接输出属性值
  - param和paramValues获取请求参数
  - header和headerValues获取请求头信息
  - cookie获取Cookie信息
  - initParam获取web.xml中的`<context-param>`属性值



#### JSTL标签库

### 文件上传与下载

###  Cookie

服务器创建`Cookie`对象--->>>通知客户端保存Cookie--->>>（通过响应头set-Cookie通知客户端保存Cookie）客户端通过响应头设置Cookie，有就修改，没有就保存。

Cookie可以设置多个Cookie。

Cookie生成在服务器端，保存在客户端，服务器端可再次获取

#### Cookie生命控制

确认Cookie什么时候被销毁

> setMaxAge() 正数表示存活时间，负数表示Session--关闭浏览器就删除

#### 设置有效路径Path



#### 用户名免登录---Cookie实现

通过设置Servlet程序中登录成功逻辑将Username添加到Cookie中，并在JSP页面中设置默认username的value值。由于Cookie保存在浏览器中，再次访问时可以直接获取username（使用EL表达式）

### session会话

创建Session和获取--->>>getSession第一次调用创建Session，之后都是获取创建好的SessionID，isNew判断是不是最新创建出来的 

每个Session都有一个唯一的ID，通过getID获取

Session同样作为域可以存取数据

Session也可以设置存活时间，默认存活时间为30min存活时间是指两次浏览器请求间隔，另外存活时间不能设置为0，可以通过调用`invalidAPI`进行销毁

浏览器和Session之间的关联

1. 在没有Cookie情况下发送请求给服务器
2. 服务器将会创建一个会话对象，并同时创建一个Cookie对象，此Cookie对象的Key是JSESSIONID 
3. 浏览器收到数据，就马上创建Cookie
4. 浏览器再次访问服务器时带着Cookie发送
5. 服务器会根据Cookie中的SessionId值查找内存中的Session并返回 
6. 关闭浏览器后默认为Session类型的Cookie会被自动删除，**此时再次请求将会重复上述步骤。也就到达了即便设置session超时时间在关闭浏览器之后依旧会失效。**

## javaEE三层架构

### 客户端（浏览器）

前端技术栈：

Html+CSS+JavaScript

### web层

- 获取请求参数
- 调用业务层处理业务
- 响应数据给客户端，请求转发，重定向

#### 技术栈

- Servlet程序
- WebWork
- SpringMVC

### Service业务层

- 处理业务逻辑
- 调用持久层保存到数据库

#### 技术栈

- SPRING框架

### Dao持久层

- 负责与数据库进行交互---baseCRUD

#### 技术栈

- Mybaits\MyBatis Plus
- JDBC
- JPA
- DbUnits



