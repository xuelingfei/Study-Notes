## Layui Table


- [获取表格全部数据](#获取表格全部数据) 
- [修改表格样式](#修改表格样式)  
 

#### 获取表格全部数据
通过 `layui.table.cache['demoTableId']` 的方式可以获取表格全部数据，其中 demoTableId 是 `table.render()` 中定义的表格 id。
```js
//头工具栏事件
table.on('toolbar(demoTableFilter)', function (obj) {
  switch (obj.event) {
    case 'export':
      var tableData
      var checkStatus = table.checkStatus(obj.config.id)  //复选框选中的数据
      if (checkStatus.data.length > 0) {
        tableData = checkStatus.data
      } else {
        tableData = table.cache[obj.config.id]
      }
      break
  }
})
```


#### 修改表格样式
```js
table.render({
  elem: '#demoTable',
  id: 'demoTableId',
  url: '/demo/get_table_data',
  cols: [],
  done: function (res, curr, count) {
    //修改单元格样式
    var cells = document.querySelectorAll('.layui-table-cell');
    for (var i = 0; i < cells.length; i++) {
      cells[i].style.height = '42px';
      cells[i].style.lineHeight = '40px';
    }
    //修改特定行样式
    var that = this.elem.next()
    res.data.forEach(function (item, index) {  //item 是行数据
      if (item.color === 'yellow') {
        var tr = that.find(".layui-table-box tbody tr[data-index='" + index + "']")
        tr.css("background-color", "yellow")
      }
    });
  },
});
```
