# data-table
data-table是用于后台表格插件，他基于underscore.js和jquery。所在使用的时候需要引入这两个库。
data-table是用来展示的，数据是ajax请求之后得到的；展示成怎么样,而是由模板来控制的。


# 第一个例子
```html
<div id="datatable">
    <script type="text/html" class="head-tpl">
        <tr >
            <td width="200">序号</td>
            <td width="200">LOGO</td>
            <td>品牌名称</td>

        </tr>
    </script>
    <script type="text/html" class="body-tpl">
        <tr>
            <td width="200"><%= id %></td>
            <td width="200"><img src="<%= image_src %>" width="60" height="60" alt="" /></td>
            <td><%= name %></td>
        </tr>
    </script>
</div>
```
上面是html部分的内容，主要是用来控制显示的。一个表格是由两部分模板组成的，头部模板类名写成"head-tpl"， 内容部分类名写成"body-tpl" 这个是约定的，不能随意改。
```js
//只要传入ajax请求地址即可。
$('#datatable').dataTable({
    url: '/member/list'
});
```







data-table help
```js
/**
* 表格插件 用法一
* url 接口的地址
* headTemplate 渲染头部模板
* rowTemplate  要渲染的模板
* page_size多少行数据后分页
*/
var table = $('#datatable').dataTable({
    url: url,
    headTemplate:function(rowData, inex, list){
       var tpl = '';
        tpl += '<tr>';
        tpl += '    <td data-name="id" width="90" class="tac"><%= id %></td>';
        tpl += '    <td data-name="title" width="200"><%= title %></td>';
        tpl += '</tr>';
        return $.template(tpl, rowData);
    },
    rowTemplate: function (rowData, inex, list) {
        var tpl = '';
        tpl += '<tr>';
        tpl += '    <td data-name="id" width="90" class="tac"><%= id %></td>';
        tpl += '    <td data-name="title" width="200"><%= title %></td>';
        tpl += '    <td data-name="cate_id" width="100" class="tac"><%= cate.title %></td>';
        tpl += '    <td data-name="status" width="100" class="tac"><%= status %></td>';
        tpl += '    <td data-name="admin_id" width="100" class="tac"><%= admin.name %></td>';
        tpl += '    <td data-name="auditor_id" width="100" class="tac"><%= auditor %></td>';
        tpl += '    <td data-name="create_at" class="tac" width="150"><%= update_at %></td>';
        tpl += '</tr>';
        return $.template(tpl, rowData);
    },
    param: {
        page_size: 20
    }
});
```
```js
/**
* 表格插件 用法二
* url 接口的地址
* headTemplate 渲染头部模板从页面中获得 获取的类名为head-tpl
* rowTemplate  要渲染的模板从页面中获得 获取的类名为body-tpl
* 或者headTemplate和rowTemplate中任意一个写在模板用id为datatableb包裹起来,其中r任何一部分在页面中渲染，
* 另外一部分放在js中均可，例如：方法三
* page_size多少行数据后分页
*/
<!--html中的代码如下-->
<div id="datatable">
    <script type="text/html" data-status="0" class="head-tpl">
        <tr >
            <td  class="tac" data-name="id" width="200" class="tac">序号</td>
            <td  class="tac" data-name="logo" width="200">LOGO</td>
            <td  class="tac" data-name="name" width="" class="tac">品牌名称</td>
            <td data-name="opration" width="100" class="tac">操作</td>
        </tr>
    </script>

    <script type="text/html" data-status="0" class="body-tpl">
        <tr>
            <td data-name="id" width="200" class="tac"><%= id %></td>
            <td data-name="logo" width="200" class="tac"><img src="<%= image_src %>" width="120" height="60"></td>
            <td data-name="name" class="tac"><%= name %></td>
            <td data-name="opration"  width="100" class="tac">
                <a href="/brand/add?brand_id=<%= id %>" class="ui-btn" title="编辑"><span class="ui-icon ui-icon-pencil"></span></a> 
                <a data-id="<%= id %>" data-arr="[1, 2]" href="javascript:;" class="del ui-btn" title="删除"><span class="ui-icon ui-icon-close"></span></a>
            </td>
        </tr>
    </script>
</div>
<!--js如下-->
var table = $('#datatable').dataTable({
    url: url,
    param: {
        page_size: 20
    }
});
```
```js
/**
* 表格插件 用法三
* url 接口的地址
* 或者headTemplate和rowTemplate中任意一个写在模板用id为datatableb包裹起来,其中r任何一部分在页面中渲染，
* page_size多少行数据后分页，可写可不写。
* 另外一部分放在js中均可，例如：方法三
*/
<!--html中的代码如下-->
<div id="datatable">
    <script type="text/html" data-status="0" class="head-tpl">
        <tr >
            <td  class="tac" data-name="id" width="200" class="tac">序号</td>
            <td  class="tac" data-name="logo" width="200">LOGO</td>
            <td  class="tac" data-name="name" width="" class="tac">品牌名称</td>
            <td data-name="opration" width="100" class="tac">操作</td>
        </tr>
    </script>
</div>
var table = $('#datatable').dataTable({
    url: url,
    rowTemplate: function (rowData, inex, list) {
        var tpl = '';
        tpl += '<tr>';
        tpl += '    <td data-name="id" width="90" class="tac"><%= id %></td>';
        tpl += '    <td data-name="title" width="200"><%= title %></td>';
        tpl += '    <td data-name="cate_id" width="100" class="tac"><%= cate.title %></td>';
        tpl += '    <td data-name="status" width="100" class="tac"><%= status %></td>';
        tpl += '    <td data-name="admin_id" width="100" class="tac"><%= admin.name %></td>';
        tpl += '    <td data-name="auditor_id" width="100" class="tac"><%= auditor %></td>';
        tpl += '    <td data-name="create_at" class="tac" width="150"><%= update_at %></td>';
        tpl += '</tr>';
        return $.template(tpl, rowData);
    },
    param: {
        page_size: 20
    }
});
```
```js
[underscorejs模板使用示例](template.md)
