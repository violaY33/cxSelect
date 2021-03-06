# jQuery cxSelect

>cxSelect 是基于 jQuery 的多级联动菜单插件，适用于省市、商品分类等联动菜单。
>
>列表数据通过 AJAX 获取，也可以自定义，数据内容使用 JSON 格式。
>
>同时兼容 Zepto，方便在移动端使用。
>
>国内省市县数据来源：[basecss/cityData](https://github.com/basecss/cityData) Date: 2014.03.31
>
>全球主要城市数据来源：整理国内常用网站和软件 Date: 2014.07.29


修改后文件为jquery.cxselect2.js，将原生`<div>`元素改为`<div>`模拟的下拉选择框。



**版本：**

* jQuery v1.7+ | Zepto v1.0+
* jQuery cxSelect v1.4.2

文档：http://code.ciaoca.com/jquery/cxSelect/

示例：http://code.ciaoca.com/jquery/cxSelect/demo/

## 使用方法
### 载入 JavaScript 文件
```html
<script src="jquery.js"></script>
<script src="jquery.cxselect.js"></script>
```

### DOM 结构
```html
<!--
.select-box 的 class 任意取值，也可以附加多个 class，如 class="province otherclass"，在调用时只需要输入其中一个即可，但是不能重复
-->
<div id="element_id">
    <div class="select-box province" style="display: none">
        <div class="select-value"></div>
    </div>
    <div class="select-box city" style="display: none">
        <div class="select-value"></div>
    </div>
    <div class="select-box area" style="display: none">
        <div class="select-value"></div>
    </div>
</div>
```

### 设置默认值
```html
<!--
使用 .select-box 的 data-value 属性
--> 
<select class="province" data-value="浙江省"></select> 
```

### 调用 cxSelect
``` javascript
$('#element_id').cxSelect({
  url: 'cityData.min.json',               // 提示：如果服务器不支持 .json 类型文件，请将文件改为 .js 文件
  selects: ['province', 'city', 'area'],  // selects 为数组形式，请注意顺序
  emptyStyle: 'none'
});
```

### 设置参数全局默认值
``` javascript
// 需在引入 <script src="jquery.cxselect.js"></script> 之后，调用之前设置
$.cxSelect.defaults.url = 'cityData.min.json';
$.cxSelect.defaults.emptyStyle = 'none';
```

### API 接口
``` javascript
var cxSelectApi;

// 方法一：
cxSelectApi = $.cxSelect($('#element_id'), {
  selects: ['province', 'city', 'area']
});

// 方法二：
$('#element_id').cxSelect({
  selects: ['province', 'city', 'area']
}, function(api) {
  cxSelectApi = api;
});

cxSelectApi.attach();
cxSelectApi.detach();
cxSelectApi.clear();
cxSelectApi.setOptions();
```

## 参数说明
名称|默认值|说明
---|---|---
selects|[]|下拉选框组。<br>输入 select 的 className
url|null|整合数据接口地址（URL）；<br>每个选框的内容使用各自的接口地址，详见 [DEMO](http://code.ciaoca.com/jquery/cxselect/demo/oneself.html)
data|null|自定义数据，类型为数组，使用 JSON 格式。[DEMO](http://code.ciaoca.com/jquery/cxselect/demo/custom.html)
emptyStyle|null|子集无数据时 select 元素的显示状态。<br>可设置为：**"none"**(display:none), **"hidden"**(visibility:hidden)
required|false|是否为必选。<br>设为 `false` 时，会在列表头部添加 `<option value="firstValue">firstTitle</option>` 选项。
firstTitle|'请选择'|选框第一个项目的标题（仅在 `required` 为 `false` 时有效）
jsonSpace|''|数据命名空间
jsonName|'n'|数据标题字段名称（用于 option 的标题）
jsonValue|''|数据值字段名称（用于 option 的 value，没有值字段时使用标题作为 value）
jsonSub|'s'|子集数据字段名称


## data 属性参数
### 父元素的 data- 属性
```html
<div id="element_id" data-url="cityData.min.json" data-required="true"></div>
```

名称|说明
---|---
data-selects|下拉选框组。<br>输入 select 的 className，使用英文逗号分隔的字符串
data-url|列表数据接口地址
data-empty-style|子集无数据时 select 的显示状态
data-required|是否为必选
data-first-title|选框第一个项目的标题
data-json-space|数据命名空间
data-json-name|数据标题字段名称
data-json-value|数据值字段名称
data-json-sub|子集数据字段名称

### select 元素的 data- 属性
```html
<div class="select-box province" data-value="浙江省" data-first-title="选择省"></div>
```

名称|说明
---|---
data-value|默认选中值
data-url|列表数据接口地址
data-required|是否为必选
data-query-name|传递上一个选框值的参数名称（默认使用上一个选框的 name 属性值）
data-first-title|选框第一个项目的标题
data-json-space|数据命名空间
data-json-name|数据标题字段名称
data-json-value|数据值字段名称

## API 接口

名称|说明
---|---
attach()|绑定。<br>调用时会自动进行绑定，用于使用detach解除绑定后，进行重新绑定。
detach()|解除绑定。<br>解除绑定后，不再具有联动效果。
clear(index)|清空选项。<br>清空第 index 个 .select-box 自身及之后的 .select-box 的选项。<br>`index`: select 的序号，从 0 开始
setOptions(settings)|重新设置参数。<br>`settings`: 与调用时参数一致

## 自定义数据及使用纯数组数据
可以使用任何类型的数据作为值，但最终都会被转化为文本。

[自定义数据 DEMO](http://code.ciaoca.com/jquery/cxselect/demo/custom.html)


## 各选项数据接口独立
可以为每个```select```设置一个接口，根据接口返回的数据结构，设置```json-space```、```json-name```、```json-value```适应 JSON 结构（包括纯数组）。
当页面加载时，第一个选框已有选项数据，可以不设置第一个选框的接口。

[独立接口 DEMO](http://code.ciaoca.com/jquery/cxselect/demo/oneself.html)
