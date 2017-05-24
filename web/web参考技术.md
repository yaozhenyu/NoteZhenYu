# Jeesite技术选型

## 1、后端

- 核心框架：Spring Framework 4.1
- 安全框架：Apache Shiro 1.2
- 视图框架：Spring MVC 4.1
- 服务端验证：Hibernate Validator 5.2
- 布局框架：SiteMesh 2.4
- 工作流引擎：Activiti 5.21
- 任务调度：Spring Task 4.1
- 持久层框架：MyBatis 3.2
- 数据库连接池：Alibaba Druid 1.0
- 缓存框架：Ehcache 2.6、Redis
- 日志管理：SLF4J 1.7、Log4j
- 工具类：Apache Commons、Jackson 2.2、Xstream 1.4、Dozer 5.3、POI 3.9
## 2、前端

- JS框架：jQuery 1.9。
- CSS框架：Twitter Bootstrap 2.3.1（稳定是后台，UI方面根据需求自己升级改造吧）。
- 客户端验证：JQuery Validation Plugin 1.11。
- 富文本在线编辑：CKEditor
- 在线文件管理：CKFinder
- 动态页签：Jerichotab
- 手机端框架：Jingle
- 数据表格：jqGrid
- 对话框：jQuery jBox 替代：layui
- 下拉选择框：jQuery Select2
- 树结构控件：jQuery zTree
- 日期控件： My97DatePicker 替代：layui
## 3、平台

- 服务器中间件：在Java EE 5规范（Servlet 2.5、JSP 2.1）下开发，支持应用服务器中间件 有Tomcat 6+、Jboss 7+、WebLogic 10+、WebSphere 8+。
- 数据库支持：目前仅提供MySql和Oracle数据库的支持，但不限于数据库，平台留有其它数据库支持接口， 你可以很方便的更改为其它数据库，如：SqlServer 2008、MySql 5.5、H2等
- 开发环境：Java、Eclipse Java EE 4.3、Maven 3.1、Git

