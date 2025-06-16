---
title: 二维码
createTime: 2025/01/10 09:30:40
permalink: /EleAdminPro/nq3hodr0/
---
## 6.19.1. 快速使用  

二维码组件支持 canvas 和 svg 两种渲染方式，对应两个组件，推荐使用 svg 方式，`v1.8.0新增`，使用示例：

```vue
<template>
    <div>
        <!-- canvas 渲染 -->
        <ele-qr-code :value="text" :size="120" />
        <!-- svg 渲染 -->
        <ele-qr-code-svg :value="text" :size="120" />
    </div>
</template>

<script setup>
    import { ref } from 'vue';

    const text = ref('https://eleadmin.com');
</script>
```

<br/>

## 6.19.2. 属性列表  

| 属性          | 说明                           | 类型                  | 默认值             |
| :------------ | :----------------------------- | :-------------------- | :----------------- |
| value         | 二维码内容                     | String                |                    |
| size          | 尺寸                           | Number                | 128                |
| level         | 容错等级                       | String                | 'L'、'M'、'Q'、'H' |
| bgColor       | 背景色                         | String                | '#FFFFFF'          |
| fgColor       | 颜色                           | String                | '#000000'          |
| margin        | 外间距                         | Number                | 0                  |
| imageSettings | 自定义图片                     | Object: ImageSettings |                    |
| margin        | 自定义样式(仅用于 canvas 渲染) | Object: CSSProperties |                    |

value 改变后会自动刷新二维码，自定义图片的参数 ImageSettings 字段说明：

- src   String   图片地址
- height   Number   图片高度
- width   Number   图片宽度
- x   Number   x 坐标, x、y 都为空就是居中
- y   Number   y 坐标, x、y 都为空就是居中
- excavate   Boolean   是否擦除图片区域的二维码

添加自定义图片 logo 示例：

::: code-tabs

@tab TypeScript

```vue
<template>
    <div>
        <!-- canvas 渲染 -->
        <ele-qr-code :value="text" :size="120" :image-settings="image" />
        <!-- svg 渲染 -->
        <ele-qr-code-svg :value="text" :size="120" :image-settings="image" />
    </div>
</template>

<script lang="ts" setup>
    import { ref, reactive } from 'vue';
    import type { ImageSettings } from 'ele-admin-pro/es/ele-qr-code/types';

    const text = ref('https://eleadmin.com');

    const image = reactive<ImageSettings>({
        src: 'https://cdn.eleadmin.com/20200610/logo-radius.png',
        width: 28,
        height: 28
    });
</script>
```

@tab JavaScript

```vue
<template>
    <div>
        <!-- canvas 渲染 -->
        <ele-qr-code :value="text" :size="120" :image-settings="image" />
        <!-- svg 渲染 -->
        <ele-qr-code-svg :value="text" :size="120" :image-settings="image" />
    </div>
</template>

<script setup>
    import { ref, reactive } from 'vue';

    const text = ref('https://eleadmin.com');

    const image = reactive({
        src: 'https://cdn.eleadmin.com/20200610/logo-radius.png',
        width: 28,
        height: 28
    });
</script>
```

:::

<br/>

## 6.19.3. 事件列表  

| 事件名称 | 说明           | 回调参数 |
| :------- | :------------- | :------- |
| done     | 渲染完成的事件 | 无       |
