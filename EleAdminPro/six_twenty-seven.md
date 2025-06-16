---
title: 仪表盘组件
createTime: 2025/01/10 09:30:40
permalink: /EleAdminPro/n2fg6nxq/
---
## 6.27.1. 快速使用  

仪表盘组件名称 `ele-dashboard` ，`v1.11.0新增`，使用示例：

```vue
<template>
    <ele-dashboard type="success">
        <div style="line-height: 1">
            <span style="font-size: 48px">100</span>
            <span style="font-size: 12px; margin-left: 4px">分</span>
        </div>
        <div style="margin-top: 4px">安全</div>
    </ele-dashboard>
</template>
```

<br/>

## 6.27.2. 属性列表  

| 属性  | 说明       | 类型          | 默认值                                 |
| :---- | :--------- | :------------ | :------------------------------------- |
| type  | 类型       | String        | 'primary','success','warning','danger' |
| color | 自定义颜色 | String        |                                        |
| size  | 尺寸       | Number,String |                                        |
| space | 间距       | Number,String |                                        |

<br/>

## 6.27.3. 插槽列表  

| 插槽名  | 说明     | 参数 |
| :------ | :------- | :--- |
| default | 默认插槽 |      |