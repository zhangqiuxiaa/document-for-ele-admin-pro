---
title: 分割面板
createTime: 2025/01/10 09:30:40
permalink: /EleAdminPro/0mqgkj1z/
---
## 6.23.1. 快速使用  

左右分割布局组件名 `ele-split-layout` ，用于左侧固定宽度，右侧铺满的布局，`v1.8.0新增`，使用示例：

```vue
<template>
    <div class="ele-body">
        <a-card :bordered="false">
            <ele-split-layout
                width="266px"
                allow-collapse
                :right-style="{ overflow: 'hidden' }"
                :style="{ minHeight: 'calc(100vh - 152px)' }"
            >
                <div>左边区域</div>
                <template #content>
                    <div>右边区域</div>
                </template>
            </ele-split-layout>
        </a-card>
    </div>
</template>
```

ele-split-layout 只负责左右内容的展示，折叠展开等功能，你可以为你的左右内容区域都加上边框样式，效果会更好。

<br/>

## 6.23.2. 属性列表  

| 属性             | 说明                             | 类型                  | 默认值                   |
| :--------------- | :------------------------------- | :-------------------- | :----------------------- |
| width            | 左侧宽度                         | String                | '200px'                  |
| space            | 间距                             | String                | '16px'                   |
| collapse         | 是否折叠 v-model                 | Boolean               |                          |
| allowCollapse    | 是否可折叠                       | Boolean               |                          |
| leftStyle        | 左侧样式                         | Object: CSSProperties |                          |
| rightStyle       | 右侧样式                         | Object: CSSProperties |                          |
| collapseBtnStyle | 折叠按钮样式                     | Object: CSSProperties |                          |
| minSize          | 自动拉伸时最小尺寸 `v1.11.0新增` | Number                |                          |
| maxSize          | 自动拉伸时最大尺寸 `v1.11.0新增` | Number                |                          |
| vertical         | 是否垂直方向 `v1.11.0新增`       | Boolean               |                          |
| reverse          | 是否反向布局 `v1.11.0新增`       | Boolean               |                          |
| resizable        | 是否可拉伸宽度 `v1.11.0新增`     | Boolean               |                          |
| responsive       | 是否开启响应式 `v1.11.0新增`     | Boolean               | 默认取 pro-layout 的属性 |

<br/>

## 6.23.3. 事件列表  

| 事件名称        | 说明                     | 回调参数          |
| :-------------- | :----------------------- | :---------------- |
| update:collapse | 用于 collapse 的 v-model | collapse: boolean |

<br/>

## 6.23.4. 插槽列表  

| 插槽名   | 说明                           | 参数       |
| :------- | :----------------------------- | :--------- |
| default  | 边栏插槽                       |            |
| content  | 内容插槽                       |            |
| collapse | 折叠展开按钮插槽 `v1.11.0新增` | collapse |