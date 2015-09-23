#渲染模板
- [template中的变量](#变量) 
- [template中的HTML转义](#转义)
- [template通过with取值方法](#取值) 
- [如何解决标签冲突](#标签冲突) 
- [template中的三元运算](#三元运算) 
- [提供的forEach方法](#提供的foreach方法) 
- [print的使用](#使用print方法) 
```js
语法
_.template(templateString,[settings])
```
```js
_.template的作用
将javascript模板编译为可以用于页面呈现的函数，对于通过JSON数据源生成复杂的html并呈现出来操作非常有用。
```
#变量
```js
_.template可以用<%= xxxx %>作为变量，也可以用<% … %>执行任意的 JavaScript 代码
例:
var compiled = _.template("hello: <%= name %>");
console.log(compiled({name: 'moe'});）
=> "hello: moe"

```
#转义
```js
插入一个值并进行HTML转义用<%- … %>
例:
var template = _.template("<b><%- value %></b>");
console.log(template({value: '<script>'});)
=> "<b>&lt;script&gt;</b>"

```
#取值
```js

 template 通过 with 语句来取得 data 所有的值，也可以在 variable 设置里指定一个变量名. 这样能显著提升模板的渲染速度。
例:
_.template("Using 'with': <%= data.answer %>", {variable: 'data'})({answer: 'no'});
=> "Using 'with': no"

```
#改变模板设置
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
#标签冲突
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
#三元运算
```js

例1：<div>性别:<%= gender == 0 ? "女" : "男"%></div> 

输出表达式结果，如果gender为0则返回字符串为<div>女</div>，反之为<div>男</div>
 
例2：class="timelist <%= is_overdue == 1? "expire" : "" %>"

输出class样式结果：如果is_overdue为1则class="expire",反之为空

```
#提供的forEach方法

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
#使用print方法

```js
在JavaScript代码中使用 print. 有时候会比使用 <%= ... %> 更方便

例：
var compiled = _.template("<% print('Hello ' + epithet); %>");
compiled({epithet: "stooge"});
=> "Hello stooge"
```


