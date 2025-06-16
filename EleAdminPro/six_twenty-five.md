---
title: 漫游式引导
createTime: 2025/01/10 09:30:40
permalink: /EleAdminPro/pxqbyqz1/
---
## 6.25.1. 快速使用  

漫游式引导组件名称 `ele-tour` ，支持多种组合形式， `v1.11.0新增`，使用示例：

```vue
<template>
    <div>
        <a-button type="primary" @click="onStart">开始引导</a-button>
        <a-button ref="uploadRef">Upload</a-button>
        <a-button ref="saveRef" type="primary">Save</a-button>
        <a-button ref="moreRef">More</a-button>
        <ele-tour v-model="current" :steps="steps" />
    </div>
</template>

<script setup>
    import { ref } from 'vue';

    // 当前步骤
    const current = ref(null);

    const uploadRef = ref(null);
    const saveRef = ref(null);
    const moreRef = ref(null);

    // 步骤
    const steps = ref([
        {
            target: () => uploadRef.value?.$el,
            title: '如何进行文件上传',
            description: '点击这个按钮在弹出框中选择想要上传的文件即可.'
        },
        {
            target: () => saveRef.value?.$el,
            title: '如何提交数据',
            description: '数据录入完成后点击这个按钮即可提交数据到后台.'
        },
        {
            target: () => moreRef.value?.$el,
            title: '如何进行更多的操作',
            description: '鼠标移入到此按钮上即可展示出更多的操作功能.'
        }
    ]);
    
    /* 开始引导 */
    const onStart = () => {
        current.value = 0;
    }
</script>
```

<br/>

## 6.25.2. 属性列表  

| 属性       | 说明                    | 类型    | 默认值 |
| :--------- | :---------------------- | :------ | :----- |
| modelValue | 当前处于第几步, v-model | Number  |        |
| steps      | 步骤                    | Array   |        |
| mask       | 是否开启遮罩层          | Boolean |        |
| padding    | 高亮区内间距            | Number  | 6      |
| zIndex     | 层级                    | Number  |        |
| locale     | 底部按钮文本            | Object  |        |

steps 步骤数据格式说明：

| 属性         | 说明           | 类型                           |
| :----------- | :------------- | :----------------------------- |
| target       | 指向元素       | HTMLElement, () => HTMLElement |
| title        | 标题           | String                         |
| description  | 描述           | String                         |
| padding      | 高亮区内间距   | Number                         |
| mask         | 是否开启遮罩层 | Boolean                        |
| popoverProps | 气泡组件属性   | Object                         |

<br/>

## 6.25.3. 插槽列表  

| 插槽名 | 说明               | 参数            |
| :----- | :----------------- | :-------------- |
| title  | 自定义标题         | step, current |
| text   | 自定义内容区       | step, current |
| footer | 自定义底部操作按钮 | step, current |
