---
title: 弹窗扩展
createTime: 2025/01/10 09:30:40
permalink: /EleAdminPro/5wnw48jn/
---
## 6.18.1. 快速使用  

组件名 `ele-modal` ，是对 `a-modal` 的扩展，增加了拖拽、拉伸、全屏切换、多弹窗点击置顶、内容区域弹出等功能，v1.7.0新增，使用方式：

```vue
<template>
    <ele-modal
        :width="400"
        title="弹窗示例"
        v-model:visible="visible"
        :resizable="true"
        :maxable="true"
    >
        <div style="padding: 30px 0;">EleAdminPro</div>
    </ele-modal>
</template>

<script setup>
    import { ref } from 'vue';

    const visible = ref(false);
</script>
```

<br/>

## 6.18.2. 属性列表  

支持 a-modal 所有的属性及插槽，此外增加的属性有：

| 属性名          | 类型             | 说明                                                 | 默认值 |
| :-------------- | :--------------- | :--------------------------------------------------- | :----- |
| movable         | Boolean          | 是否可以拖动                                         | true   |
| moveOut         | Boolean          | 是否可以拖出边界                                     | false  |
| moveOutPositive | Boolean          | 如果可以拖出边界是否只允许下右拖出                   | false  |
| resizable       | Boolean, String  | 是否可以拉伸, 可选值: true、'horizontal'、'vertical' | false  |
| maxable         | Boolean          | 是否显示最大化切换按钮                               | false  |
| multiple        | Boolean          | 是否支持打开多个                                     | false  |
| fullscreen      | Boolean          | 是否默认全屏                                         | false  |
| inner           | Boolean          | 是否限制在主体内部                                   | false  |
| resetOnClose    | Boolean          | 是否在弹窗关闭后重置位置和大小                       | true   |
| maskClass       | String           | 遮罩的 class                                         |        |
| position        | [String, Object] | 弹窗打开的位置 `v1.10.1新增`                         |        |

resizable 为 horizontal 只能拉伸改变宽度，为 vertical 只能拉伸改变高度，为 true 宽高都能拉伸改变，弹窗的更多属性请查看 [a-modal](https://next.antdv.com/components/modal-cn#API) 。

position 属性的类型定义：

```typescript
type PositionType = PositionStringType | PositionObjectType;

type PositionStringType = 'top' | 'bottom' | 'left' | 'right' | 'leftTop' 
    | 'leftBottom' | 'rightTop' | 'rightBottom' | 'center';

interface PositionObjectType {
    top?: string | number;
    left?: string | number;
}
```

如果为 `Object` 类型可以有两个字段 `top` 和 `bottom` 指定顶部和左边的坐标，如果为 `String` 类型可以为以下几个取值：

- top   顶部，左右居中
- bottom   底部，左右居中
- left   左边，上下居中
- right   右边，上下居中
- leftTop   左上角
- leftBottom   左下角
- rightTop   右上角
- rightBottom   右下角
- center   正中间，可替代原来的垂直居中属性

<br/>

## 6.18.3. 事件列表  

| 事件名称          | 说明                                 | 回调参数            |
| :---------------- | :----------------------------------- | :------------------ |
| update:fullscreen | 弹窗全屏切换事件                     | fullscreen: boolean |
| update:visible    | 更新 visible                         | visible: boolean    |
| change            | visible 改变回调                     | visible: boolean    |
| cancel            | 点击遮罩层或右上角叉或取消按钮的回调 |                     |
| ok                | 点击确定回调                         |                     |

后面四个事件是 a-modal 提供的，[前往查看](https://next.antdv.com/components/modal-cn#事件) 。

<br/>

## 6.18.4. 旧版使用  

v1.6.0 以及之前的版本没有封装组件，可以使用插件形式使用拖拽、拉伸、全屏切换、多弹窗点击置顶的功能，需要先在 main.ts 中安装：

```javascript
// v1.6.0版本使用方式
import { ModalPlugin } from 'ele-admin-pro';
// v1.7.0兼容旧版可以下载旧版代码, 并且还要引入样式
//import { ModalPlugin } from './utils/modal-plugin';
//import 'ele-admin-pro/es/ele-modal/style';

app.use(ModalPlugin);

// v1.5.0版本使用方式
import ModalUtil from 'ele-admin-pro/packages/modal-util';

app.use(ModalUtil);
```

只需要在 a-modal 上增加 `wrap-class-name` 即可，因为 a-modal 不能用自定义指令，所以用 class 实现的：

```vue
<template>
    <a-modal
        :width="400"
        title="拖拽弹窗"
        v-model:visible="visible"
        wrap-class-name="ele-modal-movable ele-modal-resizable">
    </a-modal>
</template>

<script>
    export default {
        data() {
            return {
                visible: false
            };
        }
    }
</script>
```

- ele-modal-movable - 支持拖拽移动位置，不可拖出屏幕范围
- ele-modal-move-out - 支持拖拽移动位置，可拖出屏幕范围
- ele-modal-resizable - 支持右下角拉伸改变大小
- ele-modal-multiple - 去掉遮罩层后支持打开多个，还需要加`:mask="false"`
- ele-modal-wrap-fullscreen - 默认全屏打开

wrap-class-name 可以加多个使用空格分隔，如：

```vue
<template>
    <!-- 去掉遮罩层并支持多开多个 -->
    <a-modal
        :width="400"
        :mask="false"
        v-model:visible="visible"
        wrap-class-name="ele-modal-move-out ele-modal-resizable ele-modal-multiple">
    </a-modal>

    <!-- 默认全屏打开 -->
    <a-modal
        :width="400"
        :mask="false"
        v-model:visible="visible"
        wrapClassName="ele-modal-movable ele-modal-wrap-fullscreen">
    </a-modal>
</template>
```

在标题栏增加全屏切换按钮，使用 `title` 插槽自定义标题栏，增加全屏切换的图标按钮：

```vue
<template>
    <a-modal
        :width="400"
        v-model:visible="visible"
        wrap-class-name="ele-modal-movable">
        <template #title>
            <div class="ele-cell">
                <div class="ele-cell-content">我是标题</div>
                <compress-outlined class="ele-modal-icon-compress"/>
                <expand-outlined class="ele-modal-icon-expand"/>
            </div>
        </template>
        <div style="min-height: 66px;">我是弹窗内容</div>
    </a-modal>
</template>

<script>
    import { ExpandOutlined, CompressOutlined } from '@ant-design/icons-vue';
    
    export default {
        components: {ExpandOutlined, CompressOutlined},
        data() {
            return {
                visible: false
            };
        }
    }
</script>
```

弹窗默认右下角打开，增加 class 自定义样式到右下角，写一个没有 scoped 的 style 在里面加样式：

```vue
<template>
    <a-modal
        :width="400"
        title="默认左下角打开"
        class="my-demo-modal"
        v-model:visible="visible"
        wrap-class-name="ele-modal-movable">
    </a-modal>
</template>

<style>
    .my-demo-modal {
        position: absolute;
        right: 0;
        bottom: 0;
        top: auto;
        left: auto;
        margin: 0;
    }
</style>
```