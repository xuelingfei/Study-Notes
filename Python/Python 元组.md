# Python 元组

<!-- markdown="1" is required for GitHub Pages to render the TOC properly. -->
<detail markdown="1">
<summary>目录</summary>

- [定义](#定义)
- [tuple 函数](#tuple-函数)
- [创建元组](创建元组)

</detail>

## 定义

【a = tuple()或 a = ()，eggs = ('hello', 42, 0.5)】：
Python 用圆括号()来标识元组，元组即不可变的列表。
如果元组中只有一个值，则应该在括号内该值的后面跟上一个逗号，表明这种情况，如('hello',)。否则 Python 将认为， 你只是在普通括号内输入了一个值。

## tuple 函数

1. 描述
   tuple 函数将可迭代系列（如字符串、列表、字典、集合）转换为元组。

2. 语法

```
tuple(iterable)
```

3. 参数

- iterable -- 要转换为元组的可迭代序列。

4. 实例

```py
>>> tuple({'a': 1, 'b': 2, 'c': 3})  # 将字段转换为元组时，只保留键
('a', 'b', 'c')
```

## 创建元组

1. 创建空元组

```py
>>> ()
>>> tuple()
```

2. 通过 tuple 函数将可迭代序列转换为元组

```py
('a', 'b', 'c')
>>> tuple('abc')
>>> tuple(['a', 'b', 'c'])
>>> tuple({'a': 1, 'b': 2, 'c': 3})
>>> tuple(set('abc'))
```
