# Note

<!-- markdown="1" is required for GitHub Pages to render the TOC properly. -->
<details markdown="1">
  <summary>目录</summary>

- [便笺](#便笺)
- [链接](#链接)
</details>

## 便笺

- Python 格式化时间报错：
  UnicodeEncodeError: 'locale' codec can't encode character '\u5e74' in position 2: Illegal byte sequence  
  import time
  print(time.strftime("%Y 年%m 月%d 日 %H 时%M 分%S 秒",time.localtime()))  
  报错原因为有中文字符，修改为下面代码即可  
  import time  
  print(time.strftime('%Y{y}%m{m}%d{d} %H{h}%M{f}%S{s}').format(y='年',m='月',d='日',h='时',f='分',s='秒'))

- python-pptx 从模板 ppt 文件导出 ppt 时报错'rId2'  
  原因是模板文件中存在复制粘贴来的图表  
  解决方法是右键图表选择另存为模板，然后从模板生成新的图表，删掉涉及的就图表

- vue 3.x 增加了 v-slot 的指令，去掉了原来的 slot，slot-scope 属性。  
  el-dropdown-menu 标签外面加上<template v-slot:dropdown> </el-dropdown-menu>

- python引用
    初始化
    date_list = timeUtil.getEveryDay(start_date_str, end_date_str)
    date_value = {date: None for date in date_list}
    ident_value_dict = {ident: date_value.copy() for ident in ident_list}

- js获取高度
1. $("#div_id").height(); // 获得的是该div本身的高度, (不包含padding,margin,border)
2. $("#div_id").outerHeight(); // 包含该div本身的高度, padding上下的高度, 以及border上下的高度(不包含margin的高度)
3. $("#div_id").outerHeight(true); // 包含该div本身的高度, 以及padding,border,margin上下的总高度

$("#divId").height(); //不包括内边距、边框或外边距
$("#divId").innerHeight();//包括内边距
$("#divId").outerHeight();//包括内边距和边框


## 链接

https://blog.csdn.net/qq_43561840/article/details/105137417

https://blog.csdn.net/weixin_44111864/article/details/106079486?utm_medium=distribute.pc_relevant.none-task-blog-BlogCommendFromMachineLearnPai2-1.channel_param&depth_1-utm_source=distribute.pc_relevant.none-task-blog-BlogCommendFromMachineLearnPai2-1.channel_param

https://blog.csdn.net/xiazeqiang2018/article/details/81190275

https://blog.csdn.net/JavaGreenhand520/article/details/107376395/

https://www.cnblogs.com/rogerwu/p/9274193.html

https://blog.csdn.net/weixin_43788115/article/details/103494316?utm_medium=distribute.pc_aggpage_search_result.none-task-blog-2~all~sobaiduend~default-2-103494316.nonecase&utm_term=elementui%E4%B8%8A%E4%BC%A0%E5%9B%BE%E7%89%87%20%E5%BF%85%E5%A1%AB

https://blog.csdn.net/liona_koukou/article/details/82998069?utm_medium=distribute.pc_aggpage_search_result.none-task-blog-2~all~first_rank_v2~rank_v25-1-82998069.nonecase&utm_term=elementui%E4%B8%8A%E4%BC%A0%E5%9B%BE%E7%89%87%20%E5%BF%85%E5%A1%AB

https://www.jianshu.com/p/4b6040f326f5

https://blog.csdn.net/weixin_41166682/article/details/106722754

https://www.runoob.com/js/js-obj-array.html

https://www.runoob.com/jsref/jsref-indexof-array.html

https://www.runoob.com/jsref/jsref-string-includes.html

https://www.runoob.com/jsref/jsref-includes.html

https://blog.csdn.net/weixin_43254676/article/details/89636887

https://segmentfault.com/q/1010000014179934

https://blog.csdn.net/weixin_46743083/article/details/107788275
、
、
https://blog.csdn.net/weixin_43743804/article/details/106691954?utm_medium=distribute.pc_relevant_t0.none-task-blog-BlogCommendFromMachineLearnPai2-1.channel_param&depth_1-utm_source=distribute.pc_relevant_t0.none-task-blog-BlogCommendFromMachineLearnPai2-1.channel_param

https://blog.csdn.net/wy6250000/article/details/83755114

https://blog.csdn.net/qq_36451496/article/details/90633072

https://blog.csdn.net/qq_24642909/article/details/103612058

https://www.runoob.com/jsref/met-win-settimeout.html

https://blog.csdn.net/weixin_42042250/article/details/89147348

https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Array/isArray
https://jingyan.baidu.com/article/3aed632ed6a9d77010809193.html

https://blog.csdn.net/sinat_27088253/article/details/51459443?utm_medium=distribute.pc_relevant.none-task-blog-BlogCommendFromMachineLearnPai2-2.edu_weight&depth_1-utm_source=distribute.pc_relevant.none-task-blog-BlogCommendFromMachineLearnPai2-2.edu_weight

https://zhuanlan.zhihu.com/p/71744331
https://blog.csdn.net/weixin_34202952/article/details/91445844?utm_medium=distribute.pc_relevant.none-task-blog-BlogCommendFromMachineLearnPai2-4.edu_weight
https://www.cnblogs.com/lhl66/p/9195901.html

https://www.runoob.com/jsref/prop-win-localstorage.html

https://www.jianshu.com/p/9cefe3d27449

https://blog.csdn.net/AiHuanhuan110/article/details/89160241?utm_medium=distribute.pc_relevant_t0.none-task-blog-BlogCommendFromMachineLearnPai2-1.edu_weight&depth_1-utm_source=distribute.pc_relevant_t0.none-task-blog-BlogCommendFromMachineLearnPai2-1.edu_weight
https://blog.csdn.net/qq_44469200/article/details/103679882?utm_medium=distribute.pc_relevant.none-task-blog-BlogCommendFromMachineLearnPai2-2.edu_weight&depth_1-utm_source=distribute.pc_relevant.none-task-blog-BlogCommendFromMachineLearnPai2-2.edu_weight
https://blog.csdn.net/weixin_40402192/article/details/80052887?utm_medium=distribute.pc_relevant.none-task-blog-BlogCommendFromMachineLearnPai2-4.edu_weight&depth_1-utm_source=distribute.pc_relevant.none-task-blog-BlogCommendFromMachineLearnPai2-4.edu_weight
https://blog.csdn.net/ll594317566/article/details/90666836

https://blog.csdn.net/XuM222222/article/details/98961165

JS 方法
https://www.runoob.com/jsref/jsref-splice.html
https://www.runoob.com/jsref/jsref-substring.html
https://www.runoob.com/jsref/jsref-includes.html

vue 计算属性和侦听属性
https://www.cnblogs.com/guozongzhang/p/10687989.html
https://www.cnblogs.com/malakaochi/p/9047458.html

前端 vue 中文件下载的几种方式
https://blog.csdn.net/mobile18611667978/article/details/88988884
https://blog.csdn.net/ju__ju/article/details/102463327?utm_medium=distribute.pc_relevant.none-task-blog-BlogCommendFromMachineLearnPai2-2.channel_param&depth_1-utm_source=distribute.pc_relevant.none-task-blog-BlogCommendFromMachineLearnPai2-2.channel_param
https://blog.csdn.net/qq_36940740/article/details/105792983?utm_medium=distribute.pc_relevant.none-task-blog-BlogCommendFromMachineLearnPai2-2.channel_param&depth_1-utm_source=distribute.pc_relevant.none-task-blog-BlogCommendFromMachineLearnPai2-2.channel_param
https://blog.csdn.net/qq_19313497/article/details/104234723?utm_medium=distribute.pc_relevant.none-task-blog-BlogCommendFromMachineLearnPai2-2.channel_param
https://segmentfault.com/q/1010000014569305
https://www.jianshu.com/p/6705e2eabaa1

js 数组、对象深拷贝
https://blog.csdn.net/weixin_42600599/article/details/108704342
https://blog.csdn.net/FungLeo/article/details/54931379?utm_medium=distribute.pc_relevant_t0.none-task-blog-OPENSEARCH-1.channel_param&depth_1-utm_source=distribute.pc_relevant_t0.none-task-blog-OPENSEARCH-1.channel_param

PDF 预览
https://ld246.com/article/1571550833725
https://www.jianshu.com/p/2f39de746900

hash 和 history 区别
https://blog.csdn.net/sayoko06/article/details/85321802

css 居中
https://www.cnblogs.com/clj2017/p/9293363.html
https://www.cnblogs.com/lxlw/p/11771001.html

滚动的玻璃球
https://wow.techbrood.com/fiddle/25757

python IRR
https://blog.csdn.net/huiguixian/article/details/90714331

圆环
https://www.cnblogs.com/xinsir/p/12074608.html

var htmlImage = '<img src="{% static '/assets/img/main/station_default.png' %}" alt=""/>'

实现 ECharts 双 Y 轴左右刻度线一致
https://blog.csdn.net/qq_40845885/article/details/82108525

https://www.jq22.com/webqd2565

同一个 dom 元素绑定的两个 class 优先级问题
https://blog.csdn.net/xinzaitt/article/details/84929040?utm_medium=distribute.pc_aggpage_search_result.none-task-blog-2~all~top_click~default-1-84929040.nonecase&utm_term=%E4%B8%A4%E4%B8%AAclass%E4%BC%98%E5%85%88%E7%BA%A7&spm=1000.2123.3001.4430

https://blog.csdn.net/qq_19306197/article/details/62885212

https://blog.csdn.net/eca33/article/details/90265563?utm_medium=distribute.pc_relevant.none-task-blog-BlogCommendFromMachineLearnPai2-1.compare&depth_1-utm_source=distribute.pc_relevant.none-task-blog-BlogCommendFromMachineLearnPai2-1.compare

p 标签首行缩进
https://blog.csdn.net/weixin_42223833/article/details/87987931

css 图片填充的几种方式
https://www.cnblogs.com/zly430/p/11375247.html

CSS3 自定义滚动条样式
https://www.cnblogs.com/ranyonsue/p/9487599.html

https://www.jianshu.com/p/c2addb233acd

https://jingyan.baidu.com/article/c275f6ba30b752e33d7567c7.html

https://www.cnblogs.com/nelsonlei/p/10154978.html

https://caniuse.com/
https://blog.csdn.net/weixin_42042250/article/details/89147348
