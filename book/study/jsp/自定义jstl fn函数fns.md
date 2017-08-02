**1、引入函数声明：**

jsp页面需要引入自定义fns函数声明：`<%@ taglib prefix="fns" uri="/WEB-INF/tlds/fns.tld" %>`，自定义的tld文件位于`/WEB-INF/tlds/fns.tld`

一般需要C标签配合使用，同时引入C标签声明：`<%@ taglib prefix="c" uri="http://java.sun.com/jsp/jstl/core" %>`


**2、fns.tld代码，仿照jstl的fn函数fn.tld的书写格式：**

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

      <function>
        <description>项目访问地址</description>
        <name>getProjectUrl</name>
        <function-class>common.config.Global</function-class>
        <function-signature>java.lang.String getProjectUrl()</function-signature>
        <example>${fns:getProjectUrl()}</example>
      </function>
      <function>
        <description>获取配置</description>
        <name>getConfig</name>
        <function-class>common.config.Global</function-class>
        <function-signature>java.lang.String getConfig(java.lang.String)</function-signature>
        <example>${fns:getConfig(key)}</example>
      </function>

    </taglib>

**3、为表达式函数(标签函数)提供后台服务类：common.config.Global:**

	/**
     * fns表达式函数服务类
     */
    public class Global {

        /**
         * 当前对象实例
         */
        private static Global global = new Global();

        /**
         * 保存全局属性值
         */
        private static Map<String, String> map = Maps.newHashMap();

        /**
         * 属性文件加载对象
         */
        private static PropertiesLoader loader = new PropertiesLoader("pro.properties");

        /**
         * 是/否
         */
        public static final Integer YES = 1;
        public static final Integer NO = 0;

        /**
         * 对/错
         */
        public static final String TRUE = "true";
        public static final String FALSE = "false";

        /**
         * 获取当前对象实例
         */
        public static Global getInstance() {
            return global;
        }

        /**
         * 获取配置
         * @see ${fns:getConfig('project.url')}
         */
        public static String getConfig(String key) {
            String value = map.get(key);
            return value;
        }

        /**
         * 访问地址
         */
        public static String getProjectUrl() {
            return getConfig("project.url");
        }
    }


**4、在jsp页面或js中使用自定义的表达式函数fns：**

	<script type="text/javascript">
            top.location.href='${fns:getProjectUrl()}/index.jsp';
    </script>