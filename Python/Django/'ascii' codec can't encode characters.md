 Python3 出现'ascii' codec can't encode characters问题

当使用urllib.request.urlopen打开包含中文的链接时报错：

from urllib import request

url = 'https://baike.baidu.com/item/糖尿病'
response = request.urlopen(url)

提示错误：UnicodeEncodeError: ‘ascii’ codec can’t encode characters in position 10-12: ordinal not in range(128)

参考https://www.zhihu.com/question/22899135 和https://blog.csdn.net/sijiaqi11/article/details/78449770 得知，
urllib.request.urlopen不支持中英文混合的字符串。
应使用urllib.parse.quote进行转换。

# coding=utf-8
from urllib import request
from urllib.parse import quote
import string

url = 'https://baike.baidu.com/item/糖尿病'
s = quote(url,safe=string.printable)
response = request.urlopen(s)


UnicodeEncodeError: 'ascii' codec can't encode characters in position 0-22: ordinal not in range(128)

header['Content-Type'] = 'application/json'
req = request.Request(url + params, headers=header, method='GET')  # GET方法
res = request.urlopen(req).read()
res = res.decode('utf-8')
return res
改为
header['Content-Type'] = 'application/json'
rurl = parse.quote(url + params, safe=string.printable)
req = request.Request(rurl, headers=header, method='GET')  # GET方法
res = request.urlopen(req).read()
res = res.decode('utf-8')
return res