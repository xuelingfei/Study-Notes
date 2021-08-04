## ECharts 使用技巧


- [通用技巧](#通用技巧)  
- [折线图](#折线图)  
- [柱状图](#柱状图)  
- [饼图](#饼图)  
- [ECharts 字体大小适配 rem 布局](#ECharts-字体大小适配-rem-布局)  


#### 通用技巧
- 刷新图表  
  - ECharts + JavaScript  
    ```js
    if (document.getElementById('exampleChart') !== null) {
      echarts.dispose(document.getElementById('exampleChart'))
    }
    ```

  - jQuery  
    ```js
    if ($("#exampleChart") !== null) {
      $("#exampleChart").removeAttr("_echarts_instance_");
    }
    ```

- legend 中数据要和 series 中 name 的值保持一致，否则添加图例没有效果  

- tooltip 内容自定义  
```
tooltip: {
  trigger: 'axis',
  axisPointer: {
    type: 'shadow'
  },
  formatter: function (params) {
    return params[0].name + '<br/>'
        + params[0].seriesName + ': ' + params[0].value + '<br/>'
        + params[1].seriesName + ': ' + params[1].value;
  }
},
```
其中
> - params[i].name -- x 轴对应的数据名，类目名
> - params[i].seriesName -- 系列名，也就是 sreies 里的 name 的值
> - params[i].value -- 传入的数据值

- 渐变色  
  - 线性渐变  
    前四个参数 x, y, x2, y2，范围从 0~1，相当于在图形包围盒中的百分比，如果 globalCoord 为 `true`，则该四个值是绝对的像素位置
    ```
    color: {
      type: 'linear',
      x: 0,
      y: 0,
      x2: 0,
      y2: 1,
      colorStops: [
        {offset: 0, color: 'red'},  //0% 处的颜色
        {offset: 1, color: 'blue'},  //100% 处的颜色
      ],
      globalCoord: false,  //缺省为 false
    },
    ```
    等同于
    ```
    color: new echarts.graphic.LinearGradient(0, 0, 0, 1, [  //颜色渐变函数，前四个参数分别表示四个位置依次为左、下、右、上
      {offset: 0, color: 'rgba(250,217,97,1)'},  //0% 处的颜色   
      {offset: 1, color: 'rgba(247,107,28,1)'},  //100% 处的颜色
    ]),
    ```
  - 径向渐变  
    前三个参数分别是圆心 (x, y) 和半径 r，取值同线性渐变
    ```
    color: {
      type: 'radial',
      x: 0.5,
      y: 0.5,
      r: 0.5,
      colorStops: [
        {offset: 0, color: 'red'},  //0% 处的颜色
        {offset: 1, color: 'blue'}, //100% 处的颜色
      ],
      globalCoord: false //缺省为 false
    },
    ```
    等同于
    ```
    color: new echarts.graphic.RadialGradient(0.3, 0.3, 0.8, [
      {offset: 0, color: '#f7f8fa'},
      {offset: 1, color: '#cdd0d5'}
    ]),
    ```
    参考链接：<https://blog.csdn.net/qq_31135027/java/article/details/79635038>


#### 折线图
- series-line.symbol  
`string` `Function`  
标记的图形。  
ECharts 提供的标记类型包括`'circle'`, `'rect'`, `'roundRect'`, `'triangle'`, `'diamond'`, `'pin'`, `'arrow'`, `'none'`  
可以通过 `'image://url'` 设置为图片，其中 URL 为图片的链接，或者 dataURI。  
URL 为图片链接例如：`image://http://xxx.xxx.xxx/a/b.png`  
URL 为 dataURI 例如：
```
'image://data:image/gif;base64,R0lGODlhEAAQAMQAAORHHOVSKudfOulrSOp3WOyDZu6QdvCchPGolfO0o/XBs/fNwfjZ0frl3/zy7////wAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAACH5BAkAABAALAAAAAAQABAAAAVVICSOZGlCQAosJ6mu7fiyZeKqNKToQGDsM8hBADgUXoGAiqhSvp5QAnQKGIgUhwFUYLCVDFCrKUE1lBavAViFIDlTImbKC5Gm2hB0SlBCBMQiB0UjIQA7'
```
可以通过 `'path://'` 将图标设置为任意的矢量路径。这种方式相比于使用图片的方式，不用担心因为缩放而产生锯齿或模糊，而且可以设置为任意颜色。路径图形会自适应调整为合适的大小。路径的格式参见 SVG PathData。可以从 Adobe Illustrator 等工具编辑导出。  
例如：
```
'path://M30.9,53.2C16.8,53.2,5.3,41.7,5.3,27.6S16.8,2,30.9,2C45,2,56.4,13.5,56.4,27.6S45,53.2,30.9,53.2z M30.9,3.5C17.6,3.5,6.8,14.4,6.8,27.6c0,13.3,10.8,24.1,24.101,24.1C44.2,51.7,55,40.9,55,27.6C54.9,14.4,44.1,3.5,30.9,3.5z M36.9,35.8c0,0.601-0.4,1-0.9,1h-1.3c-0.5,0-0.9-0.399-0.9-1V19.5c0-0.6,0.4-1,0.9-1H36c0.5,0,0.9,0.4,0.9,1V35.8z M27.8,35.8 c0,0.601-0.4,1-0.9,1h-1.3c-0.5,0-0.9-0.399-0.9-1V19.5c0-0.6,0.4-1,0.9-1H27c0.5,0,0.9,0.4,0.9,1L27.8,35.8L27.8,35.8z'
```
如果需要每个数据的图形不一样，有两种方法：
1. 将 symbol 设置为如下格式的回调函数：  
`(value: Array|number, params: Object) => string`  
其中第一个参数 value 为 data 中的数据值。第二个参数params 是其它的数据项参数。  
注：在 ECharts5 之前的版本中会报错 `Uncaught TypeError: symbolType.indexOf is not a function` 。
2. 直接修改 series-line.series.data.symbol  
```
for (var item of seriesData) {
  if (item.sign) {
    if (item.sign === 1) {
      item.symbol = 'image://{% static '/assets/img/icon-green.png' %}'
    } else if (item.sign === 2) {
      item.symbol = 'image://{% static '/assets/img/icon-grey.png' %}'
    } else if (item.sign === 3) {
      item.symbol = 'image://{% static '/assets/img/icon-yellow.png' %}'
    } else if (item.sign === 4) {
      item.symbol = 'image://{% static '/assets/img/icon-red.png' %}'
    }
  }
}
```


#### 柱状图
- series-bar.barGap  
`string`  
不同系列的柱间距离，为百分比（如 '30%'，表示柱子宽度的 30%）。  
如果想要两个系列的柱子重叠，可以设置 barGap 为 '-100%'。这在用柱子做背景的时候有用。  
在同一坐标系上，此属性会被多个 bar 系列共享。此属性应设置于此坐标系中最后一个 bar 系列上才会生效，并且是对此坐标系中所有 bar 系列生效。  

- series-bar.barCategoryGap  
`string`  
同一系列的柱间距离，默认为类目间距的 20% ，可设固定值。  
在同一坐标系上，此属性会被多个 bar 系列共享。此属性应设置于此坐标系中最后一个 bar 系列上才会生效，并且是对此坐标系中所有 bar 系列生效。  



#### 饼图
- series-pie.center  
`Array`  
饼图的中心（圆心）坐标，数组的第一项是横坐标，第二项是纵坐标。  
支持设置成百分比，设置成百分比时第一项是相对于容器宽度，第二项是相对于容器高度。  
使用示例：
```
// 设置成绝对的像素值
center: [400, 300]
// 设置成相对的百分比
center: ['50%', '50%']
```
- series-pie.radius  
`number` `string` `Array`  
饼图的半径。可以为如下类型：
> - number：直接指定外半径值。
> - string：例如，'20%'，表示外半径为可视区尺寸（容器高宽中较小一项）的 20% 长度。
> - Array.<number|string>：数组的第一项是内半径，第二项是外半径。每一项遵从上述 number string 的描述。可以将内半径设大显示成圆环图（Donut chart）。


#### ECharts 字体大小适配 rem 布局
```js
function chartFont(remSize) {
  var clientWidth =
      window.innerWidth ||
      document.documentElement.clientWidth ||
      document.body.clientWidth;  //获取到屏幕的宽度
  if (!clientWidth) return;  //报错拦截
  var unitSize = 10 * (clientWidth / 1920);  //单位rem对应的实际大小
  return remSize * unitSize;
}
```
该方法适用于 1920 尺寸、以 1rem = 10px 换算关系的设计稿，使用时直接调用，例如设置字体大小为 20px ：`fontSize: chartFont(0.2)`  
若设计稿尺寸不是 1920 则修改 1920 处，比如 2560 尺寸修改为 `var unitSize = 10 * (clientWidth / 2560)`  
若换算关系不是 1rem = 10px 时，比如 1rem = 100px 则修改为 `var unitSize = 100 * (clientWidth / 1920)`

参考链接：<https://blog.csdn.net/weixin_43140300/article/details/106009294>
