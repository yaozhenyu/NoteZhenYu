jsp的三种自定义标签

**1、自定义方法标签**

引入方式示例：

	<%@ taglib prefix="fns" uri="/WEB-INF/tlds/fns.tld" %>
 
写法示例：

```
<?xml version="1.0" encoding="UTF-8" ?>

<taglib xmlns="http://java.sun.com/xml/ns/j2ee"
  xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xsi:schemaLocation="http://java.sun.com/xml/ns/j2ee http://java.sun.com/xml/ns/j2ee/web-jsptaglibrary_2_0.xsd"
  version="2.0">
    
  <description>JSTL 1.1 functions library</description>
  <display-name>JSTL functions sys</display-name>
  <tlib-version>1.1</tlib-version>
  <short-name>fns</short-name>
  <uri>http://java.sun.com/jsp/jstl/functionss</uri>

  <!-- DictUtils -->
  
  <function>
    <description>获取字典对象列表</description>
    <name>getDictList</name>
    <function-class>com.sdyy.base.sys.utils.DictUtils</function-class>
    <function-signature>java.util.List getDictList(java.lang.String)</function-signature>
    <example>${fns:getDictList(typeCode)}</example>  
  </function>
  
  <function>
    <description>获取字典对象列表</description>
    <name>getDictListJson</name>
    <function-class>com.sdyy.base.sys.utils.DictUtils</function-class>
    <function-signature>java.lang.String getDictListJson(java.lang.String)</function-signature>
    <example>${fns:getDictListJson(typeCode)}</example>  
  </function>
  
  
  <function>
    <description>对象变json</description>
    <name>toJSONString</name>
    <function-class>com.alibaba.fastjson.JSON</function-class>
    <function-signature>java.lang.String toJSONString(java.lang.Object)</function-signature>
  </function>
</taglib>
```

function-class就是该方法的实体所在类路径，
function-signature就是该方法的方法名，值得一提的是，这个方法必须是个static方法。
example就是使用方法示例


**2、通过jsp自定义标签(虽然里面是jsp的写法，但是扩展名要命名为.tag )**
```
<%@ taglib prefix="sys" tagdir="/WEB-INF/tags/sys" %>
```
如此 tags目录下的sys目录下的jsp(虽然里面是jsp的写法，但是扩展名要命名为.tag）形式的tag就会自动被加载，标签jsp写法示例：

```
<%@ tag language="java" pageEncoding="UTF-8"%>
<%@ taglib uri="http://java.sun.com/jsp/jstl/core" prefix="c"%>
<%@ taglib prefix="fns" uri="/WEB-INF/tlds/fns.tld" %>
<%@ attribute name="typeCode" type="java.lang.String" required="true" description="字典code"%>
<%@ attribute name="defaultValue" type="java.lang.String" description="默认选中"%>
<%@ attribute name="style" type="java.lang.String" description="默认选中"%>
<%@ attribute name="cls" type="java.lang.String" description="默认选中"%>
<%@ attribute name="name" type="java.lang.String" description="默认选中"%>
<select style="${style}" class="${cls}" name="${name}" id="${name}" >
    <option value="" >请选择...      </option>
    <c:if test="${not empty typeCode}">
        <c:forEach items="${fns:getDictList(typeCode)}" var='dict'>
            <option value='${dict.VALUE}' ${defaultValue==dict.VALUE?'selected':''}>${dict.TEXT}</option>
        </c:forEach>
    </c:if>
</select>
```

如此，jsp名就是标签名，例如这个jsp叫 select.tag（扩展名要命名为.tag），那么它的用法就是
    
    <sys:select cls="formselect" name="MODULE_TYPE" typeCode="HOME_MODULE_TYPE" defaultValue="${record.MODULE_TYPE }" />
     

 

**3、通过tld文件和java代码自定义元素标签**

引入方式和1相同，写法示例如下：

```
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE taglib PUBLIC "-//Sun Microsystems, Inc.//DTD JSP Tag Library 1.2//EN"   
    "http://java.sun.com/dtd/web-jsptaglibrary_1_2.dtd">
<taglib>
    <tlib-version>1.0</tlib-version>
    <jsp-version>2.0</jsp-version>
    <short-name>bgt</short-name>
    <!-- backGroundTag -->
    <uri>http://www.sdyy.tag</uri>
    <tag>
        <name>hasUrlPerm</name>
        <tag-class>com.sdyy.common.tags.HasUrlPermissionTag</tag-class>
        <attribute>
            <name>link</name>
            <required>false</required>
            <rtexprvalue>true</rtexprvalue><!-- 是否支持恶劣表达式 -->
            <type>java.lang.String</type>
            <description>示例：acApplication/forMain.do</description>
        </attribute>
        
    </tag>
</taglib>
```

+ A、【判断标签】HasUrlPermissionTag标签是一个判断标签，通过该标签来决定标签包裹的内容是否显示，写法如下：

```
package com.sdyy.common.tags;

import javax.servlet.http.HttpServletRequest;
import javax.servlet.jsp.JspException;
import javax.servlet.jsp.tagext.BodyTagSupport;

import com.sdyy.common.spring.interceptor.PermissionInterceptor;
/**
 * 
 * @ClassName: HasUrlPermissionTag
 * @Description: 根据url判断权限标签
 * @author: liuyx
 * @date: 2015年12月21日上午11:15:32
 */
public class HasUrlPermissionTag extends BodyTagSupport {

    private String link;//  acApplication/forMain.do

    @Override
    public int doStartTag() throws JspException { // 在标签开始处出发该方法
        
        HttpServletRequest request=(HttpServletRequest) pageContext.getRequest();
        //获取session中存放的权限
        
        //判断是否有权限访问
        if (PermissionInterceptor.isOperCanAccess(request, link)) {
            //允许访问标签body
            return BodyTagSupport.EVAL_BODY_INCLUDE;// 返回此则执行标签body中内容，SKIP_BODY则不执行
        } else {
            return BodyTagSupport.SKIP_BODY;
        }

    }

    @Override
    public int doEndTag() throws JspException {
        return BodyTagSupport.EVAL_BODY_INCLUDE;
    }

    public String getLink() {
        return link;
    }

    public void setLink(String link) {
        this.link = link;
    }

}
```


在JSP中的使用方式：

    <bgt:hasUrlPerm link="abc.do">
    <div>tttttttttttttttttest</div>
    </bgt:hasUrlPerm>
+ B、【控件标签】，这种标签直接返回一个控件，不过是通过java代码生成的控件内容，写法示例：

```
package com.sdyy.common.tags;

import java.io.IOException;
import java.util.ArrayList;
import java.util.List;

import javax.servlet.jsp.JspTagException;
import javax.servlet.jsp.JspWriter;
import javax.servlet.jsp.tagext.BodyTagSupport;

/*import com.sdyy.base.ac.ac_permission.model.AcPermission;*/

public class ButtonUrlTag extends BodyTagSupport {
    private static final long serialVersionUID = -7811902545513255473L; 


    //标签属性用户名
    private String user = null;
    //标签属性操作url
    private String url = null;
    //标签属性 js方法
    private String jsmethod = null;
    //标签属性image 按钮图片
    private String image = null;
    //标签属性 alt 提示
    private String alt = null;
    //标签属性操作value 按钮文本
    private String value  = null;




    /* 标签初始方法 */
    public int doStartTag() throws JspTagException{
        return super.EVAL_BODY_INCLUDE;
    }


    /* 标签结束方法 */
    public int doEndTag() throws JspTagException{
        pageContext.getSession();
        Boolean b = false;
        List list = new ArrayList();
        /*AcPermission p = new AcPermission();*/
        /*JDBCHibernate jdbca = new JDBCHibernate();*/
        try {
            /*list = jdbca.getHaveURLByUsernameList(user);*/
        } catch (Exception e1) {
            // TODO Auto-generated catch block
            e1.printStackTrace();
        }
        for(int i = 0;i < list.size(); i++){
            /*p = (AcPermission) list.get(i);*/
            if(1==1) {//p.getUrl().trim().equals(url.trim())){
                b = true;
                //如果jsmethod属性不为null 则把超链接href改为调用js
                if(jsmethod!=null){
                    url = jsmethod;
                }
            }
        }
        JspWriter out = pageContext.getOut();
        if(b){
            try {
                //有权限 显示操作按钮
                out.println("<a href='" +url+ "' class='regular'><img src='" + image + "' alt='" + alt +"' />" + value + "</a>");
            } catch (IOException e) {
                e.printStackTrace();
            }
        }

        return super.SKIP_BODY;
    }

    /* 释放资源 */
    public void release(){
        super.release();
    }


    public String getUser() {
        return user;
    }


    public void setUser(String user) {
        this.user = user;
    }


    public String getUrl() {
        return url;
    }


    public void setUrl(String url) {
        this.url = url;
    }


    public String getImage() {
        return image;
    }


    public void setImage(String image) {
        this.image = image;
    }


    public String getAlt() {
        return alt;
    }


    public void setAlt(String alt) {
        this.alt = alt;
    }


    public String getValue() {
        return value;
    }


    public void setValue(String value) {
        this.value = value;
    }

    public String getJsmethod() {
        return jsmethod;
    }


    public void setJsmethod(String jsmethod) {
        this.jsmethod = jsmethod;
    }
}
```