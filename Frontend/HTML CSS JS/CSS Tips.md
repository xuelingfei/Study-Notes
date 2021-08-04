## CSS Tips


- [a 标签去掉下划线](#a-标签去掉下划线)  
- [去除相邻元素之间的空隙](#去除相邻元素之间的空隙)  
- [div 或 p 标签单行和多行显示方法](#div-或-p-标签单行和多行显示方法)  


#### a 标签去掉下划线
```css
a {
  text-decoration: none;
}
```


#### 去除相邻元素之间的空隙
有两种方法：
- 将父级元素的字体设置为 0px(`font-size: 0px`)
- 两个标签的代码写在一行中


#### 列表移除默认设置
```css
ul, ol {
  list-style-type: none;  /*移除默认图标*/
}
li {
  display: inline;  /*列表项显示在同一行*/
}
```


#### div 或 p 标签单行和多行显示方法
1. 单行
```css
div, p {
  text-overflow: ellipsis;
  white-space: nowrap;
  overflow: hidden;
}
```

2. 多行
```css
div, p {
  text-overflow: ellipsis;
  overflow: hidden;
  word-wrap: break-word;
  -webkit-line-clamp: 2;
  display: -webkit-box;
  -webkit-box-orient: vertical;
}
```
注：
- `text-overflow`，可以用来多行文本的情况下，用省略号“...”隐藏超出范围的文本 。
- `-webkit-line-clamp` 是一个不规范的属性（unsupported WebKit property），它没有出现在 CSS 规范草案中。限制在一个块元素显示的文本的行数。为了实现该效果，它需要组合其他外来的 WebKit 属性。常见结合属性：
  - `display: -webkit-box` 必须结合的属性，将对象作为弹性伸缩盒子模型显示
  - `-webkit-box-orient` 必须结合的属性，设置或检索伸缩盒对象的子元素的排列方式
