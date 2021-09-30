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
