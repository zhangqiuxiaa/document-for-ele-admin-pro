---
title: 标签组件
createTime: 2025/01/10 09:30:40
permalink: /EleAdminPro/iulhqtub/
---
## 6.5.1. 快速使用  

组件名 `ele-tag` ，是对 `a-tag` 的扩展，增加了圆角、圆形、尺寸等属性，使用方式：

```vue
<template>
    <ele-tag color="blue">标签一</ele-tag>
</template>
```

![](/images/6-5-1.png)

```vue
<script>
    import EleTag from '@/components/EleTag'

    export default {
        components: { EleTag }
    }
</script>
```

<br/>

## 6.5.2. 属性列表  

| 属性名   | 类型    | 说明                              | 默认值  |
| :------- | :------ | :-------------------------------- | :------ |
| color    | String  | 标签色                            |         |
| size     | String  | 尺寸(large、middle、small、mini ) | mini    |
| shape    | String  | 形状(round、default、circle)      | default |
| closable | Boolean | 标签是否可以关闭                  | false   |

color 属性同 a-tag 的 color 属性，请查看 ant-design-vue 的 tag 文档。

<br/>

## 6.5.3. 标签输入组件  

组件名 `ele-edit-tag` ，使用方式：

```vue
<template>
    <ele-edit-tag v-model:data="tags" color="blue" />
</template>

<script setup>
    import { ref } from 'vue';

    // 标签输入
    const tags = ref(['标签一', '标签二', '标签三']);
</script>
```

![](/images/6-5-2.png)

**属性列表：**

| 属性名        | 类型                     | 说明                              | 默认值             |
| :------------ | :----------------------- | :-------------------------------- | :----------------- |
| color         | String                   | 标签色                            |                    |
| size          | String                   | 尺寸(large、middle、small、mini ) | 'mini'             |
| data          | `Array<string>`          | 数据(v-model)                     |                    |
| placeholder   | String                   | 提示文本                          | 'New Tag'          |
| unique        | Boolean                  | 添加 tag 是否唯一                 | true               |
| validator     | Function/`Array<string>` | 自定义添加校验                    |                    |
| beforeRemove  | Function                 | 自定义移除校验                    |                    |
| uniqueMessage | String                   | 唯一校验提示信息 `v1.6.0新增`     | '${value}已经存在' |
| inputStyle    | String/Object            | 输入框样式 `v1.6.0新增`           |                    |

`validator` 属性 Function 类型定义，`Array<string>` 类型为正则表达式验证详见文档下一节：

```typescript
type ValidatorPromise = (value: string) => Promise<void>;
```

`beforeRemove` 属性类型定义：

```typescript
type BeforeRemovePromise = (index: number, value?: string) => Promise<void>;
```

注意，v1.0.0 版本需要手动引入:

```vue
<script>
    import EleEditTag from '@/components/EleTag/EleEditTag';

    export default {
        components: { EleEditTag }
    }
</script>
```

<br/>

## 6.5.4. 标签输入验证  

自定义校验方法:

::: code-tabs

@tab TypeScript

```vue
<template>
    <ele-edit-tag v-model:data="tags" :validator="validator" />
</template>

<script lang="ts" setup>
    import { ref } from 'vue';
    
    // 标签输入
    const tags = ref(['标签一', '标签二', '标签三']);
    
    // 自定义校验，注意返回 Promise 是 v1.6.0新增，之前版本只能返回 false 阻止添加或者不返回
    const validator = (value: string) => {
        return new Promise<void>((resolve, reject) => {
            setTimeout(() => {
                //resolve();
                reject(new Error(value + '不合法, 请重新输入'));
            }, 1000);
        });
    };
    /*const validator = (value: string) => {
        return false;
    };*/
</script>
```

@tab JavaScript

```vue
<template>
    <ele-edit-tag v-model:data="tags" :validator="validator" />
</template>

<script setup>
    import { ref } from 'vue';
    
    // 标签输入
    const tags = ref(['标签一', '标签二', '标签三']);
    
    // 自定义校验，注意返回 Promise 是 v1.6.0新增，之前版本只能返回 false 阻止添加或者不返回
    const validator = (value) => {
        return new Promise((resolve, reject) => {
            setTimeout(() => {
                //resolve();
                reject(new Error(value + '不合法, 请重新输入'));
            }, 1000);
        });
    };
    /*const validator = (value) => {
        return false;
    };*/
</script>
```

:::

使用正则表达式验证:

```vue
<template>
    <ele-edit-tag v-model:data="tags" :validator="['/^1\d{10}$/', '不符合手机号格式']" />
</template>
```