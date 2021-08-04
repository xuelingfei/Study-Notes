## Layui Form


- [Select 下拉选择框](#Select-下拉选择框) 
- [Switch 开关](#Switch-开关)  
 

#### Select 下拉选择框
- Layui select 根据后台数据显示下拉选择框：
```html
<div class="layui-form-item">
  <select id="demoSelect" name="demoSelect" lay-verify="required">
    <option value=""></option>
  </select>
 </div>
```
```js
layui.use(['jquery', 'form', 'layer'], function () {
  var $ = layui.jquery
  var form = layui.form
  var layer = layui.layer

  //页面打开时异步加载数据
  $(function () {
    $.ajax({
      type: 'POST',
      //提交的网址
      url: '/demo/get_select_data',
      //提交的数据
      //data: {},
      //返回数据的格式
      datatype: 'json',  //'xml', 'html', 'script', 'json', 'jsonp', 'text'
      //成功返回之后调用的函数
      success: function (data) {
        $('#demoSelect').empty()
        $.each(data, function (index, item) {
          $('#demoSelect').append(new Option(item.name, item.id))  //下拉选择框里添加元素
        });
        form.render('select');
      }, error: function () {
        layer.msg('出错了')
      }
    });
  })
})
```

- Django Form 与 Layui Select  
当用 Django Form 生成表单用 Layui Form 进行渲染时，对于 select 选择框，`required` 属性是无用的，如要设置，需在 form 中加上 `lay-verify`。
```python
widgets = {
    'demoSelect': wid.Select(choices=DEMO_CHOICE, attrs={"lay-verify": "required"}),
}
```


#### Switch 开关
1. 开关  
其实就是 checkbox 复选框的“变种”，通过设定 `lay-skin="switch"` 形成了开关风格
```html
<input type="checkbox" name="xxx" lay-skin="switch" lay-text="ON|OFF" lay-filter="demo_switch" checked disabled>
```
> - 属性 `checked` 可设定默认开
> - 属性 `disabled` 开启禁用
> - 属性 `lay-text` 可自定义开关两种状态的文本
> - 设置 `value="1"` 可自定义值，否则打开时返回的就是默认的 `on`，关闭时没有对应返回值

2. 监听  
开关被点击时触发：
```js
form.on('switch(demo_switch)', function (data) {
    console.log(data.elem);  //得到checkbox原始DOM对象
    console.log(data.elem.checked);  //开关是否开启，true或者false
    console.log(data.value);  //开关value值，也可以通过data.elem.value得到
    console.log(data.othis);  //得到美化后的DOM对象
});
```
当存在二次确认时，可以通过 `data.elem.checked` 取反然后重新渲染来设置取消操作：
```js
form.on('switch(demo_switch)', function (data) {
    layer.confirm('请确认操作', {
        btn: ['确定','取消'] //按钮
    }, function () {
        console.log(data.elem);  //得到checkbox原始DOM对象
        console.log(data.elem.checked);  //开关是否开启，true或者false
        console.log(data.value);  //开关value值，也可以通过data.elem.value得到
        console.log(data.othis);  //得到美化后的DOM对象
    }, function () {
        data.elem.checked = !data.elem.checked;
        form.render();
    });
});
```
