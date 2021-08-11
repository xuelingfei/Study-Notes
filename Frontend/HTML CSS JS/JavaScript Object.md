# JavaScript Object

<!-- markdown="1" is required for GitHub Pages to render the TOC properly. -->
<details markdown="1">
  <summary>目录</summary>

- [Object.assign()](#objectassign)
</details>

## Object.assign()

1. 定义和用法

   Object.assign() 方法用于将所有可枚举属性的值从一个或多个源对象分配到目标对象。它将返回目标对象。

   注意：

   - 如果目标对象中的属性具有相同的键，则属性将被源对象中的属性覆盖。后面的源对象的属性将类似地覆盖前面的源对象的属性。

   - `Object.assign()` 方法只会拷贝源对象自身的并且可枚举的属性到目标对象。String 类型和 Symbol 类型的属性都会被拷贝。针对深拷贝，需要使用其他办法，因为 Object.assign()拷贝的是（可枚举）属性值。假如源值是一个对象的引用，它仅仅会复制其引用值。

   - 在出现错误的情况下，例如，如果属性不可写，会引发 TypeError，如果在引发错误之前添加了任何属性，则可以更改 target 对象。

   - 注意，Object.assign 不会在那些 source 对象值为 null 或 undefined 的时候抛出错误。

2. 语法

   ```js
   Object.assign(target, ...sources)
   ```

3. 参数说明

   | 参数    | 描述     |
   | ------- | -------- |
   | target  | 目标对象 |
   | sources | 源对象   |

4. 返回值

   目标对象。

5. 示例

   复制一个对象：

   ```js
   const obj = { a: 1 }
   const copy = Object.assign({}, obj)
   console.log(copy) // { a: 1 }
   ```

   深拷贝问题：

   ```js
   let obj1 = { a: 0, b: { c: 0 } }
   let obj2 = Object.assign({}, obj1)
   console.log(JSON.stringify(obj2))
   // { a: 0, b: { c: 0}}

   obj1.a = 1
   console.log(JSON.stringify(obj1))
   // { a: 1, b: { c: 0}}
   console.log(JSON.stringify(obj2))
   // { a: 0, b: { c: 0}}

   obj2.a = 2
   console.log(JSON.stringify(obj1))
   // { a: 1, b: { c: 0}}
   console.log(JSON.stringify(obj2))
   // { a: 2, b: { c: 0}}

   obj2.b.c = 3
   console.log(JSON.stringify(obj1))
   // { a: 1, b: { c: 3}}
   console.log(JSON.stringify(obj2))
   // { a: 2, b: { c: 3}}

   // Deep Clone
   obj1 = { a: 0, b: { c: 0 } }
   let obj3 = JSON.parse(JSON.stringify(obj1))
   obj1.a = 4
   obj1.b.c = 4
   console.log(JSON.stringify(obj3))
   // { a: 0, b: { c: 0}}
   ```

   合并对象：

   ```js
   const o0 = {}
   const o1 = { a: 1, b: 1, c: 1 }
   const o2 = { b: 2, c: 2 }
   const o3 = { c: 3 }

   const obj = Object.assign(o0, o1, o2, o3)
   console.log(obj) // { a: 1, b: 2, c: 3 }
   console.log(o0) // { a: 1, b: 2, c: 3 }, 注意目标对象自身也会改变
   console.log(o1) // { a: 1, b: 1, c: 1 }, 源对象不变
   ```

   继承属性和不可枚举属性是不能拷贝的：

   ```js
   const obj = Object.create(
     { foo: 1 }, // foo 是个继承属性
     {
       bar: {
         value: 2, // bar 是个不可枚举属性
       },
       baz: {
         value: 3, // baz 是个自身可枚举属性
         enumerable: true,
       },
     }
   )

   const copy = Object.assign({}, obj)
   console.log(copy) // { baz: 3 }
   ```

   原始类型会被包装为对象：

   ```js
   const v1 = "abc"
   const v2 = true
   const v3 = 10
   const v4 = Symbol("foo")

   const obj = Object.assign({}, v1, null, v2, undefined, v3, v4)
   // 原始类型会被包装，null 和 undefined 会被忽略。
   // 注意，只有字符串的包装对象才可能有自身可枚举属性。
   console.log(obj) // { "0": "a", "1": "b", "2": "c" }
   ```

   异常会打断后续拷贝任务：

   ```js
   const target = Object.defineProperty({}, "foo", {
     value: 1,
     writable: false,
   }) // target 的 foo 属性是个只读属性。

   Object.assign(target, { bar: 2 }, { foo2: 3, foo: 3, foo3: 3 }, { baz: 4 })
   // TypeError: "foo" is read-only
   // 注意这个异常是在拷贝第二个源对象的第二个属性时发生的。

   console.log(target.bar) // 2，说明第一个源对象拷贝成功了。
   console.log(target.foo2) // 3，说明第二个源对象的第一个属性也拷贝成功了。
   console.log(target.foo) // 1，只读属性不能被覆盖，所以第二个源对象的第二个属性拷贝失败了。
   console.log(target.foo3) // undefined，异常之后 assign 方法就退出了，第三个属性是不会被拷贝到的。
   console.log(target.baz) // undefined，第三个源对象更是不会被拷贝到的。
   ```
