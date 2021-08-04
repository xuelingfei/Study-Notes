#### pip常用命令
  - python -m pip install --upgrade pip
Windows下更新pip版本
  - pip show openpyxl
显示openpyxl的信息

  - pip freeze > requirements.txt
生成Python环境下的所有类库
  - pip install pipreqs
    pipreqs path
生成当前项目中用到的类库
  - pip install -r requirements.txt
安装项目所需要的类库

#### pip安装超时报错解决办法：
  - pip --default-timeout=1000 install pandas

#### pip安装opencv
  - pip install opencv-python

#### numpy报错: ValueError: numpy.ufunc size changed, may indicate binary incompatibility.
  - 应是numpy版本过低的缘故
    1.pip install --upgrade numpy
    2.pip --default-timeout=100 install --user numpy
或者pip --default-timeout=100 install --upgrade numpy


#### 换源安装
pip install pandas -i http://pypi.douban.com/simple[ --trusted-host pypi.douban.com]
pip install numpy==1.15.4 -i https://mirrors.aliyun.com/pypi/simple/



使用pip freeze
pip freeze > requirements.txt
这种方式是把整个环境中的包都列出来了，如果是在虚拟环境中可以使用。
通常情况下我们只需要导出当前项目的requirements.txt，这时候就推荐pipreqs了
使用 pipreqs
这个工具是个好帮手，可以通过对项目目录的扫描，自动发现使用了那些类库，自动生成依赖清单,只生成项目相关的依赖到requirements.txt
安装
pip install pipreqs
使用
使用也很简单 pipreqs 路径名
此处直接进到项目根目录，所以是./
pipreqs ./
可能会有个别包漏掉,还得手工再解决一下,不过至少大头的依赖都已经列出来了

报错

File "c:\users\devtao\appdata\local\programs\python\python36-32\lib\site-packages\pipreqs\pipreqs.py", line 341, in init
    extra_ignore_dirs=extra_ignore_dirs)
  File "c:\users\devtao\appdata\local\programs\python\python36-32\lib\site-packages\pipreqs\pipreqs.py", line 75, in get_all_imports
    contents = f.read()
UnicodeDecodeError: 'gbk' codec can't decode byte 0xa6 in position 186: illegal multibyte sequence


若出现类似上边的报错UnicodeDecodeError: ‘gbk’ codec can’t decode byte 0xa6 in position 186: illegal multibyte sequence
直接修改pipreqs.py的75行，将encoding改为’utf-8’

使用依赖库(安装requirements.txt)
pip install -r requirements.txt




执行 pipreqs 命令
# Successfully saved requirements file in /home/project/location/requirements.txt
$ pipreqs /home/project/location
四、项目执行

    当前项目根目录下，执行 pipreqs 命令，不加 --encoding=utf8 会出现编码问题

> pipreqs ./ --encoding=utf-8
> pipreqs . --encoding=utf-8

    强制执行命令 --force ，覆盖原有的 requirements.txt 文件

> pipreqs ./ --encoding=utf-8 --force


作者：飞行睿客
链接：https://www.jianshu.com/p/5c30f7c5aa34
来源：简书
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。

