---
title: 水印组件
createTime: 2025/01/10 09:30:40
permalink: /EleAdminPro/1c80liu4/
---
## 6.26.1. 快速使用  

水印组件名称 `ele-watermark` ，支持文本、多行文本、图片等， `v1.11.0新增`，使用示例：

```vue
<template>
    <ele-watermark content="Ele Admin Pro">
        <div style="height: 400px"></div>
    </ele-watermark>
</template>
```

<br/>

## 6.26.2. 属性列表  

| 属性    | 说明               | 类型          | 默认值       |
| :------ | :----------------- | :------------ | :----------- |
| width   | 水印宽度           | Number        | 120          |
| height  | 水印高度           | Number        | 64           |
| rotate  | 旋转角度, -180~180 | Number        | -22          |
| zIndex  | 层级               | Number        | 9            |
| image   | 水印图片源         | String        |              |
| content | 水印文字           | String, Array |              |
| font    | 文字样式           | Object        |              |
| gap     | 水印间距           | Array         | `[100, 100]` |
| offset  | 距离左上角的偏移量 | Array         | `[50, 50]`   |

`font` 属性数据格式说明：

| 属性       | 说明     | 类型                               |
| :--------- | :------- | :--------------------------------- |
| color      | 字体颜色 | String                             |
| fontSize   | 字体大小 | Number                             |
| fontWeight | 字体粗细 | 'normal','light','weight',Number   |
| fontFamily | 字体样式 | String                             |
| fontStyle  | 字体类型 | 'none','normal','italic','oblique' |

<br/>

## 6.26.3. 插槽列表  

| 插槽名  | 说明     | 参数 |
| :------ | :------- | :--- |
| default | 默认插槽 |      |