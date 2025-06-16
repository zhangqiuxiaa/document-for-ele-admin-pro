---
title: 表格下拉
createTime: 2025/01/10 09:30:40
permalink: /EleAdminPro/cep4x3hs/
---
## 6.24.1. 快速使用  

组件名 `ele-table-select` ，v1.8.0 新增 ，使用方式：

::: code-tabs

@tab TypeScript

```vue
<template>
    <ele-table-select
        allow-clear
        v-model:value="selectedValue"
        placeholder="请选择"
        value-key="userId"
        label-key="nickname"
        :table-config="tableConfig"
        :overlay-style="{ width: '520px' }"
    >
        <!-- 角色列 -->
        <template #bodyCell="{ column, record }">
            <template v-if="column.key === 'roles'">
                <a-tag v-for="item in record.roles" :key="item.roleId" color="blue">
                    {{ item.roleName }}
                </a-tag>
            </template>
        </template>
    </ele-table-select>
</template>

<script lang="ts" setup>
    import { ref, reactive } from 'vue';
    import { pageUsers } from '@/api/system/user';
    import type { ProTableProps } from 'ele-admin-pro/es/ele-pro-table/types';
    import type { User } from '@/api/system/user/model';

    const selectedValue = ref<number | null>(null);

    // 表格配置
    const tableConfig = reactive<ProTableProps>({
        datasource: ({ page, limit, where, orders }) => {
            return pageUsers({ ...where, ...orders, page, limit });
        },
        columns: [
            {
                title: '用户账号',
                dataIndex: 'username',
                sorter: true
            },
            {
                title: '用户名',
                key: 'nickname',
                dataIndex: 'nickname',
                sorter: true
            },
            {
                title: '性别',
                dataIndex: 'sexName',
                width: 80,
                sorter: true
            },
            {
                title: '角色',
                key: 'roles'
            }
        ],
        toolbar: false,
        pageSize: 5,
        pageSizeOptions: ['5', '10', '15', '20'],
        size: 'small'
    });
</script>
```

@tab JavaScript

```vue
<template>
    <ele-table-select
        allow-clear
        v-model:value="selectedValue"
        placeholder="请选择"
        value-key="userId"
        label-key="nickname"
        :table-config="tableConfig"
        :overlay-style="{ width: '520px' }"
    >
        <!-- 角色列 -->
        <template #bodyCell="{ column, record }">
            <template v-if="column.key === 'roles'">
                <a-tag v-for="item in record.roles" :key="item.roleId" color="blue">
                    {{ item.roleName }}
                </a-tag>
            </template>
        </template>
    </ele-table-select>
</template>

<script setup>
    import { ref, reactive } from 'vue';
    import { pageUsers } from '@/api/system/user';

    const selectedValue = ref(null);

    // 表格配置
    const tableConfig = reactive({
        datasource: ({ page, limit, where, orders }) => {
            return pageUsers({ ...where, ...orders, page, limit });
        },
        columns: [
            {
                title: '用户账号',
                dataIndex: 'username',
                sorter: true
            },
            {
                title: '用户名',
                key: 'nickname',
                dataIndex: 'nickname',
                sorter: true
            },
            {
                title: '性别',
                dataIndex: 'sexName',
                width: 80,
                sorter: true
            },
            {
                title: '角色',
                key: 'roles'
            }
        ],
        toolbar: false,
        pageSize: 5,
        pageSizeOptions: ['5', '10', '15', '20'],
        size: 'small'
    });
</script>
```

:::

可以通过 ref 调用 proTable 的方法：

::: code-tabs

@tab TypeScript

```vue
<template>
    <ele-table-select ref="selectRef" v-model:value="selectedValue"></ele-table-select>
</template>

<script lang="ts" setup>
    import { ref } from 'vue';
    import type { EleTableSelect } from 'ele-admin-pro';

    const selectRef = ref<InstanceType<typeof EleTableSelect> | null>(null);

    const reload = () => {
        selectRef.value?.tableRef?.reload({ page: 1 });
    }
</script>
```

@tab JavaScript

```vue
<template>
    <ele-table-select ref="selectRef" v-model:value="selectedValue"></ele-table-select>
</template>

<script setup>
    import { ref } from 'vue';

    const selectRef = ref(null);

    const reload = () => {
        selectRef.value?.tableRef?.reload({ page: 1 });
    }
</script>
```

:::

<br/>

## 6.24.2. 多选示例  

::: code-tabs

@tab TypeScript

```vue
<template>
    <ele-table-select
        multiple
        v-model:value="selectedValue"
        placeholder="请选择"
        value-key="userId"
        label-key="nickname"
        :table-config="tableConfig"
        :overlay-style="{ width: '520px' }"
    >
        <!-- 角色列 -->
        <template #bodyCell="{ column, record }">
            <template v-if="column.key === 'roles'">
                <a-tag v-for="item in record.roles" :key="item.roleId" color="blue">
                    {{ item.roleName }}
                </a-tag>
            </template>
        </template>
    </ele-table-select>
</template>

<script lang="ts" setup>
    import { ref, reactive } from 'vue';
    import { pageUsers } from '@/api/system/user';
    import type { ProTableProps } from 'ele-admin-pro/es/ele-pro-table/types';
    import type { User } from '@/api/system/user/model';

    const selectedValue = ref<number[]>([]);

    // 表格配置
    const tableConfig = reactive<ProTableProps>({
        datasource: ({ page, limit, where, orders }) => {
            return pageUsers({ ...where, ...orders, page, limit });
        },
        columns: [
            {
                title: '用户账号',
                dataIndex: 'username',
                sorter: true
            },
            {
                title: '用户名',
                key: 'nickname',
                dataIndex: 'nickname',
                sorter: true
            },
            {
                title: '性别',
                dataIndex: 'sexName',
                width: 80,
                align: 'center',
                sorter: true
            },
            {
                title: '角色',
                key: 'roles'
            }
        ],
        toolbar: false,
        pageSize: 5,
        pageSizeOptions: ['5', '10', '15', '20'],
        size: 'small',
        rowSelection: {
            preserveSelectedRowKeys: true
        }
    });
</script>
```

@tab JavaScript

```vue
<template>
    <ele-table-select
        multiple
        v-model:value="selectedValue"
        placeholder="请选择"
        value-key="userId"
        label-key="nickname"
        :table-config="tableConfig"
        :overlay-style="{ width: '520px' }"
    >
        <!-- 角色列 -->
        <template #bodyCell="{ column, record }">
            <template v-if="column.key === 'roles'">
                <a-tag v-for="item in record.roles" :key="item.roleId" color="blue">
                    {{ item.roleName }}
                </a-tag>
            </template>
        </template>
    </ele-table-select>
</template>

<script setup>
    import { ref, reactive } from 'vue';
    import { pageUsers } from '@/api/system/user';

    const selectedValue = ref([]);

    // 表格配置
    const tableConfig = reactive({
        datasource: ({ page, limit, where, orders }) => {
            return pageUsers({ ...where, ...orders, page, limit });
        },
        columns: [
            {
                title: '用户账号',
                dataIndex: 'username',
                sorter: true
            },
            {
                title: '用户名',
                key: 'nickname',
                dataIndex: 'nickname',
                sorter: true
            },
            {
                title: '性别',
                dataIndex: 'sexName',
                width: 80,
                align: 'center',
                sorter: true
            },
            {
                title: '角色',
                key: 'roles'
            }
        ],
        toolbar: false,
        pageSize: 5,
        pageSizeOptions: ['5', '10', '15', '20'],
        size: 'small',
        rowSelection: {
            preserveSelectedRowKeys: true
        }
    });
</script>
```

:::

<br/>

## 6.24.3. 属性列表  

| 属性名             | 类型                  | 说明                       | 默认值                 |
| :----------------- | :-------------------- | :------------------------- | :--------------------- |
| value              | Number, String, Array | 绑定值(v-model)            |                        |
| multiple           | Boolean               | 是否多选                   |                        |
| disabled           | Boolean               | 是否禁用                   |                        |
| size               | String                | 尺寸                       | large / small / middle |
| allowClear         | Boolean               | 是否可以清空选项           |                        |
| placeholder        | String                | 占位符                     |                        |
| id                 | String                | input 的 id 属性           |                        |
| valueKey           | String                | value 的属性名             | 'value'                |
| labelKey           | String                | label 的属性名             | 'label'                |
| initValue          | Object, Array         | 用于后端分页时回显数据使用 |                        |
| tableConfig        | Object                | 表格 EleProTable 的配置    |                        |
| bordered           | Boolean               | 是否有边框                 |                        |
| maxTagCount        | Number                | 最多显示多少个 tag         |                        |
| maxTagTextLength   | Number                | 最大显示的 tag 文本长度    |                        |
| arrowPointAtCenter | Boolean               | 箭头是否指向目标元素中心   |                        |
| autoAdjustOverflow | Boolean               | 气泡被遮挡时自动调整位置   | true                   |
| overlayStyle       | Object                | 下拉框样式                 |                        |
| overlayClassName   | String                | 卡片类名                   |                        |
| placement          | String                | 气泡框位置                 | 'bottomLeft'           |

  因为 value 是 valueKey 对应属性的值，如果分页时用 value 回显当前页不存在的数据无法显示 label ， 所以增加了 initValue 属性用于回显数据，initValue 是 Object 类型，需要至少有 valueKey 和 labelKey 配置的字段。

例如单选时通过 initValue 回显数据：

::: code-tabs

@tab TypeScript

```vue
<template>
    <ele-table-select
        v-model:value="selectedValue"
        value-key="userId"
        label-key="nickname"
        :table-config="tableConfig"
        :init-value="initValue"
    >
    </ele-table-select>
</template>

<script lang="ts" setup>
    import { ref, reactive } from 'vue';
    import { pageUsers } from '@/api/system/user';
    import type { ProTableProps } from 'ele-admin-pro/es/ele-pro-table/types';
    import type { User } from '@/api/system/user/model';

    const selectedValue = ref<number | null>(null);

    // 表格配置
    const tableConfig = reactive<ProTableProps>({
        datasource: ({ page, limit, where, orders }) => {
            return pageUsers({ ...where, ...orders, page, limit });
        },
        columns: [
            {
                title: '用户账号',
                dataIndex: 'username',
                sorter: true
            },
            {
                title: '用户名',
                key: 'nickname',
                dataIndex: 'nickname',
                sorter: true
            }
        ],
        toolbar: false,
        pageSize: 5,
        pageSizeOptions: ['5', '10', '15', '20'],
        size: 'small'
    });

    // 回显值
    const initValue = ref<User | null>(null);

    /* 回显数据 */
    const setInitValue = () => {
        initValue.value = {
            userId: 14,
            nickname: '管理员'
        };
    };
</script>
```

@tab JavaScript

```vue
<template>
    <ele-table-select
        v-model:value="selectedValue"
        value-key="userId"
        label-key="nickname"
        :table-config="tableConfig"
        :init-value="initValue"
    >
    </ele-table-select>
</template>

<script setup>
    import { ref, reactive } from 'vue';
    import { pageUsers } from '@/api/system/user';

    const selectedValue = ref(null);

    // 表格配置
    const tableConfig = reactive({
        datasource: ({ page, limit, where, orders }) => {
            return pageUsers({ ...where, ...orders, page, limit });
        },
        columns: [
            {
                title: '用户账号',
                dataIndex: 'username',
                sorter: true
            },
            {
                title: '用户名',
                key: 'nickname',
                dataIndex: 'nickname',
                sorter: true
            }
        ],
        toolbar: false,
        pageSize: 5,
        pageSizeOptions: ['5', '10', '15', '20'],
        size: 'small'
    });

    // 回显值
    const initValue = ref(null);

    /* 回显数据 */
    const setInitValue = () => {
        initValue.value = {
            userId: 14,
            nickname: '管理员'
        };
    };
</script>
```

:::

多选时通过 initValue 回显数据：

::: code-tabs

@tab TypeScript

```vue
<template>
    <ele-table-select
        multiple
        v-model:value="selectedValue"
        value-key="userId"
        label-key="nickname"
        :table-config="tableConfig"
        :init-value="initValue"
    >
    </ele-table-select>
</template>

<script lang="ts" setup>
    import { ref, reactive } from 'vue';
    import { pageUsers } from '@/api/system/user';
    import type { ProTableProps } from 'ele-admin-pro/es/ele-pro-table/types';
    import type { User } from '@/api/system/user/model';

    const selectedValue = ref<number[]>([]);

    // 表格配置
    const tableConfig = reactive<ProTableProps>({
        datasource: ({ page, limit, where, orders }) => {
            return pageUsers({ ...where, ...orders, page, limit });
        },
        columns: [
            {
                title: '用户账号',
                dataIndex: 'username',
                sorter: true
            },
            {
                title: '用户名',
                key: 'nickname',
                dataIndex: 'nickname',
                sorter: true
            }
        ],
        toolbar: false,
        pageSize: 5,
        pageSizeOptions: ['5', '10', '15', '20'],
        size: 'small',
        rowSelection: {
            preserveSelectedRowKeys: true
        }
    });

    // 回显值
    const initValue = ref<User[]>();

    /* 回显数据 */
    const setInitValue = () => {
        initValue.value = [
            {
                userId: 14,
                nickname: '管理员'
            },
            {
                userId: 18,
                nickname: '用户四'
            },
            {
                userId: 19,
                nickname: '用户五'
            }
        ];
    };
</script>
```

@tab JavaScript

```vue
<template>
    <ele-table-select
        multiple
        v-model:value="selectedValue"
        value-key="userId"
        label-key="nickname"
        :table-config="tableConfig"
        :init-value="initValue"
    >
    </ele-table-select>
</template>

<script setup>
    import { ref, reactive } from 'vue';
    import { pageUsers } from '@/api/system/user';

    const selectedValue = ref([]);

    // 表格配置
    const tableConfig = reactive({
        datasource: ({ page, limit, where, orders }) => {
            return pageUsers({ ...where, ...orders, page, limit });
        },
        columns: [
            {
                title: '用户账号',
                dataIndex: 'username',
                sorter: true
            },
            {
                title: '用户名',
                key: 'nickname',
                dataIndex: 'nickname',
                sorter: true
            }
        ],
        toolbar: false,
        pageSize: 5,
        pageSizeOptions: ['5', '10', '15', '20'],
        size: 'small',
        rowSelection: {
            preserveSelectedRowKeys: true
        }
    });

    // 回显值
    const initValue = ref();

    /* 回显数据 */
    const setInitValue = () => {
        initValue.value = [
            {
                userId: 14,
                nickname: '管理员'
            },
            {
                userId: 18,
                nickname: '用户四'
            },
            {
                userId: 19,
                nickname: '用户五'
            }
        ];
    };
</script>
```

:::

<br/>

## 6.24.4. 事件列表  

| 事件名称       | 说明                                     | 回调参数                      |
| :------------- | :--------------------------------------- | :---------------------------- |
| update:value   | 用于 v-model                             |                               |
| change         | 选中值发生变化时触发                     | 目前的选中值                  |
| focus          | 当 input 获得焦点时触发                  |                               |
| blur           | 当 input 失去焦点时触发                  |                               |
| clear          | 可清空的单选模式下用户点击清空按钮时触发 |                               |
| remove-tag     | 多选模式下移除 tag 时触发                | 移除的 tag 值对应的数据       |
| visible-change | 下拉框出现/隐藏时触发                    | 出现则为 true，隐藏则为 false |
| select         | 表格单选、多选选中事件 `v1.9.1新增`      | data(选中的行数据)            |