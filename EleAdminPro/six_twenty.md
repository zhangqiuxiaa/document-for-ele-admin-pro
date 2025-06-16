---
title: 条形码
createTime: 2025/01/10 09:30:40
permalink: /EleAdminPro/a81mvie8/
---
## 6.20.1. 快速使用  

组件名称 `ele-bar-code` ，基于 [jsbarcode](https://github.com/lindell/JsBarcode) 封装 `v1.9.0新增`，使用示例：

```vue
<template>
    <ele-bar-code :value="text" tag="svg" :options="{ height: 60 }" />
</template>

<script setup>
    import { ref } from 'vue';

    const text = ref('1234567890128');
</script>
```

<br/>

## 6.20.2. 属性列表  

| 属性    | 说明                             | 类型   | 默认值 |
| :------ | :------------------------------- | :----- | :----- |
| value   | 条码内容                         | String |        |
| tag     | 渲染标签, 'svg'、'img'、'canvas' | String | 'svg'  |
| options | jsbarcode 的参数                 | Object |        |

options 参数具体可以到 jsbarcode 的 github 查看，这里只做简单说明：

| 属性         | 说明                          | 类型     | 默认值          |
| :----------- | :---------------------------- | :------- | :-------------- |
| format       | 条码内容                      | String   | 'auto'(CODE128) |
| width        | 线条的宽度                    | Number   | 2               |
| height       | 条码的高度                    | Number   | 100             |
| displayValue | 是否显示文字                  | Boolean  | true            |
| fontOptions  | 字体加粗斜体等, 'bold italic' | String   |                 |
| font         | 文字的字体                    | String   | 'monospace'     |
| textAlign    | 文字的对齐方式                | String   | 'center'        |
| textPosition | 文字的显示位置                | String   | 'bottom'        |
| textMargin   | 文字的间距                    | Number   | 2               |
| fontSize     | 文字的大小                    | Number   | 20              |
| background   | 背景的颜色                    | String   | '#ffffff'       |
| lineColor    | 条码的颜色                    | String   | '#000000'       |
| margin       | 间距                          | Number   | 10              |
| marginTop    | 上间距                        | Number   |                 |
| marginBottom | 下间距                        | Number   |                 |
| marginLeft   | 左间距                        | Number   |                 |
| marginRight  | 右间距                        | Number   |                 |
| valid        | valid                         | Function | 100             |

<br/>

## 6.20.3. 条码类型  

jsbarcode 支持多种编码类型，默认为 `CODE128` ，例如实现常见的商品条码 `EAN-13` 类型：

```vue
<template>
    <ele-bar-code :value="text" tag="svg" :options="options" />
</template>

<script setup>
    import { ref, reactive } from 'vue';

    const text = ref('1234567890128');

    const options = reactive({
        height: 60,
        fontSize: 14,
        format: 'EAN13'
    });
</script>
```