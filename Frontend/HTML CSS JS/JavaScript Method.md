## JS 方法


- [setTimeout()](#setTimeout)  
- [setInterval()](#setInterval)  
- [parseInt()](#parseInt)  
- [parseFloat()](#parseFloat)  


#### setTimeout
用于在指定的毫秒数后调用函数或计算表达式。

- 语法
```text
setTimeout(code, milliseconds, param1, param2, ...)
setTimeout(function, milliseconds, param1, param2, ...)
```

- 参数
> - code/function - 必需，要调用一个代码串，也可以是一个函数
> - milliseconds - 可选，执行或调用 code/function 需要等待的时间，以毫秒计，默认为 0
> - param1, param2, ... - 可选，传给执行函数的其他参数（IE9 及其更早版本不支持该参数）

- 返回值

返回一个 ID（数字），可以将这个 ID 传递给 `clearTimeout()` 来取消执行。

- 示例
```js
setTimeout('test()', 2000);  // 2000 毫秒后执行 test() 函数，只执行一次
```


#### setInterval
按照指定的周期（以毫秒计）来调用函数或计算表达式。
setInterval() 方法会不停地调用函数，直到 clearInterval() 被调用或窗口被关闭。由 setInterval() 返回的 ID 值可用作 clearInterval() 方法的参数。

- 语法
```text
setInterval(code, milliseconds, param1, param2, ...)
setInterval(function, milliseconds, param1, param2, ...)
```

- 参数
> - code/function - 必需，要调用一个代码串，也可以是一个函数
> - milliseconds - 必需，周期性执行或调用 code/function 之间的时间间隔，以毫秒计
> - param1, param2, ... - 可选，传给执行函数的其他参数（IE9 及其更早版本不支持该参数）

- 返回值  
返回一个 ID（数字），可以将这个 ID 传递给 `clearInterval()`，`clearTimeout()` 来取消执行。

- 示例
```js
setInterval('alert("Hello");', 3000);  // 每三秒（3000 毫秒）弹出 "Hello"
```


#### parseInt
解析字符串，并返回一个整数。

- 语法
```text
parseInt(string, radix)
```

- 参数
> - string -- 必需。要被解析的字符串。
> - radix -- 可选。表示要解析的数字的基数。该值介于 2 ~ 36 之间。

注意：
1. 当参数 radix 的值为 0，或没有设置该参数时，parseInt() 会根据 string 来判断数字的基数：  
    - 如果 string 以 "0" 开头，旧的浏览器默认使用八进制基数。ECMAScript 5 及以后，默认的是十进制的基数。
    - 如果 string 以 "0x" 开头，parseInt() 会把 string 的其余部分解析为十六进制的整数。
    - 如果 string 以 "1" ~ "9" 开头，parseInt() 将把它解析为十进制的整数。

2. 只返回字符串中第一个数字。
3. 开头和结尾的空格是允许的。
4. 如果字符串的第一个字符不能被转换为数字，那么 parseInt() 会返回 NaN。

- 示例
```js
parseInt("10")  // 10
parseInt("10.33")  // 10
parseInt("34 45 66")  // 34
parseInt(" 60 ")  // 60
parseInt("40 years")  // 40
parseInt("He was 40")  // NaN

parseInt("10", 10)  // 10
parseInt("010")  // 10
parseInt("10", 8)  // 8
parseInt("0x10")  // 16
parseInt("10", 16)  // 16
```


#### parseFloat
解析字符串，并返回一个浮点数。

- 语法
```text
parseFloat(string)
```

- 参数
> - string -- 必需。要被解析的字符串。

注意：
1. 只返回字符串中第一个数字。
2. 开头和结尾的空格是允许的。
3. 如果字符串的第一个字符不能被转换为数字，那么 parseFloat() 会返回 NaN。

- 示例
```js
parseFloat("10")  // 10
parseFloat("10.33")  // 10.33
parseFloat("34 45 66")  // 34
parseFloat(" 60 ")  // 60
parseFloat("40 years")  // 40
parseFloat("He was 40")  // NaN
```