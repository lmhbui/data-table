# data-table
data-table help
    //表格插件
    var table = $('#datatable').dataTable({
        url: '/cms/bulletins',
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

[underscorejs模板使用示例](template.md)
