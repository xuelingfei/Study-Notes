## 表单


- [输入框字数限制](#输入框字数限制)  
- [锁定输入框内容不可修改](#锁定输入框内容不可修改)  
- [隐藏的 input 元素不能使用 required 属性](#隐藏的-input-元素不能使用-required-属性)  
- [oninvalid 事件属性](#oninvalid-事件属性)  
- [表单提交和输入校验](#表单提交和输入校验)  
- [阻止表单提交](#阻止表单提交)  


#### 输入框字数限制
`maxlength` 属性规定文本区域的最大长度，以字符个数计。适用于 `input` 和 `textarea` 标签。
- 语法
```html
<input maxlength="number">
<textarea maxlength="number">
```


#### 锁定输入框内容不可修改
`readonly` 和 `disabled` 都可以锁定输入框内容不可修改，但需要注意的是，`form` 表单提交时，只提交非 `disabled` 的 `input` 标签。


#### 隐藏的 input 元素不能使用 required 属性
如
```html
<input type="hidden" required/>
<input type="text" style="display: none;" required/>
```
会报错 `An invalid form control with name='' is not focusable`，去掉 `required` 属性即可消除错误。


#### oninvalid 事件属性
`oninvalid` 事件仅支持 `<input>` 标签，常与 `required` 属性配合使用，当 `<input>` 元素无效时，会触发 `oninvalid` 事件。

- 语法
```
<element oninvalid="myScript">
```

- 示例
```html
<input type="text" name="example" oninvalid="alert('请填写表单');return false;" required>
```


#### 表单提交和输入校验
提交表单时，点击 `type="submit"` 的按钮会先进行输入校验，再进行提交，而调用 `form` 的 `submit()` 函数会直接进行提交。  
自定义提交方法，又想使用原生输入校验时，可以隐藏一个 `submit` 按钮，提交时调用该按钮的 `click()` 方法。

- 示例
```html
<form class="search_form" action="/submit">
  <input type="text" name="search_text" required>
  <button id="submitButton" type="submit" hidden>button提交</button>
  <!--<input type="submit" value="input提交" hidden/>-->
  <a onclick="submitForm()">a提交</a>
</form>
<script>
  function submitForm() {
    //$("form.search_form").submit()  // 直接提交
    $("button#submitButton").click()  // 先校验后提交
  }
</script>
```


#### 阻止表单提交
`onsubmit` 事件在表单提交时触发。通过 `onsubmit` 事件的 `return false` 可以阻止表单提交，以便在表单提交之前对相关字段进行验证。

- 示例
```html
<form id="exampleForm" method="post" action="" οnsubmit="return check()">
</form>

<form id="exampleForm" method="post" action="" onsubmit="return check()">
  <input type="text" name="example" value=""/>
  <button type="submit">提交</button>
</form>

<script type="text/javascript">
  function check() {
    if ('验证条件不通过') {
      return false
    }else{
      return true  // 不写此返回值也行，此时就直接提交了
    }
  }
</script>
```
注意：`onsubmit="return check()"` 中的 `return` 是一定要加上的，不然 `check` 的返回值哪怕是 `false`，仍然提交。也就是说，`onsubmit="return false"` 为不执行提交；`onsubmit="return true"` 或 `onsubmit="check()"` 都执行提交。
