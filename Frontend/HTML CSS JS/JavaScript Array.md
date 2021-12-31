# JavaScript Array

<!-- markdown="1" is required for GitHub Pages to render the TOC properly. -->
<details markdown="1">
  <summary>目录</summary>

- [forEach()](#forEach)
- [filter()](#filter)
- [map()](#map)
</details>

# forEach()

1. 定义和用法  
    forEach() 方法用于调用数组的每个元素，并将元素传递给回调函数。

   注意：

   - forEach() 对于空数组是不会执行回调函数的。
   - forEach() 会不会改变原始数组取决于数组元素的数据类型：  
     在 forEach() 回调函数中改变当前元素时，  
     如果当前元素是基本类型，则原始数组对应元素不会改变；  
     如果当前元素是引用类型，则原始数组对应元素也会相应改变。

2. 语法

   ```js
   array.forEach(function(currentValue, index, arr), thisValue)
   ```

3. 参数说明
   | 参数 | 是否必须 | 描述 |
   | -- | -- | -- |
   | function(currentValue, index, arr) | 必需 | 数组中每个元素需要调用的函数 |
   | &nbsp;&nbsp; currentValue | 必需 | 当前元素 |
   | &nbsp;&nbsp; index | 可选 | 当前元素的索引值 |
   | &nbsp;&nbsp; arr | 可选 | 当前元素所属的数组对象 |
   | thisValue | 可选 | 传递给函数的值一般用 "this" 值，<br/>如果这个参数为空， "undefined" 会传递给 "this" 值 |

4. 返回值  
   undefined

5. 实例

   ```js
   let arr = [{ name: "a" }, { name: "b" }]
   arr.forEach((el, index) => {
     el.id = index
   })
   console.log(arr) // [{ name: "a", id: 0 }, { name: "b", id: 1 }]
   ```

6. forEach() 的 continue 和 break  
   forEach() 本身是不支持的 continue 与 break 语句的，但可以通过 some 和 every 使用 return 语句实现。
   - continue 实现
     ```js
     var arr = [1, 2, 3, 4, 5]
     arr.forEach(function (item) {
       if (item === 3) {
         return // 3 的元素跳过
       }
       console.log(item)
     }) // 1 /n 2 /n 4 /n 5
     ```
     ```js
     var arr = [1, 2, 3, 4, 5]
     arr.some(function (item) {
       if (item === 2) {
         return // 不能为 return false
       }
       console.log(item)
     }) // 1 /n 2 /n 4 /n 5 /n false
     ```
   - break 实现
     ```js
     var arr = [1, 2, 3, 4, 5]
     arr.every(function (item) {
       console.log(item)
       return item !== 3
     }) // 1 /n 2 /n 3 /n false
     ```

## filter()

1. 定义和用法
   filter() 方法创建一个新的数组，新数组中的元素是通过检查指定数组中符合条件的所有元素。

注意：

- filter() 不会对空数组进行检测（空数组用 `filter()` 方法不会报错）。
- filter() 不会改变原始数组。

2. 语法

```js
array.filter(function(currentValue, index, arr), thisValue)
```

3. 参数说明
   | 参数 | 是否必须 | 描述 |
   | -- | -- | -- |
   | function(currentValue, index,arr) | 必须 | 函数，数组中的每个元素都会执行这个函数 |
   | &nbsp;&nbsp; currentValue | 必须 | 当前元素的值 |
   | &nbsp;&nbsp; index | 可选 | 当前元素的索引值 |
   | &nbsp;&nbsp; arr | 可选 | 当前元素属于的数组对象 |
   | thisValue | 可选 | 对象，作为该执行回调时使用，传递给函数，用作 "this" 的值，<br/>如果省略了 thisValue ，"this" 的值为 "undefined" |

4. 返回值

   返回数组，包含了符合条件的所有元素。如果没有符合条件的元素则返回空数组。

5. 实例

   筛选数组 ageList 中所有小于 18 的元素:

   ```js
   let ageList = [32, 33, 16, 40, 14]
   let nonageList = ageList.filter((item) => item < 18)
   console.log(nonageList) // [16, 14]
   ```

## map()

1. 定义和用法

   map() 方法返回一个新数组，数组中的元素为原始数组元素调用函数处理后的值。map() 方法按照原始数组元素顺序依次处理元素。

   注意：

   - map() 不会对空数组进行检测（空数组用 `map()` 方法不会报错）。
   - map() 不会改变原始数组。

2. 语法

   ```js
   array.map(function(currentValue, index, arr), thisValue)
   ```

3. 参数说明
   | 参数 | 是否必须 | 描述 |
   | -- | -- | -- |
   | function(currentValue, index,arr) | 必须 | 函数，数组中的每个元素都会执行这个函数 |
   | &nbsp;&nbsp; currentValue | 必须 | 当前元素的值 |
   | &nbsp;&nbsp; index | 可选 | 当前元素的索引值 |
   | &nbsp;&nbsp; arr | 可选 | 当前元素属于的数组对象 |
   | thisValue | 可选 | 对象，作为该执行回调时使用，传递给函数，用作 "this" 的值。<br>如果省略了 thisValue，或者传入 null、undefined，那么回调函数的 this 为全局对象。 |

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
