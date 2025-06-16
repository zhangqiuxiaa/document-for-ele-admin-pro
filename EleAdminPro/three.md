---
title: 快速上手
createTime: 2025/01/10 09:30:40
permalink: /EleAdminPro/73uv5lye/
---
## 3.1. 增加一个页面  

在 `src/views/` 下面新建一个文件夹 `/demo/test/` ，然后在里面新建一个 `index.vue` ：

::: code-tabs

@tab TypeScript

```vue
<template>
    <div class="ele-body">
        <a-card :bordered="false">
            <div>Test</div>
        </a-card>
    </div>
</template>

<script lang="ts" setup>
</script>

<script lang="ts">
    export default {
        name: 'DemoTest'
    };
</script>
```

@tab JavaScript

```vue
<template>
    <div class="ele-body">
        <a-card :bordered="false">
            <div>Test</div>
        </a-card>
    </div>
</template>

<script setup>
</script>

<script>
    export default {
        name: 'DemoTest'
    };
</script>
```

:::

![](/images/3-1.png)

如果购买了配套后端可以在后端中添加创建的菜单，没有的话可以在前端直接指定菜单数据，修改 `src/config/setting.ts` 中的 `USER_MENUS` ：

```javascript
export const USER_MENUS = [
    {
        path: '/system',
        redirect: '/system/user',
        meta: {title: '系统管理', icon: 'setting-outlined'},
        children: [
            {
                path: '/system/user',
                component: '/system/user',
                meta: {title: '用户管理', icon: 'team-outlined'}
            }
        ]
    },
    {
        path: '/demo',
        redirect: '/demo/test',
        meta: {title: '演示', icon: 'setting-outlined'},
        children: [
            {
                path: '/demo/test',
                component: '/demo/test',
                meta: {title: '测试页面', icon: 'team-outlined'}
            }
        ]
    }
];
```

![](/images/3-2.png)

然后运行项目，访问 http://localhost:5173/demo/test 就可以显示刚才添加的页面了。

![](/images/3-3.png)

<br/>

## 3.2. 实现表格查询  

在刚才添加的页面里面继续添加表格 `ele-pro-table` 和搜索表单的代码：

::: code-tabs

@tab TypeScript

```vue
<template>
    <div class="ele-body">
        <a-card :bordered="false">
            <div>Test</div>
            <!-- 搜索表单 -->
            <a-form
                :label-col="{ md: { span: 6 }, sm: { span: 24 } }"
                :wrapper-col="{ md: { span: 18 }, sm: { span: 24 } }"
            >
                <a-row>
                    <a-col :lg="6" :md="12" :sm="24" :xs="24">
                        <a-form-item label="用户账号">
                            <a-input
                                v-model:value.trim="where.username"
                                placeholder="请输入"
                                allow-clear
                            />
                        </a-form-item>
                    </a-col>
                    <a-col :lg="6" :md="12" :sm="24" :xs="24">
                        <a-form-item label="性别">
                            <a-select v-model:value="where.sex" placeholder="请选择" allow-clear>
                                <a-select-option value="1">男</a-select-option>
                                <a-select-option value="2">女</a-select-option>
                            </a-select>
                        </a-form-item>
                    </a-col>
                    <a-col :lg="6" :md="12" :sm="24" :xs="24">
                        <a-form-item :wrapper-col="{ span: 24 }">
                            <em></em>
                            <a-space>
                                <a-button type="primary" @click="reload">查询</a-button>
                                <a-button @click="reset">重置</a-button>
                            </a-space>
                        </a-form-item>
                    </a-col>
                </a-row>
            </a-form>
            <!-- 表格 -->
            <ele-pro-table
                ref="tableRef"
                row-key="userId"
                :columns="columns"
                :datasource="datasource"
                :scroll="{ x: 1000 }"
            >
                <!--  -->
            </ele-pro-table>
        </a-card>
    </div>
</template>

<script lang="ts" setup>
    import { ref } from 'vue';
    import type { EleProTable } from 'ele-admin-pro';
    import type { DatasourceFunction, ColumnItem } from 'ele-admin-pro/es/ele-pro-table/types';
    import { toDateString } from 'ele-admin-pro';
    import { pageUsers } from '@/api/system/user';
    import type { UserParam } from '@/api/system/user/model';
    import useFormData from '@/utils/use-form-data';

    // 表格实例
    const tableRef = ref<InstanceType<typeof EleProTable> | null>(null);

    // 表格列配置
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
            dataIndex: 'nickname',
            sorter: true
        },
        {
            title: '创建时间',
            dataIndex: 'createTime',
            sorter: true,
            ellipsis: true,
            customRender: ({ text }) => toDateString(text)
        }
    ]);

    // 表格数据源
    const datasource: DatasourceFunction = ({ page, limit, where, orders }) => {
        return pageUsers({ ...where, ...orders, page, limit });
    };

    // 表单数据
    const { form: where, resetFields } = useFormData<UserParam>({
        username: '',
        sex: undefined
    });

    /* 搜索 */
    const reload = () => {
        tableRef?.value?.reload({ page: 1, where: where });
    };

    /*  重置 */
    const reset = () => {
        resetFields();
        reload();
    };
</script>

<script lang="ts">
    export default {
        name: 'DemoTest'
    };
</script>
```

@tab JavaScript

```vue
<template>
    <div class="ele-body">
        <a-card :bordered="false">
            <div>Test</div>
            <!-- 搜索表单 -->
            <a-form
                :label-col="{ md: { span: 6 }, sm: { span: 24 } }"
                :wrapper-col="{ md: { span: 18 }, sm: { span: 24 } }"
            >
                <a-row>
                    <a-col :lg="6" :md="12" :sm="24" :xs="24">
                        <a-form-item label="用户账号">
                            <a-input
                                v-model:value.trim="where.username"
                                placeholder="请输入"
                                allow-clear
                            />
                        </a-form-item>
                    </a-col>
                    <a-col :lg="6" :md="12" :sm="24" :xs="24">
                        <a-form-item label="性别">
                            <a-select v-model:value="where.sex" placeholder="请选择" allow-clear>
                                <a-select-option value="1">男</a-select-option>
                                <a-select-option value="2">女</a-select-option>
                            </a-select>
                        </a-form-item>
                    </a-col>
                    <a-col :lg="6" :md="12" :sm="24" :xs="24">
                        <a-form-item :wrapper-col="{ span: 24 }">
                            <em></em>
                            <a-space>
                                <a-button type="primary" @click="reload">查询</a-button>
                                <a-button @click="reset">重置</a-button>
                            </a-space>
                        </a-form-item>
                    </a-col>
                </a-row>
            </a-form>
            <!-- 表格 -->
            <ele-pro-table
                ref="tableRef"
                row-key="userId"
                :columns="columns"
                :datasource="datasource"
                :scroll="{ x: 1000 }"
            >
                <!--  -->
            </ele-pro-table>
        </a-card>
    </div>
</template>

<script setup>
    import { ref } from 'vue';
    import { toDateString } from 'ele-admin-pro';
    import { pageUsers } from '@/api/system/user';
    import useFormData from '@/utils/use-form-data';

    // 表格实例
    const tableRef = ref(null);

    // 表格列配置
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
            dataIndex: 'nickname',
            sorter: true
        },
        {
            title: '创建时间',
            dataIndex: 'createTime',
            sorter: true,
            ellipsis: true,
            customRender: ({ text }) => toDateString(text)
        }
    ]);

    // 表格数据源
    const datasource = ({ page, limit, where, orders }) => {
        return pageUsers({ ...where, ...orders, page, limit });
    };

    // 表单数据
    const { form: where, resetFields } = useFormData({
        username: '',
        sex: undefined
    });

    /* 搜索 */
    const reload = () => {
        tableRef?.value?.reload({ page: 1, where: where });
    };

    /*  重置 */
    const reset = () => {
        resetFields();
        reload();
    };
</script>

<script>
    export default {
        name: 'DemoTest'
    };
</script>
```

:::

`ele-pro-table` 是封装的表格在文档 6.2 ，可以很方便的实现数据展示，数据源直接使用了模板中用户管理的 api，实际项目中可以改为自己接口。

![img](https://cdn.eleadmin.com/20210217/jkbCOP.png)

<br/>

## 3.3. 实现删除功能  

继续给表格添加一列操作按钮实现删除功能，在 `columns` 中添加操作列，并通过 `bodyCell` 插槽添加自定义列：

:::code-tabs

@tab TypeScript

```vue
<template>
    <div class="ele-body">
        <a-card :bordered="false">
            <div>Test</div>
            <!-- 搜索表单 -->
            <a-form
                :label-col="{ md: { span: 6 }, sm: { span: 24 } }"
                :wrapper-col="{ md: { span: 18 }, sm: { span: 24 } }"
            >
                <a-row>
                    <a-col :lg="6" :md="12" :sm="24" :xs="24">
                        <a-form-item label="用户账号">
                            <a-input
                                v-model:value.trim="where.username"
                                placeholder="请输入"
                                allow-clear
                            />
                        </a-form-item>
                    </a-col>
                    <a-col :lg="6" :md="12" :sm="24" :xs="24">
                        <a-form-item label="性别">
                            <a-select v-model:value="where.sex" placeholder="请选择" allow-clear>
                                <a-select-option value="1">男</a-select-option>
                                <a-select-option value="2">女</a-select-option>
                            </a-select>
                        </a-form-item>
                    </a-col>
                    <a-col :lg="6" :md="12" :sm="24" :xs="24">
                        <a-form-item :wrapper-col="{ span: 24 }">
                            <em></em>
                            <a-space>
                                <a-button type="primary" @click="reload">查询</a-button>
                                <a-button @click="reset">重置</a-button>
                            </a-space>
                        </a-form-item>
                    </a-col>
                </a-row>
            </a-form>
            <!-- 表格 -->
            <ele-pro-table
                ref="tableRef"
                row-key="userId"
                :columns="columns"
                :datasource="datasource"
                :scroll="{ x: 1000 }"
            >
                <template #toolbar>
                    <a-space>
                        <a-button type="primary" @click="openEdit()">
                            <template #icon>
                                <plus-outlined />
                            </template>
                            <span>新建</span>
                        </a-button>
                    </a-space>
                </template>
                <template #bodyCell="{ column, record }">
                    <template v-if="column.key === 'action'">
                        <a-space>
                            <a @click="openEdit(record)">修改</a>
                            <a-divider type="vertical" />
                            <a-popconfirm
                                title="确定要删除此用户吗？"
                                @confirm="remove(record)"
                            >
                                <a class="ele-text-danger">删除</a>
                            </a-popconfirm>
                        </a-space>
                    </template>
                </template>
            </ele-pro-table>
        </a-card>
        <!-- 用户添加、修改弹窗 -->
        <a-modal
            :width="460"
            v-model:visible="visible"
            :confirm-loading="loading"
            :title="isUpdate ? '修改用户' : '新建用户'"
            :body-style="{ paddingBottom: '8px' }"
            @ok="save"
        >
            <a-form
                ref="formRef"
                :model="form"
                :rules="rules"
                :label-col="{ md: { span: 7 }, sm: { span: 24 } }"
                :wrapper-col="{ md: { span: 17 }, sm: { span: 24 } }"
            >
                <a-form-item label="用户账号" name="username">
                    <a-input
                        allow-clear
                        :maxlength="20"
                        placeholder="请输入用户账号"
                        v-model:value="form.username"
                    />
                </a-form-item>
                <a-form-item label="用户名" name="nickname">
                    <a-input
                        allow-clear
                        :maxlength="20"
                        placeholder="请输入用户名"
                        v-model:value="form.nickname"
                    />
                </a-form-item>
                <a-form-item label="性别" name="sex">
                    <a-select
                        allow-clear
                        v-model:value="form.sex"
                        placeholder="请选择性别"
                    >
                        <a-select-option value="1">男</a-select-option>
                        <a-select-option value="2">女</a-select-option>
                    </a-select>
                </a-form-item>
            </a-form>
        </a-modal>
    </div>
</template>

<script lang="ts" setup>
    import { ref, reactive } from 'vue';
    import type { EleProTable } from 'ele-admin-pro';
    import type { DatasourceFunction, ColumnItem } from 'ele-admin-pro/es/ele-pro-table/types';
    import { toDateString } from 'ele-admin-pro';
    import { pageUsers, removeUser, addUser, updateUser } from '@/api/system/user';
    import type { User, UserParam } from '@/api/system/user/model';
    import useFormData from '@/utils/use-form-data';
    import { message } from 'ant-design-vue';
    import type { FormInstance, Rule } from 'ant-design-vue/es/form';

    // 表格实例
    const tableRef = ref<InstanceType<typeof EleProTable> | null>(null);

    // 表格列配置
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
            dataIndex: 'nickname',
            sorter: true
        },
        {
            title: '创建时间',
            dataIndex: 'createTime',
            sorter: true,
            ellipsis: true,
            customRender: ({ text }) => toDateString(text)
        },
        {
            title: '操作',
            key: 'action',
            width: 200,
            align: 'center',
            hideInSetting: true
        }
    ]);

    // 表格数据源
    const datasource: DatasourceFunction = ({ page, limit, where, orders }) => {
        return pageUsers({ ...where, ...orders, page, limit });
    };

    // 表单数据
    const { form: where, resetFields } = useFormData<UserParam>({
        username: '',
        sex: undefined
    });

    /* 搜索 */
    const reload = () => {
        tableRef?.value?.reload({ page: 1, where: where });
    };

    /*  重置 */
    const reset = () => {
        resetFields();
        reload();
    };
    
    /* 删除单个 */
    const remove = (row: User) => {
        const hide = message.loading('请求中..', 0);
        removeUser(row.userId).then((msg) => {
            hide();
            message.success(msg);
            reload();
        }).catch((e) => {
            hide();
            message.error(e.message);
        });
    };

    // 当前选中数据
    const current = ref<User>();

    // 弹窗是否打开
    const visible = ref(false);

    // 是否是修改
    const isUpdate = ref(false);

    // 提交状态
    const loading = ref(false);

    //
    const formRef = ref<FormInstance | null>(null);

    // 表单数据
    const { form, resetFields: resetFormFields } = useFormData<User>({
        userId: undefined,
        username: '',
        nickname: '',
        sex: undefined
    });

    // 表单验证规则
    const rules = reactive<Record<string, Rule[]>>({
        username: [
            {
                required: true,
                message: '请输入用户账号',
                type: 'string',
                trigger: 'blur'
            }
        ],
        nickname: [
            {
                required: true,
                message: '请输入用户名',
                type: 'string',
                trigger: 'blur'
            }
        ],
        sex: [
            {
                required: true,
                message: '请选择性别',
                type: 'string',
                trigger: 'blur'
            }
        ]
    });

    /* 打开编辑弹窗 */
    const openEdit = (record?: User) => {
        resetFormFields();
        formRef.value?.clearValidate();
        current.value = record;
        visible.value = true;
        Object.assign(form, record ?? {});
        isUpdate.value = !!record;
    };

    /* 保存编辑 */
    const save = () => {
        if (!formRef.value) {
            return;
        }
        formRef.value.validate().then(() => {
            loading.value = true;
            const saveOrUpdate = isUpdate.value ? updateUser : addUser;
            saveOrUpdate(form).then((msg: string) => {
                loading.value = false;
                message.success(msg);
                visible.value = false;
                reload();
            }).catch((e: Error) => {
                loading.value = false;
                message.error(e.message);
            });
        }).catch(() => {
        });
    };
</script>

<script lang="ts">
    export default {
        name: 'DemoTest'
    };
</script>
```

@tab JavaScript

```vue
<template>
    <div class="ele-body">
        <a-card :bordered="false">
            <div>Test</div>
            <!-- 搜索表单 -->
            <a-form
                :label-col="{ md: { span: 6 }, sm: { span: 24 } }"
                :wrapper-col="{ md: { span: 18 }, sm: { span: 24 } }"
            >
                <a-row>
                    <a-col :lg="6" :md="12" :sm="24" :xs="24">
                        <a-form-item label="用户账号">
                            <a-input
                                v-model:value.trim="where.username"
                                placeholder="请输入"
                                allow-clear
                            />
                        </a-form-item>
                    </a-col>
                    <a-col :lg="6" :md="12" :sm="24" :xs="24">
                        <a-form-item label="性别">
                            <a-select v-model:value="where.sex" placeholder="请选择" allow-clear>
                                <a-select-option value="1">男</a-select-option>
                                <a-select-option value="2">女</a-select-option>
                            </a-select>
                        </a-form-item>
                    </a-col>
                    <a-col :lg="6" :md="12" :sm="24" :xs="24">
                        <a-form-item :wrapper-col="{ span: 24 }">
                            <em></em>
                            <a-space>
                                <a-button type="primary" @click="reload">查询</a-button>
                                <a-button @click="reset">重置</a-button>
                            </a-space>
                        </a-form-item>
                    </a-col>
                </a-row>
            </a-form>
            <!-- 表格 -->
            <ele-pro-table
                ref="tableRef"
                row-key="userId"
                :columns="columns"
                :datasource="datasource"
                :scroll="{ x: 1000 }"
            >
                <template #bodyCell="{ column, record }">
                    <template v-if="column.key === 'action'">
                        <a-space>
                            <a-popconfirm
                                title="确定要删除此用户吗？"
                                @confirm="remove(record)"
                            >
                                <a class="ele-text-danger">删除</a>
                            </a-popconfirm>
                        </a-space>
                    </template>
                </template>
            </ele-pro-table>
        </a-card>
    </div>
</template>

<script setup>
    import { ref } from 'vue';
    import { toDateString } from 'ele-admin-pro';
    import { pageUsers, removeUser } from '@/api/system/user';
    import useFormData from '@/utils/use-form-data';
    import { message } from 'ant-design-vue';

    // 表格实例
    const tableRef = ref(null);

    // 表格列配置
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
            dataIndex: 'nickname',
            sorter: true
        },
        {
            title: '创建时间',
            dataIndex: 'createTime',
            sorter: true,
            ellipsis: true,
            customRender: ({ text }) => toDateString(text)
        },
        {
            title: '操作',
            key: 'action',
            width: 200,
            align: 'center',
            hideInSetting: true
        }
    ]);

    // 表格数据源
    const datasource = ({ page, limit, where, orders }) => {
        return pageUsers({ ...where, ...orders, page, limit });
    };

    // 表单数据
    const { form: where, resetFields } = useFormData({
        username: '',
        sex: undefined
    });

    /* 搜索 */
    const reload = () => {
        tableRef?.value?.reload({ page: 1, where: where });
    };

    /*  重置 */
    const reset = () => {
        resetFields();
        reload();
    };
    
    /* 删除单个 */
    const remove = (record) => {
        const hide = message.loading('请求中..', 0);
        removeUser(record.userId).then((msg) => {
            hide();
            message.success(msg);
            reload();
        }).catch((e) => {
            hide();
            message.error(e.message);
        });
    };
</script>

<script>
    export default {
        name: 'DemoTest'
    };
</script>
```

:::

点击删除调用 `removeUser` 的 api 请求删除接口，用演示接口会提示没有权限，实际项目可以换成自己的接口。

![](/images/3-4.png)

<br/>

## 3.4. 实现新增修改  

通过插槽 `toolbar` 在表头增加添加按钮，在删除按钮的旁边增加修改按钮，再增加表单弹窗实现添加和修改功能：

::: code-tabs

@tab TypeScript

```vue
<template>
    <div class="ele-body">
        <a-card :bordered="false">
            <div>Test</div>
            <!-- 搜索表单 -->
            <a-form
                :label-col="{ md: { span: 6 }, sm: { span: 24 } }"
                :wrapper-col="{ md: { span: 18 }, sm: { span: 24 } }"
            >
                <a-row>
                    <a-col :lg="6" :md="12" :sm="24" :xs="24">
                        <a-form-item label="用户账号">
                            <a-input
                                v-model:value.trim="where.username"
                                placeholder="请输入"
                                allow-clear
                            />
                        </a-form-item>
                    </a-col>
                    <a-col :lg="6" :md="12" :sm="24" :xs="24">
                        <a-form-item label="性别">
                            <a-select v-model:value="where.sex" placeholder="请选择" allow-clear>
                                <a-select-option value="1">男</a-select-option>
                                <a-select-option value="2">女</a-select-option>
                            </a-select>
                        </a-form-item>
                    </a-col>
                    <a-col :lg="6" :md="12" :sm="24" :xs="24">
                        <a-form-item :wrapper-col="{ span: 24 }">
                            <em></em>
                            <a-space>
                                <a-button type="primary" @click="reload">查询</a-button>
                                <a-button @click="reset">重置</a-button>
                            </a-space>
                        </a-form-item>
                    </a-col>
                </a-row>
            </a-form>
            <!-- 表格 -->
            <ele-pro-table
                ref="tableRef"
                row-key="userId"
                :columns="columns"
                :datasource="datasource"
                :scroll="{ x: 1000 }"
            >
                <template #toolbar>
                    <a-space>
                        <a-button type="primary" @click="openEdit()">
                            <template #icon>
                                <plus-outlined />
                            </template>
                            <span>新建</span>
                        </a-button>
                    </a-space>
                </template>
                <template #bodyCell="{ column, record }">
                    <template v-if="column.key === 'action'">
                        <a-space>
                            <a @click="openEdit(record)">修改</a>
                            <a-divider type="vertical" />
                            <a-popconfirm
                                title="确定要删除此用户吗？"
                                @confirm="remove(record)"
                            >
                                <a class="ele-text-danger">删除</a>
                            </a-popconfirm>
                        </a-space>
                    </template>
                </template>
            </ele-pro-table>
        </a-card>
        <!-- 用户添加、修改弹窗 -->
        <a-modal
            :width="460"
            v-model:visible="visible"
            :confirm-loading="loading"
            :title="isUpdate ? '修改用户' : '新建用户'"
            :body-style="{ paddingBottom: '8px' }"
            @ok="save"
        >
            <a-form
                ref="formRef"
                :model="form"
                :rules="rules"
                :label-col="{ md: { span: 7 }, sm: { span: 24 } }"
                :wrapper-col="{ md: { span: 17 }, sm: { span: 24 } }"
            >
                <a-form-item label="用户账号" name="username">
                    <a-input
                        allow-clear
                        :maxlength="20"
                        placeholder="请输入用户账号"
                        v-model:value="form.username"
                    />
                </a-form-item>
                <a-form-item label="用户名" name="nickname">
                    <a-input
                        allow-clear
                        :maxlength="20"
                        placeholder="请输入用户名"
                        v-model:value="form.nickname"
                    />
                </a-form-item>
                <a-form-item label="性别" name="sex">
                    <a-select
                        allow-clear
                        v-model:value="form.sex"
                        placeholder="请选择性别"
                    >
                        <a-select-option value="1">男</a-select-option>
                        <a-select-option value="2">女</a-select-option>
                    </a-select>
                </a-form-item>
            </a-form>
        </a-modal>
    </div>
</template>

<script lang="ts" setup>
    import { ref, reactive } from 'vue';
    import type { EleProTable } from 'ele-admin-pro';
    import type { DatasourceFunction, ColumnItem } from 'ele-admin-pro/es/ele-pro-table/types';
    import { toDateString } from 'ele-admin-pro';
    import { pageUsers, removeUser, addUser, updateUser } from '@/api/system/user';
    import type { User, UserParam } from '@/api/system/user/model';
    import useFormData from '@/utils/use-form-data';
    import { message } from 'ant-design-vue';
    import type { FormInstance, Rule } from 'ant-design-vue/es/form';

    // 表格实例
    const tableRef = ref<InstanceType<typeof EleProTable> | null>(null);

    // 表格列配置
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
            dataIndex: 'nickname',
            sorter: true
        },
        {
            title: '创建时间',
            dataIndex: 'createTime',
            sorter: true,
            ellipsis: true,
            customRender: ({ text }) => toDateString(text)
        },
        {
            title: '操作',
            key: 'action',
            width: 200,
            align: 'center',
            hideInSetting: true
        }
    ]);

    // 表格数据源
    const datasource: DatasourceFunction = ({ page, limit, where, orders }) => {
        return pageUsers({ ...where, ...orders, page, limit });
    };

    // 表单数据
    const { form: where, resetFields } = useFormData<UserParam>({
        username: '',
        sex: undefined
    });

    /* 搜索 */
    const reload = () => {
        tableRef?.value?.reload({ page: 1, where: where });
    };

    /*  重置 */
    const reset = () => {
        resetFields();
        reload();
    };
    
    /* 删除单个 */
    const remove = (row: User) => {
        const hide = message.loading('请求中..', 0);
        removeUser(row.userId).then((msg) => {
            hide();
            message.success(msg);
            reload();
        }).catch((e) => {
            hide();
            message.error(e.message);
        });
    };

    // 当前选中数据
    const current = ref<User>();

    // 弹窗是否打开
    const visible = ref(false);

    // 是否是修改
    const isUpdate = ref(false);

    // 提交状态
    const loading = ref(false);

    //
    const formRef = ref<FormInstance | null>(null);

    // 表单数据
    const { form, resetFields: resetFormFields } = useFormData<User>({
        userId: undefined,
        username: '',
        nickname: '',
        sex: undefined
    });

    // 表单验证规则
    const rules = reactive<Record<string, Rule[]>>({
        username: [
            {
                required: true,
                message: '请输入用户账号',
                type: 'string',
                trigger: 'blur'
            }
        ],
        nickname: [
            {
                required: true,
                message: '请输入用户名',
                type: 'string',
                trigger: 'blur'
            }
        ],
        sex: [
            {
                required: true,
                message: '请选择性别',
                type: 'string',
                trigger: 'blur'
            }
        ]
    });

    /* 打开编辑弹窗 */
    const openEdit = (record?: User) => {
        resetFormFields();
        formRef.value?.clearValidate();
        current.value = record;
        visible.value = true;
        Object.assign(form, record ?? {});
        isUpdate.value = !!record;
    };

    /* 保存编辑 */
    const save = () => {
        if (!formRef.value) {
            return;
        }
        formRef.value.validate().then(() => {
            loading.value = true;
            const saveOrUpdate = isUpdate.value ? updateUser : addUser;
            saveOrUpdate(form).then((msg: string) => {
                loading.value = false;
                message.success(msg);
                visible.value = false;
                reload();
            }).catch((e: Error) => {
                loading.value = false;
                message.error(e.message);
            });
        }).catch(() => {
        });
    };
</script>

<script lang="ts">
    export default {
        name: 'DemoTest'
    };
</script>
```

@tab JavaScript

```vue
<template>
    <div class="ele-body">
        <a-card :bordered="false">
            <div>Test</div>
            <!-- 搜索表单 -->
            <a-form
                :label-col="{ md: { span: 6 }, sm: { span: 24 } }"
                :wrapper-col="{ md: { span: 18 }, sm: { span: 24 } }"
            >
                <a-row>
                    <a-col :lg="6" :md="12" :sm="24" :xs="24">
                        <a-form-item label="用户账号">
                            <a-input
                                v-model:value.trim="where.username"
                                placeholder="请输入"
                                allow-clear
                            />
                        </a-form-item>
                    </a-col>
                    <a-col :lg="6" :md="12" :sm="24" :xs="24">
                        <a-form-item label="性别">
                            <a-select v-model:value="where.sex" placeholder="请选择" allow-clear>
                                <a-select-option value="1">男</a-select-option>
                                <a-select-option value="2">女</a-select-option>
                            </a-select>
                        </a-form-item>
                    </a-col>
                    <a-col :lg="6" :md="12" :sm="24" :xs="24">
                        <a-form-item :wrapper-col="{ span: 24 }">
                            <em></em>
                            <a-space>
                                <a-button type="primary" @click="reload">查询</a-button>
                                <a-button @click="reset">重置</a-button>
                            </a-space>
                        </a-form-item>
                    </a-col>
                </a-row>
            </a-form>
            <!-- 表格 -->
            <ele-pro-table
                ref="tableRef"
                row-key="userId"
                :columns="columns"
                :datasource="datasource"
                :scroll="{ x: 1000 }"
            >
                <template #toolbar>
                    <a-space>
                        <a-button type="primary" @click="openEdit()">
                            <template #icon>
                                <plus-outlined />
                            </template>
                            <span>新建</span>
                        </a-button>
                    </a-space>
                </template>
                <template #bodyCell="{ column, record }">
                    <template v-if="column.key === 'action'">
                        <a-space>
                            <a @click="openEdit(record)">修改</a>
                            <a-divider type="vertical" />
                            <a-popconfirm
                                title="确定要删除此用户吗？"
                                @confirm="remove(record)"
                            >
                                <a class="ele-text-danger">删除</a>
                            </a-popconfirm>
                        </a-space>
                    </template>
                </template>
            </ele-pro-table>
        </a-card>
        <!-- 用户添加、修改弹窗 -->
        <a-modal
            :width="460"
            v-model:visible="visible"
            :confirm-loading="loading"
            :title="isUpdate ? '修改用户' : '新建用户'"
            :body-style="{ paddingBottom: '8px' }"
            @ok="save"
        >
            <a-form
                ref="formRef"
                :model="form"
                :rules="rules"
                :label-col="{ md: { span: 7 }, sm: { span: 24 } }"
                :wrapper-col="{ md: { span: 17 }, sm: { span: 24 } }"
            >
                <a-form-item label="用户账号" name="username">
                    <a-input
                        allow-clear
                        :maxlength="20"
                        placeholder="请输入用户账号"
                        v-model:value="form.username"
                    />
                </a-form-item>
                <a-form-item label="用户名" name="nickname">
                    <a-input
                        allow-clear
                        :maxlength="20"
                        placeholder="请输入用户名"
                        v-model:value="form.nickname"
                    />
                </a-form-item>
                <a-form-item label="性别" name="sex">
                    <a-select
                        allow-clear
                        v-model:value="form.sex"
                        placeholder="请选择性别"
                    >
                        <a-select-option value="1">男</a-select-option>
                        <a-select-option value="2">女</a-select-option>
                    </a-select>
                </a-form-item>
            </a-form>
        </a-modal>
    </div>
</template>

<script setup>
    import { ref, reactive } from 'vue';
    import { toDateString } from 'ele-admin-pro';
    import { pageUsers, removeUser, addUser, updateUser } from '@/api/system/user';
    import useFormData from '@/utils/use-form-data';
    import { message } from 'ant-design-vue';

    // 表格实例
    const tableRef = ref(null);

    // 表格列配置
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
            dataIndex: 'nickname',
            sorter: true
        },
        {
            title: '创建时间',
            dataIndex: 'createTime',
            sorter: true,
            ellipsis: true,
            customRender: ({ text }) => toDateString(text)
        },
        {
            title: '操作',
            key: 'action',
            width: 200,
            align: 'center',
            hideInSetting: true
        }
    ]);

    // 表格数据源
    const datasource = ({ page, limit, where, orders }) => {
        return pageUsers({ ...where, ...orders, page, limit });
    };

    // 表单数据
    const { form: where, resetFields } = useFormData({
        username: '',
        sex: undefined
    });

    /* 搜索 */
    const reload = () => {
        tableRef?.value?.reload({ page: 1, where: where });
    };

    /*  重置 */
    const reset = () => {
        resetFields();
        reload();
    };
    
    /* 删除单个 */
    const remove = (row) => {
        const hide = message.loading('请求中..', 0);
        removeUser(row.userId).then((msg) => {
            hide();
            message.success(msg);
            reload();
        }).catch((e) => {
            hide();
            message.error(e.message);
        });
    };

    // 当前选中数据
    const current = ref(null);

    // 弹窗是否打开
    const visible = ref(false);

    // 是否是修改
    const isUpdate = ref(false);

    // 提交状态
    const loading = ref(false);

    //
    const formRef = ref(null);

    // 表单数据
    const { form, resetFields: resetFormFields } = useFormData({
        userId: undefined,
        username: '',
        nickname: '',
        sex: undefined
    });

    // 表单验证规则
    const rules = reactive({
        username: [
            {
                required: true,
                message: '请输入用户账号',
                type: 'string',
                trigger: 'blur'
            }
        ],
        nickname: [
            {
                required: true,
                message: '请输入用户名',
                type: 'string',
                trigger: 'blur'
            }
        ],
        sex: [
            {
                required: true,
                message: '请选择性别',
                type: 'string',
                trigger: 'blur'
            }
        ]
    });

    /* 打开编辑弹窗 */
    const openEdit = (record) => {
        resetFormFields();
        formRef.value?.clearValidate();
        current.value = record;
        visible.value = true;
        Object.assign(form, record ?? {});
        isUpdate.value = !!record;
    };

    /* 保存编辑 */
    const save = () => {
        if (!formRef.value) {
            return;
        }
        formRef.value.validate().then(() => {
            loading.value = true;
            const saveOrUpdate = isUpdate.value ? updateUser : addUser;
            saveOrUpdate(form).then((msg) => {
                loading.value = false;
                message.success(msg);
                visible.value = false;
                reload();
            }).catch((e) => {
                loading.value = false;
                message.error(e.message);
            });
        }).catch(() => {
        });
    };
</script>

<script>
    export default {
        name: 'DemoTest'
    };
</script>
```

:::

  特别要注意修改的时候使用 `Object.assign()` 把表格的行数据 `record` 克隆给 `form`， 如果直接让 `form = record` ，会存在引用传递的问题，导致表单弹窗里面输入改变了， 表格行里面也会同步改变，即使没有点击保存也会改变，所以需要避免引用传递， 这里添加和修改用的一个弹窗，用一个变量标识是添加还是修改，还可以参考用户管理部分拆分成组件，更利于后期维护。

![](/images/3-5.png)