#### JSON.parse 函数 (JavaScript)

 
将 JavaScript 对象表示法 (JSON) 字符串转换为对象。
语法```JSON.parse(text [, reviver])```

* 参数text必需,一个有效的 JSON 字符串。
* reviver可选,一个转换结果的函数。将为对象的每个成员调用此函数。如果成员包含嵌套对象，则先于父对象转换嵌套对象。对于每个成员，会发生以下情况：
* 如果 reviver 返回一个有效值，则成员值将替换为转换后的值。
* 如果 reviver 返回它接收的相同值，则不修改成员值。
* 如果 reviver 返回 null 或 undefined，则删除成员。
* 返回值
一个对象或数组。
* 异常 如果此函数引发 JavaScript 分析器错误（如“SCRIPT1014：无效字符”），则输入文本将不遵循 JSON 语法。若要更正此错误，请执行下列操作之一：
修改 text 参数以遵循 JSON 语法。有关更多信息，请参见 JSON 对象的 BNF 语法表示法。
例如，如果响应的格式为 JSONP 而非纯 JSON，请在响应对象上尝试此代码：

		JavaScript
		var fixedResponse = response.responseText.replace(/\\'/g, "'");

确保通过 JSON 兼容的实现（如 JSON.stringify）对 text 参数进行序列化。
在 JSON 验证程序（如 JSLint）中运行 text 参数以帮助找到语法错误。
Exception	Condition
以下示例使用 JSON.parse 将 JSON 字符串转换成对象。

	JavaScript
	var jsontext = '{"firstname":"Jesper","surname":"Aaberg","phone":["555-0100","555-0120"]}';
	var contact = JSON.parse(jsontext);
	document.write(contact.surname + ", " + contact.firstname);

// Output: Aaberg, Jesper
以下示例演示了如何使用 JSON.stringify 将数组转换成 JSON 字符串，然后使用 JSON.parse 将该字符串重新转换成数组。
	
	JavaScript
	var arr = ["a", "b", "c"];
	var str = JSON.stringify(arr);
	document.write(str);
	document.write ("<br/>");
	
	var newArr = JSON.parse(str);
	
	while (newArr.length > 0) {
	    document.write(newArr.pop() + "<br/>");
	}

	
	// Output:
	// ["a","b","c"]
	// c
	// b
	// a

reviver 函数通常用于将国际标准化组织 (ISO) 日期字符串的 JSON 表示形式转换为协调世界时 (UTC) 格式 Date 对象。此示例使用 JSON.parse 来反序列化 ISO 格式的日期字符串。 dateReviver 函数为格式为 ISO 日期字符串的成员返回 Date 对象。
	
	JavaScript
	var jsontext = '{ "hiredate": "2008-01-01T12:00:00Z", "birthdate": "2008-12-25T12:00:00Z" }';
	var dates = JSON.parse(jsontext, dateReviver);
	document.write(dates.birthdate.toUTCString());
	
	function dateReviver(key, value) {
	    var a;
	    if (typeof value === 'string') {
	        a = /^(\d{4})-(\d{2})-(\d{2})T(\d{2}):(\d{2}):(\d{2}(?:\.\d*)?)Z$/.exec(value);
	        if (a) {
	            return new Date(Date.UTC(+a[1], +a[2] - 1, +a[3], +a[4],
	                            +a[5], +a[6]));
	        }
	    }
	    return value;
	};
	
	// Output:
	// Thu, 25 Dec 2008 12:00:00 UTC


-------------------------------------------------------------------------------------------------------------------------------------------

JSON.stringify 函数 (JavaScript)

 
将 JavaScript 值转换为 JavaScript 对象表示法 (Json) 字符串。
语法

JSON.stringify(
value [, replacer] [, space])
参数
value
必需。要转换的 JavaScript 值（通常为对象或数组）。
replacer
可选。用于转换结果的函数或数组。
如果 replacer 为函数，则 JSON.stringify 将调用该函数，并传入每个成员的键和值。使用返回值而不是原始值。如果此函数返回 undefined，则排除成员。根对象的键是一个空字符串：""。
如果 replacer 是一个数组，则仅转换该数组中具有键值的成员。成员的转换顺序与键在数组中的顺序一样。当 value 参数也为数组时，将忽略 replacer 数组。
space
可选。向返回值 JSON 文本添加缩进、空格和换行符以使其更易于读取。
如果省略 space，则将生成返回值文本，而没有任何额外空格。
如果 space 是一个数字，则返回值文本在每个级别缩进指定数目的空格。如果 space 大于 10，则文本缩进 10 个空格。
如果 space 是一个非空字符串（例如“\t”），则返回值文本在每个级别中缩进字符串中的字符。
如果 space 是长度大于 10 个字符的字符串，则使用前 10 个字符。
返回值
一个包含 JSON 文本的字符串。
异常
Exception   Condition
例外
条件
替换器参数无效
replacer 参数不是函数或数组。
值参数中不支持循环引用
value 参数包含循环引用。
备注
如果 value 具有 toJSON 方法，则 JSON.stringify 函数将使用该方法的返回值。如果 toJSON 方法的返回值为 undefined，则不转换成员。这使对象能够确定自己的 JSON 表示形式。
将不会转换不具有 JSON 表示形式的值，例如 undefined。在对象中，将丢弃这些值。在数组中，会将这些值替换为 null。
字符串值以引号开始和结束。所有 Unicode 字符可括在引号中，但必须使用反斜杠进行转义的字符除外。以下字符的前面必须是反斜杠：
引号 (")
反斜杠 (\)
退格键 (b)
换页符 (f)
换行符 (n)
回车符 (r)
水平制表符 (t)
四个十六进制数字 (uhhhh)
执行顺序
在序列化过程中，如果 value 参数对应有 toJSON 方法，则 JSON.stringify 将首先调用 toJSON 方法。如果该方法不存在，则使用原始值。接下来，如果提供 replacer 参数，则该值（原始值或 toJSON 返回值）将替换为 replacer 参数的返回值。最后，根据可选 space 参数向该值添加空格以生成最终的 JSON 文本。
此示例使用 JSON.stringify 将 contact 对象转换为 JSON 文本。定义 memberfilter 数组以便只转换 surname 和 phone 成员。省略 firstname 成员。
JavaScript
var contact = new Object();
contact.firstname = "Jesper";
contact.surname = "Aaberg";
contact.phone = ["555-0100", "555-0120"];

var memberfilter = new Array();
memberfilter[0] = "surname";
memberfilter[1] = "phone";
var jsonText = JSON.stringify(contact, memberfilter, "\t");
document.write(jsonText);
// Output:
// { "surname": "Aaberg", "phone": [ "555-0100", "555-0120" ] }
此示例将 JSON.stringify 与一个数组一起使用。 replaceToUpper 函数将数组中的每个字符串转换为大写形式。
JavaScript
var continents = new Array();
continents[0] = "Europe";
continents[1] = "Asia";
continents[2] = "Australia";
continents[3] = "Antarctica";
continents[4] = "North America";
continents[5] = "South America";
continents[6] = "Africa";

var jsonText = JSON.stringify(continents, replaceToUpper);

function replaceToUpper(key, value) {
    return value.toString().toUpperCase();
}

//Output:
// "EUROPE,ASIA,AUSTRALIA,ANTARCTICA,NORTH AMERICA,SOUTH AMERICA,AFRICA"

此示例使用 toJSON 方法将字符串值转换为大写形式。
JavaScript
var contact = new Object();
contact.firstname = "Jesper";
contact.surname = "Aaberg";
contact.phone = ["555-0100", "555-0120"];

contact.toJSON = function(key)
 {
    var replacement = new Object();
    for (var val in this)
    {
        if (typeof (this[val]) === 'string')
            replacement[val] = this[val].toUpperCase();
        else
            replacement[val] = this[val]
    }
    return replacement;
};

var jsonText = JSON.stringify(contact);
document.write(jsonText);

// Output:
{"firstname":"JESPER","surname":"AABERG","phone":["555-0100","555-0120"]}



'{"firstname":"JESPER","surname":"AABERG","phone":["555-0100","555-0120"]}'
*/



========================================================================================================================================
toJSON 方法 (Date) (JavaScript)

 
由 JSON.stringify 方法用于允许转换某个对象的数据以进行 JavaScript Object Notation (JSON) 序列化。
语法

objectname.toJSON()
参数
objectname
必需。需要进行 JSON 序列化的对象。
备注
toJSON 方法由 JSON.stringify 函数使用。 JSON.stringify 将 JavaScript 值序列化为 JSON 文本。如果向 JSON.stringify 提供了 toJSON 方法，则在调用 JSON.stringify 时将调用 toJSON 方法。
toJSON 方法是 Date JavaScript 对象的内置成员。它返回 UTC 时区的 ISO 格式日期字符串（由后缀 Z 表示）。
可重写 Date 类型的 toJSON 方法，也可为其他对象类型定义 toJSON 方法，以实现在 JSON 序列化之前对特定对象类型进行数据转换。
以下示例使用 toJSON 方法将大写的字符串成员值序列化。在调用 JSON.stringify 时调用 toJSON 方法。
JavaScript
var contact = new Object();
contact.firstname = "Jesper";
contact.surname = "Aaberg";
contact.phone = ["555-0100", "555-0120"];

contact.toJSON = function(key)
 {
    var replacement = new Object();
    for (var val in this)
    {
        if (typeof (this[val]) === 'string')
            replacement[val] = this[val].toUpperCase();
        else
            replacement[val] = this[val]
    }
    return replacement;
};

var jsonText = JSON.stringify(contact);

/* The value of jsonText is:
'{"firstname":"JESPER","surname":"AABERG","phone":["555-0100","555-0120"]}'
*/
以下示例演示如何使用作为 Date 对象的内置成员的 toJSON 方法。
JavaScript
var dt = new Date('8/24/2009');
dt.setUTCHours(7, 30, 0);
var jsonText = JSON.stringify(dt);

/* The value of jsonText is:
'"2009-08-24T07:30:00Z"'
*/

****************************************************************************************************************************
JSON 对象 (JavaScript)

 
一个内部对象，提供用于在 JavaScript 值和 JavaScript 对象表示法 (JSON) 格式之间来回转换的函数。 JSON.stringify 函数可将 JavaScript 值序列化为 JSON 文本。 JSON.parse 函数可反序列化 JSON 文本以生成 JavaScript 值。
语法

JSON.[method]

参数
Method
必需。 JSON 对象的某一个方法的名称。
备注
你无法使用 new 运算符创建 JSON 对象。如果尝试执行此操作，则将出错。但是，你可以重写或修改 JSON 对象。
加载脚本引擎时，该引擎将创建 JSON 对象。其方法始终对你的脚本可用。
若要使用内部 JSON 对象，请确保不使用脚本中定义的另一个 JSON 对象来重写该对象。你可能需要修改将检测 JSON 对象的状态的现有脚本语句，因为这些语句将以不同的方式计算。下面的示例说明了这一点。
JavaScript
if (!this.JSON) {
    // JSON object does not exist.
    }