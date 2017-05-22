# [ decorator（HTML装饰器）                    ](http://blog.csdn.net/jzh440/article/details/7770013)                                                                            

> 1:每当遇到一个新的技术，首先我会问自己，这个技术是做神马的？用这个技术有神马好处？相比其它方式他的优势在哪里？我该怎样实现这个技术？
>  首先这个Decorator解释一下这个单词：“装饰器”，我觉得其实可以这样理解，他就像我们用到的Frame，他把每个页面都有的东东提炼了出来，也可能我们也会用各种各样的include标签，将我们的常用页面给包括进来：比如说页面的top,bottom这些每个页面几乎都有，而且都一样，如果我们在每个页面都include,可以发现我们的程序是多吗的冗余，重复。相比之下装饰器给我们提供了一个较好的选择，他在你要显示的页面根本看不出任何include信息，可以说完全解耦。> 2:decorator的原理：

     sitemesh应用Decorator模式，用filter截取request和response,把页面组件head,content,banner、bottom结合为一个完整的视图。通常我们都是用include标签在每个jsp页面中来不断的包含各种header, stylesheet, scripts and footer.

> decorator的实现

     首先我们[http://www.opensymphony.com/sitemesh/](http://www.opensymphony.com/sitemesh/)下载我们需要的jar包：sitemesh-2.4.jar

    在我们的web.xml中配置如下信息：

```&lt;filter&gt;   
<filter-name>sitemesh</filter-name>   
	<filter-class>com.opensymphony.module.sitemesh.filter.PageFilter</filter-class>   
	</filter>   
	<filter-mapping>   
	<filter-name>sitemesh</filter-name>   
	<url-pattern>/*</url-pattern>   
</filter-mapping>  
```

    在WEB-INF目录下新建一个decorators.xml文件（/decorator是你的包装jsp根路径在这里main.jsp和panel.jsp都是包装jsp,a.jsp，b,jsp是被包装jsp）

defaultdir: 包含装饰器页面的目录

page : 页面文件名

name : 别名

role : 角色，用于安全

webapp : 可以另外指定此文件存放目录

Patterns : 匹配的路径，可以用*,那些被访问的页面需要被装饰。


    <?xml version="1.0" encoding="utf-8" ?>   
    <decorators defaultdir="/decorator">  
    <decorator name="main" page="main.jsp">  
    <pattern>/page/a.jsp</pattern>   
    <pattern>/page/b.jsp</pattern>  
    </decorator>  
    <decorator name="panel" page="panel.jsp"></decorator>  
    </decorators>  


 建立我们的包装jsp在WEBROOT->decorator下面：这里有两个分别是main.jsp和panel.jsp
panel.jsp

插入原始页面(被包装页面)的head标签中的内容(不包括head标签本身)。

插入原始页面(被包装页面)的body标签中的内容。

插入原始页面(被包装页面)的title标签中的内容，还可以添加一个缺省值。

下面介绍一下<page:applyDecorator name="  " page=" ">

其实这里是一样的name指的是我们要用的包装器名字也就是在decorator.xml中定义好的，page指的是被包装页面。

还有就是<decorator:getProperty property="" [default=""][writeEntireProperty=""]/>

插入原始页面的property属性指定的值同名的属性。

property:指定那个属性将要被插入

default:如果没有发现指定的属性，则插入此值

writeEntireProperty:表示是否将（空格 属性名=“属性值”）整个插入，允许时的值是true或yes或1

例如下面例子中的：当你访问a.jsp时，焦点会定在text上。



1. <%@ page language="java" import="java.util.*" pageEncoding="utf-8"%>  
2. <%@ taglib uri="http://www.opensymphony.com/sitemesh/decorator" prefix="decorator" %>  
3. <!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN">  
4. <html>  
5. <head>  
6. <title>  
7. <decorator:title default="默认包装器。。。"/>  
8. </title>  
9. <decorator:head/>  
10. </head>  
11. <body>  
12. <hr width="100" color="red"/>  
13. <decorator:body/>  
14. <hr width="100" color="blue"/>  
15. </body>  
16. </html>  



main.jsp

1.    ​
2.    <%@ page language="java" import="java.util.*" pageEncoding="utf-8"%>  
3.    <%@ taglib uri="<a href="http://www.opensymphony.com/sitemesh/page">http://www.opensymphony.com/sitemesh/page</a>" prefix="page"%>  
4.    <%@ taglib uri="<a href="http://www.opensymphony.com/sitemesh/decorator">http://www.opensymphony.com/sitemesh/decorator</a>" prefix="decorator" %>  
5.    <!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN">  
6.    <html>  
7.    <head>   
8.    <title><decorator:title default="装饰器页面..." /></title>   
9.    <decorator:head />   
10.    </head>   
11.    <body bgcolor="gray"<decorator:getProperty property="body.onload" writeEntireProperty="true" />>  
12.    ​
13.    <page:applyDecorator page="/common/top.jsp" name="panel"/>  
14.    <div align="center">  
15.    <p><font color="red">this is style's header</font></p>   
16.    <decorator:body/>  
17.    <p><font color="red">this is style's footer</font></p>   
18.    </div>  
19.    <page:applyDecorator page="/common/bottom.jsp" name="panel"/>  
20.    </body>   
21.    </html>  

**[html]** [view plain](http://blog.csdn.net/jzh440/article/details/7770013#) [copy](http://blog.csdn.net/jzh440/article/details/7770013#) [print](http://blog.csdn.net/jzh440/article/details/7770013#)[?](http://blog.csdn.net/jzh440/article/details/7770013#)

1. a.jsp  

**[html]** [view plain](http://blog.csdn.net/jzh440/article/details/7770013#) [copy](http://blog.csdn.net/jzh440/article/details/7770013#) [print](http://blog.csdn.net/jzh440/article/details/7770013#)[?](http://blog.csdn.net/jzh440/article/details/7770013#)

1. <%@ page language="java" import="java.util.*" pageEncoding="ISO-8859-1"%>  
2. <!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN">  
3. <html>  
4. <head>  
5. <title>My JSP 'a.jsp' starting page</title>  
6. </head>  
7. <body onload="document.someform.a.focus();">  
8. <form name="someform">  
9. <font color="red">this is my JSP page. </font><br>  
10. <input type="text" id="a"/>  
11. </form>  
12. </body>  
13. </html>  
```

```