# Python 常用函数

<!-- markdown="1" is required for GitHub Pages to render the TOC properly. -->
<detail markdown="1">
<summary>目录</summary>

- [内置函数](#内置函数)
  - [help](#help)
- [外部函数](外部函数)

</detail>

## 内置函数

### help()

1. 语法

   ```
   help(thing)
   ```

2. 描述  
   打印 python 对象 thing 的帮助信息（pydoc.help），如 help(print), help('lambda').

### print()
1. 语法
    ```
    print(value, ..., sep=' ', end='\n', file=sys.stdout, flush=False)
    ```
2. 描述
Prints the values to a stream, or to sys.stdout by default.
Optional keyword arguments:

- file: a file-like object (stream); defaults to the current sys.stdout.
- sep: string inserted between values, default a space.
- end: string appended after the last value, default a newline.
- flush: whether to forcibly flush the stream.

3. 参数

- value -- 输出对象，可以一次输出多个，用","分隔。
- sep -- 用来间隔多个对象，默认值是一个空格。
- end -- 用来设定以什么结尾，默认值是换行符"\n"。
- file -- 要写入的文件对象。

注：print 默认调用 sys.stdout.write() 方法将输出打印到控制台，也可通过 file 参数将输出打印到其他文件中，如

```python
f = open('test.txt', 'a')
print('This is a test', file=f)
```

### input() 函数：

```python
input(prompt=None, /)
```

- Read a string from standard input. The trailing newline is stripped.
- The prompt string, if given, is printed to standard output without a trailing newline before reading input.
- If the user hits EOF (\*nix: Ctrl-D, Windows: Ctrl-Z+Return), raise EOFError.
- On \*nix systems, readline is used if available.
  <br/>
- 从标准输入中读取字符串，并删除尾随换行符。
- 如果给出了提示字符串 prompt，则在读取输入之前将其打印到标准输出而没有尾随换行符。
- 如果用户点击 EOF（\*nix：Ctrl-D，Windows：Ctrl-Z + Return），则引发 EOFError。
- 在\*nix 系统上，如果可用，则使用 readline。

### range() 函数：

```python
range([start, ]stop[, step])
```

参数：

- start -- 计数从 start 开始，默认是从 0 开始。例如：range(5)等价于 range(0, 5)；
- end -- 计数到 end 结束，但不包括 end。例如：range(0, 5)是[0, 1, 2, 3, 4]没有 5；
- step -- 步长，默认为 1。例如：range(0, 5)等价于 range(0, 5, 1)。

注：Python3.x 中 range() 函数返回的结果是一个整数序列的对象 <class 'range'>，而不是列表。

### len() 函数：

```python
len(obj)
```

返回对象（字符、列表、元组、字典等）的长度或项目个数（整型值）。

### max() 函数：

```python
max(obj)
```

返回给定对象的最大值，对象可以为序列、字符串、列表。

### min() 函数：

```python
min(obj)
```

返回给定对象的最小值，对象可以为序列、字符串、列表。<br/>
注：max(str)、min(str)不只判断字母，会判断字符串中的所有字符，按照字符在 unicode 中的编码值来决定大小。

### round() 函数：

```python
round(number, ndigits=None)
```

Round a number to a given precision in decimal digits. The return value is an integer if ndigits is omitted or None. Otherwise the return value has the same type as the number. 'ndigits' may be negative.

- 函数 round() 通常让小数精确到小数点后多少位，其中小数位数是由第二个实参 ndigits 指定的。
- 如果将 ndigits 指定为负数，round() 将圆整到 10 的 -ndigits 次方的整数倍。
- 如果省略 ndigits，则返回值为整数。
- 如果 ndigits 为 0，则 number 为整数时，返回原值；number 为小数时，圆整到整数位，小数位置 0 且保留一位。

### id() 函数：

```python
id(obj)
```

获取 obj 的内存地址(10 进制)

### zip() 函数：

```python
zip(iter1[, iter2[, ...]])
```

参数：<br>
&nbsp;&nbsp;&nbsp;&nbsp;iter1, 2, ... -- 迭代器。<br>
返回值：<br>
&nbsp;&nbsp;&nbsp;&nbsp;返回一个对象(zip object)。

1. zip() 函数将可迭代的对象作为参数，将对象中对应的元素打包成一个个元组，然后返回由这些元组组成的对象，这样做的好处是节约了不少的内存；
2. zip 对象可迭代，但其本身没有 length 属性，可使用 list()函数转换来输出列表；
3. 如果各个迭代器的元素个数不一致，则返回列表长度（用 list()函数转换后）与最短的对象相同；
4. 利用 \* 号操作符，可以将 zip 对象解压为元组，如 a1, a2 = zip(\*zip(a, b))。

```py
a = [1, 2, 3, 4]
b = ['x', 'y', 'x', 'w']
c = list(zip(a, b))
c[(1, 'x'), (2, 'y'), (3, 'x'), (4, 'w')]
```

```py
c = [(1, 'x'), (2, 'y'), (3, 'x'), (4, 'w')]
a, b = zip(*c)
a(1, 2, 3, 4)
b('x', 'y', 'x', 'w')
```

### enumerate() 函数：

```python
enumerate(sequence, [start=0])
```

参数

- sequence -- 一个序列、迭代器或其他支持迭代对象。
- start -- 下标起始位置。

返回值<br/>
&nbsp;&nbsp;&nbsp;&nbsp;返回 enumerate (枚举) 对象。

实例

```python
seq = ['one', 'two', 'three']
for i, element in enumerate(seq):
    print i, element
```

输出结果为：

```
0 one
1 two
2 three
```

### getattr() 函数：

```python
getattr(object, name[, default])
```

参数

- object -- 对象。
- name -- 字符串，对象属性。
- default -- 默认返回值，如果不提供该参数，在没有对应属性时，将触发 AttributeError。

返回值<br/>
&nbsp;&nbsp;&nbsp;&nbsp;返回对象属性值。

实例

```python
class A(object):
    bar = 1

a = A()
getattr(a, 'bar')  # 1
```

### setattr() 函数：

```python
setattr(object, name, value)
```

参数

> - object -- 对象。
> - name -- 字符串，对象属性。
> - value -- 属性值。

无返回值

实例

```python
class A(object):
    bar = 1

a = A()
getattr(a, 'bar')  # 获取属性 bar 值，1
setattr(a, 'bar', 5)  # 设置属性 bar 值
a.bar  # 5
```

## 外部函数

### random.randint(a, b)

函数求值为传递给它的两个整数之间的一个随机整数。

### sys.exit()

终止或退出程序。

### sys.getdefaultencoding()

获取系统默认的编码格式。

### copy.copy()

用来复制列表或字典这样的可变值，而不只是复制引用。

### copy.deepcopy()

如果要复制的列表或字典中包含了列表或字典，就使用 deepcopy() 函数来代替，deepcopy() 函数将同时复制它们内部的列表，此时使用 copy() 函数复制的是列表或字典的引用。

### time.sleep(t)

推迟调用线程的运行，可通过参数 t 指定推迟执行的秒数，表示进程挂起的时间。
