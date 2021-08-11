## JS & jQuery Tips


- [获取上传的文件](#获取上传的文件)  
- [页面自动刷新](#页面自动刷新)  
- [页面跳转](#页面跳转)  
- [判断当前是否在 iframe 中](#判断当前是否在-iframe-中)  
- [图片加载失败时的处理](#图片加载失败时的处理)  
- [生成指定长度的数组](#生成指定长度的数组)  
- [判断元素是否存在](#判断元素是否存在)  


#### 获取上传的文件
```html
<input type="file" id="uploadFiles" name="uploadFiles" multiple="multiple">
<button onclick="upload()">Upload</button>

<script>
  function upload() {
    var objArray = document.getElementById("uploadFiles").files;
    for (var i = 0; i < objArray.length; i++) {
      console.log("Uploading: " + objArray[i].name);
    }
  }
</script>
```


#### 页面自动刷新
- 通过在 `<head>` 标签中添加 `<meta>` 标签来实现，但这种跳转方式不方便加倒计时，不够友好。
```html
<meta http-equiv="Refresh" content="1"/>  <!--页面每1秒刷新一次-->
<meta http-equiv="Refresh" content="5; url=http://www.baidu.com"/>  <!--5秒钟后页面自动跳转到http://www.baidu.com-->
```

- 通过 `js` 来实现页面刷新或者跳转
```html
<script>
  function refresh() {
    window.location.reload()
  }
  setTimeout('refresh()', 1000)  // 1秒刷新一次
</script>
```


#### 页面跳转
- 通过 `<a>` 标签 `href` 属性实现
```html
<a href="https://www.baidu.com/">百度一下</a>
```

- 通过 `onclick` 属性使用 `JavaScript` 实现
```html
<button onclick="window.location.href='https://www.baidu.com/'">百度一下</button>
或
<button onclick="window.open('https://www.baidu.com/')">百度一下</button>
```

- 通过 `form` 表单的 `action` 定向提交跳转
```html
<form action="xxx.html" method="post">
  <input type="submit" value="提交">
</from>
```

注：
> `<a target="_blank|_self|_parent|_top|framename">`
> - `_blank` - 在新窗口中打开被链接文档。
> - `_self` - 默认。在相同的框架中打开被链接文档。
> - `_parent` - 在父框架集中打开被链接文档。
> - `_top` - 在整个窗口中打开被链接文档。
> - `framename` - 在指定的框架中打开被链接文档。
>   
> `window.open(URL, _blank|_self|_parent|_top|name, specs, replace)`
> - `_blank`  默认。URL加载到一个新的窗口。
> - `_self`  URL替换当前页面。
> - `_parent`  URL加载到父框架。
> - `_top`  URL替换任何可加载的框架集。
> - `name`  窗口名称。


#### 判断当前是否在 `iframe` 中
判断当前代码是否在 `iframe` 中有三种方法：
```js
if (self !== top) {
　　console.log('This is in iframe.')
}
```
或
```js
if (window.frames.length !== parent.frames.length) {
　　console.log('This is in iframe.')
}
```
或
```js
if (self.frameElement && self.frameElement.tagName === "IFRAME") {
　　console.log('This is in iframe.')
}
```


#### 图片加载失败时的处理
```html
<img src="abc.jpg" alt="" onerror="this.onerror=null;this.src='123.jpg'"/>
<img src="abc.jpg" alt="" onerror="onerror=null;src='123.jpg'" />
```


#### 生成指定长度的数组
1. 生成包含 n 个 undefind 元素的数组
```js
let a = new Array(n)
```
2. 生成包含 n 个指定元素（比如 null）的数组
```js
let a = new Array(n).fill(null)
```
3. 生成包含 n 个空字符串的数组
```js
let a = new Array(n).join(',').split(',')
```
4. 生成从 0 开始长度为 n 的递增数组
```js
let a = [...new Array(n).keys()]  //[0, 1, 2, ..., n-1]
```
参考链接：<https://blog.csdn.net/bangbDIV/article/details/90692290>


####　判断元素是否存在
```js
if ($("#id_example").length > 0) {
  console.log("元素存在")
}
```