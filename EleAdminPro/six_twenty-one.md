---
title: markdown
createTime: 2025/01/10 09:30:40
permalink: /EleAdminPro/gtd49deg/
---
## 6.21.1. 快速使用  

markdown 编辑器位于 `src/components/ByteMdEditor` ，基于字节开源的 [bytemd](https://github.com/bytedance/bytemd) 封装，使用方式：

```vue
<template>
    <div>
        <byte-md-editor
            v-model:value="content"
            :locale="zh_Hans"
            :plugins="plugins"
            height="600px"
        />
        <a-button @click="setContent">修改内容</a-button>
    </div>
</template>

<script setup>
    import { ref } from 'vue';
    import ByteMdEditor from '@/components/ByteMdEditor/index.vue';
    // 中文语言文件
    import zh_Hans from 'bytemd/lib/locales/zh_Hans.json';
    // 链接、删除线、复选框、表格等的插件
    import gfm from '@bytemd/plugin-gfm';
    // 插件的中文语言文件
    import zh_HansGfm from '@bytemd/plugin-gfm/lib/locales/zh_Hans.json';
    // 预览界面的样式，这里用的 github 的 markdown 主题
    import 'github-markdown-css/github-markdown-light.css';

    // 编辑器内容，双向绑定
    const content = ref('');

    // 插件
    const plugins = ref([
        gfm({
            locale: zh_HansGfm
        })
    ]);

    /* 修改编辑器内容 */
    const setContent = () => {
        content.value = '**EleAdminPro后台管理模板**';
    };
</script>
```

bytemd 官方的 vue 版本目前还是 vue2 版本的，在 vue3 中无法使用，所以这里参考官网的版本进行了重新封装。

<br/>

## 6.21.2. 属性列表  

| 属性名          | 类型                  | 说明                                                         | 默认值 |
| :-------------- | :-------------------- | :----------------------------------------------------------- | :----- |
| value           | String                | 编辑器内容, v-model                                          |        |
| plugins         | `Array<BytemdPlugin>` | 插件列表                                                     |        |
| sanitize        | Function              | Sanitize strategy                                            |        |
| mode            | String                | 展示模式, 可选值: 'split'、'tab'、'auto'                     |        |
| previewDebounce | Number                | 实时预览的节流时间                                           | 300    |
| placeholder     | String                | 提示文本                                                     |        |
| editorConfig    | Object                | [CodeMirror](https://codemirror.net/doc/manual.html#config) 的配置 | false  |
| locale          | Object: BytemdLocale  | 国际化语言                                                   |        |
| uploadImages    | Function              | 自定义上传图片的实现                                         |        |
| overridePreview | Function              | 自定义图片预览的实现                                         |        |
| maxLength       | Number                | 最大输入限制                                                 |        |
| height          | String                | 编辑器的高度                                                 |        |
| fullZIndex      | Number                | 全屏时的 z-index                                             | 999    |

<br/>

## 6.21.3. 事件列表  

| 事件名称     | 说明         | 回调参数      |
| :----------- | :----------- | :------------ |
| change       | 输入改变事件 | value: string |
| update:value | 用于 v-model | value: string |

<br/>

## 6.21.4. 展示组件  

markdown 内容解析展示组件 `src/components/ByteMdViewer` ，使用方式：

```vue
<template>
    <byte-md-viewer :value="content" :plugins="plugins" />
</template>

<script setup>
    import { ref } from 'vue';
    import ByteMdViewer from '@/components/ByteMdViewer/index.vue';
    // 链接、删除线、复选框、表格等的插件
    import gfm from '@bytemd/plugin-gfm';
    // 内容样式，这里用的 github 的 markdown 主题
    import 'github-markdown-css/github-markdown-light.css';
    // 这里演示请求远程数据，实际项目可以写在 api 中
    import request from '@/utils/request';

    // markdown 内容
    const content = ref('');

    // 插件
    const plugins = ref([gfm()]);

    /* 获取远程数据 */
    request.get('https://cdn.eleadmin.com/20200610/md-demo.md').then((res) => {
        content.value = res.data;
    });
</script>
```

属性列表：

| 属性名   | 类型                  | 说明              | 默认值 |
| :------- | :-------------------- | :---------------- | :----- |
| value    | String                | 内容, v-model     |        |
| plugins  | `Array<BytemdPlugin>` | 插件列表          |        |
| sanitize | Function              | Sanitize strategy |        |