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