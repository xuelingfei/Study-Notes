## CSS 定位


- [position 属性](#position-属性)  
  - [static](#static)  
  - [fixed](#fixed)  
  - [relative](#relative)  
  - [absolute](#absolute)  
  - [sticky](#sticky)  
- [z-index 属性](#z-index-属性)  


### position 属性


#### static
HTML 元素的默认值，即没有定位，遵循正常的文档流对象。  
静态定位的元素不会受到 top, bottom, left, right影响。


#### fixed
元素的位置相对于浏览器窗口是固定位置，即使窗口是滚动的它也不会移动。  
注意：`fixed` 定位使元素的位置与文档流无关，因此不占据空间。`fixed` 定位的元素和其他元素重叠。


#### relative
相对定位。相对定位元素的定位是相对其正常位置，且移动相对定位元素，它原本所占的空间不会改变。
相对定位元素经常被用来作为绝对定位元素的容器块。


#### absolute
绝对定位。绝对定位的元素的位置相对于最近的已定位父元素，如果元素没有已定位的父元素，那么它的位置相对于 `<html>`。  
注意：`absolute` 定位使元素的位置与文档流无关，因此不占据空间。`absolute` 定位的元素和其他元素重叠。


#### sticky
粘性定位。粘性定位的元素基于用户的滚动位置来定位，在 `position: relative` 与 `position: fixed` 定位之间切换。它的行为就像 `position: relative`，而当页面滚动超出目标区域时，它的表现就像 `position:fixed`，会固定在目标位置。元素定位表现为在跨越特定阈值前为相对定位，之后为固定定位。这个特定阈值指的是 `top`，`right`，`bottom` 或 `left` 之一，换言之，指定 `top`，`right`，`bottom` 或 `left` 四个阈值其中之一，才可使粘性定位生效。否则其行为与相对定位相同。


### z-index 属性
z-index属性指定了一个元素的堆叠顺序（哪个元素应该放在前面，或后面），具有更高堆叠顺序的元素总是在较低的堆叠顺序元素的前面。  
注意：如果两个定位元素重叠，没有指定z-index，最后定位在HTML代码中的元素将被显示在最前面。