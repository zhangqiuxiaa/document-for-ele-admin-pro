---
title: 图片裁剪
createTime: 2025/01/10 09:30:40
permalink: /EleAdminPro/cm9yz8a4/
---
## 6.3.1. 快速使用  

组件名 `ele-cropper-model` ，基于 [cropperjs](https://github.com/fengyuanchen/cropperjs) 开发 ，使用方式：

::: code-tabs

@tab TypeScript

```vue
<template>
    <div class="ele-body">
        <img :src="avatar" @click="visible=true" />
        <!-- 图片裁剪弹窗 -->
        <ele-cropper-modal v-model:visible="visible" :src="avatar" @done="onDone" />
    </div>
</template>

<script lang="ts" setup>
    import { ref } from 'vue';

    const avatar = ref('http://cdn.eleadmin.com/20200610/avatar.jpg');

    // 是否显示裁剪弹窗
    const visible = ref(false);

    /* 裁剪完成回调 */
    const onDone = (res: string) => {
        avatar.value = res;
        visible.value = false;
    };
</script>
```

@tab JavaScript

```vue
<template>
    <div class="ele-body">
        <img :src="avatar" @click="visible=true" />
        <!-- 图片裁剪弹窗 -->
        <ele-cropper-modal v-model:visible="visible" :src="avatar" @done="onDone" />
    </div>
</template>

<script setup>
    import { ref } from 'vue';

    const avatar = ref('http://cdn.eleadmin.com/20200610/avatar.jpg');

    // 是否显示裁剪弹窗
    const visible = ref(false);

    /* 裁剪完成回调 */
    const onDone = (res) => {
        avatar.value = res;
        visible.value = false;
    };
</script>
```

:::

![](/images/6-3-1.png)

注意，v1.5.0 以及之前的版本需要手动引入：

```vue
<script>
    import EleCropperModal from 'ele-admin-pro/packages/ele-cropper-modal';
    // v1.0.0 版本的引入方式为如下写法
    //import EleCropperModal from '@/components/EleCropperModal';

    export default {
        components: { EleCropperModal }
    }
</script>
```

<br/>

## 6.3.2. 属性列表  

| 属性名                | 类型            | 说明                                            | 默认值                         |
| :-------------------- | :-------------- | :---------------------------------------------- | :----------------------------- |
| visible               | Boolean         | 是否显示(v-model)                               |                                |
| src                   | String          | 裁剪的图片地址                                  |                                |
| imageType             | String          | 默认的图片地址对应的图片类型 `1.9.0新增`        | 'image/png'                    |
| accept                | String          | 允许上传的图片类型                              | 'image/*'                      |
| tools                 | String/Boolean  | 操作按钮布局, false 不显示                      |                                |
| showPreview           | Boolean         | 是否显示预览组件                                | true                           |
| previewWidth          | Number          | 预览组件宽度 `1.8.0新增`                        | 120                            |
| okText                | String          | 完成按钮文字                                    |                                |
| toBlob                | Boolean         | 裁剪后是否返回 blob 类型                        | 默认 false 返回 base64         |
| options               | Object          | cropperjs 的参数 `1.9.0新增`                    |                                |
| croppedOptions        | Object          | cropperjs 裁剪方法的参数 `1.9.0新增`            |                                |
| aspectRatio           | Number          | 裁剪比例 `1.9.0移除`                            | 1/1                            |
| viewMode              | Number          | 裁剪器的视图模式 `1.9.0移除`                    | 0                              |
| dragMode              | String          | 裁剪器的拖动模式 `1.9.0移除`                    | 'crop'                         |
| initialAspectRatio    | Number          | 裁剪框的初始高宽比 `1.9.0移除`                  |                                |
| minContainerWidth     | Number          | 容器的最小宽度 `1.9.0移除`                      | 200                            |
| minContainerHeight    | Number          | 容器的最小高度 `1.9.0移除`                      | 100                            |
| minCanvasWidth        | Number          | 画布的最小宽度 `1.9.0移除`                      | 0                              |
| minCanvasHeight       | Number          | 画布的最小高度 `1.9.0移除`                      | 0                              |
| minCropBoxWidth       | Number          | 裁剪框的最小宽度 `1.9.0移除`                    | 0                              |
| minCropBoxHeight      | Number          | 裁剪框的最小高度 `1.9.0移除`                    | 0                              |
| croppedWidth          | Number          | 裁剪后输出宽度 `1.9.0移除`                      |                                |
| croppedHeight         | Number          | 裁剪后输出高度 `1.9.0移除`                      |                                |
| croppedMinWidth       | Number          | 裁剪后输出最小宽度 `1.9.0移除`                  |                                |
| croppedMinHeight      | Number          | 裁剪后输出最小高度 `1.9.0移除`                  | 0                              |
| croppedMaxWidth       | Number          | 裁剪后输出最大宽度 `1.9.0移除`                  | Infinity                       |
| croppedMaxHeight      | Number          | 裁剪后输出最大高度 `1.9.0移除`                  | Infinity                       |
| croppedFillColor      | Number          | 裁剪后输出的背景填充色 `1.9.0移除`              | 'transparent'                  |
| imageSmoothingEnabled | Boolean         | `1.9.0移除`                                     | true                           |
| imageSmoothingQuality | String          | 裁剪后输出质量 `1.9.0移除`                      | 默认 'low' , 可选 medium、high |
| title                 | String          | 弹窗的标题                                      | '裁剪图片'                     |
| width                 | String          | 弹窗的宽度                                      | '680px'                        |
| dialogStyle           | Object          | 弹窗样式                                        |                                |
| centered              | Boolean         | 垂直居中展示 Modal                              | false                          |
| closable              | Boolean         | 是否显示右上角的关闭按钮                        | true                           |
| destroyOnClose        | Boolean         | 关闭时销毁 Modal 里的子元素                     | false                          |
| forceRender           | Boolean         | 强制渲染 Modal                                  | false                          |
| keyboard              | Boolean         | 是否支持键盘 esc 关闭                           | true                           |
| mask                  | Boolean         | 是否展示遮罩                                    | true                           |
| maskClosable          | Boolean         | 点击蒙层是否允许关闭                            | true                           |
| maskStyle             | Object          | 遮罩样式                                        | {}                             |
| wrapClassName         | String          | 对话框外层容器的类名                            |                                |
| zIndex                | Number          | 设置 Modal 的 z-index                           | 1000                           |
| dialogClass           | String          | 可用于设置浮层的类名                            |                                |
| movable               | Boolean         | 是否可以拖动 `v1.7.0新增`                       | true                           |
| moveOut               | Boolean         | 是否可以拖出边界 `v1.7.0新增`                   | false                          |
| moveOutPositive       | Boolean         | 如果可以拖出边界是否只允许下右拖出 `v1.7.0新增` | false                          |
| resizable             | Boolean, String | 是否可以拉伸 `v1.7.0新增`                       | false                          |
| maxable               | Boolean         | 是否显示最大化切换按钮 `v1.7.0新增`             | false                          |
| multiple              | Boolean         | 是否支持打开多个 `v1.7.0新增`                   | false                          |
| fullscreen            | Boolean         | 是否默认全屏 `v1.7.0新增`                       | false                          |
| inner                 | Boolean         | 是否限制在主体内部 `v1.7.0新增`                 | false                          |
| resetOnClose          | Boolean         | 是否在弹窗关闭后重置位置和大小 `v1.7.0新增`     | true                           |
| maskClass             | String          | 遮罩的类名 `v1.9.0新增`                         |                                |

弹窗相关的几个参数同 ant-design-vue 的 a-modal 组件的参数及弹窗扩展 ele-modal 组件。

<br/>

## 6.3.3. 事件列表  

| 事件名称       | 说明                        | 回调参数             |
| :------------- | :-------------------------- | :------------------- |
| open           | Dialog 打开的回调           |                      |
| closed         | Dialog 关闭的回调           |                      |
| done           | 裁剪完成的回调              | 裁剪后的 base64 数据 |
| update:visible | 用于 visible 属性的 v-model | visible: Boolean     |

<br/>

## 6.3.4. 上传服务端  

裁剪后返回 base64 字符，服务端可以接收字符类型，如果服务端非要使用 File 接收，先设置 `:to-blob="true"` 属性，然后使用如下方法上传：

::: code-tabs

@tab TypeScript

```vue
<template>
    <ele-cropper-modal :to-blob="true" @done="onDone" />
</template>

<script lang="ts" setup>
    import request from '@/utils/request';

    const onDone = (blob: Blob | null) => {  // 裁剪完成的回调
        const formData = new FormData();
        formData.append('file', blob);
        //formData.append('file', blob, 'avatar.png');  // 参数三可以设定文件名称
        // 使用 axios 上传
        request.post('/file/upload', formData).then(res => {
            console.log(res);
        }).catch(e => {
            console.error(e);
        });
    };
</script>
```

@tab JavaScript

```vue
<template>
    <ele-cropper-modal :to-blob="true" @done="onDone" />
</template>

<script setup>
    import request from '@/utils/request';

    const onDone = (blob) => {  // 裁剪完成的回调
        const formData = new FormData();
        formData.append('file', blob);
        //formData.append('file', blob, 'avatar.png');  // 参数三可以设定文件名称
        // 使用 axios 上传
        request.post('/file/upload', formData).then(res => {
            console.log(res);
        }).catch(e => {
            console.error(e);
        });
    };
</script>
```

:::