## jQuery 方法


- [属性操作](#属性操作)  
- [删除元素](#删除元素)  
- [遍历](#遍历)  
- [事件](#事件)  
- [jQuery 事件中 return false 的含义](#jQuery-事件中-return-false-的含义)  


#### 属性操作
- `addClass()` 向匹配的元素添加指定的类名
- `attr()` 设置或返回匹配元素的属性和值
- `hasClass()` 检查匹配的元素是否拥有指定的类
- `html()` 设置或返回匹配的元素集合中的 HTML 内容
- `removeAttr()` 从所有匹配的元素中移除指定的属性
- `removeClass()` 从所有匹配的元素中删除全部或者指定的类
- `toggleClass()` 从匹配的元素中添加或删除一个类
- `val()` 设置或返回匹配元素的值


#### 删除元素
- `empty()`  移除被选元素所有的子节点和内容
- `detach()`  移除被选元素所有的子节点和文本，但会保留数据和事件，该方法会保留移除元素的副本，允许它们在以后被重新插入
- `remove()`  移除被选元素及其所有子节点和文本，该方法也会移除被选元素的数据和事件


#### 遍历
- `add()` 把元素添加到匹配元素的集合中
- `children()` 返回被选元素的所有直接子元素
- `closest()` 返回被选元素的第一个祖先元素
- `contents()` 返回被选元素的所有直接子元素（包含文本和注释节点）
- `each()` 为每个匹配元素执行函数
- `filter()` 把匹配元素集合缩减为匹配选择器或匹配函数返回值的新元素
- `find()` 返回被选元素的后代元素
- `first()` 返回被选元素的第一个元素
- `last()` 返回被选元素的最后一个元素
- `next()` 返回被选元素的后一个同级元素
- `nextAll()` 返回被选元素之后的所有同级元素
- `parent()` 返回被选元素的直接父元素
- `parents()` 返回被选元素的所有祖先元素
- `parentsUntil()` 返回介于两个给定元素之间的所有祖先元素（例 $("span").parentsUntil("div");)
- `prev()` 返回被选元素的前一个同级元素
- `prevAll()` 返回被选元素之前的所有同级元素
- `siblings()` 返回被选元素的所有同级元素


#### 事件
- `trigger()`  
触发所有匹配元素上指定的事件以及事件的默认行为（比如表单提交）。  
语法：`$(selector).trigger(event,[param1,param2,...])`

- `triggerHandler()`  
触发第一个被匹配元素上指定的事件，不会引起事件（比如表单提交）的默认行为。  
语法：`$(selector).triggerHandler(event,[param1,param2,...])`

- `animate()`  
执行 CSS 属性集的自定义动画，通过 CSS 样式将元素从一个状态改变为另一个状态。CSS 属性值是逐渐改变的，这样就可以创建动画效果。只有数字值可创建动画，字符串值无法创建动画。使用 `+=` 或 `-=` 来创建相对动画（relative animations）。  
语法：`$(selector).animate(styles[,speed,easing,callback])`  
参数：
> - styles: 必需。规定产生动画效果的 CSS 样式和值。CSS 样式使用 DOM 名称（比如 `fontSize`）来设置，而非 CSS 名称（比如 `font-size`），即该属性名称必须是驼峰写法：您必须使用 `paddingLeft` 代替 `padding-left`，`marginRight` 代替 `margin-right`，依此类推。
> - speed: 可选。规定动画的速度。默认是 `normal`。可能的值：毫秒，`slow`，`normal`，`fast`。
> - easing: 可选。规定在动画的不同点中元素的速度。默认值是 `swing`。可能的值：`swing`（在开头/结尾移动慢，在中间移动快），`linear`（匀速移动）。
> - callback：可选。`animate()` 函数执行完之后，要执行的函数。

- `click()`  
触发 click 事件，或规定当发生 click 事件时运行的函数。  
语法：`$(selector).click(function)`



#### jQuery 事件中 `return false` 的含义
```js
$(selector).on('click', function() {
  ...
  return false;
});
```
等价于
```js
$(selector).on('click', function(event) {
  ...
  event.preventDefault();  // 阻止事件的默认行为
  event.stopPropagation();  // 阻止该dom节点往上冒泡
})
```
例如，如果 selector 是一个 a 标签，那它的默认行为是跳转，当设置了 `return false` 时，它就不会跳转。
