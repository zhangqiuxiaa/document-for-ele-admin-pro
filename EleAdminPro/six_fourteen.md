---
title: 富文本框
createTime: 2025/01/10 09:30:40
permalink: /EleAdminPro/o9qdubyv/
---
## 6.14.1. 快速使用  

组件名 `TinymceEditor` ，位于 `src/components/TinymceEditor` ，使用方式：

```vue
<template>
    <tinymce-editor v-model:value="content" :init="{ height: 525 }" />
</template>

<script setup>
    import TinymceEditor from '@/components/TinymceEditor/index.vue';

    const content = ref('');

    /* 修改编辑器内容 */
    const setContent = () => {
        content.value = '<div>EleAdminPro后台管理模板</div>';
    };
</script>
```

详细使用说明请查看 [TinyMCE中文文档](http://tinymce.ax-z.cn/) ，本组件基于 TinymceVue 封装，Github 地址：[tinymce/tinymce-vue](https://github.com/tinymce/tinymce-vue) 。

全部属性：

| 属性名      | 类型                      | 说明                              | 默认值                   |
| :---------- | :------------------------ | :-------------------------------- | :----------------------- |
| value       | String                    | 绑定值(v-model)                   |                          |
| init        | Object: RawEditorSettings | 编辑器配置                        |                          |
| disabled    | Boolean                   | 是否禁用                          |                          |
| autoTheme   | Boolean                   | 是否跟随框架主题                  | true                     |
| darkTheme   | Boolean                   | 是否使用暗黑主题 `v1.6.0新增`     |                          |
| id          | String                    | 编辑器唯一 id `v1.10.0新增`       |                          |
| inline      | Boolean                   | 是否内联模式 `v1.10.0新增`        | false                    |
| modelEvents | String                    | 实现 v-model 的事件 `v1.10.0新增` | 'change input undo redo' |
| tagName     | String                    | 内联模式标签名 `v1.10.0新增`      | 'div'                    |

默认会跟随框架切换暗黑主题，当 autoTheme 配置为 false 时你可以通过 darkTheme 属性设置是否使用暗黑主题。

编辑器配置 `init` 属性的默认值：

```javascript
const DEFAULT_CONFIG = {
    height: 300,
    branding: false,
    skin_url: '/tinymce/skins/ui/oxide',
    content_css: '/tinymce/skins/content/default/content.css',
    language_url: '/tinymce/langs/zh_CN.js',
    language: 'zh_CN',
    plugins: 'code preview fullscreen searchreplace save autosave link autolink image media table codesample lists advlist charmap emoticons anchor directionality pagebreak quickbars nonbreaking visualblocks visualchars wordcount',
    toolbar: 'fullscreen preview code | undo redo | forecolor backcolor | bold italic underline strikethrough | alignleft aligncenter alignright alignjustify | outdent indent | numlist bullist | formatselect fontselect fontsizeselect | link image media emoticons charmap anchor pagebreak codesample | ltr rtl',
    draggable_modal: true,
    toolbar_drawer: 'sliding',
    // 图片上传处理，默认转为 base64 格式
    images_upload_handler: (blobInfo, success) => {
        success('data:image/jpeg;base64,' + blobInfo.base64());
    },
    file_picker_types: 'media',
    file_picker_callback: () => {
    }
}
```

实例方法 `alert` 弹出提示框 `v1.10.0新增` ，可用于实现文件上传做校验提示，因为编辑器层级较高，使用 `message.error` 会被遮挡：

::: code-tabs

@tab TypeScript

```vue
<template>
    <div class="ele-body">
        <tinymce-editor ref="editorRef" v-model:value="content" />
        <a-button type="primary" @click="openAlert">弹出提示框</a-button>
    </div>
</template>

<script lang="ts" setup>
    import { ref } from 'vue';
    import TinymceEditor from '@/components/TinymceEditor/index.vue';

    const editorRef = ref<InstanceType<typeof TinymceEditor> | null>(null);

    const content = ref('');

    const openAlert = () => {
        editorRef.value?.alert({
            title: '提示',
            content: '请选择正确的文件格式'
        });
    };
</script>
```

@tab JavaScript

```vue
<template>
    <div class="ele-body">
        <tinymce-editor ref="editorRef" v-model:value="content" />
        <a-button type="primary" @click="openAlert">弹出提示框</a-button>
    </div>
</template>

<script setup>
    import { ref } from 'vue';
    import TinymceEditor from '@/components/TinymceEditor/index.vue';

    const editorRef = ref(null);

    const content = ref('');

    const openAlert = () => {
        editorRef.value?.alert({
            title: '提示',
            content: '请选择正确的文件格式'
        });
    };
</script>
```

:::

![](/images/6-14-1.png)

<br/>

## 6.14.2. 图片上传  

自定义图片上传需要重写 `images_upload_handler` 方法，这里演示使用 axios 上传的方式：

```vue
<template>
    <tinymce-editor v-model:value="content" :init="config" />
</template>

<script setup>
    import TinymceEditor from '@/components/TinymceEditor';
    import request from '@/utils/request';

    const content = ref('');

    const config = ref({
        height: 525,
        images_upload_handler: (blobInfo, success, error) => {
            const file = blobInfo.blob();
            // 使用 axios 上传，实际开发这段建议写在 api 中再调用 api
            const formData = new FormData();
            formData.append('file', file, file.name);
            request.post('/file/upload', formData).then(res => {
                if (res.data.data.url) {
                    success(res.data.data.url);
                } else {
                    error(res.data.message);
                }
            }).catch(e => {
                console.error(e);
                error(e.message);
            });
        }
    });
</script>
```

<br/>

## 6.14.3. 文件上传  

自定义文件上传需要重写 `file_picker_callback` 方法，这里演示使用 axios 上传的方式：

```vue
<template>
    <tinymce-editor v-model:value="content" :init="config" />
</template>

<script setup>
    import TinymceEditor from '@/components/TinymceEditor';
    import request from '@/utils/request';
    import { message } from 'ant-design-vue';

    const content = ref('');

    const config = ref({
        height: 525,
        file_picker_callback: (callback, value, meta) => {
            const input = document.createElement('input');
            input.setAttribute('type', 'file');
            // 设定文件可选类型
            if (meta.filetype === 'image') {
                input.setAttribute('accept', 'image/*');
            } else if (meta.filetype === 'media') {
                input.setAttribute('accept', 'video/*');
                //input.setAttribute('accept', 'audio/*');
            }
            input.onchange = () => {
                const file = input.files[0];
                // 使用 axios 上传，实际开发这段建议写在 api 中再调用 api
                const formData = new FormData();
                formData.append('file', file, file.name);
                request.post('/file/upload', formData).then(res => {
                    if (res.data.data.url) {
                        callback(res.data.data.url);
                    } else {
                        message.error(res.data.message);
                    }
                }).catch(e => {
                    console.error(e);
                    message.error(e.message);
                });
            }
            input.click();
        }
    });
</script>
```

文件上传与图片不一样，需要准备一个 input 触发文件选择，这里你也可以不用 input，比如弹窗选择服务器文件库的文件。