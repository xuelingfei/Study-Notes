# Note

<!-- markdown="1" is required for GitHub Pages to render the TOC properly. -->
<details markdown="1">
  <summary>目录</summary>

- [文件下载](#文件下载)
  - [通过链接下载](#通过链接下载)
  - [通过文件流下载](#通过文件流下载)
  </details>

## 文件下载

### 下载动作触发

1. 借用 form 的提交触发下载，触发完成后还原 form 的 action

```js
function getExcelButton(url, backUrl) {
  $("#searchForm").attr("action", url)
  $("#searchForm").submit()
  $("#searchForm").attr("action", backUrl)
}
```

2. 通过 xhr 请求

```js
function downloadFile(url, filename) {
  var layer_index = layer.open({
    type: 1,
    title: false, //不显示标题栏
    closeBtn: false,
    skin: 'layui-layer-molv',
    content: '<img style="width:50px;margin:20px 10px 20px 20px;" src="{% static '/images/loading.gif' %}"><span style="margin-right:20px;font-size:14px;">正在导出，请稍候……</span>'
  });
  var xhr = new XMLHttpRequest();
  xhr.open("GET", url, true);
  xhr.responseType = "blob";
  xhr.onload = function (oEvent) {
    var content = xhr.response;

    var elink = document.createElement('a');
    elink.download = filename;
    elink.style.display = 'none';

    var blob = new Blob([content]);
    elink.href = URL.createObjectURL(blob);
    document.body.appendChild(elink);
    elink.onclick = function () {
      document.body.removeChild(elink);
      layer.close(layer_index);
    };
    elink.click();
  };
  xhr.send();
}
```

3. vue 中
   发送请求，然后在 request.js 中通过 responseType 识别，识别为 blob 的直接下载

```js
function fetchNewsList(data) {
  return request({
    method: "POST",
    url: "/BeTradingMall_interface/moduleContent/list",
    responseType: "blob",
    data,
  })
}
request.js
function downloadFile() {}
```

也可以写在一起

```js
request({
  method: "POST",
  url: "/BeTradingMall_interface/moduleContent/list",
  responseType: "blob",
  data,
}).then((res) => {
  const { success, data } = res
  if (success && data) {
    this.loading = false
  }
})
```

### 通过链接下载

```py
def export_powerpoint(request):
    prs = Presentation(BASE_FILES_WEEKLY_REVIEW_TEMPLATE_DIR)
    slides = prs.slides
    for slide in slides:
        sorted_shapes = sorted(slide.shapes, key=lambda x: (x.top, x.left))
        if sorted_shapes:
            flag = sorted_shapes[0].text
            if flag == 'Facility Weekly Report':
                sorted_shapes[1].text = 'Prepared by：' + SessionUser.getSessionUser().username
                sorted_shapes[2].text = 'Date：' + today.strftime('%Y{y}%m{m}%d{d}').format(y='年', m='月', d='日') + ' WK' + str(current_week)
            else:
                if flag in option_dict.keys():
                    draw_slide_chart(sorted_shapes, option_dict[flag])

    file_path = MEDIA_ROOT + BASE_FILES_WEEKLY_REVIEW_DIR
    if not os.path.exists(file_path):
        os.makedirs(file_path)
    file_full_path = file_path + str(today.year) + '厂务系统用量周报（WK' + str(current_week) + '）.pptx'
    prs.save(file_full_path)
    return Util.download(file_full_path, str(today.year) + '厂务系统用量周报（WK' + str(current_week) + '）.pptx')

@staticmethod
def download(filePath, name):
    response = StreamingHttpResponse(Util.readFile(filePath))
    response['content-type'] = 'application/octet-stream'
    response['Content-Disposition'] = "attachment; filename*=utf-8''{}".format(escape_uri_path(name))
    return response

@staticmethod
def readFile(filename, chunk_size=512):
    with open(filename, 'rb') as f:
        while True:
            c = f.read(chunk_size)
            if c:
                yield c
            else:
                break
```

```js
let target = document.createElement("a")
target.setAttribute("download", "download")
target.setAttribute("href", url)
target.click()
```

### 通过文件流下载

```py
def export_excel(request):
    response = HttpResponse(content_type='application/vnd.ms-excel')
    response["Access-Control-Expose-Headers"] = "Content-Disposition"
    response['Content-Disposition'] = "attachment; filename*=utf-8''{}".format(escape_uri_path("分类台帐详情表.xlsx"))
    output = BytesIO()

    workbook = Workbook(output)
    worksheet = workbook.add_worksheet()
    workbook.close()

    output.seek(0)
    response.write(output.getvalue())
    return response

```

```js
function downloadFile(response) {
  const res = response.data
  const resHeader = response.headers

  //下载的文件类型
  const donwloadType = resHeader["content-type"] || resHeader["Content-Type"]
  const contentDisposition =
    resHeader["content-disposition"] || resHeader["Content-Disposition"]

  let deposition = decodeURIComponent(contentDisposition)
  let key = "filename="
  let filenameIndex = deposition.indexOf(key)
  //下载的文件名称
  const downloadName =
    filenameIndex !== -1 ? deposition.substr(filenameIndex + key.length) : ""

  let blob = new Blob([res], { type: donwloadType })
  if (window.navigator && window.navigator.msSaveOrOpenBlob) {
    window.navigator.msSaveOrOpenBlob(blob, downloadName)
  } else {
    const url = window.URL.createObjectURL(blob)
    let link = document.createElement("a")
    link.download = downloadName
    link.href = url
    link.click()
    window.URL.revokeObjectURL(url)
  }
}
```

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

- python 引用（多次使用推导式导致的引用问题
  初始化
  date_list = timeUtil.getEveryDay(start_date_str, end_date_str)
  date_value = {date: None for date in date_list}
  ident_value_dict = {ident: date_value.copy() for ident in ident_list}

- js 获取高度

1. $("#div_id").height(); // 获得的是该 div 本身的高度, (不包含 padding,margin,border)
2. $("#div_id").outerHeight(); // 包含该 div 本身的高度, padding 上下的高度, 以及 border 上下的高度(不包含 margin 的高度)
3. $("#div_id").outerHeight(true); // 包含该 div 本身的高度, 以及 padding,border,margin 上下的总高度

$("#divId").height(); //不包括内边距、边框或外边距
$("#divId").innerHeight();//包括内边距
$("#divId").outerHeight();//包括内边距和边框

- Object.assign()导致的组件复用覆盖问题

```vue
<template>
  <template v-if="title">
    <div
      class="floor-title one-line"
      :style="`margin: 0 0 ${gapObj.title}px;color: ${colorObj.title}`"
    >
      {{ title }}
    </div>
  </template>
  <template v-if="subTitle">
    <div
      class="floor-sub_title"
      v-html="subTitle"
      :style="`margin: 0 0 ${gapObj.subTitle}px;color: ${colorObj.subTitle}`"
    ></div>
  </template>
</template>

<script>
//默认间距
const defaultGapObj = {
  //标题底部的间距
  title: 22,
  //副标题底部的间距
  subTitle: 36,
}
//默认颜色
const defaultColorObj = {
  //标题
  title: "#303133",
  //副标题
  subTitle: "#767979",
}
export default {
  props: {
    title: {
      type: String,
      default: "",
    },
    subTitle: {
      type: String,
      default: "",
    },
    //间距
    gap: {
      type: Object,
      default: () => defaultGapObj,
    },
    //颜色
    color: {
      type: Object,
      default: () => defaultColorObj,
    },
  },
  name: "FloorTitle",
  data() {
    return {
      gapObj: {},
      colorObj: {},
    }
  },
  mounted() {
    const { gap, color } = this
    this.gapObj = Object.assign({}, defaultGapObj, gap)
    this.colorObj = Object.assign({}, defaultColorObj, color)
  },
}
</script>

<style lang="scss" scoped>
@import "./index.scss";
</style>
```


#### 跳出指定循环
```js
loopTableData: for (let i = 0; i < this.tableData.length; i++) {
              let item = this.tableData[i]
              for (let j = 0; j < item.children.length; j++) {
                if (item.children[j].id === record.id) {
                  item.children[j].total_amount = val
                  break loopTableData
                }
              }
            }
```


#### 输入控制
浮点数
 onkeyup="this.value=this.value.match(/\d+(\.\d*)?/g)?this.value.match(/\d+(\.\d*)?/g)[0]:''"
非负整数
onkeyup="this.value=this.value.replace(/[^\d]/g,'')"
三位小数
onkeyup="this.value=this.value.match(/\d+(\.\d{0,3})?/g)?this.value.match(/\d+(\.\d{0,3})?/g)[0]:''"


#### vuex分模块后如何通过mapState调用
MainLayoutHeader, order-submit



if (config.data instanceof FormData) {
      config.timeout = 10000 // OCR文件识别所需时间较长
      console.log("FormData pass")
    } else if (config.method === "post") {
      if (isObject(config.data)) {
        const config_data = {}

        for (let key in config.data) {
          let item = config.data[key]
          if (!isArray(item)) {
            if (isNull(item) || isUndefined(item)) {
              config_data[key] = ""
            } else {
              config_data[key] = item
            }
          } else {
            config_data[key] = item
          }
        }

const loadingStyle = {
        text: "加载中...",
        color: "#009688",
        textColor: "#009688",
        fontSize: chartFont(1.2),
        lineWidth: 2,
      }
      echarts.init(document.getElementById("yearlySales")).showLoading(loadingStyle)
      echarts.init(document.getElementById("monthlySales")).showLoading(loadingStyle)
      myChart.hideLoading()
  

  window.onresize = function () {
        myChart.resize()
      }
   window.onresize = null

antd table 序号列
   {
        title: "序号",
        dataIndex: "index",
        align: "center",
        customRender: ({ index }) => `${index + 1}`,
        width: 80,
      },


Ant-design-vue Table组件customRow属性的使用说明
 更新时间：2020年10月28日 10:51:44   作者：EasonGG  
这篇文章主要介绍了Ant-design-vue Table组件customRow属性的使用说明，具有很好的参考价值，希望对大家有所帮助。一起跟随小编过来看看吧
官网示例

使用方式
   // 表格中加入customRow属性并绑定一个custom方法
   <a-table
    rowKey="stockOrderCode"
    :columns="columns"
    :dataSource="dataSource"
    :pagination="pagination"
    :customRow="customRow"
   >
   </a-table>
 
   // methods中定义方法
   customRow(record, index) {
 return {
 // 这个style就是我自定义的属性，也就是官方文档中的props
 style: {
  // 字体颜色
  color: record.remarkDesc ? record.remarkDesc.fontColor : 'rgba(0, 0, 0, 0.65)',
  // 行背景色
  'background-color': record.remarkDesc ? record.remarkDesc.bgColor : '#ffffff',
  // 字体类型
  'font-family': record.remarkDesc ? record.remarkDesc.fontType : 'Microsoft YaHei',
  // 下划线
  'text-decoration':
  record.remarkDesc && record.remarkDesc.underline ? 'underline' : 'unset',
  // 字体样式-斜体
  'font-style': record.remarkDesc && record.remarkDesc.italics ? 'italic' : 'unset',
  // 字体样式-斜体
  'font-weight': record.remarkDesc && record.remarkDesc.bold ? 'bolder' : 'unset',
 },
 on: {
  // 鼠标单击行
  click: event => {
  // do something
  },
 },
 }
},



CSS3 :nth-child() 选择器
定义和用法
:nth-child(n) 选择器匹配属于其父元素的第 N 个子元素，不论元素的类型。

n 可以是数字、关键词或公式。

提示：请参阅 :nth-of-type() 选择器，该选择器选取父元素的第 N 个指定类型的子元素。

实例 1
Odd 和 even 是可用于匹配下标是奇数或偶数的子元素的关键词（第一个子元素的下标是 1）。

在这里，我们为奇数和偶数 p 元素指定两种不同的背景色：

p:nth-child(odd)
{
background:#ff0000;
}
p:nth-child(even)
{
background:#0000ff;
}
亲自试一试

实例 2
使用公式 (an + b)。描述：表示周期的长度，n 是计数器（从 0 开始），b 是偏移值。

在这里，我们指定了下标是 3 的倍数的所有 p 元素的背景色：

p:nth-child(3n+0)
{
background:#ff0000;
}
