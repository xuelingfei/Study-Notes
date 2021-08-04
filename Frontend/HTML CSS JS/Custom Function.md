## 自定义函数

- [加法函数](#加法函数)  
- [截取后缀名](#截取后缀名)  
- [获取 URL 中参数值](#获取-URL-中参数值)  
- [POST 带参跳转](#POST-带参跳转)  
- [文字转图像](#文字转图像)  
- [图片弹窗预览](#图片弹窗预览)  
- [判断元素是否隐藏](#判断元素是否隐藏)  


#### 加法函数
JavaScript 的加法结果会有误差，在两个浮点数相加的时候会比较明显，下面这个函数返回较为精确的加法结果。  
思路总结：
1. 遍历所有相加的数的最大小数位
2. 所有数乘以10的最大小数位次幂，把小数变成整数，再相加
3. 所得数的总和再除以10的最大小数位次幂，得出最终结果
```js
function add() {
  var args = arguments,  //获取所有的参数
      lens = args.length,  //获取参数的长度
      d = 0,  //定义小数位的初始长度，默认为整数，即小数位为 0
      sum = 0;  //定义 sum 来接收所有数据的和
  for(var key in args){  //遍历所有的参数
    var str = "" + args[key];  //把数字转为字符串
    if(str.indexOf(".") != -1){  //判断数字是否为小数
      var temp = str.split(".")[1].length;  //获取小数位的长度
      d = d < temp ? temp : d;  //比较此数的小数位与原小数位的长度，取小数位较长的存储到 d 中
    }
  }
  var m = Math.pow(10, d);  //计算需要乘的数值
  for(var key in args){  //遍历所有参数并相加
    sum += args[key] * m;
  }
  return sum / m;  //返回结果
}
```


#### 截取后缀名
- `split()` 方法
```js
function getSuffix(strObj) {
  return strObj.split('.').pop().toLowerCase()
}
```
- `substring()` 方法
```js
function getSuffix(strObj) {
  var dotIndex = strObj.lastIndexOf(".")  //取最后一个点的索引
  var strLen = strObj.length  //取总长度
  return strObj.substring(dotIndex + 1, strLen )  //截取获得后缀名
}
```
或
```js
function getSuffix(strObj) {
  return strObj.substring(strObj.lastIndexOf('.') + 1)
}
```


#### 获取 URL 中参数值
获取指定URL中的某个参数的值，参数不存在时返回空字符串（""），`url` 参数不指定时为会自己获取当前地址栏中的 `url`
```js
/** 
* 获取指定 URL 中的某个参数的值
* @param    {string}  name    参数名
* @param    {string}  url     为空表示当前url
* @returns  {string}  参数值
*/
function getUrlParams(name, url) {
  if (!url) url = window.location.href;
  var params = {};
  var urlStr = decodeURI(url);
  var idx = urlStr.indexOf("?");
  if (idx > 0) {
    var queryStr = urlStr.substring(idx + 1);
    var args = queryStr.split("&");
    for (var i = 0, a, nv; a = args[i]; i++) {
      nv = args[i] = a.split("=");
      params[nv[0]] = nv.length > 1 ? nv[1] : true;
    }
  }
  return params[name] === undefined ? "" : params[name]
}
```
或
```js
/** 
* 获取指定 URL 中的某个参数的值
* @param    {string}  name    参数名
* @param    {string}  url     为空表示当前url
* @returns  {string}  参数值
*/
function getUrlParams(name, url) {
  if (!url) url = window.location.search;
  let urlStr = decodeURI(url);
  let reg = new RegExp('(^|&)' + name + '=([^&]*)(&|$)', 'i');
  let r = urlStr.substr(1).match(reg);
  return r === null ? "" : r[2]
}
```



#### POST 带参跳转
通过模拟 `form` 表单的提交以 `post` 方式带参数跳转到指定 `url`
```js
/** 
* 通过模拟 form 表单的提交以 post 方式带参数跳转到指定 url
* @param {string}  url       指定跳转地址
* @param {array}   params    参数列表 [{name: '', value: ''}, ...]
*/
function postToUrl(url, params) {
  var tempForm = document.createElement("form");  //创建form表单
  tempForm.action = url;
  tempForm.target = "_self";  //如需打开新窗口，target属性要设置为'_blank'
  tempForm.method = "post";
  tempForm.style.display = "none";
  for (var item of params) {  //添加参数
    var newElement = document.createElement("textarea");
    newElement.name = item.name;
    newElement.value = item.value;
    tempForm.appendChild(newElement);
  }
  document.body.appendChild(tempForm);
  tempForm.submit();  //提交数据
}
```


#### 文字转图像
使用 `canvas` 将文字转换成图像数据 `base64`
```js
/** 
* js使用canvas将文字转换成图像数据base64
* @param {string}    text             文字内容  "abc"
* @param {number}    fontsize         文字大小  20
* @param {string}    fontcolor        文字颜色  "#000"
* @returns {string}       imgBase64Data    图像数据
*/
function textBecomeImg(text, fontsize, fontcolor) {
    var canvas = document.createElement('canvas');
    //小于32字加1    小于60字加2    小于80字加4    小于100字加6
    var $buHeight = 0;
    if(fontsize <= 32){ $buHeight = 1; }
    else if(fontsize > 32 && fontsize <= 60 ){ $buHeight = 2;}
    else if(fontsize > 60 && fontsize <= 80 ){ $buHeight = 4;}
    else if(fontsize > 80 && fontsize <= 100 ){ $buHeight = 6;}
    else if(fontsize > 100 ){ $buHeight = 10;}
    //对于 g j 等有时会有遮挡，这里增加一些高度
    canvas.height = fontsize + $buHeight ;
    var context = canvas.getContext('2d');
    //擦除 (0,0) 位置大小为 200x200 的矩形，擦除的意思是把该区域变为透明
    context.clearRect(0, 0, canvas.width, canvas.height);
    context.fillStyle = fontcolor;
    context.font = fontsize + "px Arial";
    //top（顶部对齐） hanging（悬挂） middle（中间对齐） bottom（底部对齐） alphabetic是默认值
    context.textBaseline = 'middle'; 
    context.fillText(text, 0, fontsize/2)

    //如果在这里直接设置宽度和高度会造成内容丢失 , 暂时未找到原因 , 可以用以下方案临时解决
    //canvas.width = context.measureText(text).width;

    //方案一：可以先复制内容  然后设置宽度 最后再黏贴    
    //方案二：上边设置完宽度后，再设置一遍文字

    //方案一：这个经过测试有问题，字体变大后，显示不全,原因是 canvas 默认的宽度不够，
    //如果一开始就给 canvas 一个很大的宽度的话，这个是可以的。	
    //var imgData = context.getImageData(0, 0, canvas.width, canvas.height);  //这里先复制原来的canvas里的内容	
    //canvas.width = context.measureText(text).width;  //然后设置宽和高	
    //context.putImageData(imgData, 0, 0);  //最后黏贴复制的内容

    //方案二：改变大小后，重新设置一次文字
    canvas.width = context.measureText(text).width;
    context.fillStyle = fontcolor;
    context.font = fontsize + "px Arial";
    context.textBaseline = 'middle'; 
    context.fillText(text, 0, fontsize/2)

    return canvas.toDataURL('image/png')  // 注意这里背景透明的话，需要使用 png
}
```


#### 图片弹窗预览
```js
function imagePreview(imgSrc) {
  var img = new Image();
  img.src = imgSrc;
  var height = img.height; //获取图片高度
  var width = img.width; //获取图片宽度
  var winHeight = $(window).height() - 80;  // 浏览器可视部分高度
  var winWidth = $(window).width() - 100;  // 浏览器可视部分宽度

  // 如果图片高度或者宽度大于限定的高度或者宽度则进行等比例尺寸压缩
  if (height > winHeight || width > winWidth) {
    // 1.原图片宽高比例 大于等于 图片框宽高比例
    if (winWidth / winHeight <= width / height) {
      height = winWidth * (height / width);
      width = winWidth;   //以框的宽度为标准
    }

    // 2.原图片宽高比例 小于 图片框宽高比例
    if (winWidth / winHeight > width / height) {
      width = winHeight * (width / height);
      height = winHeight;   //以框的高度为标准
    }
  }
  
  layer.open({
    id: 'image_preview', //设定id，只允许同时弹出一个
    type: 1,
    title: false,
    closeBtn: 1,  //右上关闭，1、2两种风格，0不显示
    area: [width + 'px', height + 'px'],  //宽高自适应['auto']
    resize: false,  //不允许弹窗拉伸，因为图片不会同时拉伸
    //skin: 'layui-layer-nobg', //没有背景色
    shadeClose: true,  //点击遮罩关闭
    move: '.layui-layer-content',  //开启拖曳
    scrollbar: false,  //屏蔽滚动
    content: '<img src="' + imgSrc + '" alt="">'
  });
}
```


#### 判断元素是否隐藏
- 原生 JS 判断
```js
var em = document.getElementById("#example");
var display = em.style.display;
var visibility = em.style.visibility;
if (display == 'none' || visibility == 'hidden') {
  // 元素是隐藏的
} else {
  //
}
```

- 使用is结合伪类:hidden进行判断
```js
if ($("#example").is(":hidden")) {
  // 元素是隐藏的
} else {
  //
}
```

- 使用is结合伪类:visible进行判断
```js
if ($("#example").is(":visible")) {
  //
} else {
  // 元素是隐藏的
}
```