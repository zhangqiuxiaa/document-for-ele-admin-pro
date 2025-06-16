---
title: 图标选择
createTime: 2025/01/10 09:30:40
permalink: /EleAdminPro/v9mbi1ly/
---
## 6.4.1. 快速使用  

组件名 `ele-icon-picker` ，使用方式，这里演示使用 ant-design 的图标，需要全部引入：

::: code-tabs

@tab TypeScript

```vue
<template>
    <ele-icon-picker v-model:value="icon" placeholder="请选择图标" :data="iconData">
        <template #icon="{ icon }">
            <component :is="icon" />
        </template>
    </ele-icon-picker>
</template>

<script lang="ts" setup>
    import { ref } from 'vue';
    import iconData from 'ele-admin-pro/es/ele-icon-picker/icons';
    
    const icon = ref('');
</script>

<script lang="ts">
    import * as icons from '@ant-design/icons-vue';
    
    export default {
        components: icons
    };
</script>
```

@tab JavaScript

```vue
<template>
    <ele-icon-picker v-model:value="icon" placeholder="请选择图标" :data="iconData">
        <template #icon="{ icon }">
            <component :is="icon" />
        </template>
    </ele-icon-picker>
</template>

<script setup>
    import { ref } from 'vue';
    import iconData from 'ele-admin-pro/es/ele-icon-picker/icons';
    
    const icon = ref('');
</script>

<script>
    import * as icons from '@ant-design/icons-vue';
    
    export default {
        components: icons
    };
</script>
```

:::

![](/images/6-4-1.png)

注意，v1.5.0 以及之前的版本需要手动引入，不需要写图标插槽，使用方式如下：

```vue
<template>
    <ele-icon-picker v-model:value="icon" placeholder="请选择图标"/>
</template>

<script>
    import EleIconPicker from 'ele-admin-pro/packages/ele-icon-picker';
    // v1.0.0 版本的引入方式为如下写法:
    //import EleIconPicker from '@/components/EleIconPicker';

    export default {
        components: { EleIconPicker },
        data() {
            return {
                icon: ''
            };
        }
    }
</script>
```

<br/>

## 6.4.2. 属性列表  

| 属性名            | 类型    | 说明                         | 默认值      |
| :---------------- | :------ | :--------------------------- | :---------- |
| value             | String  | 图标(v-model)                |             |
| placeholder       | String  | 提示文本                     |             |
| disabled          | Boolean | 是否禁用                     |             |
| size              | String  | 尺寸(large、default、small ) | default     |
| allowClear        | Boolean | 是否显示清除                 | true        |
| allowSearch       | Boolean | 是否显示搜索框               | true        |
| searchPlaceholder | String  | 搜索框提示文本               | 搜索..      |
| data              | Array   | 图标数据                     |             |
| trigger           | Array   | 触发下拉的行为               | `['click']` |
| placement         | String  | 菜单弹出位置                 | bottomLeft  |

后面几个参数同 ant-design-vue 的 dropdown 组件的参数。

<br/>

## 6.4.3. 自定义图标  

通过属性 data 自定义图标数据，数据需要的格式为：

```javascript
[
    {
        title: '线框风格',
        children: [
            {
                title: '方向性图标',
                icons: [
                    'StepBackwardOutlined',
                    'StepForwardOutlined',
                    'FastBackwardOutlined'
                ]
            },
            {
                title: '提示建议性',
                icons: [
                    'QuestionOutlined',
                    'QuestionCircleOutlined',
                    'PlusOutlined',
                ]
            }
        ]
    },
    {
        title: '实底风格',
        children: [
            {
                title: '方向性图标',
                icons: [
                    'StepBackwardFilled',
                    'StepForwardFilled',
                    'FastBackwardFilled'
                ]
            },
            {
                title: '提示建议性',
                icons: [
                    'QuestionCircleFilled',
                    'PlusCircleFilled',
                    'PauseCircleFilled'
                ]
            }
        ]
    }
]
```

<br/>

## 6.4.4. 菜单管理中使用  

在菜单管理编辑弹窗中增加图标选择器需要引入所有的图标，会增加打包体积，可参考下面操作增加。

修改 `src/views/system/menu/components/menu-edit.vue` 把菜单图标的输入框换成图标选择器，还要引入所有的图标：

::: code-tabs

@tab TypeScript

```vue
<template>
    <!-- ......代码省略 -->
    <a-form-item label="菜单图标" v-bind="validateInfos.icon">
        <ele-icon-picker
            :data="iconData"
            v-model:value="form.icon"
            :disabled="form.menuType === 1"
            placeholder="请选择菜单图标"
        >
            <template #icon="{ icon }">
                <component :is="icon" />
            </template>
        </ele-icon-picker>
        <!-- <a-input
              allow-clear
              v-model:value="form.icon"
              placeholder="请输入菜单图标"
              :disabled="form.menuType === 1"
        /> -->
    </a-form-item>
    <!-- ......代码省略 -->
</template>

<script lang="ts" setup>
    //......代码省略
    import iconData from 'ele-admin-pro/es/ele-icon-picker/icons';
</script>

<!-- 增加下面代码引入所有图标 -->
<script lang="ts">
    import * as icons from '@/layout/menu-icons';

    export default {
        components: icons
    };
</script>
```

@tab JavaScript

```vue
<template>
    <!-- ......代码省略 -->
    <a-form-item label="菜单图标" v-bind="validateInfos.icon">
        <ele-icon-picker
            :data="iconData"
            v-model:value="form.icon"
            :disabled="form.menuType === 1"
            placeholder="请选择菜单图标"
        >
            <template #icon="{ icon }">
                <component :is="icon" />
            </template>
        </ele-icon-picker>
        <!-- <a-input
              allow-clear
              v-model:value="form.icon"
              placeholder="请输入菜单图标"
              :disabled="form.menuType === 1"
        /> -->
    </a-form-item>
    <!-- ......代码省略 -->
</template>

<script setup>
    //......代码省略
    import iconData from 'ele-admin-pro/es/ele-icon-picker/icons';
</script>

<!-- 增加下面代码引入所有图标 -->
<script>
    import * as icons from '@/layout/menu-icons';

    export default {
        components: icons
    };
</script>
```

:::

修改 `src/layout/menu-icons.ts` 为：

```javascript
export * from '@ant-design/icons-vue';
```