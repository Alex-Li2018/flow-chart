# 变量编辑器

变量编辑是一种富文本加mention的技术。场景一: 用户输入的词是一个变量时需高亮; 场景二: 直接从用户托蓝处(光标)插入变量的一种编辑器; 场景三: 使用mention的方式选择插入变量(类似@的功能).

## 组件设计

### 组件入参

|props|type|desc|default|
|---|---|---|---|
|value|string|富文本框里的内容|null|
|placeholder|string|富文本框的占位符内容|null|
|variable|string|单个变量|''|
|tooltip|string|富文本框上问号提示里的内容|''|
|popoverList|array|变量列表 (当变量列表里有值,variable将会失效)|[]|

### 方法

|method|desc|arguments|
|---|---|---|
|getHtmlContent|获取富文本里的html|
|getTextContent|获取富文本里的纯文本|
|setContent|设置富文本里的内容| value: string (html)
|clearContent|清楚富文本里的内容| 

## 功能实现

### 富文本的实现

HTML中的contentEditable的属性可以打开某些元素的可编辑状态．可以让div或整个网页，以及span等等元素设置为可写。我们最常用的输入文本内容便是input与textarea，使用contentEditable属性后，可以在div,table,p,span,body,等等很多元素中输入内容．

```html
<div
    id="variable-edit-core_wrap"
    class="variable-edit-core_wrap"
    contenteditable="true"
    :placeholder="placeholder"
    @input="handlerInput"
    @click.stop="nullFN"
>
</div>
```

#### placeholder的问题

input 和 textarea 能轻松实现 placeholder 提示语的效果，但作用于 contenteditable 的元素，placeholder 不起作用，可以通过 css 的 :empty 解决

```css
[contenteditable='true']:empty::before {
  content: attr(placeholder);
}
```

#### 获取富文本的内容

可以利用 innerHTML、innerText、textContent 获取输入框的内容，下面详细对比介绍一下这几个方法：

- innerHTML 返回或修改标签之间的内容，包括标签和文本信息，基本上所有浏览器都支持。

- innerText 打印标签之间的纯文本信息，会将标签过滤掉,此功能最初由 Internet Explorer 引入，在 Firefox 上存在兼容问题。

  - <p style="font-weight: bold; font-size: 20px;">innerText !== textContent</p>

- innerText 和 textContent 均能获取标签的内容，但二者存在差别，使用的时候还需注意浏览器兼容性：

    1. textContent 会获取 style 元素里的文本（若有 script 元素也是这样），而 innerText 不会

    2. textContent 会保留空行、空格与换行符

    3. innerText 并不是标准，而 textContent 更早被纳入标准中

    4. innerText 会忽略 display: none 标签内的内容，textContent 则不会

    5. 性能上 textContent > innerText

```js
getTextContent() {
    const root = document.getElementById('variable-edit-core_wrap')

    return root.textContent
}
```

### 光标处(托蓝处)插入变量



### mention的实现