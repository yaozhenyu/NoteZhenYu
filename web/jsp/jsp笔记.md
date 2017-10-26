
**查询条件：**

	<ul class="ul-form">
	    <li><label class="control-label">客户姓名：</label>
	        <form:input path="fullName" htmlEscape="false" maxlength="80" class="input-xlarge"/>
	    </li>
	</ul>

**下拉框：**

*固定：*
```
<form:select path="actName" class="input-medium">
  	<form:option value=" " label=" "/>
  	<form:option value="审核" label="审核"/>
  	<form:option value="线上调查" label="线上调查"/>
  	<form:option value="征信审查" label="征信审查"/>
 	<form:option value="审批" label="审批"></form:option>
 </form:select>
```

*从数据字典获取：*

	<sys:treeselect id="office" name="id" value="${office.id}" labelName="name" 
	labelValue="${office.name}" title="机构" url="/office/treeData" extId="${office.id}" cssClass=""/>

**角色权限**

	<shiro:hasAnyRoles name ="0313,0315,0317">
	<th>贷款用途</th>
	</shiro:hasAnyRoles>

**日期格式化**

	<fmt:formatDate value="${actCreditApplyProcess.beginDate}" pattern="yyyy-MM-dd"/>

24小时表示法

pattern="yyyy-MM-dd HH:mm:ss"(h要大写,小写的为12小时表示法)


**页面科学计数法正常显示为数字**

	<fmt:formatNumber value="${custHouse1.houseval}" pattern="#0.##"/>

例子表示保留两位小数

**获得sys_dict表中name值**

	${fns:getDictList('sex')}
	${fns:getDictLabel(testData.sex, 'sex', '')}


**日期控件**

*layui*

[http://www.layui.com](http://www.layui.com)

	<form:input path="c.applyDate" 
	htmlEscape="false" maxlength="20" 
	class="input-medium laydate-icon" 
	onclick="laydate({istime: false, format: 'YYYY-MM-DD',max: laydate.now()})" 
	readonly="true"/>


