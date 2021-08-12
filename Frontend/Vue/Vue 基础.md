# Vue 基础

<!-- markdown="1" is required for GitHub Pages to render the TOC properly. -->
<details markdown="1">
  <summary>目录</summary>

- [特殊锚点](#特殊锚点)
- [空格](#空格)
</details>

## 特殊锚点

绑定监听@click

@click.stop 阻止事件冒泡

@click.prevent 阻止事件的默认行为，

<a href="http://www.baidu.com" @click.prevent="test4">百度一下</a>   //阻止a标签跳转，仅执行函数test4

<form  action="/xxx"   @submit.prevent="test5">   //阻止表单提交，仅执行函数test5

         <input type="submit" value="注册">
</form>

@keyup.enter

//按下enter时，执行方法test7

<input type="text" @keyup.enter="test7">