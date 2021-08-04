## CSS 属性


- [提高属性优先级](#提高属性优先级)  
- [anchor 伪类](#anchor-伪类)  
- [行高](#行高)  
- [空白和换行](#空白和换行)  
- [溢出滚动](#溢出滚动)  
- [背景透明](#背景透明)  
- [背景图片引入 svg](#背景图片引入-svg)  
- [边框指定圆角](#边框指定圆角)  
- [边框样式](#边框样式)  


#### 提高属性优先级
`Selector {sRule !important;}`  
提升指定样式规则的应用优先权，即 `!important` 可以增加样式权重，让浏览器首选执行这个语句。


#### anchor 伪类
> - `a:link` - 未访问的链接
> - `a:visited` - 已访问的链接
> - `a:hover` - 鼠标划过链接
> - `a:active` - 已选中的链接

注意：
> - 在 CSS 定义中，当这些伪类同时出现时，`a:hover` 必须被置于 `a:link` 和 `a:visited` 之后才是有效的，`a:active` 必须被置于 `a:hover` 之后才是有效的。伪类的名称不区分大小写。
> - 在 CSS 中，`:hover`，`:focus` 等这些伪类与类名之间不能有空格，否则不生效。


#### 行高
`line-height`
JavaScript 语法：`object.style.lineHeight = "2"`
> - normal - 默认，设置合理的行间距
> - number - 设置数字，此数字会与当前的字体尺寸相乘来设置行间距
> - length - 设置固定的行间距
> - % - 基于当前字体尺寸的百分比行间距
> - inherit - 规定应该从父元素继承 `line-height` 属性的值


#### 空白和换行
`white-space`
JavaScript 语法：`object.style.whiteSpace = "pre"`
> - normal - 默认，空白会被浏览器忽略
> - pre - 空白会被浏览器保留，其行为方式类似 HTML 中的 `<pre>` 标签
> - nowrap - 文本不会换行，文本会在在同一行上继续，直到遇到 `<br>` 标签为止
> - pre-wrap - 保留空白符序列，但是正常地进行换行
> - pre-line - 合并空白符序列，但是保留换行符
> - inherit - 规定应该从父元素继承 `white-space` 属性的值


#### 溢出滚动
`overflow` | `overflow-x` | `overflow-y`  
用于控制内容溢出元素框时显示的方式，可以控制内容溢出元素框时在对应的元素区间内添加滚动条。
> - visible - 默认值，内容不会被修剪，会呈现在元素框之外
> - hidden - 内容会被修剪，并且其余内容是不可见的
> - scroll - 内容会被修剪，但是浏览器会显示滚动条以便查看其余的内容
> - auto - 如果内容被修剪，则浏览器会显示滚动条以便查看其余的内容
> - inherit - 规定应该从父元素继承 overflow 属性的值

注意：`overflow` 属性只工作于指定高度的块元素上。在 OS X Lion（Mac 系统）系统上，滚动条默认是隐藏的，使用的时候才会显示（设置 `overflow: scroll` 也是一样的）。


#### 背景透明
- `background: transparent;`  
完全透明
- `opacity: 0.7;`  
整个按钮的不透明度，会影响到文字，`0` 完全透明，`1` 完全不透明
- `background: rgba(255, 255, 255, 0.7);`  
仅调节背景的色彩，并加上不透明度，此例为 70% 不透明的白色


#### 背景图片引入 svg
```css
div {
  background: url("data:image/svg+xml;utf8,<svg></svg>");
}
```


#### 边框指定圆角
- `border-radius`
> - 一个值：四个圆角值相同
> - 两个值：第一个值为左上角与右下角，第二个值为右上角与左下角
> - 三个值：第一个值为左上角, 第二个值为右上角和左下角，第三个值为右下角
> - 四个值：第一个值为左上角，第二个值为右上角，第三个值为右下角，第四个值为左下角
- 单独加圆角
> - `border-bottom-left-radius` - 左下
> - `border-top-left-radius` - 左上
> - `border-top-right-radius` - 右上
> - `border-bottom-right-radius` - 右下


#### 边框样式
`border-style` | `border-top-style` | `border-right-style` | `border-bottom-style` | `border-left-style`
> - `none` - 定义无边框。
> - `hidden` - 与 `none` 相同。不过应用于表时除外，对于表，hidden 用于解决边框冲突
> - `dotted` - 定义点状边框。在大多数浏览器中呈现为实线
> - `dashed` - 定义虚线，在大多数浏览器中呈现为实线
> - `solid` - 定义实线
> - `double` - 定义双线，双线的宽度等于 `border-width` 的值
> - `groove` - 定义 3D 凹槽边框（沟槽边框），其效果取决于 `border-color` 的值
> - `ridge` - 定义 3D 垄状边框（脊边框），其效果取决于 `border-color` 的值
> - `inset` - 定义 3D 嵌入边框，其效果取决于 `border-color` 的值
> - `outset` - 定义 3D 突出边框，其效果取决于 `border-color` 的值
> - `inherit` - 规定应该从父元素继承边框样式
