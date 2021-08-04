## [Laydate](https://www.layui.com/laydate/)


- [初始值和初始值填充](#初始值和初始值填充)  
- [部分实现点击即选中](#部分实现点击即选中)  
- [修复一闪就关闭的问题](#修复一闪就关闭的问题)  


#### 初始值和初始值填充
1. 初始值 `value`  
类型：`String`，默认值：`new Date()`  
支持传入符合 `format` 参数设定的日期格式字符，或者 `new Date()`
```js
//传入符合 format 格式的字符给初始值
laydate.render({ 
  elem: '#test',
  value: '2018-08-18',  //必须遵循 format 参数设定的格式
});
 
//传入 Date 对象给初始值
laydate.render({ 
  elem: '#test',
  value: new Date(1534766888000),  //参数即为：2018-08-20 20:08:08 的时间戳
});
```

2.初始值填充 `isInitValue`  
类型：`Boolean`，默认值：`true`  
用于控制是否自动向元素填充初始值（需配合 `value` 参数使用）  
> - false - 只给 laydate 弹出框初始值，input 输入框不填充
> - true - laydate 弹出框和 input 输入框同时填充初始值
```js
laydate.render({
  elem: '#test',
  value: '2017-09-10',
  isInitValue: false,  //是否允许填充初始值，默认为 true
});
```


#### 部分实现点击即选中
laydate 实现点击即选中，不用点确定按钮（这种方法只有 date 格式能实现）
```js
laydate.render({
    elem: '#demoDate',
    type: 'date',
    showBottom: false,
});
```


#### 修复一闪就关闭的问题
- 问题描述：谷歌浏览器上，laydate 控件在页面高度不足时会出现一闪就关闭的问题
- 解决方法：
```js
laydate.render({
  elem: '#demoDate',
  trigger: 'click',
})
```