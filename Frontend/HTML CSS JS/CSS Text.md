## CSS 文本


- [text-decoration 属性](#text-decoration-属性)  
  - [使用说明](#使用说明)  
  - [语法](#语法)  
  - [属性值](#属性值)  


### text-decoration 属性


#### 使用说明
`text-decoration` 属性规定添加到文本的修饰，下划线、上划线、删除线等。  
`text-decoration` 属性是 `text-decoration-line | text-decoration-color | text-decoration-style` 三种属性的简写。


#### 语法
- CSS 语法
```text
text-decoration-line: none|underline|overline|line-through|initial|inherit;
text-decoration-color: color|initial|inherit;
text-decoration-style: solid|double|dotted|dashed|wavy|initial|inherit;
text-decoration: line color style;  /*无序，line 必需*/
```
- JavaScript 语法
```js
object.style.textDecorationLine="overline"
object.style.textDecorationColor="red"
object.style.textDecorationStyle="wavy"
object.style.textDecoration="overline"
```


#### 属性值
- `text-decoration-line`
> - `none` - 默认值。规定文本修饰没有线条。
> - `underline` - 规定文本的下方将显示一条线。
> - `overline` - 规定文本的上方将显示一条线。
> - `line-through` - 规定文本的中间将显示一条线。
> - `initial` - 设置该属性为它的默认值。
> - `inherit` - 从父元素继承该属性。
- `text-decoration-color`
> - `color` - 规定文本修饰的颜色。
> - `initial` - 设置该属性为它的默认值。
> - `inherit` - 从父元素继承该属性。
- `text-decoration-style`
> - `solid` - 默认值。线条将显示为单线。
> - `double` - 线条将显示为双线。
> - `dotted` - 线条将显示为点状线。
> - `dashed` - 线条将显示为虚线。
> - `wavy` - 线条将显示为波浪线。
> - `initial` - 设置该属性为它的默认值。
> - `inherit` - 从父元素继承该属性。
- `text-decoration` 全局值
> - `none` - 默认。定义标准的文本。
> - `initial` - 设置该属性为它的默认值。
> - `inherit` - 规定应该从父元素继承 `text-decoration` 属性的值。
> - `unset` - 设置该属性为其父元素的值（如果有继承）或其初始值。


注意：`text-decoration-line` 属性可以同时使用多个值，比如 `underline` 和 `overline`，来在文本的上方和下方都显示线条；只有在带有可见的 `text-decoration` 的元素上，`text-decoration-color` 属性才起作用。