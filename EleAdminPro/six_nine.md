---
title: 颜色选择
createTime: 2025/01/10 09:30:40
permalink: /EleAdminPro/h466j58k/
---
## 6.9.1. 快速使用  

组件名 `ele-color-picker` ，`v1.4.0新增` 使用方式：

```vue
<template>
    <ele-color-picker :show-alpha="true" v-model:value="color" :predefine="predefineColors" />
</template>

<script setup>
    import { ref } from 'vue';
    
    // 选中颜色
    const color = ref('rgba(255, 69, 0, 0.68)');
    
    // 预设颜色
    const predefineColors = ref([
        '#ff4500',
        '#ff8c00',
        '#ffd700',
        '#90ee90',
        '#00ced1',
        '#1e90ff',
        '#c71585',
        'rgba(255, 69, 0, 0.68)',
        'rgb(255, 120, 0)',
        'hsv(51, 100, 98)',
        'hsva(120, 40, 94, 0.5)',
        'hsl(181, 100%, 37%)',
        'hsla(209, 100%, 56%, 0.73)',
        '#c7158577'
    ]);
</script>
```

![](/images/6-9-1.png)

```vue
<script>
    import EleColorPicker from 'ele-admin-pro/packages/ele-color-picker';

    export default {
        components: {EleColorPicker}
    }
</script>
```

<br/>

## 6.9.2. 属性列表  

| 属性名       | 类型            | 说明                      | 默认值                             |
| :----------- | :-------------- | :------------------------ | :--------------------------------- |
| value        | String          | v-model                   | 绑定值                             |
| disabled     | Boolean         | 是否禁用                  | false                              |
| size         | String          | 尺寸                      | 可选 'large' / 'default' / 'small' |
| show-alpha   | Boolean         | 是否支持透明度选择        | false                              |
| color-format | String          | 写入 v-model 的颜色的格式 | 可选 'hsl' / 'hsv' / 'hex' / 'rgb' |
| popper-class | String          | 自定义下拉框的类名        |                                    |
| predefine    | `Array<string>` | 预定义颜色                |                                    |
| customClass  | String          | 自定义类名 `v1.6.0新增`   |                                    |

<br/>

## 6.9.3. 事件列表  

| 事件名称      | 说明                               | 回调参数         |
| :------------ | :--------------------------------- | :--------------- |
| change        | 当绑定值变化时触发                 | 当前值           |
| active-change | 面板中当前显示的颜色发生改变时触发 | 当前显示的颜色值 |
| update:value  | 用于 v-model                       | value: string    |