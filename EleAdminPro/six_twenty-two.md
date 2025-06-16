---
title: 图片上传
createTime: 2025/01/10 09:30:40
permalink: /EleAdminPro/4j1xpi72/
---
## 6.22.1. 快速使用  

多图上传组件名称 `ele-image-upload` ，支持拖动排序， `v1.9.0新增`，使用示例：

::: code-tabs

@tab TypeScript

```vue
<template>
    <ele-image-upload v-model:value="images" :limit="8" @upload="onUpload" />
</template>

<script lang="ts" setup>
    import { ref } from 'vue';
    import type { ItemType } from 'ele-admin-pro/es/ele-image-upload/types';

    // 已上传数据, 可赋初始值用于回显
    const images = ref([
        {
            uid: 1,
            url: 'https://cdn.eleadmin.com/20200609/c184eef391ae48dba87e3057e70238fb.jpg',
            status: 'done'
        },
        {
            uid: 2,
            url: 'https://cdn.eleadmin.com/20200610/WLXm7gp1EbLDtvVQgkeQeyq5OtDm00Jd.jpg',
            status: 'done'
        }
    ]);

    const onUpload = (d: ItemType) => {
        console.log(d);
    }
</script>
```

@tab JavaScript

```vue
<template>
    <ele-image-upload v-model:value="images" :limit="8" @upload="onUpload" />
</template>

<script setup>
    import { ref } from 'vue';

    // 已上传数据, 可赋初始值用于回显
    const images = ref([
        {
            uid: 1,
            url: 'https://cdn.eleadmin.com/20200609/c184eef391ae48dba87e3057e70238fb.jpg',
            status: 'done'
        },
        {
            uid: 2,
            url: 'https://cdn.eleadmin.com/20200610/WLXm7gp1EbLDtvVQgkeQeyq5OtDm00Jd.jpg',
            status: 'done'
        }
    ]);

    const onUpload = (d) => {
        console.log(d);
    }
</script>
```

:::

<br/>

## 6.22.2. 属性列表  

| 属性          | 说明                                 | 类型     | 默认值                 |
| :------------ | :----------------------------------- | :------- | :--------------------- |
| value         | 已上传列表, v-model                  | Array    |                        |
| disabled      | 是否禁用                             | Boolean  |                        |
| limit         | 最大上传数量                         | Number   |                        |
| accept        | 接受上传的文件类型                   | String   | 'image/png,image/jpeg' |
| preview       | 是否可以点击预览                     | Boolean  | true                   |
| itemStyle     | 自定义 item 样式                     | Object   |                        |
| buttonStyle   | 自定义上传按钮样式                   | Object   |                        |
| autoUpload    | 是否在选取文件后立即进行上传         | Boolean  | true                   |
| drag          | 是否启用拖拽上传                     | Boolean  |                        |
| beforeUpload  | 上传文件之前的钩子                   | Function |                        |
| beforeRemove  | 删除文件之前的钩子                   | Function |                        |
| multiple      | 是否支持多选文件, 同 a-upload 的属性 | Boolean  |                        |
| directory     | 是否选择文件夹, 同 a-upload 的属性   | Boolean  |                        |
| uploadHandler | 完全自己控制上传的处理               | Function |                        |
| removeHandler | 完全自己控制删除的处理               | Function |                        |

v-model 绑定的数据格式说明：

- uid   唯一 id , 用于循环的 key
- url   String, 显示的图片地址, 为空时会显示为文件图标
- name   String, 文件名称, 非必须
- status   String, 状态, null(未上传)、uploading(上传中)、exception(上传失败)、done(上传完成)
- progress   Nunber, 上传的进度, 0 - 100
- file   File, 选择的文件
- 也可以加自己需要的其它字段

可以通过 itemStyle 自定义显示图片的宽高等，以及 buttonStyle 自定义上传按钮的宽高，默认是 100px 的正方形，也可以改为长方形：

```vue
<template>
    <ele-image-upload 
        v-model:value="images" 
        :item-style="{width: '120px', height: '60px'}"
        :button-style="{width: '60px', height: '60px'}"
    />
</template>
```

可以通过 beforeUpload 对选择的文件进行大小、格式校验等，格式可以通过 accept 属性控制，beforeUpload 中再检查一遍是双保险：

::: code-tabs

@tab TypeScript

```vue
<template>
    <ele-image-upload v-model:value="images" :before-upload="onBeforeUpload"/>
</template>

<script lang="ts" setup>
    import { ref } from 'vue';
    import { message } from 'ant-design-vue';
    import type { ItemType, BeforeUploadType } from 'ele-admin-pro/es/ele-image-upload/types';

    const images = ref<ItemType[]>([]);

    /* beforeUpload 返回 false 则阻止上传, 也可以返回 Promise , resolve 继续上传, reject 阻止上传 */
    const onBeforeUpload: BeforeUploadType = (file: File) => {
        // file 即选择后的文件
        if (!file.type.startsWith('image')) {
            message.error('只能选择图片');
            return false;
        }
        if (file.size / 1024 / 1024 > 2) {
            message.error('大小不能超过 2MB');
            return false;
        }
    };
</script>
```

@tab JavaScript

```vue
<template>
    <ele-image-upload v-model:value="images" :before-upload="onBeforeUpload"/>
</template>

<script setup>
    import { ref } from 'vue';
    import { message } from 'ant-design-vue';

    const images = ref([]);

    /* beforeUpload 返回 false 则阻止上传, 也可以返回 Promise , resolve 继续上传, reject 阻止上传 */
    const onBeforeUpload = (file) => {
        // file 即选择后的文件
        if (!file.type.startsWith('image')) {
            message.error('只能选择图片');
            return false;
        }
        if (file.size / 1024 / 1024 > 2) {
            message.error('大小不能超过 2MB');
            return false;
        }
    };
</script>
```

:::

beforeRemove 返回 false 则阻止删除，也支持返回 Promise ，例如实现删除前的弹窗确认：

::: code-tabs

@tab TypeScript

```vue
<template>
    <ele-image-upload v-model:value="images" :before-remove="onBeforeRemove"/>
</template>

<script lang="ts" setup>
    import { ref, createVNode } from 'vue';
    import { message, Modal } from 'ant-design-vue';
    import { ExclamationCircleOutlined } from '@ant-design/icons-vue';
    import type { ItemType, BeforeUploadType } from 'ele-admin-pro/es/ele-image-upload/types';

    const images = ref<ItemType[]>([]);

    const onBeforeRemove: BeforeUploadType = (item: File) => {
        // item 即 v-model 绑定的数据中的要删除的数据
        console.log('item:', item);
        return new Promise<void>((resolve, reject) => {
            Modal.confirm({
                title: '提示',
                content: '确定要删除吗?',
                icon: createVNode(ExclamationCircleOutlined),
                maskClosable: true,
                onOk: () => {
                    resolve();
                },
                onCancel: () => {
                    reject();
                }
            });
        });
    }
</script>
```

@tab JavaScript

```vue
<template>
    <ele-image-upload v-model:value="images" :before-remove="onBeforeRemove"/>
</template>

<script setup>
    import { ref, createVNode } from 'vue';
    import { message, Modal } from 'ant-design-vue';
    import { ExclamationCircleOutlined } from '@ant-design/icons-vue';

    const images = ref([]);

    const onBeforeRemove = (item) => {
        // item 即 v-model 绑定的数据中的要删除的数据
        console.log('item:', item);
        return new Promise((resolve, reject) => {
            Modal.confirm({
                title: '提示',
                content: '确定要删除吗?',
                icon: createVNode(ExclamationCircleOutlined),
                maskClosable: true,
                onOk: () => {
                    resolve();
                },
                onCancel: () => {
                    reject();
                }
            });
        });
    }
</script>
```

:::

<br/>

## 6.22.3. 事件列表  

| 事件名称   | 说明            | 回调参数 |
| :--------- | :-------------- | :------- |
| upload     | item 上传的事件 | item     |
| remove     | item 删除的事件 | item     |
| item-click | item 点击的事件 | item     |

如果没有设置 `uploadHandler` 且开启了 `autoUpload` 时选择文件后就会触发 `upload` 事件，当上传失败点击重新上传按钮时也会触发:

::: code-tabs

@tab TypeScript

```vue
<!-- 这里演示使用 axios 上传文件并获取上传进度的示例 -->
<template>
    <div>
        <ele-image-upload v-model:value="images" @upload="onUpload" />
        <a-button @click="onSubmit">提交</a-button>
    </div>
</template>

<script lang="ts" setup>
    import { ref } from 'vue';
    import type { ItemType } from 'ele-admin-pro/es/ele-image-upload/types';
    import request from '@/utils/request';

    const images = ref<ItemType[]>([]);

    const onUpload = (d: ItemType) => {
        const item = images.value.find((t) => t.uid === d.uid) ?? d;
        // item 包含的字段参考前面说明
        console.log('item:', item);
        item.status = 'uploading';
        const formData = new FormData();
        formData.append('file', item.file);
        request({
            url: '/file/upload',
            method: 'post',
            data: formData,
            onUploadProgress: (e: any) => {  // 文件上传进度回调
                if (e.lengthComputable) {
                    item.progress = e.loaded / e.total * 100;
                }
            }
        }).then((res) => {
            if(res.data.code === 0) {
                item.status = 'done';
                item.url = res.data.data.url;
                // 如果你上传的不是图片格式, 建议将 url 字段作为缩略图, 再添加其它字段作为最后提交数据
                //item.url = res.data.data.thumbnail;  // 也可以不赋值 url 字段, 默认会显示为一个文件图标
                item.fileUrl = res.data.data.url;
            }
        }).catch((e: Error) => {
            item.status = 'exception';
        });
    };

    const onSubmit = () => {
        // 这里你可以获取上传后的数据与其它表单数据一起提交
        const urls = images.value.map(d => d.url);
        // 如果你是将 url 作为缩略图, 其它字段作为文件 url 可取你自定义的字段
        //const urls = images.value.map(d => d.fileUrl);
        console.log('urls:', urls);
    };
</script>
```

@tab JavaScript

```vue
<!-- 这里演示使用 axios 上传文件并获取上传进度的示例 -->
<template>
    <div>
        <ele-image-upload v-model:value="images" @upload="onUpload" />
        <a-button @click="onSubmit">提交</a-button>
    </div>
</template>

<script setup>
    import { ref } from 'vue';
    import request from '@/utils/request';

    const images = ref([]);

    const onUpload = (d) => {
        const item = images.value.find((t) => t.uid === d.uid) ?? d;
        // item 包含的字段参考前面说明
        console.log('item:', item);
        item.status = 'uploading';
        const formData = new FormData();
        formData.append('file', item.file);
        request({
            url: '/file/upload',
            method: 'post',
            data: formData,
            onUploadProgress: (e) => {  // 文件上传进度回调
                if (e.lengthComputable) {
                    item.progress = e.loaded / e.total * 100;
                }
            }
        }).then((res) => {
            if(res.data.code === 0) {
                item.status = 'done';
                item.url = res.data.data.url;
                // 如果你上传的不是图片格式, 建议将 url 字段作为缩略图, 再添加其它字段作为最后提交数据
                //item.url = res.data.data.thumbnail;  // 也可以不赋值 url 字段, 默认会显示为一个文件图标
                item.fileUrl = res.data.data.url;
            }
        }).catch((e) => {
            item.status = 'exception';
        });
    };

    const onSubmit = () => {
        // 这里你可以获取上传后的数据与其它表单数据一起提交
        const urls = images.value.map(d => d.url);
        // 如果你是将 url 作为缩略图, 其它字段作为文件 url 可取你自定义的字段
        //const urls = images.value.map(d => d.fileUrl);
        console.log('urls:', urls);
    };
</script>
```

:::

<br/>

## 6.22.4. 插槽列表  

| 插槽名   | 说明                     | 参数 |
| :------- | :----------------------- | :--- |
| icon     | 自定义上传图标插槽       | 无   |
| progress | 自定义上传进度条插槽     | item |
| image    | 自定义非图片类型文件插槽 | item |

默认上传图标是加号，例如换成其它图标：

```vue
<template>
    <ele-image-upload>
        <template #icon>
            <cloud-upload-outlined class="ele-image-upload-icon" />
            <!-- 也可以加点其它的 -->
            <div style="color: #999;font-size: 12px;margin-top: 4px;">请点击上传</div>
        </template>
    </ele-image-upload>
</template>

<script setup>
    import { CloudUploadOutlined } from '@ant-design/icons-vue';
</script>
```

默认上传进度条是条形的，如果你不会实现上传进度，可以换成一个一直转的圈圈：

```vue
<template>
    <ele-image-upload>
        <template #progress="{ item }">
            <loading-outlined style="font-size: 28px;" />
            <div style="margin-top: 4px;">
                {{ item.status === 'exception' ? '上传失败' : '上传中' }}
            </div>
        </template>
    </ele-image-upload>
</template>

<script setup>
    import { LoadingOutlined } from '@ant-design/icons-vue';
</script>
```

如果选择的不是图片类型的文件默认会显示一个文件图标，可以自定义当 item.url 为空时的显示：

```vue
<template>
    <ele-image-upload>
        <template #image="{ item }">
            <!-- 例如选的是视频文件显示视频的图标，否则显示文件的图标 -->
            <div v-if="item.file.type.startsWith('video')" class="ele-image-upload-thumbnail">
                <youtube-outlined />
            </div>
            <div v-else class="ele-image-upload-thumbnail">
                <file-text-outlined />
            </div>
        </template>
    </ele-image-upload>
</template>

<script setup>
    import { YoutubeOutlined, FileTextOutlined } from '@ant-design/icons-vue';
</script>
```

<br/>

## 6.22.5. 手动上传  

如果你不想选一个上传一个，想最后与其它表单数据一起直接提交 file 数组，只需要将 `autoUpload` 设为 false 即可：

::: code-tabs

@tab TypeScript

```vue
<template>
    <div>
        <ele-image-upload v-model:value="images" :auto-upload="false" />
        <a-button @click="onSubmit">提交</a-button>
    </div>
</template>

<script lang="ts" setup>
    import { ref } from 'vue';
    import type { ItemType } from 'ele-admin-pro/es/ele-image-upload/types';

    const images = ref<ItemType[]>([]);

    const onSubmit = () => {
        // 这里你可以获取上传后的数据与其它表单数据一起提交
        const files = images.value.map(d => d.file);
        console.log('files:', files);
    };
</script>
```

@tab JavaScript

```vue
<template>
    <div>
        <ele-image-upload v-model:value="images" :auto-upload="false" />
        <a-button @click="onSubmit">提交</a-button>
    </div>
</template>

<script setup>
    import { ref } from 'vue';

    const images = ref([]);

    const onSubmit = () => {
        // 这里你可以获取上传后的数据与其它表单数据一起提交
        const files = images.value.map(d => d.file);
        console.log('files:', files);
    };
</script>
```

:::

如果你是想点击表单提交按钮再一个一个上传，最后还是把所有上传后的 url 再与表单一起提交可以这样写：

::: code-tabs

@tab TypeScript

```vue
<template>
    <div>
        <ele-image-upload v-model:value="images" :auto-upload="false" />
        <a-button :loading="loading" @click="onSubmit">提交</a-button>
    </div>
</template>

<script lang="ts" setup>
    import { ref } from 'vue';
    import { message } from 'ant-design-vue';
    import type { ItemType } from 'ele-admin-pro/es/ele-image-upload/types';
    import request from '@/utils/request';

    const images = ref<ItemType[]>([]);

    const loading = ref(false);

    /* 手动上传 */
    const onSubmit = () => {
        if (checkDone()) {  // 检查是否全部上传完毕
            submitForm();  // 全部上传完毕后与其它表单数据一起提交
            return;
        }
        loading.value = true;
        // 循环一个一个上传
        images.value.forEach((item) => {
            if (!item.status) {
                uploadItem(item);
            }
        });
    };

    /* 上传图片 */
    const uploadItem = (item: ItemType) => {
        // 这里与前面介绍的一样还是使用 axios 上传并监听上传进度
        item.status = 'uploading';
        const formData = new FormData();
        formData.append('file', item.file);
        request({
            url: '/file/upload',
            method: 'post',
            data: formData,
            onUploadProgress: (e: any) => {  // 文件上传进度回调
                if (e.lengthComputable) {
                    item.progress = e.loaded / e.total * 100;
                }
            }
        }).then((res) => {
            if(res.data.code === 0) {
                item.status = 'done';
                item.url = res.data.data.url;
                // 每个图片上传完成后都检查是否全部上传完成
                if (checkDone()) {
                    submitForm();
                }
            }
        }).catch((e) => {
            item.status = 'exception';
        });
    };

    /* 检查是否全部上传完毕 */
    const checkDone = () => {
        return !images.value.some((d) => d.status !== 'done');
    };

    /* 全部上传完毕后与其它表单数据一起提交 */
    const submitForm = () => {
        message.success('已全部上传完毕');
        console.log('data:', this.images);
        loading.value = false;
    };
</script>
```

@tab JavaScript

```vue
<template>
    <div>
        <ele-image-upload v-model:value="images" :auto-upload="false" />
        <a-button :loading="loading" @click="onSubmit">提交</a-button>
    </div>
</template>

<script setup>
    import { ref } from 'vue';
    import { message } from 'ant-design-vue';
    import request from '@/utils/request';

    const images = ref([]);

    const loading = ref(false);

    /* 手动上传 */
    const onSubmit = () => {
        if (checkDone()) {  // 检查是否全部上传完毕
            submitForm();  // 全部上传完毕后与其它表单数据一起提交
            return;
        }
        loading.value = true;
        // 循环一个一个上传
        images.value.forEach((item) => {
            if (!item.status) {
                uploadItem(item);
            }
        });
    };

    /* 上传图片 */
    const uploadItem = (item) => {
        // 这里与前面介绍的一样还是使用 axios 上传并监听上传进度
        item.status = 'uploading';
        const formData = new FormData();
        formData.append('file', item.file);
        request({
            url: '/file/upload',
            method: 'post',
            data: formData,
            onUploadProgress: (e) => {  // 文件上传进度回调
                if (e.lengthComputable) {
                    item.progress = e.loaded / e.total * 100;
                }
            }
        }).then((res) => {
            if(res.data.code === 0) {
                item.status = 'done';
                item.url = res.data.data.url;
                // 每个图片上传完成后都检查是否全部上传完成
                if (checkDone()) {
                    submitForm();
                }
            }
        }).catch((e) => {
            item.status = 'exception';
        });
    };

    /* 检查是否全部上传完毕 */
    const checkDone = () => {
        return !images.value.some((d) => d.status !== 'done');
    };

    /* 全部上传完毕后与其它表单数据一起提交 */
    const submitForm = () => {
        message.success('已全部上传完毕');
        console.log('data:', this.images);
        loading.value = false;
    };
</script>
```

:::

<br/>

## 6.22.6. 支持多选  

开启多选或文件夹上传建议用 `upload-handler` 自定义上传的逻辑(以免 v-model 数据更新不及时)，`a-upload` 开启多选是会回调多次上传的方法：

::: code-tabs

@tab TypeScript

```vue
<template>
    <ele-image-upload
        v-model:value="images"
        :multiple="true"
        :upload-handler="uploadHandler"
        @upload="onUpload"
    />
</template>

<script lang="ts" setup>
    import { ref } from 'vue';
    import { message } from 'ant-design-vue';
    import type { ItemType } from 'ele-admin-pro/es/ele-image-upload/types';
    import request from '@/utils/request';

    // 已上传数据
    const images = ref<ItemType[]>([
        {
            uid: 1,
            url: 'https://cdn.eleadmin.com/20200609/c184eef391ae48dba87e3057e70238fb.jpg',
            status: 'done'
        },
        {
            uid: 2,
            url: 'https://cdn.eleadmin.com/20200610/WLXm7gp1EbLDtvVQgkeQeyq5OtDm00Jd.jpg',
            status: 'done'
        }
    ]);

    /* 上传事件 */
    const uploadHandler = (file: File) => {
        const item: ItemType = {
            file,
            uid: (file as any).uid,
            name: file.name,
            progress: 0,
            status: undefined
        };
        if (!file.type.startsWith('image')) {
            message.error('只能选择图片');
            return;
        }
        if (file.size / 1024 / 1024 > 2) {
            message.error('大小不能超过 2MB');
            return;
        }
        item.url = window.URL.createObjectURL(file);
        // 关键就是这里要自己 push 添加数据而不是靠 v-modal 自动更新
        images.value.push(item);
        onUpload(item);
    };

    /* 上传 item */
    const onUpload = (d: ItemType) => {
        const item = images.value.find((t) => t.uid === d.uid) ?? d;
        console.log('item:', item);
        item.status = 'uploading';
        const formData = new FormData();
        formData.append('file', item.file);
        request({
            url: '/file/upload',
            method: 'post',
            data: formData,
            onUploadProgress: (e) => {  // 文件上传进度回调
                if (e.lengthComputable) {
                    item.progress = e.loaded / e.total * 100;
                }
            }
        }).then((res) => {
            if(res.data.code === 0) {
                item.status = 'done';
                item.url = res.data.data.url;
            }
        }).catch((e) => {
            item.status = 'exception';
        });
    };
</script>
```

@tab JavaScript

```vue
<template>
    <ele-image-upload
        v-model:value="images"
        :multiple="true"
        :upload-handler="uploadHandler"
        @upload="onUpload"
    />
</template>

<script setup>
    import { ref } from 'vue';
    import { message } from 'ant-design-vue';
    import request from '@/utils/request';

    // 已上传数据
    const images = ref([
        {
            uid: 1,
            url: 'https://cdn.eleadmin.com/20200609/c184eef391ae48dba87e3057e70238fb.jpg',
            status: 'done'
        },
        {
            uid: 2,
            url: 'https://cdn.eleadmin.com/20200610/WLXm7gp1EbLDtvVQgkeQeyq5OtDm00Jd.jpg',
            status: 'done'
        }
    ]);

    /* 上传事件 */
    const uploadHandler = (file) => {
        const item = {
            file,
            uid: file.uid,
            name: file.name,
            progress: 0,
            status: undefined
        };
        if (!file.type.startsWith('image')) {
            message.error('只能选择图片');
            return;
        }
        if (file.size / 1024 / 1024 > 2) {
            message.error('大小不能超过 2MB');
            return;
        }
        item.url = window.URL.createObjectURL(file);
        // 关键就是这里要自己 push 添加数据而不是靠 v-modal 自动更新
        images.value.push(item);
        onUpload(item);
    };

    /* 上传 item */
    const onUpload = (d) => {
        const item = images.value.find((t) => t.uid === d.uid) ?? d;
        console.log('item:', item);
        item.status = 'uploading';
        const formData = new FormData();
        formData.append('file', item.file);
        request({
            url: '/file/upload',
            method: 'post',
            data: formData,
            onUploadProgress: (e) => {  // 文件上传进度回调
                if (e.lengthComputable) {
                    item.progress = e.loaded / e.total * 100;
                }
            }
        }).then((res) => {
            if(res.data.code === 0) {
                item.status = 'done';
                item.url = res.data.data.url;
            }
        }).catch((e) => {
            item.status = 'exception';
        });
    };
</script>
```

:::

使用 `upload-handler` 后不会自动往数据里面追加 item ， `upload` 事件上传的时候也不会触发，在上传失败点击重试按钮的时候才会触发。

<br/>

## 6.22.7. 进阶示例  

可以设置 `uploadHandler` 属性，这样当选择文件后会调用此方法完全由你处理，不会自动往数据里面追加 item ：

::: code-tabs

@tab TypeScript

```vue
<template>
    <ele-image-upload v-model:value="images" :upload-handler="uploadHandler" />
</template>

<script lang="ts" setup>
    import { ref } from 'vue';
    import type { ItemType } from 'ele-admin-pro/es/ele-image-upload/types';

    const images = ref<ItemType[]>([]);

    /* 完全自己控制选择文件后的处理 */
    const uploadHandler = (file) => {
        // file 即选择的文件
        console.log('file:', file);
        // 你可以在这里做一些自己需要的校验, 因为不会经过 beforeUpload 了
        // 校验完成后要往数据里面追加一条
        images.value.push({
            uid: file.uid,
            url: '',  // url 看你是想读 file 为 base64 或 blob 还是先上传到后端从后端取
            //url: window.URL.createObjectURL(file),  // 将 file 转为 blob
            //status: 'done', // 如果是先上传后端再添加数据的这个 status 要设置为 done
            file: file
        });
        // 需要上传到后端也可以自己使用 axios 上传
        // upload 事件也不会触发了, 看你是想在这里就上传后端, 还是通过其它按钮点击再上传
    };
</script>
```

@tab JavaScript

```vue
<template>
    <ele-image-upload v-model:value="images" :upload-handler="uploadHandler" />
</template>

<script setup>
    import { ref } from 'vue';

    const images = ref([]);

    /* 完全自己控制选择文件后的处理 */
    const uploadHandler = (file) => {
        // file 即选择的文件
        console.log('file:', file);
        // 你可以在这里做一些自己需要的校验, 因为不会经过 beforeUpload 了
        // 校验完成后要往数据里面追加一条
        images.value.push({
            uid: file.uid,
            url: '',  // url 看你是想读 file 为 base64 或 blob 还是先上传到后端从后端取
            //url: window.URL.createObjectURL(file),  // 将 file 转为 blob
            //status: 'done', // 如果是先上传后端再添加数据的这个 status 要设置为 done
            file: file
        });
        // 需要上传到后端也可以自己使用 axios 上传
        // upload 事件也不会触发了, 看你是想在这里就上传后端, 还是通过其它按钮点击再上传
    };
</script>
```

:::

设置 `removeHandler` 属性后点击删除时就会调用此方法，也不会经过 `beforeRemove` 和触发 `remove` 事件了：

::: code-tabs

@tab TypeScript

```vue
<template>
    <ele-image-upload v-model:value="images" :remove-handler="removeHandler" />
</template>

<script lang="ts" setup>
    import { ref } from 'vue';
    import type { ItemType } from 'ele-admin-pro/es/ele-image-upload/types';

    const images = ref<ItemType[]>([]);

    /* 完全自己控制点击删除后的处理 */
    const removeHandler = (item) => {
        // item 即点击删除对应的 item 数据
        console.log('item:', item);
        // 这里你可以弹可询问弹窗, 需要删除数据可以这样写:
        images.value = images.value.filter((d) => d !== item);
        // 需要请求接口删除也可以写在这里
    }
</script>
```

@tab JavaScript

```vue
<template>
    <ele-image-upload v-model:value="images" :remove-handler="removeHandler" />
</template>

<script setup>
    import { ref } from 'vue';

    const images = ref([]);

    /* 完全自己控制点击删除后的处理 */
    const removeHandler = (item) => {
        // item 即点击删除对应的 item 数据
        console.log('item:', item);
        // 这里你可以弹可询问弹窗, 需要删除数据可以这样写:
        images.value = images.value.filter((d) => d !== item);
        // 需要请求接口删除也可以写在这里
    }
</script>
```

:::

**例如实现删除时标记字段逻辑删除而不是删除数据：**

::: code-tabs

@tab TypeScript

```vue
<template>
    <ele-image-upload
        :value="imageList"
        :remove-handler="removeHandler"
        :upload-handler="uploadHandler"
    />
    <a-button @click="onSubmit">上传</a-button>
</template>

<script lang="ts" setup>
    import { ref, computed } from 'vue';
    import { message } from 'ant-design-vue';
    import type { ItemType } from 'ele-admin-pro/es/ele-image-upload/types';

    // 已上传数据
    const images = ref<ItemType[]>([
        {
            uid: 1,
            url: 'https://cdn.eleadmin.com/20200609/c184eef391ae48dba87e3057e70238fb.jpg',
            status: 'done'
        },
        {
            uid: 2,
            url: 'https://cdn.eleadmin.com/20200610/WLXm7gp1EbLDtvVQgkeQeyq5OtDm00Jd.jpg',
            status: 'done'
        }
    ]);

    const loading = ref(false);

    const imageList = computed(() => {
        return images.value.filter(d => d.deleted !== 1);
    });

    /* 上传前钩子 */
    const onSubmit = () => {
        if (checkDone()) {  // 检查是否全部上传完毕
            submitForm();  // 全部上传完毕后与其它表单数据一起提交
            return;
        }
        loading.value = true;
        // 循环一个一个上传
        images.value.forEach((item) => {
            if (!item.status && item.deleted !== 1) {
                uploadItem(item);
            }
        });
    };

    /* 删除的处理 */
    const removeHandler = (item) => {
        images.value.forEach(d => {
            if(d.uid === item.uid) {
                d.deleted = 1;
            }
        });
    };

    /* 选择文件后的处理 */
    const uploadHandler = (file: File) => {
        if (!file.type.startsWith('image')) {
            message.error('只能选择图片');
            return;
        }
        if (file.size / 1024 / 1024 > 2) {
            message.error('大小不能超过 2MB');
            return;
        }
        images.value.push({
            uid: file.uid,
            url: window.URL.createObjectURL(file),
            file: file
        });
    };

    /* 上传图片 */
    const uploadItem = (item: ItemType) => {
        item.status = 'uploading';
        const formData = new FormData();
        formData.append('file', item.file);
        request({
            url: '/file/upload',
            method: 'post',
            data: formData,
            onUploadProgress: (e: any) => {  // 文件上传进度回调
                if (e.lengthComputable) {
                    item.progress = e.loaded / e.total * 100;
                }
            }
        }).then((res) => {
            if(res.data.code === 0) {
                item.status = 'done';
                item.url = res.data.data.url;
                // 每个图片上传完成后都检查是否全部上传完成
                if (checkDone()) {
                    submitForm();
                }
            }
        }).catch((e) => {
            item.status = 'exception';
        });
    };

    /* 检查是否全部上传完毕 */
    const checkDone = () => {
        return !images.value.some((d) => d.status !== 'done' && d.deleted !== 1);
    };

    /* 全部上传完毕后与其它表单数据一起提交 */
    const submitForm = () => {
        message.success('已全部上传完毕');
        console.log('images:', images.value);
        loading.value = false;
    };
</script>
```

@tab JavaScript

```vue
<template>
    <ele-image-upload
        :value="imageList"
        :remove-handler="removeHandler"
        :upload-handler="uploadHandler"
    />
    <a-button @click="onSubmit">上传</a-button>
</template>

<script setup>
    import { ref, computed } from 'vue';
    import { message } from 'ant-design-vue';

    // 已上传数据
    const images = ref([
        {
            uid: 1,
            url: 'https://cdn.eleadmin.com/20200609/c184eef391ae48dba87e3057e70238fb.jpg',
            status: 'done'
        },
        {
            uid: 2,
            url: 'https://cdn.eleadmin.com/20200610/WLXm7gp1EbLDtvVQgkeQeyq5OtDm00Jd.jpg',
            status: 'done'
        }
    ]);

    const loading = ref(false);

    const imageList = computed(() => {
        return images.value.filter(d => d.deleted !== 1);
    });

    /* 上传前钩子 */
    const onSubmit = () => {
        if (checkDone()) {  // 检查是否全部上传完毕
            submitForm();  // 全部上传完毕后与其它表单数据一起提交
            return;
        }
        loading.value = true;
        // 循环一个一个上传
        images.value.forEach((item) => {
            if (!item.status && item.deleted !== 1) {
                uploadItem(item);
            }
        });
    };

    /* 删除的处理 */
    const removeHandler = (item) => {
        images.value.forEach(d => {
            if(d.uid === item.uid) {
                d.deleted = 1;
            }
        });
    };

    /* 选择文件后的处理 */
    const uploadHandler = (file) => {
        if (!file.type.startsWith('image')) {
            message.error('只能选择图片');
            return;
        }
        if (file.size / 1024 / 1024 > 2) {
            message.error('大小不能超过 2MB');
            return;
        }
        images.value.push({
            uid: file.uid,
            url: window.URL.createObjectURL(file),
            file: file
        });
    };

    /* 上传图片 */
    const uploadItem = (item) => {
        item.status = 'uploading';
        const formData = new FormData();
        formData.append('file', item.file);
        request({
            url: '/file/upload',
            method: 'post',
            data: formData,
            onUploadProgress: (e) => {  // 文件上传进度回调
                if (e.lengthComputable) {
                    item.progress = e.loaded / e.total * 100;
                }
            }
        }).then((res) => {
            if(res.data.code === 0) {
                item.status = 'done';
                item.url = res.data.data.url;
                // 每个图片上传完成后都检查是否全部上传完成
                if (checkDone()) {
                    submitForm();
                }
            }
        }).catch((e) => {
            item.status = 'exception';
        });
    };

    /* 检查是否全部上传完毕 */
    const checkDone = () => {
        return !images.value.some((d) => d.status !== 'done' && d.deleted !== 1);
    };

    /* 全部上传完毕后与其它表单数据一起提交 */
    const submitForm = () => {
        message.success('已全部上传完毕');
        console.log('images:', images.value);
        loading.value = false;
    };
</script>
```

:::