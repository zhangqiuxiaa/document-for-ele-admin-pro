---
title: 滚动数字
createTime: 2025/01/10 09:30:40
permalink: /EleAdminPro/1mgku9k2/
---
## 6.16.1. 快速使用  

滚动数字是基于 [CountUp.js](https://github.com/inorganik/CountUp.js) 封装的 vue3 组件 `ele-count-up` ，使用方式：

```vue
<template>
    <div>
        <ele-count-up :end-val="countUpVal" />
        <a-button @click="updateCountUp">更新数字</a-button>
    </div>
</template>

<script setup>
    import { ref } from 'vue';

    // countUp 值
    const countUpVal = ref(6317);
    
    // 绑定的值改变后自动以动画形式过渡到改变后的数字
    const updateCountUp = () => {
        countUpVal.value = 9600;
    };
</script>
```

<br/>

## 6.16.2. 属性列表  

| 属性名  | 类型                   | 说明           | 默认值 |
| :------ | :--------------------- | :------------- | :----- |
| endVal  | Number                 | 动画结束值     |        |
| delay   | Number                 | 动画延迟时间   | 300    |
| options | Object: CountUpOptions | CountUpJs 配置 |        |

<br/>

## 6.16.3. 事件列表  

| 事件名称 | 说明                            | 回调参数          |
| :------- | :------------------------------ | :---------------- |
| ready    | 渲染完成回调，返回 CountUp 实例 | instance: CountUp |

高级用法，通过 options 设置更多参数，通过 ready 事件获取实例操作更多方法：

::: code-tabs

@tab TypeScript

```vue
<template>
    <div>
        <ele-count-up
            :end-val="countUpVal"
            :options="countUpOptions"
            @ready="onCountUpReady" />
        <a-button @click="startCountUp">重新开始</a-button>
        <a-button @click="updateCountUp">更新数字</a-button>
    </div>
</template>

<script lang="ts" setup>
    import { ref, reactive } from 'vue';
    import type { CountUp } from 'countup.js';
    
    // countUp 值
    const countUpVal = ref(6317);
    
    // countUp 配置
    const countUpOptions = reactive({
        useEasing: true,
        useGrouping: true,
        separator: ',',
        decimal: '.',
        prefix: '',
        suffix: ''
    });
    
    // countUp 实例
    let countUpIns: CountUp;
    
    /* countUp 渲染完成 */
    const onCountUpReady = (instance: CountUp) => {
        countUpIns = instance;
    };
    
    /* countUp 重新开始 */
    const startCountUp = () => {
        if (!countUpIns) {
            return;
        }
        countUpIns.reset();
        countUpIns.start();
    };
    
    /* countUp 更新 */
    const updateCountUp = () => {
        if (!countUpIns) {
            return;
        }
        countUpIns.update(1000 + Math.round(Math.random() * 9000));
    };
</script>
```

@tab JavaScript

```vue
<template>
    <div>
        <ele-count-up
            :end-val="countUpVal"
            :options="countUpOptions"
            @ready="onCountUpReady" />
        <a-button @click="startCountUp">重新开始</a-button>
        <a-button @click="updateCountUp">更新数字</a-button>
    </div>
</template>

<script setup>
    import { ref, reactive } from 'vue';
    
    // countUp 值
    const countUpVal = ref(6317);
    
    // countUp 配置
    const countUpOptions = reactive({
        useEasing: true,
        useGrouping: true,
        separator: ',',
        decimal: '.',
        prefix: '',
        suffix: ''
    });
    
    // countUp 实例
    let countUpIns;
    
    /* countUp 渲染完成 */
    const onCountUpReady = (instance) => {
        countUpIns = instance;
    };
    
    /* countUp 重新开始 */
    const startCountUp = () => {
        if (!countUpIns) {
            return;
        }
        countUpIns.reset();
        countUpIns.start();
    };
    
    /* countUp 更新 */
    const updateCountUp = () => {
        if (!countUpIns) {
            return;
        }
        countUpIns.update(1000 + Math.round(Math.random() * 9000));
    };
</script>
```

:::