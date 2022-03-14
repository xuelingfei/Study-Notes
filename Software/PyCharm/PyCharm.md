# PyCharm

<!-- markdown="1" is required for GitHub Pages to render the TOC properly. -->
<detail markdown="1">
<summary>目录</summary>

- [PyCharm 插件](#PyCharm-插件)
- [Python 文件模板](Python-文件模板)
- [Enable Django Support](#Enable-Django-Support)
- [选择性忽略代码风格警告信息](#选择性忽略代码风格警告信息)
- [Too broad exception](#Too-broad-exception)
- [Cannot resolve directory 'static'](#Cannot-resolve-directory-static)

</detail>

## PyCharm 插件

- CodeGlance  
  代码缩略图
- Statistic  
  代码统计

## Python 文件模板

```py
# -*- coding: utf-8 -*-
"""
@File    : ${NAME}.py
@Author  : ${USER}
@Time    : ${DATE} ${TIME}
@Software: ${PRODUCT_NAME}
@Desc    :
"""

```

## Enable Django Support

在配置 Djange Tests 时，需要设置 Enable Django Support，打开 File -> Settings -> Languages & Frameworks -> Django，勾选 Enable Django Support 并设置相关内容如下：

> - Django project root：选择包含 manage.py 文件的目录
> - Settings：选择 settings.py 文件

## 选择性忽略代码风格警告信息

1. 方法一  
   将鼠标移到提示的地方，按 <kbd>Alt</kbd>+<kbd>Enter</kbd>，选择忽略（Ignore）这个错误即好。

2. 方法二

- PEP8 coding style violation  
  打开 File -> Settings -> Editor -> Inspections -> Python -> PEP8 coding style violation，在右下角的 Ignore errors 里可以添加忽略的警告信息 ID，部分 ID 及其对应警告信息如下：
  > - E501(^) -- line too long (82 > 79 characters)
- PEP 8 naming convention violation  
  打开 File -> Settings -> Editor -> Inspections -> Python -> PEP 8 naming convention violation，在右下角的 Ignore errors 里可以添加忽略的警告信息 ID，部分 ID 及其对应警告信息如下：
  > - N802 -- function name should be lowercase -- 函数名应该是小写字母
  > - N803 -- argument name should be lowercase -- 参数名应该是小写字母
  > - N806 -- variable in function should be lowercase -- 变量名应该是小写字母

## Too broad exception

- 问题描述：PEP8: do not use bare 'except'; Too broad exception
- 解决方法：

  1. 方法一  
     在 `try` 语句前加入 `# noinspection PyBroadException`

     ```python
     # noinspection PyBroadException
     try:
         pass
     except Exception:
         pass
     ```

  2. 方法二  
     在 `except` 语句中抓取错误信息

     ```python
     try:
         pass
     except Exception as err:
         print(err)
         pass
     ```

#### Cannot resolve directory 'static'

- 问题描述：PyCharm 报错 Cannot resolve directory 'static'
- 解决方法：File -> Settings -> Languages & Frameworks -> Python Template Languages，Template language 选择 Django
