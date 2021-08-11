# JavaScript Array

<!-- markdown="1" is required for GitHub Pages to render the TOC properly. -->
<details markdown="1">
  <summary>目录</summary>

- [filter()](#filter)
- [map()](#map)
</details>

## filter()

1. 定义和用法

   filter() 方法创建一个新的数组，新数组中的元素是通过检查指定数组中符合条件的所有元素。

   注意：

   - filter() 不会对空数组进行检测。
   - filter() 不会改变原始数组。

2. 语法

   ```js
   array.filter(function(currentValue, index, arr), thisValue)
   ```

3. 参数说明

   | 参数                              | 是否必须 | 描述                                                                                                            |
   | --------------------------------- | -------- | --------------------------------------------------------------------------------------------------------------- |
   | function(currentValue, index,arr) | 必须     | 函数，数组中的每个元素都会执行这个函数                                                                          |
   | &nbsp;&nbsp; currentValue         | 必须     | 当前元素的值                                                                                                    |
   | &nbsp;&nbsp; index                | 可选     | 当前元素的索引值                                                                                                |
   | &nbsp;&nbsp; arr                  | 可选     | 当前元素属于的数组对象                                                                                          |
   | thisValue                         | 可选     | 对象，作为该执行回调时使用，传递给函数，用作 "this" 的值，<br/>如果省略了 thisValue ，"this" 的值为 "undefined" |

4. 返回值

   返回数组，包含了符合条件的所有元素。如果没有符合条件的元素则返回空数组。

5. 实例

   筛选数组 ageList 中所有小于 18 的元素:

   ```js
   let ageList = [32, 33, 16, 40, 14]
   let nonageList = ageList.filter((item) => item < 18)
   console.log(nonageList)
   // [16, 14]
   ```

## map()

1. 定义和用法

   map() 方法返回一个新数组，数组中的元素为原始数组元素调用函数处理后的值。map() 方法按照原始数组元素顺序依次处理元素。

   注意： map() 不会对空数组进行检测。map() 不会改变原始数组。

2. 语法

   ```js
   array.map(function(currentValue, index, arr), thisValue)
   ```

3. 参数说明

   | 参数                              | 是否必须 | 描述                                                                                                                                           |
   | --------------------------------- | -------- | ---------------------------------------------------------------------------------------------------------------------------------------------- |
   | function(currentValue, index,arr) | 必须     | 函数，数组中的每个元素都会执行这个函数                                                                                                         |
   | &nbsp;&nbsp; currentValue         | 必须     | 当前元素的值                                                                                                                                   |
   | &nbsp;&nbsp; index                | 可选     | 当前元素的索引值                                                                                                                               |
   | &nbsp;&nbsp; arr                  | 可选     | 当前元素属于的数组对象                                                                                                                         |
   | thisValue                         | 可选     | 对象，作为该执行回调时使用，传递给函数，用作 "this" 的值。<br>如果省略了 thisValue，或者传入 null、undefined，那么回调函数的 this 为全局对象。 |

4. 返回值

   返回一个新数组，数组中的元素为原始数组元素调用函数处理后的值。

5. 实例

   返回一个数组，数组中元素为原始数组的平方根:

   ```js
   let numList = [4, 9, 16, 25]
   let sqrtList = numList.map(Math.sqrt)
   console.log(sqrtList)
   // [2, 3, 4, 5]
   ```
