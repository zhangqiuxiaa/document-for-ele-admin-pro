---
title: 高级表格
createTime: 2025/01/10 09:30:40
permalink: /EleAdminPro/44l71xb9/
---
## 6.2.1. 快速使用  

组件名 `ele-pro-table` ，用于简化查询、分页、排序、筛选等操作，v1.1.0 新增，使用方式：

::: code-tabs

@tab TypeScript

```vue
<template>
    <ele-pro-table 
        ref="tableRef"
        row-key="userId"
        :columns="columns"
        :datasource="datasource">
    </ele-pro-table>
</template>

<script lang="ts" setup>
    import { ref } from 'vue';
    import type { EleProTable } from 'ele-admin-pro';
    import type { DatasourceFunction, ColumnItem } from 'ele-admin-pro/es/ele-pro-table/types';
    import { pageUsers } from '@/api/system/user';

    // 表格 ref
    const tableRef = ref<InstanceType<typeof EleProTable> | null>(null);

    // 表格列配置
    const columns = ref<ColumnItem[]>([
        {
            key: 'index',
            width: 48,
            align: 'center',
            hideInSetting: true,
            // tableRef.value?.tableIndex 是序号列的起始索引，通过页码和每页数量计算得出
            customRender: ({ index }) => index + (tableRef.value?.tableIndex ?? 0)
        },
        {
            title: '用户账号',
            dataIndex: 'username'
        },
        {
            title: '用户名',
            dataIndex: 'nickname'
        },
        {
            title: '手机号',
            dataIndex: 'phone'
        },
        {
            title: '创建时间',
            dataIndex: 'createTime'
        }
    ]);

    /* 表格数据源 */
    const datasource: DatasourceFunction = ({ page, limit, where, orders }) => {
        return pageUsers({ ...where, ...orders, page, limit });
    };
</script>
```

@tab JavaScript

```vue
<template>
    <ele-pro-table 
        ref="tableRef"
        row-key="userId"
        :columns="columns"
        :datasource="datasource">
    </ele-pro-table>
</template>

<script setup>
    import { ref } from 'vue';
    import { pageUsers } from '@/api/system/user';

    // 表格 ref
    const tableRef = ref(null);

    // 表格列配置
    const columns = ref([
        {
            key: 'index',
            width: 48,
            align: 'center',
            hideInSetting: true,
            // tableRef.value?.tableIndex 是序号列的起始索引，通过页码和每页数量计算得出
            customRender: ({ index }) => index + (tableRef.value?.tableIndex ?? 0)
        },
        {
            title: '用户账号',
            dataIndex: 'username'
        },
        {
            title: '用户名',
            dataIndex: 'nickname'
        },
        {
            title: '手机号',
            dataIndex: 'phone'
        },
        {
            title: '创建时间',
            dataIndex: 'createTime'
        }
    ]);

    /* 表格数据源 */
    const datasource = ({ page, limit, where, orders }) => {
        return pageUsers({ ...where, ...orders, page, limit });
    };
</script>
```

:::

<br/>

## 6.2.2. 属性列表  

`ele-pro-table` 支持 `a-table` 的 [全部属性](https://next.antdv.com/components/table-cn#API) ，`pagination` 属性是拆平了，此外增加的属性有：

| 属性                  | 说明                                                         | 类型             | 默认值             |
| :-------------------- | :----------------------------------------------------------- | :--------------- | :----------------- |
| datasource            | 数据源 `v1.6.0 移除 String 类型`                             | Array / Function |                    |
| method                | String 数据源请求方式 `1.6.0移除`                            | String           | 'GET'              |
| where                 | Function 数据源默认参数                                      | Object           |                    |
| headers               | String 数据源请求 header `1.6.0移除`                         | Object           |                    |
| contentType           | String 数据源请求数据类型 `1.6.0移除`                        | String           | 'application/json' |
| request               | Function 数据源请求参数名称                                  | Object           |                    |
| response              | Function 数据源响应参数名称                                  | Object           |                    |
| parseData             | Function 数据源数据格式解析                                  | Function         |                    |
| parseParam            | Function 数据源参数格式解析 `1.6.0移除`                      | Function         |                    |
| ascendValue           | Function 数据源升序的值 `1.6.0新增`                          | String           | 'asc'              |
| descendValue          | Function 数据源降序的值 `1.6.0新增`                          | String           | 'desc'             |
| selection             | 选中数据(多选)，支持 v-model 绑定                            | Array            |                    |
| current               | 选中数据(单选)，支持 v-model 绑定                            | Object           |                    |
| needPage              | 是否需要分页组件                                             | Boolean          | true               |
| initLoad              | Function 数据源是否默认请求                                  | Boolean          | true               |
| loading               | 表格请求状态                                                 | Boolean          | false              |
| columns               | 表格列配置                                                   | Array            |                    |
| currentPage           | 分页组件默认页码                                             | Number           | 1                  |
| pageSize              | 分页组件每页显示数量                                         | Number           | 10                 |
| hideOnSinglePage      | 分页组件只有一页时是否隐藏                                   | Boolean          | false              |
| pageSizeOptions       | 分页组件每页显示个数的选项                                   | Array            |                    |
| showLessItems         | 分页组件是否显示较少页面内容                                 | Boolean          |                    |
| showQuickJumper       | 分页组件是否显示跳转至某页                                   | Boolean          | true               |
| showSizeChanger       | 分页组件是否显示每页个数选项                                 | Boolean          | true               |
| showTotal             | 分页组件用于显示数据总量                                     | Function         |                    |
| simple                | 分页组件是否显示为简单分页                                   | Boolean          |                    |
| position              | 指定分页显示的位置                                           | String           | 'bottom'           |
| toolsTheme            | 表头工具栏主题风格                                           | String           | 可选 'default'     |
| title                 | 表头工具栏标题                                               | String           |                    |
| subTitle              | 表头工具栏二级标题                                           | String           |                    |
| toolkit               | 表头右侧工具按钮布局，默认['reload', 'size', 'columns', 'fullscreen'] | `Array<string>`  |                    |
| columnsSort           | 是否开启列拖拽排序                                           | Boolean          | true               |
| toolbar               | 是否显示表头工具栏                                           | Boolean          | true               |
| toolStyle             | 表头工具栏样式                                               | Object           |                    |
| toolClass             | 表头工具栏 class                                             | String           |                    |
| toolkitStyle          | 表头右侧工具按钮区域样式                                     | Object           |                    |
| fullZIndex            | 表格全屏时的 z-index                                         | Number           | 999                |
| autoAmendPage         | 是否自动修正页码                                             | Boolean          | true               |
| selectionType         | 单选多选列类型 `v1.2.0新增`                                  | String           |                    |
| lazyLoad              | 树形表格是否异步加载 `v1.6.0新增`                            | Boolean          | false              |
| resizable             | 列拖动改变宽度事件封装 `v1.6.0新增`                          | Boolean          | true               |
| reloadOnChange        | 是否在 ATable 的 change 事件自动 reload `v1.7.0新增`         | Boolean          | true               |
| striped               | 是否为斑马纹 `v1.7.0新增`                                    | Boolean          |                    |
| height                | 表格高度, 可用 'calc(100vh - 200px)' 铺满 `v1.7.0新增`       | String / Number  |                    |
| fullHeight            | 表格全屏状态的高度 `v1.7.0新增`                              | String / Number  |                    |
| hasChildrenColumnName | 树形懒加载时标识是否有子级的字段 `v1.8.0新增`                | String           |                    |
| resizable             | 列是否可以拖动改变宽度 `1.8.0新增`                           | Boolean          |                    |
| customStyle           | 自定义 a-table 样式 `1.9.0新增`                              | Object           |                    |
| columnsFixed          | 是否开启开关固定列功能 `1.10.1新增`                          | Boolean          | true               |
| cacheKey              | 配置唯一的 key 后列配置、尺寸改变后会缓存在本地 `1.10.1新增` | String           | 默认不缓存         |

表头工具栏默认是带背景色，不要背景色可以设置 `tools-theme="none"` ，不要默认的表头右侧按钮可设置 `:toolkit="[]"` 。

`columns` 列配置同样支持 `a-table` 的 `columns` 全部属性，此外增加的有：

| 属性          | 说明                                      | 类型    | 默认值 |
| :------------ | :---------------------------------------- | :------ | :----- |
| hideInTable   | 列是否默认隐藏 `v1.6.0新增`               | Boolean | false  |
| hideInSetting | 列是否不显示在列设置下拉框中 `v1.6.0新增` | Boolean | false  |

<br/>

## 6.2.3. 插槽列表  

`ele-pro-table` 支持 `a-table` 的全部插槽，此外增加的插槽有：

| 名称    | 说明                                                    | 参数 |
| :------ | :------------------------------------------------------ | :--- |
| toolbar | 表头工具栏左侧                                          | 无   |
| toolkit | 表头工具栏右侧                                          | 无   |
| default | 默认插槽，自定义表头工具栏和表格之间的内容 `v1.7.0新增` | 无   |

```vue
<template>
    <ele-pro-table :columns="columns" :datasource="datasource">
        <!-- 表头工具栏左侧 -->
        <template #toolbar>
            <a-button type="primary">添加</a-button>
            <a-button type="danger">删除</a-button>
        </template>
        <!-- 表头工具栏右侧 -->
        <template #toolkit>
            <a-space size="middle">
                <!-- 普通的按钮 -->
                <a-button type="primary">卷内文件调整</a-button>
                <!-- 跟默认工具按钮外观一致的图标按钮，ele-tool-item 组件是 v1.6.0 新增，在 6.2.10 中查看详细介绍 -->
                <ele-tool-item title="我是按钮" @click="onClick">
                    <plus-outlined />
                </ele-tool-item>
            </a-space>
        </template>
    </ele-pro-table>
</template>

<script setup>
    import { PlusOutlined } from '@ant-design/icons-vue';

    const onClick = () => {
        console.log('Hello');
    };
</script>
```

**表格列自定义模板：**

像创建时间列如果要对时间进行格式化后显示，使用 columns 的 customRender 参数即可：

```vue
<template>
    <ele-pro-table :columns="columns"></ele-pro-table>
</template>

<script setup>
    import { ref } from 'vue';
    import { toDateString } from 'ele-admin-pro';
    
    // 表格列配置
    const columns = ref([
        {
            title: '用户账号',
            dataIndex: 'username'
        },
        {
            title: '用户名',
            dataIndex: 'nickname'
        },
        {
            title: '创建时间',
            dataIndex: 'createTime',
            customRender: ({ text }) => toDateString(text)
        }
    ]);
</script>
```

更复杂的列可以使用 `bodyCell` 插槽自定义，需要 AntDesignVue 的版本为 3.x ：

::: code-tabs

@tab TypeScript

```vue
<template>
    <ele-pro-table :columns="columns">
        <template #bodyCell="{ column, record }">
            <!-- 状态列 -->
            <template v-if="column.key === 'status'">
                <a-switch
                    :checked="record.status === 0"
                    @change="(checked: boolean) => editStatus(checked, record)"
                />
            </template>
            <!-- 操作列 -->
            <template v-else-if="column.key === 'action'">
                <a-space>
                    <a @click="openEdit(record)">修改</a>
                    <a-divider type="vertical" />
                    <a-popconfirm title="确定要删除此用户吗？" @confirm="remove(record)">
                        <a class="ele-text-danger">删除</a>
                    </a-popconfirm>
                </a-space>
            </template>
        </template>
    </ele-pro-table>
</template>

<script lang="ts" setup>
    import { ref } from 'vue';
    import type { ColumnItem } from 'ele-admin-pro/es/ele-pro-table/types';

    // 表格列配置
    const columns = ref<ColumnItem[]>([
        {
            title: '用户账号',
            dataIndex: 'username',
            sorter: true
        },
        {
            title: '状态',
            key: 'status',
            dataIndex: 'status',
            sorter: true
        },
        {
            title: '操作',
            key: 'action'
        }
    ]);
</script>
```

@tab JavaScript

```vue
<template>
    <ele-pro-table :columns="columns">
        <template #bodyCell="{ column, record }">
            <!-- 状态列 -->
            <template v-if="column.key === 'status'">
                <a-switch
                    :checked="record.status === 0"
                    @change="(checked) => editStatus(checked, record)"
                />
            </template>
            <!-- 操作列 -->
            <template v-else-if="column.key === 'action'">
                <a-space>
                    <a @click="openEdit(record)">修改</a>
                    <a-divider type="vertical" />
                    <a-popconfirm title="确定要删除此用户吗？" @confirm="remove(record)">
                        <a class="ele-text-danger">删除</a>
                    </a-popconfirm>
                </a-space>
            </template>
        </template>
    </ele-pro-table>
</template>

<script setup>
    import { ref } from 'vue';

    // 表格列配置
    const columns = ref([
        {
            title: '用户账号',
            dataIndex: 'username',
            sorter: true
        },
        {
            title: '状态',
            key: 'status',
            dataIndex: 'status',
            sorter: true
        },
        {
            title: '操作',
            key: 'action'
        }
    ]);
</script>
```

:::

a-table 还支持更多的插槽，比如自定义表头、总结栏、空数据、筛选图标、展开图标等等，请到 AntDesignVue 的文档查看。

<br/>

## 6.2.4. 事件列表  

ele-pro-table 支持的全部事件如下，后面四个事件同 ant-design-vue 的 a-table 的事件：

| 事件名称           | 说明                             | 回调参数                    |
| :----------------- | :------------------------------- | :-------------------------- |
| done               | 表格数据渲染完成回调             | res, curr, count, parent    |
| update:selection   | 用于 selection 属性的 v-model    | selection                   |
| update:current     | 用于 current 属性的 v-model      | current                     |
| refresh            | 直接指定数据源时点击刷新按钮触发 | 无                          |
| columns-change     | 列配置改变时触发                 | columns                     |
| size-change        | 表格尺寸改变时触发               | size                        |
| fullscreen-change  | 表格全屏切换时触发 `v1.2.0新增`  | fullscreen                  |
| expandedRowsChange | 展开的行变化时触发               | expandedRows                |
| change             | 分页、排序、筛选变化时触发       | pagination, filters, sorter |
| expand             | 点击展开图标时触发               | expanded, record            |
| resizeColumn       | 拖动列时触发                     | width, column               |

done 事件的类型定义如下，curr 是页码，count 是总数量，如果是树形表格懒加载子级加载完成回调最后一个参数 parent 是父级的数据：

```typescript
type EleProTableDone<T> = (res: EleProTableDoneRes<T>, curr: number, count: number, parent?: T) => void;

interface EleProTableDoneRes<T> {
    // 数据
    data: T[];
    // 总数量
    total: number;
    // 接口原始返回数据
    response: any;
}
```

监听行点击事件：

::: code-tabs

@tab TypeScript

```vue
<template>
    <ele-pro-table
        ref="tableRef"
        row-key="userId"
        :columns="columns"
        :datasource="datasource"
        :custom-row="customRow"
    >
    </ele-pro-table>
</template>

<script lang="ts" setup>
    import type { User } from '@/api/system/user/model';

    /* 自定义行属性 */
    const customRow = (record: User) => {
        return {
            // 行点击事件
            onClick: () => {
                console.log('record:', record);
            }
        };
    };
</script>
```

@tab JavaScript

```vue
<template>
    <ele-pro-table
        ref="tableRef"
        row-key="userId"
        :columns="columns"
        :datasource="datasource"
        :custom-row="customRow"
    >
    </ele-pro-table>
</template>

<script setup>
    /* 自定义行属性 */
    const customRow = (record) => {
        return {
            // 行点击事件
            onClick: () => {
                console.log('record:', record);
            }
        };
    };
</script>
```

:::

监听单元格点击事件：

::: code-tabs

@tab TypeScript

```vue
<template>
    <ele-pro-table
        ref="tableRef"
        row-key="userId"
        :columns="columns"
        :datasource="datasource"
    >
    </ele-pro-table>
</template>

<script lang="ts" setup>
    import { ref } from 'vue';
    import type { ColumnItem } from 'ele-admin-pro/es/ele-pro-table/types';
    import type { User } from '@/api/system/user/model';

    // 表格列配置
    const columns = ref<ColumnItem[]>([
        {
            title: '账号',
            dataIndex: 'username'
        },
        {
            title: '昵称',
            dataIndex: 'nickname',
            customCell: (record: User) => {
                return {
                    onClick: (e: MouseEvent) => {
                        e.stopPropagation();
                        console.log('record:', record);
                    }
                };
            }
        }
    ]);
</script>
```

@tab JavaScript

```vue
<template>
    <ele-pro-table
        ref="tableRef"
        row-key="userId"
        :columns="columns"
        :datasource="datasource"
    >
    </ele-pro-table>
</template>

<script setup>
    import { ref } from 'vue';

    // 表格列配置
    const columns = ref([
        {
            title: '账号',
            dataIndex: 'username'
        },
        {
            title: '昵称',
            dataIndex: 'nickname',
            customCell: (record) => {
                return {
                    onClick: (e) => {
                        e.stopPropagation();
                        console.log('record:', record);
                    }
                };
            }
        }
    ]);
</script>
```

:::

<br/>

## 6.2.5. 实例方法  

给组件增加 `ref` 属性，可以通过 ref 调用实例方法，全部实例方法：

| 方法      | 说明                            | 参数               |
| :-------- | :------------------------------ | :----------------- |
| reload    | 刷新表格数据                    | option: Object     |
| getData   | 获取表格当前页数据              | 无                 |
| setData   | 修改表格当前页数据 `v1.8.0新增` | data: Array        |
| doRequest | 获取请求参数 `v1.8.0新增`       | callback: Function |

刷新表格的方法 `reload` ：

::: code-tabs

@tab TypeScript

```vue
<template>
    <ele-pro-table ref="tableRef" :columns="columns" :datasource="datasource"></ele-pro-table>
</template>

<script lang="ts" setup>
    import type { EleProTable } from 'ele-admin-pro';
    
    // 表格 ref
    const tableRef = ref<InstanceType<typeof EleProTable> | null>(null);
    
    /* 搜索 */
    const reload = (where?: UserParam) => {
        // reload 方法的参数非必须，可不写
        tableRef?.value?.reload({ page: 1, where: where });
        //tableRef?.value?.reload();
    };
</script>
```

@tab JavaScript

```vue
<template>
    <ele-pro-table ref="tableRef" :columns="columns" :datasource="datasource"></ele-pro-table>
</template>

<script setup>
    // 表格 ref
    const tableRef = ref(null);
    
    /* 搜索 */
    const reload = (where) => {
        // reload 方法的参数非必须，可不写
        tableRef?.value?.reload({ page: 1, where: where });
        //tableRef?.value?.reload();
    };
</script>
```

:::

reload 方法的参数类型定义：

```typescript
import type { FilterValue, SorterResult } from 'ant-design-vue/es/table/interface';
import type { DefaultRecordType } from 'ant-design-vue/es/vc-table/interface';

// reload 方法的参数
interface ReloadOption {
  page?: number;  // 页码
  limit?: number;  // 每页数量
  where?: WhereType;  // 搜索参数
  sorter?: SorterType;  // 排序参数
  filters?: FilterType;  // 筛选参数
}

type WhereType = Record<string, any>;

type SorterType = SorterResult<RecordType> | SorterResult<RecordType>[];

type FilterType = Record<string, FilterValue | null | undefined>;

type RecordType = DefaultRecordType;
```

reload 方法树形表格时刷新某个子级的数据，reload 方法参数二传父级的数据即可：

::: code-tabs

@tab TypeScript

```vue
<template>
    <ele-pro-table ref="tableRef" :columns="columns" :datasource="datasource">
        <template #bodyCell="{ column, record }">
            <template v-if="column.key === 'action'">
                <a-space>
                    <a>修改</a>
                    <a-divider type="vertical" />
                    <a class="ele-text-danger">删除</a>
                    <a-divider type="vertical" />
                    <a @click="reloadChildren(record)">刷新子级</a>
                </a-space>
            </template>
        </template>
    </ele-pro-table>
</template>

<script lang="ts" setup>
    import type { EleProTable } from 'ele-admin-pro';
    
    // 表格 ref
    const tableRef = ref<InstanceType<typeof EleProTable> | null>(null);
    
    /* 刷新子级 */
    const reloadChildren = (parent: any) => {
        tableRef?.value?.reload({}, parent);
    };
</script>
```

@tab JavaScript

```vue
<template>
    <ele-pro-table ref="tableRef" :columns="columns" :datasource="datasource">
        <template #bodyCell="{ column, record }">
            <template v-if="column.key === 'action'">
                <a-space>
                    <a>修改</a>
                    <a-divider type="vertical" />
                    <a class="ele-text-danger">删除</a>
                    <a-divider type="vertical" />
                    <a @click="reloadChildren(record)">刷新子级</a>
                </a-space>
            </template>
        </template>
    </ele-pro-table>
</template>

<script setup>
    // 表格 ref
    const tableRef = ref(null);
    
    /* 刷新子级 */
    const reloadChildren = (parent) => {
        tableRef?.value?.reload({}, parent);
    };
</script>
```

:::

`doRequest` 方法获取同数据源一样的参数，可以用于导出搜索、排序、筛选后的数据：

::: code-tabs

@tab TypeScript

```vue
<template>
    <div>
        <ele-pro-table ref="tableRef" :columns="columns" :datasource="datasource"></ele-pro-table>
        <a-button @click="exportData">导出</a-button>
    </div>
</template>

<script lang="ts" setup>
    import type { EleProTable } from 'ele-admin-pro';
    import { listLoginRecords } from '@/api/system/login-record';
    import { utils, writeFile } from 'xlsx';

    // 表格 ref
    const tableRef = ref<InstanceType<typeof EleProTable> | null>(null);

    /* 导出排序、筛选、搜索后的不分页的全部的数据 */
    const exportData = () => {
        const array = [['账号', '用户名', '登录时间']];
        const hide = message.loading('请求中..', 0);
        tableRef.value?.doRequest(({ where, orders, filters }) => {
            // 方法的参数与数据源的完全一致，详见文档 6.2.6
            listLoginRecords({ ...where, ...orders, ...filters }).then((data) => {
                hide();
                data.forEach((d) => {
                    array.push([d.username, d.nickname, d.createTime]);
                });
                writeFile({
                    SheetNames: ['Sheet1'],
                    Sheets: {
                        Sheet1: utils.aoa_to_sheet(array)
                    }
                }, '登录日志.xlsx');
            }).catch((e) => {
                hide();
                message.error(e.message);
            });
        });
    };
</script>
```

@tab JavaScript

```vue
<template>
    <div>
        <ele-pro-table ref="tableRef" :columns="columns" :datasource="datasource"></ele-pro-table>
        <a-button @click="exportData">导出</a-button>
    </div>
</template>

<script setup>
    import { listLoginRecords } from '@/api/system/login-record';
    import { utils, writeFile } from 'xlsx';

    // 表格 ref
    const tableRef = ref(null);

    /* 导出排序、筛选、搜索后的不分页的全部的数据 */
    const exportData = () => {
        const array = [['账号', '用户名', '登录时间']];
        const hide = message.loading('请求中..', 0);
        tableRef.value?.doRequest(({ where, orders, filters }) => {
            // 方法的参数与数据源的完全一致，详见文档 6.2.6
            listLoginRecords({ ...where, ...orders, ...filters }).then((data) => {
                hide();
                data.forEach((d) => {
                    array.push([d.username, d.nickname, d.createTime]);
                });
                writeFile({
                    SheetNames: ['Sheet1'],
                    Sheets: {
                        Sheet1: utils.aoa_to_sheet(array)
                    }
                }, '登录日志.xlsx');
            }).catch((e) => {
                hide();
                message.error(e.message);
            });
        });
    };
</script>
```

:::

  `getData` 和 `setData` 可以用于操作表格当前页的数据，比如不重新请求接口直接删除一条数据、添加一条数据， 一般不建议这样做，因为使用 Function 数据源是用于后端分页的，如果直接在当前页添加一条数据或删除一条数据不符合分页的逻辑， 不重新请求直接修改一条数据这样是合理的，修改数据直接用插槽里面的行数据即可，也用不到这两个方法，示例：

::: code-tabs

@tab TypeScript

```vue
<template>
    <div>
        <ele-pro-table
            ref="tableRef"
            row-key="userId"
            :columns="columns"
            :datasource="datasource"
        >
            <template #bodyCell="{ column, record }">
                <template v-if="column.key === 'action'">
                    <a @click="openEdit(record)">修改</a>
                    <a @click="remove(record)">删除</a>
                </template>
            </template>
        </ele-pro-table>
        <!-- 编辑弹窗 -->
        <user-edit v-model:visible="showEdit" :data="current" @done="onEditDone" />
    </div>
</template>

<script lang="ts" setup>
    import { createVNode, ref, reactive } from 'vue';
    import { message, Modal } from 'ant-design-vue';
    import type { EleProTable } from 'ele-admin-pro';
    import type { DatasourceFunction, ColumnItem } from 'ele-admin-pro/es/ele-pro-table/types';
    import UserEdit from './components/user-edit.vue';
    import { pageUsers } from '@/api/system/user';
    import type { User } from '@/api/system/user/model';

    // 表格 ref
    const tableRef = ref<InstanceType<typeof EleProTable> | null>(null);

    // 表格列配置
    const columns = ref<ColumnItem[]>([
        {
            title: '用户账号',
            dataIndex: 'username'
        },
        {
            title: '用户名',
            dataIndex: 'nickname'
        },
        {
            title: '操作',
            key: 'action'
        }
    ]);

    // 当前编辑数据
    const current = ref<User | null>(null);

    // 是否显示编辑弹窗
    const showEdit = ref(false);

    /* 表格数据源 */
    const datasource: DatasourceFunction = ({ page, limit, where, orders }) => {
        return pageUsers({ ...where, ...orders, page, limit });
    };

    /* 打开编辑弹窗 */
    const openEdit = (row?: User) => {
        current.value = row ?? null;  // 表格的行数据赋值给 current
        showEdit.value = true;
    };

    /* 编辑完成事件 */
    const onEditDone = (form: User) => {
        // 编辑弹窗编辑完成后 done 事件回传表单数据，使用 Object.assign 赋值给 current 即可修改表格的行数据
        if(current.value) {
            Object.assign(current.value, form);
        }
    }

    /* 删除单个 */
    const remove = (row: User) => {
        // 虽然不推荐，你仍可以使用 getData 和 setData 操作当前页数据
        const data = tableRef.value?.getData() ?? [];
        tableRef.value?.setData(data.filter(d => d !== row));
    };
</script>
```

@tab JavaScript

```vue
<template>
    <div>
        <ele-pro-table
            ref="tableRef"
            row-key="userId"
            :columns="columns"
            :datasource="datasource"
        >
            <template #bodyCell="{ column, record }">
                <template v-if="column.key === 'action'">
                    <a @click="openEdit(record)">修改</a>
                    <a @click="remove(record)">删除</a>
                </template>
            </template>
        </ele-pro-table>
        <!-- 编辑弹窗 -->
        <user-edit v-model:visible="showEdit" :data="current" @done="onEditDone" />
    </div>
</template>

<script setup>
    import { createVNode, ref, reactive } from 'vue';
    import { message, Modal } from 'ant-design-vue';
    import UserEdit from './components/user-edit.vue';
    import { pageUsers } from '@/api/system/user';

    // 表格 ref
    const tableRef = ref(null);

    // 表格列配置
    const columns = ref([
        {
            title: '用户账号',
            dataIndex: 'username'
        },
        {
            title: '用户名',
            dataIndex: 'nickname'
        },
        {
            title: '操作',
            key: 'action'
        }
    ]);

    // 当前编辑数据
    const current = ref(null);

    // 是否显示编辑弹窗
    const showEdit = ref(false);

    /* 表格数据源 */
    const datasource = ({ page, limit, where, orders }) => {
        return pageUsers({ ...where, ...orders, page, limit });
    };

    /* 打开编辑弹窗 */
    const openEdit = (row) => {
        current.value = row ?? null;  // 表格的行数据赋值给 current
        showEdit.value = true;
    };

    /* 编辑完成事件 */
    const onEditDone = (form) => {
        // 编辑弹窗编辑完成后 done 事件回传表单数据，使用 Object.assign 赋值给 current 即可修改表格的行数据
        if(current.value) {
            Object.assign(current.value, form);
        }
    }

    /* 删除单个 */
    const remove = (row) => {
        // 虽然不推荐，你仍可以使用 getData 和 setData 操作当前页数据
        const data = tableRef.value?.getData() ?? [];
        tableRef.value?.setData(data.filter(d => d !== row));
    };
</script>
```

:::

如果想要纯前端添加、修改、删除数据，应该使用 Array 类型的数据源，数据源改变后表格数据也会响应改变。

<br/>

## 6.2.6. 设置数据源  

数据源 datasource 如果设置为 Array 类型是进行纯前端的分页、排序，设置为 Function 类型可以调用 api 请求数据进行后端分页、排序：

::: code-tabs

@tab TypeScript

```vue
<template>
    <ele-pro-table row-key="userId" :datasource="datasource"></ele-pro-table>
</template>

<script lang="ts" setup>
    // ......省略其它代码
    import type { DatasourceFunction } from 'ele-admin-pro/es/ele-pro-table/types';
    import { pageUsers } from '@/api/system/user';

    /* 表格数据源，v1.6.0 开始需要返回一个 Promise */
    const datasource: DatasourceFunction = ({ page, limit, where, orders }) => {
        return pageUsers({ ...where, ...orders, page, limit });
        // 如果你想改参数名也很简单
        /*return pageUsers({
            ...where,
            orderField: orders.order, 
            orderType: orders.sort,
            pageNum: page,
            pageSize: limit
        });*/
        // orders 下面的字段名其实是可以通过 request 属性配置的
    };

    /* 如果想在数据源里面获取到接口返回的数据, 增加 async 和 await */
    const datasource: DatasourceFunction = async ({ page, limit, where, orders }) => {
        const result = await pageUsers({ ...where, ...orders, page, limit });
        console.log('result:', result);
        return result;  // 这里返回的数据要包含 list 和 count 两个字段，字段名称可以通过 response 属性配置
    };
</script>
```

@tab JavaScript

```vue
<template>
    <ele-pro-table row-key="userId" :datasource="datasource"></ele-pro-table>
</template>

<script setup>
    // ......省略其它代码
    import { pageUsers } from '@/api/system/user';

    /* 表格数据源，v1.6.0 开始需要返回一个 Promise */
    const datasource = ({ page, limit, where, orders }) => {
        return pageUsers({ ...where, ...orders, page, limit });
        // 如果你想改参数名也很简单
        /*return pageUsers({
            ...where,
            orderField: orders.order, 
            orderType: orders.sort,
            pageNum: page,
            pageSize: limit
        });*/
        // orders 下面的字段名其实是可以通过 request 属性配置的
    };

    /* 如果想在数据源里面获取到接口返回的数据, 增加 async 和 await */
    const datasource = async ({ page, limit, where, orders }) => {
        const result = await pageUsers({ ...where, ...orders, page, limit });
        console.log('result:', result);
        return result;  // 这里返回的数据要包含 list 和 count 两个字段，字段名称可以通过 response 属性配置
    };
</script>
```

:::

调用 api 中请求接口查询数据方法的代码示例：

::: code-tabs

@tab TypeScript

```typescript
import request from '@/utils/request';
import type { ApiResult, PageResult } from '@/api';
import type { User, UserParam } from './model';

/** 分页查询用户 */
export async function pageUsers(params: UserParam) {
    const res = await request.get<ApiResult<PageResult<User>>>(
        '/system/user/page', { params }
    );
    if (res.data.code === 0 && res.data.data) {
        // 这里返回的数据要包含 list 和 count 两个字段，字段名称可以通过 response 属性配置
        return res.data.data;
        // 不想通过 response 属性配置也可以这样写
        /*return {
            list: res.data.rows,
            count: res.data.total
        };*/
    }
    return Promise.reject(new Error(res.data.message));
}
```

@tab JavaScript

```javascript
import request from '@/utils/request';

/** 分页查询用户 */
export async function pageUsers(params) {
    const res = await request.get(
        '/system/user/page', { params }
    );
    if (res.data.code === 0 && res.data.data) {
        // 这里返回的数据要包含 list 和 count 两个字段，字段名称可以通过 response 属性配置
        return res.data.data;
        // 不想通过 response 属性配置也可以这样写
        /*return {
            list: res.data.rows,
            count: res.data.total
        };*/
    }
    return Promise.reject(new Error(res.data.message));
}
```

:::

list 是当前页的数据，count 是数据总数，也可以返回一个数组，返回数组时 count 就是数组的长度，Function 数据源的类型定义：

```typescript
type DatasourceFunction = (params: DatasourceParams) => Promise<DatasourceResult>;

interface DatasourceParams {
    page?: number;  // 页码
    limit?: number;  // 每页数量
    where: WhereType;  // 查询参数
    orders: OrderType;  // 排序参数
    filters: FilterType;  // 筛选参数
    sorter: SorterType;  // a-table 排序的值
    orgFilters: FilterType;  // a-table 筛选的值
    parent?: RecordType;  // 树形表格懒加载子级时父级的数据
}

type DatasourceResult = RecordType[] | Record<string, any> | undefined;

// orders 和 sorter 区别，orders 的字段名是根据 request 配置的，方便你当作请求参数
// filters 和 orgFilters 区别，filters 会把数组类型的值用逗号分割，方便你当作请求参数
```

上面数据源示例代码使用了 async 和 await 可能不太直观，datasource 需要返回一个 Promise，直观一点是这样的：

```javascript
// 例如某些框架喜欢把 api 里面写的很简单
import request from '@/utils/request';

export function pageUsers(params) {
    return request.get('/system/user/page', params);
}
```

上面这种 api 的写法 datasource 可以这样写：

```javascript
import { pageUsers } from '@/api/system/user';

const datasource = ({ page, limit, where, orders }) => {
    return new Promise((resolve, reject) => {
        pageUsers({
            params: { ...where, ...orders, page, limit }  // 如果要改参数名参照上面代码
        }).then((res) => {
            if(res.data.code === 0 && res.data.data) { 
                // resolve 默认是需要 list 和 count 两个字段，字段名称可以通过 response 属性配置
                resolve(res.data);
                // 不想通过 response 属性配置也可以这样写
                /*resolve({
                    list: res.data.rows,
                    count: res.data.total
                });*/
            } else {
                reject(new Error(res.data.message));
            }
        }).catch((e) => {
            reject(e);
        });
    });
};
```

request 和 response 属性用于设置接口的数据格式，从上面可以看出设置格式的方法很多它们有点多余，但是它们还可以全局配置带来一点点的便利：

::: code-tabs

@tab TypeScript

```vue
<template>
    <ele-pro-table :request="request" :response="response"></ele-pro-table>
</template>

<script lang="ts" setup>
    import { ref } from 'vue';
    import type { RequestOption, ResponseOption } from 'ele-admin-pro/es/ele-pro-table/types';

    const request = ref<RequestOption>({
        sortName: 'sort',  // 排序字段参数名称
        orderName: 'order'  // 排序方式的参数名称
    });
    
    const response = ref<ResponseOption>({
        dataName: 'list',  // 数据列表的字段名称，支持嵌套，例如：data.rows
        countName: 'count'  // 数据总数的字段名称，例如：data.total
    });
</script>
```

@tab JavaScript

```vue
<template>
    <ele-pro-table :request="request" :response="response"></ele-pro-table>
</template>

<script setup>
    import { ref } from 'vue';

    const request = ref({
        sortName: 'sort',  // 排序字段参数名称
        orderName: 'order'  // 排序方式的参数名称
    });

    const response = ref({
        dataName: 'list',  // 数据列表的字段名称，支持嵌套，例如：data.rows
        countName: 'count'  // 数据总数的字段名称，例如：data.total
    });
</script>
```

:::

还提供了 `parseData` 属性用于处理返回数据格式，是在 `response` 属性处理之前调用：

```vue
<template>
    <ele-pro-table :parse-data="parseData"></ele-pro-table>
</template>

<script setup>
    const parseData = (res) => {
        // res 即接口返回的数据
        return {
            list: res.rows,
            count: res.total
        };
        // 有了 response 还有个 parseData 是考虑你可能需要处理复杂的数据格式时 response 无法满足
    }
</script>
```

使用 Array 类型数据源是进行纯前端的分页、排序，可以先给一个空数组，然后调用查询全部的接口获取全部数据：

::: code-tabs

@tab TypeScript

```vue
<template>
    <ele-pro-table
        ref="tableRef"
        row-key="userId"
        :columns="columns"
        :datasource="datasource"
    >
    </ele-pro-table>
</template>

<script lang="ts" setup>
    import { ref } from 'vue';
    import type { EleProTable } from 'ele-admin-pro';
    import type { ColumnItem } from 'ele-admin-pro/es/ele-pro-table/types';
    import { listUsers } from '@/api/system/user';
    import type { User } from '@/api/system/user/model';

    // 表格 ref
    const tableRef = ref<InstanceType<typeof EleProTable> | null>(null);

    // 表格列配置，sorter 可以为 true 也可以使用自定义排序函数
    const columns = ref<ColumnItem[]>([
        {
            key: 'index',
            width: 48,
            align: 'center',
            fixed: 'left',
            hideInSetting: true,
            customRender: ({ index }) => index + (tableRef.value?.tableIndex ?? 0)
        },
        {
            title: '用户账号',
            dataIndex: 'username',
            sorter: true
        },
        {
            title: '用户名',
            key: 'nickname',
            dataIndex: 'nickname',
            sorter: (a, b) => {
                return a.nickname == b.nickname ? 0 : a.nickname < b.nickname ? 1 : -1;
            }
        },
        {
            title: '手机号',
            dataIndex: 'phone',
            sorter: true
        }
    ]);

    // 表格数据源
    const datasource = ref<User[]>([]);

    // 调用查询全部数据的接口
    listUsers().then(data => {
        datasource.value = data;
    });
</script>
```

@tab JavaScript

```vue
<template>
    <ele-pro-table
        ref="tableRef"
        row-key="userId"
        :columns="columns"
        :datasource="datasource"
    >
    </ele-pro-table>
</template>

<script setup>
    import { ref } from 'vue';
    import { listUsers } from '@/api/system/user';

    // 表格 ref
    const tableRef = ref(null);

    // 表格列配置，sorter 可以为 true 也可以使用自定义排序函数
    const columns = ref([
        {
            key: 'index',
            width: 48,
            align: 'center',
            fixed: 'left',
            hideInSetting: true,
            customRender: ({ index }) => index + (tableRef.value?.tableIndex ?? 0)
        },
        {
            title: '用户账号',
            dataIndex: 'username',
            sorter: true
        },
        {
            title: '用户名',
            key: 'nickname',
            dataIndex: 'nickname',
            sorter: (a, b) => {
                return a.nickname == b.nickname ? 0 : a.nickname < b.nickname ? 1 : -1;
            }
        },
        {
            title: '手机号',
            dataIndex: 'phone',
            sorter: true
        }
    ]);

    // 表格数据源
    const datasource = ref([]);

    // 调用查询全部数据的接口
    listUsers().then(data => {
        datasource.value = data;
    });
</script>
```

:::

注意，v1.5.0 以及之前的版本 datasource 是通过 callback 回调数据而不是 Promise ：

```javascript
//import userApi from '@/api/user';

export default {
    methods: {
        datasource(option, callback) {
            console.log(option);
            // option 包含 { page: 1, limit: 10, where: {}, order: {}, filters: {} }
            // where 是传递的搜索条件，order 是排序参数，filters 是列筛选参数
            // 在这里去请求数据
            callback([ {}, {}, {} ]);  // 然后通过 callback 回调数据
            //callback([ {}, {}, {} ], total);  // callback 参数二是总数量
            //callback('请求失败');  // 参数传 string 类型是失败回调

            // 例如通过 api 模块去请求数据
            /*userApi.pageList({
                ...this.where,  // 这里是搜索表单的数据
                page: option.page,  // 第几页
                limit: option.limit  // 每页多少条
            }).then((res) => {
                callback(res.data, res.total);  // 通过 callback 回调数据
                // 也可能还需要判断状态码
                if(res.code === 0) {
                    callback(res.data, res.total);
                } else {
                    callback(res.msg);
                }
            }).catch((e) => {
                callback(e.message);
            });*/
        }
    }
}
```

<br/>

## 6.2.7. 单选多选列  

下面是表格增加多选列以及获取选中数据，使用 v-model 绑定了 `selection` 参数就会开启多选列，选中数据会自动同步：

::: code-tabs

@tab TypeScript

```vue
<template>
    <ele-pro-table
        :columns="columns"
        :datasource="datasource"
        v-model:selection="selection"
        :row-selection="{ columnWidth: 48 }"
        @done="onDone"
    >
    </ele-pro-table>
</template>

<script lang="ts" setup>
    import { ref } from 'vue';
    import type { User } from '@/api/system/user/model';
    import type { EleProTableDone } from 'ele-admin-pro/es/ele-pro-table/types';

    // 表格选中数据，如果要回显选中数据可在 done 事件里面给 selection 赋值
    const selection = ref<User[] | undefined>([]);

    /* 清空选择 */
    const clearSelection = () => {
        selection.value = [];
    };

    // 如果要动态控制是否开启多选列可以将 selection 赋值为 undefined 
    const hideSelection = () => {
        selection.value = undefined;
    };

    /* 表格数据请求完成事件 */
    const onDone: EleProTableDone<User> = ({ data }) => {
        // 回显 id 为 19、22、21 的数据的复选框
        const ids = [19, 22, 21];
        selection.value = data.filter((d) => d.userId && ids.includes(d.userId));
    };
</script>
```

@tab JavaScript

```vue
<template>
    <ele-pro-table
        :columns="columns"
        :datasource="datasource"
        v-model:selection="selection"
        :row-selection="{ columnWidth: 48 }"
        @done="onDone"
    >
    </ele-pro-table>
</template>

<script setup>
    import { ref } from 'vue';

    // 表格选中数据，如果要回显选中数据可在 done 事件里面给 selection 赋值
    const selection = ref([]);

    /* 清空选择 */
    const clearSelection = () => {
        selection.value = [];
    };

    // 如果要动态控制是否开启多选列可以将 selection 赋值为 undefined 
    const hideSelection = () => {
        selection.value = undefined;
    };

    /* 表格数据请求完成事件 */
    const onDone = ({ data }) => {
        // 回显 id 为 19、22、21 的数据的复选框
        const ids = [19, 22, 21];
        selection.value = data.filter((d) => d.userId && ids.includes(d.userId));
    };
</script>
```

:::

`row-selection` 参数可不写，里面的 `selectedRowKeys`、`onChange`、`type` 三个字段不需要写，如果需要监听选择改变：

::: code-tabs

@tab TypeScript

```vue
<template>
    <!-- 把 v-model 拆开写，或者简单一点，用 watch 监听 selection 也行 -->
    <ele-pro-table 
        :selection="selection" 
        @update:selection="onSelectionChange">
    </ele-pro-table>
</template>

<script lang="ts" setup>
    import { ref } from 'vue';
    import type { User } from '@/api/system/user/model';

    const selection = ref<User[]>([]);

    const onSelectionChange = (selection) => {
        selection.value = selection;
    };
</script>
```

@tab JavaScript

```vue
<template>
    <!-- 把 v-model 拆开写，或者简单一点，用 watch 监听 selection 也行 -->
    <ele-pro-table 
        :selection="selection" 
        @update:selection="onSelectionChange">
    </ele-pro-table>
</template>

<script setup>
    import { ref } from 'vue';

    const selection = ref([]);

    const onSelectionChange = (selection) => {
        selection.value = selection;
    };
</script>
```

:::

开启单选列与多选列类似，使用 v-model 绑定了 `current` 参数即可，也支持设置 `row-selection` 参数：

::: code-tabs

@tab TypeScript

```vue
<template>
    <ele-pro-table v-model:current="current"></ele-pro-table>
</template>

<script lang="ts" setup>
    import { ref } from 'vue';
    import type { User } from '@/api/system/user/model';

    // 表格单选选中数据
    const current = ref<User | null>(null);

    /* 清空选择 */
    const clearSelection = () => {
        current.value = null;
    }
</script>
```

@tab JavaScript

```vue
<template>
    <ele-pro-table v-model:current="current"></ele-pro-table>
</template>

<script setup>
    import { ref } from 'vue';

    // 表格单选选中数据
    const current = ref(null);

    /* 清空选择 */
    const clearSelection = () => {
        current.value = null;
    }
</script>
```

:::

也可以同时绑定 `current` 和 `selection`，并通过 `selectionType` 动态控制是单选列还是多选列：

::: code-tabs

@tab TypeScript

```vue
<template>
    <ele-pro-table 
        v-model:current="current" 
        v-model:selection="selection"
        :selection-type="selectionType">
    </ele-pro-table>
</template>

<script lang="ts" setup>
    import { ref } from 'vue';
    import type { User } from '@/api/system/user/model';

    // 表格单选选中数据
    const current = ref<User>();

    // 表格多选选中数据
    const selection = ref<User[]>([]);

    // 控制是单选还是多选
    const selectionType = ref('radio');

    /* 动态改变为多选列 */
    const changeSelectionType = () => {
        selectionType.value = 'checkbox';
    }
</script>
```

@tab JavaScript

```vue
<template>
    <ele-pro-table 
        v-model:current="current" 
        v-model:selection="selection"
        :selection-type="selectionType">
    </ele-pro-table>
</template>

<script setup>
    import { ref } from 'vue';

    // 表格单选选中数据
    const current = ref();

    // 表格多选选中数据
    const selection = ref([]);

    // 控制是单选还是多选
    const selectionType = ref('radio');

    /* 动态改变为多选列 */
    const changeSelectionType = () => {
        selectionType.value = 'checkbox';
    }
</script>
```

:::

多选列实现禁用某些数据的复选框，通过 `row-selection` 实现：

::: code-tabs

@tab TypeScript

```vue
<template>
    <ele-pro-table  v-model:selection="selection"  :row-selection="rowSelection"></ele-pro-table>
</template>

<script lang="ts" setup>
    import { ref, reactive } from 'vue';
    import type { User } from '@/api/system/user/model';

    const selection = ref<User[]>([]);

    const rowSelection = reactive({
        columnWidth: 48,
        getCheckboxProps: (record) => {
            return {
                disabled: record.id < 3  // 这里是让 id<3 的禁用，根据自己业务修改
            };
        }
    });
</script>
```

@tab JavaScript

```vue
<template>
    <ele-pro-table  v-model:selection="selection"  :row-selection="rowSelection"></ele-pro-table>
</template>

<script setup>
    import { ref, reactive } from 'vue';

    const selection = ref([]);

    const rowSelection = reactive({
        columnWidth: 48,
        getCheckboxProps: (record) => {
            return {
                disabled: record.id < 3  // 这里是让 id<3 的禁用，根据自己业务修改
            };
        }
    });
</script>
```

:::

单选实现行点击选中：

::: code-tabs

@tab TypeScript

```vue
<template>
    <ele-pro-table  v-model:current="current"  :custom-row="customRow"></ele-pro-table>
</template>

<script lang="ts" setup>
    import { ref, reactive } from 'vue';
    import type { User } from '@/api/system/user/model';

    const current = ref<User | null>(null);

    const customRow = (record: User) => {
        return {
            // 行点击事件
            onClick: () => {
                current.value = record;
            }
        };
    };
</script>
```

@tab JavaScript

```vue
<template>
    <ele-pro-table  v-model:current="current"  :custom-row="customRow"></ele-pro-table>
</template>

<script setup>
    import { ref, reactive } from 'vue';

    const current = ref(null);

    const customRow = (record) => {
        return {
            // 行点击事件
            onClick: () => {
                current.value = record;
            }
        };
    };
</script>
```

:::

多选实现行点击选中和取消选中：

::: code-tabs

@tab TypeScript

```vue
<template>
    <ele-pro-table  v-model:selection="selection"  :custom-row="customRow"></ele-pro-table>
</template>

<script lang="ts" setup>
    import { ref, reactive } from 'vue';
    import type { User } from '@/api/system/user/model';

    const selection = ref<User[]>([]);

    const customRow = (record: User) => {
        return {
            // 行点击事件
            onClick: () => {
                if (selection.value.includes(record)) {
                    selection.value = selection.value.filter((d) => d !== record);
                } else {
                    selection.value = selection.value.concat([record]);
                }
            }
        };
    };
</script>
```

@tab JavaScript

```vue
<template>
    <ele-pro-table  v-model:selection="selection"  :custom-row="customRow"></ele-pro-table>
</template>

<script setup>
    import { ref, reactive } from 'vue';

    const selection = ref([]);

    const customRow = (record) => {
        return {
            // 行点击事件
            onClick: () => {
                if (selection.value.includes(record)) {
                    selection.value = selection.value.filter((d) => d !== record);
                } else {
                    selection.value = selection.value.concat([record]);
                }
            }
        };
    };
</script>
```

:::

<br/>

## 6.2.8. 实现展开行  

通过 `a-table` 的 `expandedRowRender` 插槽实现，详见 AntDesignVue 的文档，使用示例：

```vue
<template>
    <ele-pro-table row-key="userId" :datasource="datasource" :columns="columns">
        <template #expandedRowRender="{ record }">
            <p style="margin: 0">
                {{ record.description }}
            </p>
        </template>
    </ele-pro-table>
</template>
```

<br/>

## 6.2.9. 树形懒加载  

增加 `lazy-load` 属性开启懒加载，点加载子级的时候也会通过数据源请求，并且会传递 `parent` 参数值为父级的数据：

::: code-tabs

@tab TypeScript

```vue
<template>
    <ele-pro-table
        row-key="menuId"
        :datasource="datasource"
        :columns="columns"
        :lazy-load="true"
        :need-page="false"
        :expand-icon-column-index="1">
    </ele-pro-table>
</template>

<script lang="ts" setup>
    import { ref } from 'vue';
    import type { EleProTable } from 'ele-admin-pro';
    import type { DatasourceFunction, ColumnItem } from 'ele-admin-pro/es/ele-pro-table/types';
    import { listMenus } from '@/api/system/menu';

    // 表格 ref
    const tableRef = ref<InstanceType<typeof EleProTable> | null>(null);

    // 表格列配置
    const columns = ref<ColumnItem[]>([
        {
            key: 'index',
            width: 48,
            align: 'center',
            hideInSetting: true,
            customRender: ({ index }) => index + (tableRef.value?.tableIndex ?? 0)
        },
        {
            title: '菜单名称',
            dataIndex: 'title',
            ellipsis: true
        },
        {
            title: '路由地址',
            dataIndex: 'path',
            ellipsis: true
        }
    ]);

    /* 表格数据源，初始加载时 parent 为 undefined，点击加载子级的时候 parent 为父级数据 */
    const datasource: DatasourceFunction = ({ where, parent }) => {
        return listMenus({ ...where, parentId: parent?.menuId || 0 });
    };
</script>
```

@tab JavaScript

```vue
<template>
    <ele-pro-table
        row-key="menuId"
        :datasource="datasource"
        :columns="columns"
        :lazy-load="true"
        :need-page="false"
        :expand-icon-column-index="1">
    </ele-pro-table>
</template>

<script setup>
    import { ref } from 'vue';
    import { listMenus } from '@/api/system/menu';

    // 表格 ref
    const tableRef = ref(null);

    // 表格列配置
    const columns = ref([
        {
            key: 'index',
            width: 48,
            align: 'center',
            hideInSetting: true,
            customRender: ({ index }) => index + (tableRef.value?.tableIndex ?? 0)
        },
        {
            title: '菜单名称',
            dataIndex: 'title',
            ellipsis: true
        },
        {
            title: '路由地址',
            dataIndex: 'path',
            ellipsis: true
        }
    ]);

    /* 表格数据源，初始加载时 parent 为 undefined，点击加载子级的时候 parent 为父级数据 */
    const datasource = ({ where, parent }) => {
        return listMenus({ ...where, parentId: parent?.menuId || 0 });
    };
</script>
```

:::

懒加载时默认所有级别第一次都会出现展开子级按钮，当点击请求数据后没有子级展开按钮就会自动消失，也可以通过字段控制是否还有子级：

```vue
<template>
    <ele-pro-table
        row-key="menuId"
        :datasource="datasource"
        :columns="columns"
        :lazy-load="true"
        has-children-column-name="hasChildren"
        :need-page="false"
        :expand-icon-column-index="1">
    </ele-pro-table>
</template>
```

增加 `has-children-column-name="hasChildren"` 设置字段名称，然后数据里面有字段 `hasChildren: true` 才会出现展开按钮 `v1.8.0新增`。

<br/>

## 6.2.10. 全局配置  

组件名 `ele-config-provider`，v1.7.0 新增，使用 Vue 的 provide / inject 特性，在 src/App.vue 增加：

::: code-tabs

@tab TypeScript

```vue
<template>
    <ele-config-provider :request="request" :response="response" :table="tableConfig">
        <router-view />
    </ele-config-provider>
</template>

<script lang="ts" setup>
    import { reactive } from 'vue';
    import type { RequestOption, ResponseOption } from 'ele-admin-pro/es/ele-pro-table/types';
    import type { TableGlobalConfig } from 'ele-admin-pro/es/ele-config-provider/types';

    const request = reactive<RequestOption>({
        sortName: 'sort',  // 排序字段参数名称
        orderName: 'order'  // 排序方式的参数名称
    });

    const response = reactive<ResponseOption>({
        dataName: 'list',  // 数据列表的字段名称
        countName: 'count'  // 数据总数的字段名称
    });

    // table 属性是 v1.8.0 新增
    const tableConfig = reactive<TableGlobalConfig>({
        ascendValue: 'asc',  // 排序方式升序时的值
        descendValue: 'desc',  // 排序方式降序时的值
        size: 'small',  // 表格尺寸
    });
</script>
```

@tab JavaScript

```vue
<template>
    <ele-config-provider :request="request" :response="response" :table="tableConfig">
        <router-view />
    </ele-config-provider>
</template>

<script setup>
    import { reactive } from 'vue';

    const request = reactive({
        sortName: 'sort',  // 排序字段参数名称
        orderName: 'order'  // 排序方式的参数名称
    });

    const response = reactive({
        dataName: 'list',  // 数据列表的字段名称
        countName: 'count'  // 数据总数的字段名称
    });

    // table 属性是 v1.8.0 新增
    const tableConfig = reactive({
        ascendValue: 'asc',  // 排序方式升序时的值
        descendValue: 'desc',  // 排序方式降序时的值
        size: 'small',  // 表格尺寸
    });
</script>
```

:::

v1.6.0 版本全局配置方式如下，v1.5.0 版本全局配置请到 6.2.12 查看：

```vue
<script lang="ts" setup>
    import { provide } from 'vue';
    import type { RequestOption, ResponseOption } from 'ele-admin-pro/es/ele-pro-table/types';

    provide<RequestOption>('tableRequest', {
        sortName: 'sort',  // 排序字段参数名称
        orderName: 'order'  // 排序方式的参数名称
    });

    provide<ResponseOption>('tableResponse', {
        dataName: 'list',  // 数据列表的字段名称
        countName: 'count'  // 数据总数的字段名称
    });
</script>
```

<br/>

## 6.2.11. 前端排序  

当使用 Function 数据源后端分页时如果仍想前端只排序当前页，增加 `:reload-on-change="false"` (v1.7.0 新增) ：

::: code-tabs

@tab TypeScript

```vue
<template>
    <ele-pro-table
        ref="tableRef"
        row-key="userId"
        :columns="columns"
        :datasource="datasource"
        :reload-on-change="false"
        @change="onChange"
    >
    </ele-pro-table>
</template>

<script lang="ts" setup>
    import { ref } from 'vue';
    import type { EleProTable } from 'ele-admin-pro';
    import type {
        DatasourceFunction,
        ColumnItem,
        FilterType,
        SorterType
    } from 'ele-admin-pro/es/ele-pro-table/types';
    import type { TablePaginationConfig, TableCurrentDataSource } from 'ant-design-vue/es/table/interface';
    import { pageUsers } from '@/api/system/user';
    import type { User } from '@/api/system/user/model';

    // 表格 ref
    const tableRef = ref<InstanceType<typeof EleProTable> | null>(null);

    // 表格列配置，列配置中的 `sorter` 需要为一个排序函数而不是 `true` 
    const columns = ref<ColumnItem[]>([
        {
            title: '用户账号',
            dataIndex: 'username',
            sorter: (a, b) => {
                return a.username == b.username ? 0 : a.username < b.username ? 1 : -1;
            }
        },
        {
            title: '用户名',
            dataIndex: 'nickname',
            sorter: (a, b) => {
                return a.nickname == b.nickname ? 0 : a.nickname < b.nickname ? 1 : -1;
            }
        },
        {
            title: '状态',
            dataIndex: 'status',
            sorter: (a, b) => {
                return a.status == b.status ? 0 : a.status < b.status ? 1 : -1;
            }
        }
    ]);

    /* 表格数据源 */
    const datasource: DatasourceFunction = ({ page, limit, where }) => {
        return pageUsers({ ...where, page, limit });
    };

    /* 分页、排序、筛选变化时触发 */
    let lastPage = 1;
    let lastLimit = 10;
    const onChange = (
        pagination: TablePaginationConfig,
        filters: FilterType,
        sorter: SorterType,
        extra: TableCurrentDataSource
    ) => {
        // 这里判断如果是分页改变则调用 reload 重新请求
        if (pagination.current !== lastPage || pagination.pageSize !== lastLimit) {
            lastPage = pagination.current;
            lastLimit = pagination.pageSize;
            tableRef?.value?.reload();
        }
        console.log(filters, sorter, extra);
    };
</script>
```

@tab JavaScript

```vue
<template>
    <ele-pro-table
        ref="tableRef"
        row-key="userId"
        :columns="columns"
        :datasource="datasource"
        :reload-on-change="false"
        @change="onChange"
    >
    </ele-pro-table>
</template>

<script setup>
    import { ref } from 'vue';
    import { pageUsers } from '@/api/system/user';

    // 表格 ref
    const tableRef = ref(null);

    // 表格列配置，列配置中的 `sorter` 需要为一个排序函数而不是 `true` 
    const columns = ref([
        {
            title: '用户账号',
            dataIndex: 'username',
            sorter: (a, b) => {
                return a.username == b.username ? 0 : a.username < b.username ? 1 : -1;
            }
        },
        {
            title: '用户名',
            dataIndex: 'nickname',
            sorter: (a, b) => {
                return a.nickname == b.nickname ? 0 : a.nickname < b.nickname ? 1 : -1;
            }
        },
        {
            title: '状态',
            dataIndex: 'status',
            sorter: (a, b) => {
                return a.status == b.status ? 0 : a.status < b.status ? 1 : -1;
            }
        }
    ]);

    /* 表格数据源 */
    const datasource = ({ page, limit, where }) => {
        return pageUsers({ ...where, page, limit });
    };

    /* 分页、排序、筛选变化时触发 */
    let lastPage = 1;
    let lastLimit = 10;
    const onChange = (pagination, filters, sorter, extra) => {
        // 这里判断如果是分页改变则调用 reload 重新请求
        if (pagination.current !== lastPage || pagination.pageSize !== lastLimit) {
            lastPage = pagination.current;
            lastLimit = pagination.pageSize;
            tableRef?.value?.reload();
        }
        console.log(filters, sorter, extra);
    };
</script>
```

:::

因为 ATable 的分页、排序、筛选等是一个事件 `change`，所以不太好封装这个需求，需要自己判断除了排序改变外仍调用后端请求。

<br/>

## 6.2.12. 表头工具栏  

组件名 `ele-toolbar`，v1.6.0 新增，高级表格的表头工具栏使用的就是该组件，当然你也可以独立使用此组件，使用方式：

```vue
<template>
    <ele-toolbar title="我是标题">
        <!-- 默认插槽添加左边内容 -->
        <a-button type="primary">添加</a-button>
        <a-button type="danger">删除</a-button>
        <!-- action 插槽添加右边内容 -->
        <template #action>
            <a-space class="ele-tool">
                <!-- 右侧图标按钮, ele-tool-item 也是 v1.6.0 新增 -->
                <ele-tool-item title="我是按钮" @click="onClick">
                    <plus-outlined />
                </ele-tool-item>
            </a-space>
        </template>
    </ele-toolbar>
</template>

<script setup>
    import { PlusOutlined } from '@ant-design/icons-vue';

    const onClick = () => {
        console.log('Hello');
    };
</script>
```

属性列表：

| 属性     | 说明                         | 类型   | 默认值 |
| :------- | :--------------------------- | :----- | :----- |
| title    | 标题                         | String |        |
| subTitle | 二级标题                     | String |        |
| theme    | 主题，可选 'none'、'default' | String | 'none' |

主题为 'default' 会有背景色，图标按钮有边框，`ele-tool-item` 组件可以通过默认插槽加图标，支持事件有 `click` ，支持属性有：

| 属性      | 说明                      | 类型    | 默认值 |
| :-------- | :------------------------ | :------ | :----- |
| title     | tooltip 的 title 属性     | String  |        |
| placement | tooltip 的 placement 属性 | String  |        |
| disabled  | 是否禁用 tooltip          | Boolean | false  |

对于 v1.5.0 以及之前的版本可以通过加 class 来实现与默认工具按钮外观一致的图标按钮：

```vue
<template>
    <div class="ele-tool-item">
        <a-tooltip title="我是按钮">
            <plus-outlined @click="alert('Hello')"/>
        </a-tooltip>
    </div>
</template>
```

<br/>

## 6.2.13. 旧版数据源  

  v1.5.0 以及之前的版本 datasource 可以为 string 类型直接配置后端的接口地址，最初是为了模仿 layui 表格的用法， 但主流的 vue 框架都习惯将请求的代码写在 api 中，再加上 Vue3 的 CompositionApi 不方便使用 this.$http， 所以新版本移除了 url 数据源的方式，推荐使用 Function 数据源的方式。

```vue
<template>
    <ele-pro-table ref="table" row-key="userId" :datasource="url" :columns="columns"></ele-pro-table>
</template>

<script>
    export default {
        data() {
            return {
                // 表格数据接口, datasource 为 String 类型就是配置后端接口地址
                url: '/sys/user/page',
                // 表格列配置
                columns: [
                    {
                        key: 'index',
                        width: 48,
                        customRender: ({ index }) => this.$refs.table.tableIndex + index
                    },
                    {
                        title: '用户账号',
                        dataIndex: 'username',
                        sorter: true
                    },
                    {
                        title: '用户名',
                        dataIndex: 'nickname',
                        sorter: true
                    },
                    {
                        title: '手机号',
                        dataIndex: 'phone',
                        sorter: true,
                        // 加此参数可让列默认隐藏，v1.6.0 之后改名为 hideInTable: true
                        show: false
                    },
                    {
                        title: '创建时间',
                        dataIndex: 'createTime',
                        sorter: true,
                        // 可以通过 customRender 方法格式列显示数据，还可以通过插槽渲染
                        customRender: ({ text }) => {
                            return this.$util.toDateString(text);
                        }
                    }
                ]
            };
        }
    }
</script>
```

使用 url 数据源时你还可以通过 request 和 response 属性设置接口的数据格式：

```vue
<template>
    <ele-pro-table :request="request" :response="response"></ele-pro-table>
</template>

<script>
    export default {
        data() {
            return {
                request: {
                    pageName: 'page',  // 页码的参数名称
                    limitName: 'limit',  // 每页数据量的参数名
                    sortName: 'sort',  // 排序字段参数名称
                    orderName: 'order'  // 排序方式的参数名称
                },
                response: {
                    statusName: 'code',  // 数据状态的字段名称，支持嵌套写法(xxx.xxx)
                    statusCode: 0,  // 成功的状态码，例如改成200
                    msgName: 'msg',  // 信息的字段名称，支持嵌套
                    dataName: 'data',  // 数据列表的字段名称，支持嵌套，例如：result.list
                    countName: 'count'  // 数据总数的字段名称，支持嵌套，例如：result.total
                }
            };
        }
    }
</script>
```

还提供了 parseParam 和 parseData 属性同样用于更复杂的处理接口数据格式：

```vue
<template>
    <ele-pro-table :parse-param="parseParam" :parse-data="parseData"></ele-pro-table>
</template>

<script>
    export default {
        methods: {
            parseParam(param) {
                // param即请求接口的参数(分页、排序、where等), 可以根据自己需要调整格式
                console.log(param);
                return param;
                // 例如写 return { ...param, pageNum: param.page };
            },
            // 后端接口默认需要返回的数据格式为：{"code": 0, "data": [{}, {}], "count": 10, "msg": ""}
            parseData(res) {
                return {
                    // 默认 code 为 0 是成功，比如接口 code: 200 是成功，可以这样改为 0
                    code: res.code === 200 ? 0 : res.code,
                    // 比如接口是 list 是数据，可以这样改为 data，如多是多层嵌套比如这样写：res.result.list
                    data: res.list,
                    // 比如接口 total 是总数量，可以这样改为 count
                    count: res.total,
                    msg: res.message
                };
            }
        }
    }
</script>
```

**contentType** 参数，如果是 `POST` 请求方式，参数默认为 json 格式提交，如需表单提交可以这样配置：

```vue
<template>
    <ele-pro-table 
        :url="url" 
        method="POST" 
        content-type="multipart/form-data">
    </ele-pro-table>
</template>
```

全局设置 request、response 参数，只适用于 v1.6.0 之前的版本，在 main.js 中的 use EleAdminPro 时设置：

```javascript
app.use(EleAdminPro, {
    response: {
        statusName: 'code',
        statusCode: 200
    },
    request: {
        pageName: 'current',
        limitName: 'size',
    }
});
```

<br/>

## 6.2.14. 旧版列插槽 

v1.5.0 以及的版本使用的是 AntDesignVue 2.x ，表格自定义列模板的插槽使用方式与新版完全不一样，之前的使用方式为：

```vue
<template>
    <ele-pro-table ref="table" row-key="userId" :datasource="url" :columns="columns">
        <!-- 表头工具栏 -->
        <template #toolbar>
            <a-button type="primary">添加</a-button>
            <a-button type="danger">删除</a-button>
        </template>
        <!-- 状态列 -->
        <template #state="{ record }">
            <a-switch 
                :checked="record.state===0" 
                @change="(checked) => editState(checked, record)"/>
        </template>
        <!-- 操作列 -->
        <template #action="{ record }">
            <a @click="openEdit(record)">修改</a>
            <a-popconfirm title="确定要删除此用户吗？" @confirm="remove(record)">
                <a>删除</a>
            </a-popconfirm>
        </template>
    </ele-pro-table>
</template>

<script>
    export default {
        data() {
            return {
                // 表格数据接口
                url: '/sys/user/page',
                // 表格列配置
                columns: [
                    {
                      key: 'index',
                      width: 48,
                      customRender: ({index}) => this.$refs.table.tableIndex + index
                    },
                    {
                      title: '用户账号',
                      dataIndex: 'username'
                    },
                    {
                      title: '用户名',
                      dataIndex: 'nickname'
                    },
                    {
                      title: '创建时间',
                      dataIndex: 'createTime',
                      customRender: ({ text }) => this.$util.toDateString(text)
                    },
                    {
                      title: '状态',
                      dataIndex: 'state',
                      slots: { customRender: 'state' }
                    },
                    {
                      title: '操作',
                      key: 'action',
                      slots: { customRender: 'action' }
                    }
                ]
            };
        }
    }
</script>
```

在列配置中增加 `slots` 参数并配置插槽名称， 然后在 template 中增加对应的具名插槽，也可以像创建时间那样通过 `customRender` 方法自定义。