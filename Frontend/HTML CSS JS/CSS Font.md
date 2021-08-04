## CSS 字体


- [font-weight 属性](#font-weight-属性)  
  - [使用说明](#使用说明)  
  - [属性值](#属性值)  


### font-weight 属性


#### 使用说明
- 名称：`font-weight`
- 取值：`normal` | `bold` | `bolder` | `lighter` | `inherit` | `100` | `200` | `300` | `400` | `500` | `600` | `700` | `800` | `900`
- 初始：`normal`
- 适用于：所有元素
- 继承：是
- 百分比：不适用
- 媒介：视觉
- JavaScript 语法：`object.style.fontWeight="900"`


#### 属性值
> - `normal` - 默认值，定义标准的字符，与 `400` 相同
> - `bold` - 定义粗体字符，与 `700` 相同
> - `bolder` - 定义更粗的字符（指定外观的重量大于继承的值）
> - `lighter` - 定义更细的字符（指定外观的重量小于继承的值）
> - `inherit` - 规定应该从父元素继承字体的粗细

`font-weight` 属性设置字体中字形的重量，这取决于黑度等级或笔划粗细。这些有序排列中的每个值，表示至少与其起身拥有相同黑度的重量。其大致符合下列通用重量名称：
> - `100` - Thin
> - `200` - Extra Light (Ultra Light)
> - `300` - Light
> - `400` - Normal
> - `500` - Medium
> - `600` - Semi Bold (Demi Bold)
> - `700` - Bold
> - `800` - Extra Bold (Ultra Bold)
> - `900` - Black (Heavy)

注：通常一个特定的字体家族仅会包含少数的可用重量。若一个重量所指定的字形不存在，则应当使用相近重量的字形。通常，较重的重量会映射到更重的重量、较轻的重量会映射到更轻的重量。