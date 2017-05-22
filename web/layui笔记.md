[基础参数](undefined)

我们提到的基础参数主要指调用方法时用到的配置项，如：*layer.open({content: ''})*,*layer.msg('', {time: 3})*等，其中的content和time即是基础参数，以键值形式存在，基础参数*可合理应用于任何层类型中*，您不需要所有都去配置，大多数都是可选的。而其中的layer.open、layer.msg就是内置方法。注意，从2.3开始，无需通过layer.config来加载拓展模块



[title](undefined) - 标题

类型：String/Array/Boolean，默认：'信息'

title支持三种类型的值，若你传入的是普通的字符串，如*title :'我是标题'*，那么只会改变标题文本；若你还需要自定义标题区域样式，那么你可以*title: ['文本', 'font-size:18px;']*，数组第二项可以写任意css样式；如果你不想显示标题栏，你可以*title: false*



[type](undefined) - 基本层类型

类型：Number，默认：0

layer提供了5种层类型。可传入的值有：*0*（信息框，默认）*1*（页面层）*2*（iframe层）*3*（加载层）*4*（tips层）。 若你采用*layer.open({type: 1})*方式调用，则type为必填项（信息框除外）



[content](undefined) - 内容

类型：String/DOM/Array，默认：''

content可传入的值是灵活多变的，不仅可以传入普通的html内容，还可以指定DOM，更可以随着type的不同而不同。譬如：

```javascript
/!*
 如果是页面层
 */
layer.open({
  type: 1, 
  content: '传入任意的文本或html' //这里content是一个普通的String
});
layer.open({
  type: 1,
  content: $('#id') //这里content是一个DOM，注意：最好该元素要存放在body最外层，否则可能被其它的相对元素所影响
});
//Ajax获取
$.post('url', {}, function(str){
  layer.open({
    type: 1,
    content: str //注意，如果str是object，那么需要字符拼接。
  });
});
/!*
 如果是iframe层
 */
layer.open({
  type: 2, 
  content: 'http://sentsin.com' //这里content是一个URL，如果你不想让iframe出现滚动条，你还可以content: ['http://sentsin.com', 'no']
}); 
/!*
 如果是用layer.open执行tips层
 */
layer.open({
  type: 4,
  content: ['内容', '#id'] //数组第二项即吸附元素选择器或者DOM
});        
```



[icon](undefined) - 图标。信息框和加载层的私有参数

类型：Number，默认：-1（信息框）/0（加载层）

信息框默认不显示图标。当你想显示图标时，默认皮肤可以传入*0-6*如果是加载层，可以传入*0-2*。如：

```javascript
//eg1
layer.alert('酷毙了', {icon: 1});
//eg2
layer.msg('不开心。。', {icon: 5});
//eg3
layer.load(1); //风格1的加载
```



[success](undefined) - 层弹出后的成功回调方法

类型：Function，默认：null

当你需要在层创建完毕时即执行一些语句，可以通过该回调。success会携带两个参数，分别是*当前层DOM**当前层索引*。如：

```javascript
layer.open({
  content: '测试回调',
  success: function(layero, index){
    console.log(layero, index);
  }
});
```



[yes](undefined) - 确定按钮回调方法

类型：Function，默认：null

该回调携带两个参数，分别为当前层索引、当前层DOM对象。如：

```javascript
layer.open({
  content: '测试回调',
  yes: function(index, layero){
    //do something
    layer.close(index); //如果设定了yes回调，需进行手工关闭
  }
});  
```



[cancel](undefined) - 右上角关闭按钮触发的回调

类型：Function，默认：null

该回调携带两个参数，分别为：当前层索引参数（index）、当前层的DOM对象（layero），默认会自动触发关闭。如果不想关闭，*return false*即可，如；

```javascript
cancel: function(index, layero){ 
  if(confirm('确定要关闭么')){ //只有当点击confirm框的确定时，该层才会关闭
    layer.close(index)
  }
  return false; 
}    
```





[layer.open(options)](undefined) - 原始核心方法

基本上是露脸率最高的方法，不管是使用哪种方式创建层，都是走*layer.open()*，创建任何类型的弹层都会返回一个当前层索引，上述的*options即是基础参数*，另外，该文档*统一采用options作为基础参数的标识*例子：

```javascript
var index = layer.open({
  content: 'test'
});
//拿到的index是一个重要的凭据，它是诸如layer.close(index)等方法的必传参数。       
```



[layer.alert(content, options, yes)](undefined) - 普通信息框

它的弹出似乎显得有些高调，一般用于对用户造成比较强烈的关注，类似系统alert，但却比alert更灵便。它的参数是自动向左补齐的。通过第二个参数，可以设定各种你所需要的基础参数，但如果你不需要的话，直接写回调即可。如

```javascript
//eg1
layer.alert('只想简单的提示');        
//eg2
layer.alert('加了个图标', {icon: 1}); //这时如果你也还想执行yes回调，可以放在第三个参数中。
//eg3
layer.alert('有了回调', function(index){
  //do something
  
  layer.close(index);
});  
```



[layer.close(index)](undefined) - 关闭特定层

[layer.getChildFrame(selector, index)](undefined) - 获取iframe页的DOM

```javascript
layer.open({
  type: 2,
  content: 'test/iframe.html',
  success: function(layero, index){
    var body = layer.getChildFrame('body', index);
    var iframeWin = window[layero.find('iframe')[0]['name']]; //得到iframe页的窗口对象，执行iframe页的方法：iframeWin.method();
    console.log(body.html()) //得到iframe页的body内容
    body.find('input').val('Hi，我是从父页来的')
  }
});    
//body可以替换成layero。


//例如
layer.open({
  content: '<input id="in" type="text" value="你好"',
  yes: function(index, layero){
    //do something
  	alert(layero.find("#in").val());//弹出“你好”
    layer.close(index); //如果设定了yes回调，需进行手工关闭
  }
}); 

```





