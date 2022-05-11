# Python 字典

<!-- markdown="1" is required for GitHub Pages to render the TOC properly. -->
<details markdown="1">
<summary>目录</summary>

- [定义](#定义)
- [dict 函数](#dict-函数)
- [创建字典](#创建字典)
- [字典伪切片](#字典伪切片)

</details>

## 定义

Python 用放在花括号 `{}` 中的一系列 键-值对 表示字典。

## dict 函数

1. 描述  
   dict 函数用于创建一个字典。

2. 语法

   ```
   dict(**kwarg)
   dict(mapping, **kwarg)
   dict(iterable, **kwarg)
   ```

3. 参数

   - `**kwargs` -- 关键字。
   - `mapping` -- 元素的容器，映射类型（Mapping Types）是一种关联式的容器类型，它存储了对象与对象之间的映射关系。
   - `iterable` -- 可迭代对象。

4. 示例

   ```py
   >>> dict()  # 空字典
   {}
   >>> dict(a='a', b='b', t='t')  # 关键字
   {'a': 'a', 'b': 'b', 't': 't'}
   >>> dict(zip(['one', 'two', 'three'], [1, 2, 3]))  # 映射函数
   {'one': 1, 'two': 2, 'three': 3}
   >>> dict([('one', 1), ('two', 2), ('three', 3)])  # 可迭代对象
   {'one': 1, 'two': 2, 'three': 3}
   >>> dict({'x': 4, 'y': 5}, z=8)  # 关键字参数会被传递
   {'x': 4, 'y': 5, 'z': 8}
   ```

## 创建字典

1. 创建空字典

   ```py
   >>> {}
   >>> dict()
   ```

2. 通过 `dict.fromkeys()` 创建

   ```py
   >>> dict.fromkeys('abc')
   {'a': None, 'b': None, 'c': None}
   >>> dict.fromkeys('abc', 0)
   {'a': 0, 'b': 0, 'c': 0}
   >>> dict.fromkeys(['abc'], 0)
   {'abc': 0}
   ```

3. 直接赋值创建

   ```py
   >>> {'a': 1, 'b': 2, 'c': 3}
   ```

4. 通过关键字 `dict` 和关键字参数创建

   ```py
   >>> dict(a = 1, b = 2, c =3)
   {'a': 1, 'b': 2, 'c': 3}
   ```

5. 通过字典推导式创建

   ```py
   >>> {i: i ** 3 for i in range(3)}
   {0: 0, 1: 1, 2: 8}
   ```

6. 通过二元组列表创建

   ```py
   >>> dict([('a', 1), ('b', 2), ('c', 3)])
   {'a': 1, 'b': 2, 'c': 3}
   ```

7. `dict` 和 `zip` 结合创建

   ```py
   >>> dict(zip('abc', [1, 2, 3]))
   {'a': 1, 'b': 2, 'c': 3}
   >>> dict(zip(['index', 'name', 'age'], [1, 'Tom', 23]))
   {'index': 1, 'name': 'Tom', 'age': 23}

   # 变体
   >>> data_list = ['x', 1, 'y', 2, 'z', 3]
   >>> dict(zip(data_list[::2], data_list[1::2]))
   {'x': 1, 'y': 2, 'z': 3}
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
