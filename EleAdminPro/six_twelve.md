---
title: 拖拽排序
createTime: 2025/01/10 09:30:40
permalink: /EleAdminPro/u5jdr11a/
---
## 6.12.1. 快速使用  

拖拽组件用的是 `vuedraggable` ，Github 地址：[SortableJS/vue.draggable.next](https://github.com/SortableJS/vue.draggable.next) ，快速使用：

```vue
<template>
    <div class="demo-drag-list">
        <vue-draggable v-model="list" item-key="id" :animation="300" handle=".sort-handle">
            <template #item="{ element }">
                <div class="demo-drag-list-item ele-cell">
                    <drag-outlined class="sort-handle" />
                    <div class="ele-cell-content">{{ element.name }}</div>
                </div>
            </template>
        </vue-draggable>
    </div>
</template>

<script setup>
    import { ref } from 'vue';
    import VueDraggable from 'vuedraggable';
    import { DragOutlined } from '@ant-design/icons-vue';

    const list = ref([
        { id: 1, name: '项目0000001' },
        { id: 2, name: '项目0000002' },
        { id: 3, name: '项目0000003' }
    ]);
</script>

<style scoped>
    .demo-drag-list {
        border: 1px solid hsla(0, 0%, 60%, .2);
    }

    .demo-drag-list-item {
        padding: 12px 16px;
    }

    .demo-drag-list-item + .demo-drag-list-item {
        border-top: 1px solid hsla(0, 0%, 60%, .2);
    }

    .demo-drag-list-item.sortable-chosen {
        background: hsla(0, 0%, 60%, .1);
    }

    .demo-drag-list-item .sort-handle {
        cursor: move;
        font-size: 16px;
    }
</style>
```

`handle` 属性是指定拖拽的手柄，`animation` 是设置过渡动画时间，拖拽后的数据是双向绑定的，更多参数请前往 GitHub 查看。

![](/images/6-12-1.png)

<br/>

## 6.12.2. 多列表拖拽  

多列表相互拖拽只需要把 `group` 设定一样就可以了：

```vue
<template>
    <div>
        <div class="demo-drag-list">
            <vue-draggable v-model="list1" item-key="id" group="project1" :animation="300" handle=".sort-handle">
                <template #item="{ element }">
                    <div class="demo-drag-list-item ele-cell">
                        <drag-outlined class="sort-handle" />
                        <div class="ele-cell-content">{{ element.name }}</div>
                    </div>
                </template>
            </vue-draggable>
        </div>
        <div class="demo-drag-list">
            <vue-draggable v-model="list2" item-key="id" group="project1" handle=".sort-handle" :animation="300">
                <template #item="{ element }">
                    <div class="demo-drag-list-item ele-cell">
                        <drag-outlined class="sort-handle" />
                        <div class="ele-cell-content">{{ element.name }}</div>
                    </div>
                </template>
            </vue-draggable>
        </div>
    </div>
</template>

<script setup>
    import { ref } from 'vue';
    import VueDraggable from 'vuedraggable';
    import { DragOutlined } from '@ant-design/icons-vue';

    const list1 = ref([
        { id: 1, name: '项目0000001' },
        { id: 2, name: '项目0000002' },
        { id: 3, name: '项目0000003' }
    ]);

    const list2 = ref([
        {id: 4, name: '项目0000004'},
        {id: 5, name: '项目0000005'},
        {id: 6, name: '项目0000006'}
    ]);
</script>
<!-- 样式省略... -->
```

![](/images/6-12-2.png)

<br/>

## 6.12.3. 表格行拖拽  

可以使用静态表格实现，更加灵活，如果需要多表格相互拖拽 `group` 设定一样就可以，如果需要动态循环多表格相互拖拽参考模板示例：

::: code-tabs

@tab TypeScript

```vue
<template>
    <table class="ele-table ele-table-border ele-table-medium">
        <colgroup>
            <col width="40" />
            <col />
            <col width="80" />
        </colgroup>
        <thead>
            <tr>
                <th></th>
                <th>任务名称</th>
                <th style="text-align: center;">状态</th>
            </tr>
        </thead>
        <vue-draggable
            tag="tbody"
            item-key="id"
            v-model="list"
            :animation="300"
            handle=".demo-table-drag-handle"
        >
            <template #item="{ element }">
                <tr>
                    <td style="text-align: center">
                        <menu-outlined class="demo-table-drag-handle" style="cursor: move;" />
                    </td>
                    <td>{{ element.taskName }}</td>
                    <td style="text-align: center">
                        <span
                            :class="['ele-text-warning', 'ele-text-success', 'ele-text-info'][element.status]"
                        >
                            {{ ['未开始', '进行中', '已完成'][element.status] }}
                        </span>
                    </td>
                </tr>
            </template>
            <template #footer v-if="!list.length">
                <tr style="background: none;">
                    <td colspan="3" style="text-align: center;">暂无数据</td>
                </tr>
            </template>
        </vue-draggable>
    </table>
</template>

<script lang="ts" setup>
    import { ref } from 'vue';
    import VueDraggable from 'vuedraggable';
    import { MenuOutlined } from '@ant-design/icons-vue';

    interface Task {
        id: number;
        taskName: string;
        status: number;
    }

    const list = ref<Task[]>([
        { id: 1, taskName: '测试任务1', status: 0 },
        { id: 2, taskName: '测试任务2', status: 0 },
        { id: 3, taskName: '测试任务3', status: 1 }
    ]);
</script>

<style lang="less" scoped>
    /* 表格行拖拽按下去样式 */
    .ele-table tr.sortable-chosen {
        background: hsla(0, 0%, 60%, 0.1);
    }
</style>
```

@tab JavaScript

```vue
<template>
    <table class="ele-table ele-table-border ele-table-medium">
        <colgroup>
            <col width="40" />
            <col />
            <col width="80" />
        </colgroup>
        <thead>
            <tr>
                <th></th>
                <th>任务名称</th>
                <th style="text-align: center;">状态</th>
            </tr>
        </thead>
        <vue-draggable
            tag="tbody"
            item-key="id"
            v-model="list"
            :animation="300"
            handle=".demo-table-drag-handle"
        >
            <template #item="{ element }">
                <tr>
                    <td style="text-align: center">
                        <menu-outlined class="demo-table-drag-handle" style="cursor: move;" />
                    </td>
                    <td>{{ element.taskName }}</td>
                    <td style="text-align: center">
                        <span
                            :class="['ele-text-warning', 'ele-text-success', 'ele-text-info'][element.status]"
                        >
                            {{ ['未开始', '进行中', '已完成'][element.status] }}
                        </span>
                    </td>
                </tr>
            </template>
            <template #footer v-if="!list.length">
                <tr style="background: none;">
                    <td colspan="3" style="text-align: center;">暂无数据</td>
                </tr>
            </template>
        </vue-draggable>
    </table>
</template>

<script setup>
    import { ref } from 'vue';
    import VueDraggable from 'vuedraggable';
    import { MenuOutlined } from '@ant-design/icons-vue';

    const list = ref([
        { id: 1, taskName: '测试任务1', status: 0 },
        { id: 2, taskName: '测试任务2', status: 0 },
        { id: 3, taskName: '测试任务3', status: 1 }
    ]);
</script>

<style lang="less" scoped>
    /* 表格行拖拽按下去样式 */
    .ele-table tr.sortable-chosen {
        background: hsla(0, 0%, 60%, 0.1);
    }
</style>
```

:::

![](/images/6-12-3.png)