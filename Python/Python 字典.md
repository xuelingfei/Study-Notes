# Python 字典

<!-- markdown="1" is required for GitHub Pages to render the TOC properly. -->
<detail markdown="1">
<summary>目录</summary>

- [定义](#定义)
- [dict 函数](#dict-函数)
- [创建字典的几种方式](#创建字典的几种方式)
- [字典伪切片](#字典伪切片)

</detail>

## 定义

Python 用放在花括号 `{}` 中的一系列 键-值对 表示字典。

## dict 函数

## 创建字典的几种方式

1. 字典初始化

```py
# 创建空字典
ctx = {}
# 或
ctx = dict()

# 通过 dict.fromkeys() 初始化字典
ctx = dict.fromkeys('abc')  # {'a': None, 'b': None, 'c': None}
ctx = dict.fromkeys('abc', 0)  # {'a': 0, 'b': 0, 'c': 0}
ctx = dict.fromkeys(['abc'], 0)  # {'abc': 0}
```

2. 直接赋值创建

```py
ctx = {'a': 1, 'b': 2, 'c': 3}
```

3. 通过关键字 `dict` 和关键字参数创建

```py
ctx = dict(a = 1, b = 2, c =3)
```

4. 通过字典推导式创建

```py
ctx = {i: i ** 3 for i in range(3)}
```

5. 通过二元组列表创建

```py
data_list = [('a', 1), ('b', 2), ('c', 3)]
ctx = dict(data_list)
```

6. `dict` 和 `zip` 结合创建

```py
ctx = dict(zip('abc', [1, 2, 3]))

key_list = ['index', 'name', 'age']
value_list = [1, 'Tom', 23]
ctx = dict(zip(name_list, value_list))

# 变体
data_list = ['x', 1, 'y', 2, 'z', 3]
ctx = dict(zip(data_list[::2], data_list[1::2]))
```

## 字典伪切片

```py
def dict_slice(dict_obj, start, end):
    """
    字典伪切片
    :param dict_obj: dict，字典对象
    :param start: int，切片起点
    :param end: int，切片终点
    :return: dict，切片后的字典
    """
    dict_obj_slice = {k: dict_obj[k] for k in list(dict_obj.keys())[start:end]}
    return dict_obj_slice
```
