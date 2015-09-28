#渲染模板
- [template初解](#template初解) 
- [普通使用示例](#普通使用示例) 
- [if else使用示例](#if-else使用示例) 
- [三元表达式使用示例](#三元表达式使用示例) 
- [其他](#其他) 

#template初解
```js
//语法
_.template(templateString, [settings]);
```
```js
_.template的作用
将javascript模板编译为可以用于页面呈现的函数，对于通过JSON数据源生成复杂的html并呈现出来操作非常有用。
```

#普通使用示例
```js
//_.template可以用<%= xxxx %>作为变量，也可以用<% … %>执行任意的 JavaScript 代码
var compiled = _.template("hello: <%= name %>");
console.log(compiled({name: 'moe'});）
<a href="http://jsfiddle.net/1181155148/n81dpwk8/1/"> </a>
//=> "hello: moe"

```
```js
//输入一个值并进行HTML转义用<%- … %>
var template = _.template("<b><%- value %></b>");
console.log(template({value: '<script>'});)
//=> "<b>&lt;script&gt;</b>"

```
<http://jsfiddle.net/1181155148/jcm3when/2/>
```js

// template 通过 with 语句来取得 data 所有的值，也可以在 variable 设置里指定一个变量名. 这样能显著提升模板的渲染速度。
var a= _.template("Using 'with': <%= data.answer %>", {
    variable: 'data'
})({
    answer: 'no'
});
$("#a").html(a);
//=>Using 'with': no
http://jsfiddle.net/1181155148/otd4f4kk/1/

```
#if else使用示例
```js
例1：
<% if (typeof(date) !== "undefined") { %>
    <span class="date"><%= date %></span>
<% } %>
```
```js
<%if(…){%>  html  <%}else{%> html  <%}%>
```
#三元表达式使用示例

```js

例1：<div>性别:<%= gender == 0 ? "女" : "男"%></div> 

输出表达式结果，如果gender为0则返回字符串为<div>女</div>，反之为<div>男</div>
 ```
 ```js
例2：class="timelist <%= is_overdue == 1? "expire" : "" %>"

输出class样式结果：如果is_overdue为1则class="expire",反之为空

```
#循环示例
```js
var tmp = '<% for (var i = 0; i < count; i++) { %>';
tmp += '<div class="tmp"><%= i %></div>';
tmp += '<% } %>';

var tem = _.template(tmp)({
    count: 5
});
$("#a").html(tem);
//=><div class="tmp">0</div>
//<div class="tmp">1</div>
//<div class="tmp">2</div>
//<div class="tmp">3</div>
//<div class="tmp">4</div>
http://jsfiddle.net/1181155148/b4owsLkd/
```
```js


```
```js
例2：
        var listDiv = $(list);
        var html = _.reduce(data, function (memo, value, key, list) {
            var li = '<div class="zoom">';
            li += '<img src="<%= xxxx %>" width="80" height="80" alt="">';
            li += '<a  bind-app="webview" href="javascript:;"><%= xxxx %></a><br/>';
            li += '<span  class="description ect"><%= xxxx %></span>';
            li += (function () {
                var btn;
                if (value) {
                    btn = '<a href="javascript:;" class="focus-btn" data-id="<%= xxxx %>">xxx</a>'
                } else {
                    btn = '<a href="javascript:;" class="focus-btn" data-id="<%= xxxx%>">xxxx</a>';
                }
                return btn;
            }(value));
            li += '</div>';
            return memo + _.template(li)(value);
        }, '');
        listDiv.append(html);
```
#其他
```js
改变Underscore的模板设置, 使用别的符号来嵌入代码.可利用正则表达式来逐字匹配嵌入代码的语句。 

例1：
_.templateSettings = {
    evaluate    : /<%([\s\S]+?)%>/g, //evaluate标签中间的表示为可执行的js代码
    interpolate : /<%=([\s\S]+?)%>/g, //interpolate表示输出一个js运行结果的值
    escape      : /<%-([\s\S]+?)%>/g//escapte表示输出这个变量的值并进行html标签过滤，如将相关的字符"<"转化为"&lt;"
  };
  
例2：
var template = _.template("Hello {{ name }}!");
console.log(template({name: "Mustache"});)
=> "Hello Mustache!"
```

```js

 默认的标签使用过程中，可能会造成不便比如jsp中这样的字符串会与java代码冲突，这样我们只修改覆盖默认配置就行，比如将"<%%>"替换为“{}”
 
 /** 
 * underscore template settings 
 */  
  _.templateSettings = {  
    evaluate    : /{([\s\S]+?)}/g,  
    interpolate : /{=([\s\S]+?)}/g,  
    escape      : /{-([\s\S]+?)}/g  
};  
```

```js
underscore提供的forEach
var tml = "<ul id={-id}>" +  
          "{_.forEach(data, function(<strong>item</strong>, <strong>i</strong>){}"+  
                "<li>{-<strong>i</strong>}:{-<strong>item</strong>.name}</li>"+  
          "{})}"+  
          "</ul>";          
tmp = _.template(tml, {  
    id:"students",  
    data:[  
        {name: "lily", gender: 0},   
        {name: "lilei", gender: 1},   
        {name: "jim", genger: 1}  
    ]     
}); 
 输出字符串结果为:
 <ul id=students>
     <li>0:lily</li>
     <li>1:lilei</li>
     <li>2:jim</li>
 </ul>
```
```js
在JavaScript代码中使用 print. 有时候会比使用 <%= ... %> 更方便

例：
var compiled = _.template("<% print('Hello ' + epithet); %>");
compiled({epithet: "stooge"});
=> "Hello stooge"
```



