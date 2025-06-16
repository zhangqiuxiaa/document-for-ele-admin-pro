---
title: 常用实例
createTime: 2025/01/10 09:30:40
permalink: /EleAdminPro/19hx3vju/
---
## 11.1. axios 常用示例  

**get 请求：**

```javascript
import request from '@/utils/request';

request.get('/main/user').then(res => {
    console.log(res);
}).catch(e => {
    console.error(e);
});
```

**get 请求带参数：**

```javascript
import request from '@/utils/request';

request.get('/sys/user/page', {
    params: {
        page: 1,
        limit: 10
    }
}).then(res => {
    console.log(res);
}).catch(e => {
    console.error(e);
});
```

**post json 提交：**

```javascript
import request from '@/utils/request';

request.post('/sys/role', {
    roleName: '管理员',
    comments: ''
}).then(res => {
    console.log(res);
}).catch(e => {
    console.error(e);
});
```

**post 表单提交：**

```javascript
import request from '@/utils/request';

const formData = new FormData();
formData.append('username', 'admin');
formData.append('password', '123456');
request.post('/login', formData).then(res => {
    console.log(res);
}).catch(e => {
    console.error(e);
});

// 字段比较多的时候可以循环 append
const data = { username: 'admin', password: '123456' };
const formData = new FormData();
Object.keys(data).forEach(key => {
    if (typeof data[key] != null) {
        formData.append(key, data[key]);
    }
});
```

**文件上传：**

```javascript
import request from '@/utils/request';

const formData = new FormData();
formData.append('file', file);
request.post('/file/upload', formData).then(res => {
    console.log(res);
}).catch(e => {
    console.error(e);
});
```

**request get：**

```javascript
import request from '@/utils/request';

request({
    url: '/sys/user',
    method: 'GET',
    params: { page: 1, limit: 10 },
    headers: { 'Authorization': 'xxxxx' }
}).then(res => {
    console.log(res);
}).catch(e => {
    console.error(e);
});
```

**request post：**

```javascript
import request from '@/utils/request';

request({
    url: '/sys/role',
    method: 'POST',
    data: { roleName: '管理员', comments: '' },
    headers: { 'Authorization': 'xxxxx' }
}).then(res => {
    console.log(res);
}).catch(e => {
    console.error(e);
});
```

<br/>

## 11.2. 实现弹窗选择组件  

把一些重复性的功能封装为组件，可以提高代码的阅读性和可维护性，例如一个弹窗选择功能，不封装组件直接写在页面中是这样的：

::: code-tabs

@tab TypeScript

```vue
<!-- 这个例子为页面上一个按钮，点击弹出用户列表弹窗，勾选用户，然后点击保存回调选中的数据 -->
<template>
    <div class="ele-body">
        <a-card :bordered="false">
            <a-button @click="openChooseModal" type="primary">选择用户</a-button>
            <!-- 展示选择后的数据 -->
            <div v-for="item in result" :key="item.userId">
                <div>{{ item.username }} - {{ item.nickname }}</div>
            </div>
        </a-card>
        <!-- 用户选择弹窗 -->
        <a-modal :width="720" title="用户选择" v-model:visible="visible" @cancel="onClosed" @ok="doChoose">
            <div style="padding-bottom: 15px;">
                <!-- 搜索表单 -->
                <a-form 
                    :model="where" 
                    :label-col="{sm: {span: 6}}" 
                    :wrapper-col="{sm: {span: 18}}">
                    <a-row>
                        <a-col :lg="8" :md="12" :sm="24" :xs="24">
                            <a-form-item label="用户账号:">
                                <a-input v-model:value="where.username" placeholder="请输入" allow-clear />
                            </a-form-item>
                        </a-col>
                        <a-col :lg="8" :md="12" :sm="24" :xs="24">
                            <a-form-item label="用户名:">
                                <a-input v-model:value="where.nickname" placeholder="请输入" allow-clear />
                            </a-form-item>
                        </a-col>
                        <a-col :lg="8" :md="12" :sm="24" :xs="24">
                            <a-form-item class="ele-text-right" :wrapper-col="{span: 24}">
                                <a-button type="primary" @click="reload">搜索</a-button>
                            </a-form-item>
                        </a-col>
                    </a-row>
                </a-form>
                <!-- 数据表格 -->
                <ele-pro-table
                    ref="tableRef"
                    row-key="userId"
                    :columns="columns"
                    :datasource="datasource"
                    v-model:selection="selection">
                </ele-pro-table>
            </div>
        </a-modal>
    </div>
</template>

<script lang="ts" setup>
    import { ref, reactive } from 'vue';
    import { message } from 'ant-design-vue';
    import type { EleProTable } from 'ele-admin-pro';
    import type { DatasourceFunction, ColumnItem } from 'ele-admin-pro/es/ele-pro-table/types';
    import type { User, UserParam } from '@/api/system/user/model';
    import { pageUsers } from '@/api/system/user';

    // 弹窗是否打开
    const visible = ref(false);

    // 选择后的数据
    const result = ref<User[]>([]);

    // 表格搜索条件
    const where = reactive<UserParam>({
        username: '',
        nickname: ''
    });

    // 表格选中数据
    const selection = ref<User[]>([]);

    // 表格实例
    const tableRef = ref<InstanceType<typeof EleProTable> | null>(null);

    // 表格列配置
    const columns = ref<ColumnItem[]>([
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
            title: '性别',
            dataIndex: 'sexName',
            sorter: true
        },
        {
            title: '手机号',
            dataIndex: 'phone',
            sorter: true,
        }
    ]);

    // 表格数据源
    const datasource: DatasourceFunction = ({ page, limit, where, orders }) => {
        return pageUsers({ ...where, ...orders, page, limit });
    };

    /* 搜索 */
    const reload = () => {
        tableRef?.value?.reload({ page: 1, where: where });
    };

    /* 打开选择弹窗 */
    const openChooseModal = () => {
        selection.value = [];
        visible.value = true;
    };

    /* 完成选择 */
    const doChoose = () => {
        if (!selection.value.length) {
            message.error('请选择数据');
            return;
        }
        result.value = [...selection.value];
        visible.value = false;
    };

    /* 弹窗关闭回调 */
    const onClosed = () => {
        selection.value = [];
        Object.assign(where, {
            username: '',
            nickname: ''
        });
        reload();
    }
</script>
```

@tab JavaScript

```vue
<!-- 这个例子为页面上一个按钮，点击弹出用户列表弹窗，勾选用户，然后点击保存回调选中的数据 -->
<template>
    <div class="ele-body">
        <a-card :bordered="false">
            <a-button @click="openChooseModal" type="primary">选择用户</a-button>
            <!-- 展示选择后的数据 -->
            <div v-for="item in result" :key="item.userId">
                <div>{{ item.username }} - {{ item.nickname }}</div>
            </div>
        </a-card>
        <!-- 用户选择弹窗 -->
        <a-modal :width="720" title="用户选择" v-model:visible="visible" @cancel="onClosed" @ok="doChoose">
            <div style="padding-bottom: 15px;">
                <!-- 搜索表单 -->
                <a-form 
                    :model="where" 
                    :label-col="{sm: {span: 6}}" 
                    :wrapper-col="{sm: {span: 18}}">
                    <a-row>
                        <a-col :lg="8" :md="12" :sm="24" :xs="24">
                            <a-form-item label="用户账号:">
                                <a-input v-model:value="where.username" placeholder="请输入" allow-clear />
                            </a-form-item>
                        </a-col>
                        <a-col :lg="8" :md="12" :sm="24" :xs="24">
                            <a-form-item label="用户名:">
                                <a-input v-model:value="where.nickname" placeholder="请输入" allow-clear />
                            </a-form-item>
                        </a-col>
                        <a-col :lg="8" :md="12" :sm="24" :xs="24">
                            <a-form-item class="ele-text-right" :wrapper-col="{span: 24}">
                                <a-button type="primary" @click="reload">搜索</a-button>
                            </a-form-item>
                        </a-col>
                    </a-row>
                </a-form>
                <!-- 数据表格 -->
                <ele-pro-table
                    ref="tableRef"
                    row-key="userId"
                    :columns="columns"
                    :datasource="datasource"
                    v-model:selection="selection">
                </ele-pro-table>
            </div>
        </a-modal>
    </div>
</template>

<script setup>
    import { ref, reactive } from 'vue';
    import { message } from 'ant-design-vue';
    import { pageUsers } from '@/api/system/user';

    // 弹窗是否打开
    const visible = ref(false);

    // 选择后的数据
    const result = ref([]);

    // 表格搜索条件
    const where = reactive({
        username: '',
        nickname: ''
    });

    // 表格选中数据
    const selection = ref([]);

    // 表格实例
    const tableRef = ref(null);

    // 表格列配置
    const columns = ref([
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
            title: '性别',
            dataIndex: 'sexName',
            sorter: true
        },
        {
            title: '手机号',
            dataIndex: 'phone',
            sorter: true,
        }
    ]);

    // 表格数据源
    const datasource = ({ page, limit, where, orders }) => {
        return pageUsers({ ...where, ...orders, page, limit });
    };

    /* 搜索 */
    const reload = () => {
        tableRef?.value?.reload({ page: 1, where: where });
    };

    /* 打开选择弹窗 */
    const openChooseModal = () => {
        selection.value = [];
        visible.value = true;
    };

    /* 完成选择 */
    const doChoose = () => {
        if (!selection.value.length) {
            message.error('请选择数据');
            return;
        }
        result.value = [...selection.value];
        visible.value = false;
    };

    /* 弹窗关闭回调 */
    const onClosed = () => {
        selection.value = [];
        Object.assign(where, {
            username: '',
            nickname: ''
        });
        reload();
    }
</script>
```

:::

完成后的效果：

![](/images/11-1.png)

选择后的效果:

![](/images/11-2.png)

如果做成独立的组件，新建一个 `user-choose.vue` 文件(这里是建在同目录下面)：

::: code-tabs

@tab TypeScript

```vue
<template>
    <a-modal
        :width="720"
        title="用户选择"
        :visible="visible"
        @update:visible="updateVisible"
        @ok="doChoose">
        <div style="padding-bottom: 15px;">
            <!-- 搜索表单 -->
            <a-form
                :model="where"
                :label-col="{md: {span: 6}, sm: {span: 24}}"
                :wrapper-col="{md: {span: 18}, sm: {span: 24}}">
                <a-row>
                    <a-col :lg="8" :md="12" :sm="24" :xs="24">
                        <a-form-item label="用户账号:">
                            <a-input v-model:value="where.username" placeholder="请输入" allow-clear />
                        </a-form-item>
                    </a-col>
                    <a-col :lg="8" :md="12" :sm="24" :xs="24">
                        <a-form-item label="用户名:">
                            <a-input v-model:value="where.nickname" placeholder="请输入" allow-clear />
                        </a-form-item>
                    </a-col>
                    <a-col :lg="8" :md="12" :sm="24" :xs="24">
                        <a-form-item class="ele-text-right" :wrapper-col="{span: 24}">
                            <a-button type="primary" @click="reload">搜索</a-button>
                        </a-form-item>
                    </a-col>
                </a-row>
            </a-form>
            <!-- 数据表格 -->
            <ele-pro-table
                ref="tableRef"
                row-key="userId"
                :columns="columns"
                :datasource="datasource"
                :init-load="false"
                v-model:selection="selection">
            </ele-pro-table>
        </div>
    </a-modal>
</template>

<script lang="ts" setup>
    import { ref, reactive, watch, onMounted, nextTick } from 'vue';
    import { message } from 'ant-design-vue';
    import type { EleProTable } from 'ele-admin-pro';
    import type { DatasourceFunction, ColumnItem } from 'ele-admin-pro/es/ele-pro-table/types';
    import type { User, UserParam } from '@/api/system/user/model';
    import { pageUsers } from '@/api/system/user';

    const props = defineProps<{
        visible?: boolean;
        sex?: string;
    }>();

    const emit = defineEmits<{
        (e: 'update:visible', visible?: boolean): void;
        (e: 'done', result?: User[]): void;
    }>();

    // 表格搜索条件
    const where = reactive<UserParam>({
        username: '',
        nickname: ''
    });

    // 表格选中数据
    const selection = ref<User[]>([]);

    // 表格实例
    const tableRef = ref<InstanceType<typeof EleProTable> | null>(null);

    // 表格列配置
    const columns = ref<ColumnItem[]>([
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
            title: '性别',
            dataIndex: 'sexName',
            sorter: true
        },
        {
            title: '手机号',
            dataIndex: 'phone',
            sorter: true,
        }
    ]);

    // 表格数据源
    const datasource: DatasourceFunction = ({ page, limit, where, orders }) => {
        return pageUsers({
            ...where,
            ...orders,
            page,
            limit,
            sex: props.sex
        });
    };

    /* 搜索 */
    const reload = () => {
        tableRef?.value?.reload({ page: 1, where: where });
    };

    /* 完成选择 */
    const doChoose = () => {
        if (!selection.value.length) {
            message.error('请选择数据');
            return;
        }
        updateVisible(false);
        emit('done', [...selection.value]);
    };

    /* 更新visible */
    const updateVisible = (value) => {
        emit('update:visible', value);
    }

    watch(() => props.visible, (visible) => {
        if(visible) {
            selection.value = [];
            Object.assign(where, {
                username: '',
                nickname: ''
            });
            nextTick(() => {
                reload();
            });
        }
    });

    onMounted(() => {
        if(props.visible) {
            reload();
        }
    });
</script>
```

@tab JavaScript

```vue
<template>
    <a-modal
        :width="720"
        title="用户选择"
        :visible="visible"
        @update:visible="updateVisible"
        @ok="doChoose">
        <div style="padding-bottom: 15px;">
            <!-- 搜索表单 -->
            <a-form
                :model="where"
                :label-col="{md: {span: 6}, sm: {span: 24}}"
                :wrapper-col="{md: {span: 18}, sm: {span: 24}}">
                <a-row>
                    <a-col :lg="8" :md="12" :sm="24" :xs="24">
                        <a-form-item label="用户账号:">
                            <a-input v-model:value="where.username" placeholder="请输入" allow-clear />
                        </a-form-item>
                    </a-col>
                    <a-col :lg="8" :md="12" :sm="24" :xs="24">
                        <a-form-item label="用户名:">
                            <a-input v-model:value="where.nickname" placeholder="请输入" allow-clear />
                        </a-form-item>
                    </a-col>
                    <a-col :lg="8" :md="12" :sm="24" :xs="24">
                        <a-form-item class="ele-text-right" :wrapper-col="{span: 24}">
                            <a-button type="primary" @click="reload">搜索</a-button>
                        </a-form-item>
                    </a-col>
                </a-row>
            </a-form>
            <!-- 数据表格 -->
            <ele-pro-table
                ref="tableRef"
                row-key="userId"
                :columns="columns"
                :datasource="datasource"
                :init-load="false"
                v-model:selection="selection">
            </ele-pro-table>
        </div>
    </a-modal>
</template>

<script setup>
    import { ref, reactive, watch, onMounted, nextTick } from 'vue';
    import { message } from 'ant-design-vue';
    import { pageUsers } from '@/api/system/user';

    const props = defineProps({
        visible: Boolean,
        sex: String
    });

    const emit = defineEmits([ 'update:visible', 'done']);

    // 表格搜索条件
    const where = reactive({
        username: '',
        nickname: ''
    });

    // 表格选中数据
    const selection = ref([]);

    // 表格实例
    const tableRef = ref(null);

    // 表格列配置
    const columns = ref([
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
            title: '性别',
            dataIndex: 'sexName',
            sorter: true
        },
        {
            title: '手机号',
            dataIndex: 'phone',
            sorter: true,
        }
    ]);

    // 表格数据源
    const datasource = ({ page, limit, where, orders }) => {
        return pageUsers({
            ...where,
            ...orders,
            page,
            limit,
            sex: props.sex
        });
    };

    /* 搜索 */
    const reload = () => {
        tableRef?.value?.reload({ page: 1, where: where });
    };

    /* 完成选择 */
    const doChoose = () => {
        if (!selection.value.length) {
            message.error('请选择数据');
            return;
        }
        updateVisible(false);
        emit('done', [...selection.value]);
    };

    /* 更新visible */
    const updateVisible = (value) => {
        emit('update:visible', value);
    }

    watch(() => props.visible, (visible) => {
        if(visible) {
            selection.value = [];
            Object.assign(where, {
                username: '',
                nickname: ''
            });
            nextTick(() => {
                reload();
            });
        }
    });

    onMounted(() => {
        if(props.visible) {
            reload();
        }
    });
</script>
```

:::

然后修改原来的页面：

::: code-tabs

@tab TypeScript

```vue
<template>
    <div class="ele-body">
        <a-card :bordered="false">
            <a-button @click="openChooseModal" type="primary">选择用户</a-button>
            <!-- 展示选择后的数据 -->
            <div v-for="item in result" :key="item.userId">
                <div>{{ item.username }} - {{ item.nickname }}</div>
            </div>
        </a-card>
    </div>
    <!-- 用户选择弹窗 -->
    <user-choose v-model:visible="visible" sex="1" @done="onChoose" />
</template>

<script lang="ts" setup>
    import { ref, reactive } from 'vue';
    import type { User } from '@/api/system/user/model';
    import UserChoose from './user-choose.vue';

    // 弹窗是否打开
    const visible = ref(false);

    // 选择后的数据
    const result = ref<User[]>([]);

    /* 打开选择弹窗 */
    const openChooseModal = () => {
        visible.value = true;
    };

    /* 完成选择 */
    const onChoose = (data) => {
        result.value = data;
    };
</script>
```

@tab JavaScript

```vue
<template>
    <div class="ele-body">
        <a-card :bordered="false">
            <a-button @click="openChooseModal" type="primary">选择用户</a-button>
            <!-- 展示选择后的数据 -->
            <div v-for="item in result" :key="item.userId">
                <div>{{ item.username }} - {{ item.nickname }}</div>
            </div>
        </a-card>
        <!-- 用户选择弹窗 -->
        <user-choose v-model:visible="visible" sex="1" @done="onChoose" />
    </div>
</template>

<script setup>
    import { ref, reactive } from 'vue';
    import UserChoose from './user-choose.vue';

    // 弹窗是否打开
    const visible = ref(false);

    // 选择后的数据
    const result = ref([]);

    /* 打开选择弹窗 */
    const openChooseModal = () => {
        visible.value = true;
    };

    /* 完成选择 */
    const onChoose = (data) => {
        result.value = data;
    };
</script>
```

:::

  封装为组件后其它页面有类似功能可以直接引入使用，这里在组件属性中加了一个 `sex` 参数，可以通过传递 sex 参数控制选择不同性别的用户， 有几个细节需要注意，子组件不能修改父组件传递的参数，所以传递给 `a-model` 的 `visible` 不能加 `v-model`，而是监听 `update:visible` 事件， 再用 `emit`回调给父组件，父组件就可以用 `v-model` 了。

<br/>

## 11.3. 实现鼠标滚轮监听  

比如要实现滚动鼠标让 div 横向滚动，监听 `mousewheel` 事件：

::: code-tabs

@tab TypeScript

```vue
<template>
    <div @mousewheel="onMousewheel" style="white-space: nowrap;overflow-x: auto;">
        <div 
            v-for="item in 100" 
            :key="item" 
            style="display: inline-block;padding: 20px 40px;"
        >
            {{ item }}
        </div>
    </div>
</template>

<script lang="ts" setup>
    import { ref } from 'vue';

    // 用于事件节流
    const scroll = ref(false);

    /* 鼠标滚轮事件 */
    const onMousewheel = (e: MouseEvent) => {
        const elem = e.currentTarget as HTMLElement;
        if (!scroll.value) {  // 判断节流标识
            scroll.value = true;
            const delta = e.wheelDelta || e.detail;
            if (delta > 0) {  // 向上滚动
                elem.scrollLeft -= elem.clientWidth;
            } else if (delta < 0) {  // 向下滚动
                elem.scrollLeft += elem.clientWidth;
            }
            setTimeout(() => {
                scroll.value = false;
            }, 300);
        }
        e.stopPropagation();
        e.preventDefault();
    }
</script>
```

@tab JavaScript

```vue
<template>
    <div @mousewheel="onMousewheel" style="white-space: nowrap;overflow-x: auto;">
        <div 
            v-for="item in 100" 
            :key="item" 
            style="display: inline-block;padding: 20px 40px;"
        >
            {{ item }}
        </div>
    </div>
</template>

<script setup>
    import { ref } from 'vue';

    // 用于事件节流
    const scroll = ref(false);

    /* 鼠标滚轮事件 */
    const onMousewheel = (e) => {
        const elem = e.currentTarget;
        if (!scroll.value) {  // 判断节流标识
            scroll.value = true;
            const delta = e.wheelDelta || e.detail;
            if (delta > 0) {  // 向上滚动
                elem.scrollLeft -= elem.clientWidth;
            } else if (delta < 0) {  // 向下滚动
                elem.scrollLeft += elem.clientWidth;
            }
            setTimeout(() => {
                scroll.value = false;
            }, 300);
        }
        e.stopPropagation();
        e.preventDefault();
    }
</script>
```

:::

<br/>

## 11.4. 表单验证气泡样式  

在 `src/styles/index.less` 中加入样式，对应 antdv 3.1.0 的版本，注释的地方放开就是 hover 时才显示气泡：

```less
/* ......省略其它代码 */
/* 在最下面加入样式 */
.ant-form-item-explain > .ant-form-item-explain-error {
    position: absolute;
    left: calc(100% + 8px);
    top: calc(50% - 12px);
    transform: translateY(-50%);
    width: max-content;
    max-width: 120px;
    background: rgba(245, 34, 45, 0.8);
    color: #fff;
    font-size: 12px;
    padding: 8px 10px;
    border-radius: 4px;
    z-index: 999;
    /*display: none;*/
}

/*
.ant-form-item-control-input:hover + .ant-form-item-explain > .ant-form-item-explain-error {
    display: block;
}
*/

.ant-form-item-explain > .ant-form-item-explain-error:after {
    content: '';
    border: 8px solid transparent;
    border-right-color: rgba(245, 34, 45, 0.8);
    position: absolute;
    left: -16px;
    top: 50%;
    margin-top: -8px;
}

.ant-form-item-explain > .ant-form-item-explain-error {
    animation: none !important;
    transition: none !important;
}
```

![](/images/11-3.png)

不同的版本 class 名称略有不同，以下是对应 'ant-design-vue@3.0.0-beta.9' 的实现：

```less
/* ......省略其它代码 */
/* 在最下面加入样式 */
.ant-form-item-explain.ant-form-item-explain-error > div {
    position: absolute;
    left: calc(100% + 8px);
    top: calc(50% - 12px);
    transform: translateY(-50%);
    width: max-content;
    max-width: 120px;
    background: rgba(245, 34, 45, 0.8);
    color: #fff;
    font-size: 12px;
    padding: 8px 10px;
    border-radius: 4px;
    z-index: 999;
    /*display: none;*/
}

/*
.ant-form-item-control-input:hover + .ant-form-item-explain > div {
    display: block;
}
*/

.ant-form-item-explain.ant-form-item-explain-error > div:after {
    content: '';
    border: 8px solid transparent;
    border-right-color: rgba(245, 34, 45, 0.8);
    position: absolute;
    left: -16px;
    top: 50%;
    margin-top: -8px;
}

.ant-form-item-explain.ant-form-item-explain-error {
    animation: none !important;
    transition: none !important;
}
```

<br/>

## 11.5. 直接使用 ATable  

授人以鱼不如授之以渔，当 ele-pro-table 无法满足需求时也要会灵活使用 a-table , 例如实现分页、排序、模糊搜索：

::: code-tabs

@tab TypeScript

```vue
<template>
    <div class="ele-body">
        <a-card :bordered="false">
            <!-- 搜索表单 -->
            <a-form
                :model="where"
                :label-col="{ md: { span: 6 }, sm: { span: 24 } }"
                :wrapper-col="{ md: { span: 18 }, sm: { span: 24 } }"
            >
                <a-row>
                    <a-col :lg="6" :md="12" :sm="24" :xs="24">
                        <a-form-item label="用户账号:">
                            <a-input
                                v-model:value.trim="where.username"
                                placeholder="请输入"
                                allow-clear
                            />
                        </a-form-item>
                    </a-col>
                    <a-col :lg="6" :md="12" :sm="24" :xs="24">
                        <a-form-item label="性别:">
                            <a-select
                                v-model:value="where.sex"
                                placeholder="请选择"
                                allow-clear
                            >
                                <a-select-option value="1">男</a-select-option>
                                <a-select-option value="2">女</a-select-option>
                            </a-select>
                        </a-form-item>
                    </a-col>
                    <a-col :lg="6" :md="12" :sm="24" :xs="24">
                        <a-form-item class="ele-text-right" :wrapper-col="{ span: 24 }">
                            <a-space>
                                <a-button type="primary" @click="reload">查询</a-button>
                                <a-button @click="reset">重置</a-button>
                            </a-space>
                        </a-form-item>
                    </a-col>
                </a-row>
            </a-form>
            <!-- 表格 -->
            <a-table
                row-key="userId"
                :columns="columns"
                :data-source="data"
                :loading="loading"
                :pagination="tablePagination"
                :row-selection="rowSelection"
                :scroll="{ x: 800 }"
                @change="onTableChange"
            >
                <template #action="{ record }">
                    <a-popconfirm title="确定要删除此用户吗？" @confirm="remove(record)">
                        <a class="ele-text-danger">删除</a>
                    </a-popconfirm>
                </template>
            </a-table>
        </a-card>
    </div>
</template>

<script lang="ts" setup>
    import { ref, reactive } from 'vue';
    import { message } from 'ant-design-vue';
    import type { User, UserParam } from '@/api/system/user/model';
    import { pageUsers, removeUser } from '@/api/system/user';
    import type {
        DefaultRecordType as RecordType,
        Key
    } from 'ant-design-vue/es/vc-table/interface';
    import type {
        ColumnsType,
        TableRowSelection,
        TablePaginationConfig,
        FilterValue,
        SorterResult
    } from 'ant-design-vue/es/table/interface';

    // 表格列配置
    const columns = ref<ColumnsType<RecordType>>([
        {
            key: 'index',
            customRender: ({ index }) => ((tablePagination.current ?? 1) - 1) * (tablePagination.pageSize ?? 0) + index + 1
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
            title: '性别',
            dataIndex: 'sexName',
            sorter: true
        },
        {
            title: '操作',
            key: 'action'
        }
    ]);

    // 表格数据
    const data = ref<User[]>([]);

    // 表格加载状态
    const loading = ref(true);

    // 表格分页配置
    const tablePagination = reactive({
        current: 1,
        pageSize: 10,
        total: 0,
        showSizeChanger: true,
        showTotal: (total: number) => `共 ${total} 条`
    });

    // 表格复选框列配置
    const rowSelection = reactive<TableRowSelection>({
        selectedRowKeys: [],
        onChange: (selectedRowKeys: Key[], selectedRows: RecordType[]) => {
            rowSelection.selectedRowKeys = selectedRowKeys;
            selection.value = selectedRows;
        }
    });

    // 表格选中数据
    const selection = ref<User[]>([]);

    // 表格搜索条件
    const where = reactive<UserParam>({
        username: '',
        sex: undefined
    });

    // 表格排序条件
    const tableSorter = ref<SorterResult<RecordType>>();

    /* 表格查询数据 */
    const query = () => {
        clearChoose();
        loading.value = true;
        pageUsers({
            page: tablePagination.current,
            limit: tablePagination.pageSize,
            sort: tableSorter.value?.field as string,
            order: { ascend: 'asc', descend: 'desc' }[tableSorter.value?.order ?? ''],
            ...where
        }).then(({ list, count }) => {
            loading.value = false;
            data.value = list;
            tablePagination.total = count;
        }).catch((e: Error) => {
            loading.value = false;
            message.error(e.message);
        });
    };

    /* 表格分页、排序、筛选改变回调 */
    const onTableChange = (
        pagination: TablePaginationConfig,
        _filters: Record<string, FilterValue | null | undefined>,
        sorter: SorterResult<RecordType> | SorterResult<RecordType>[]
    ) => {
        if (Array.isArray(sorter)) {
            tableSorter.value = sorter[0];
        } else {
            tableSorter.value = sorter;
        }
        Object.assign(tablePagination, pagination);
        query();
    };

    /* 搜索 */
    const reload = () => {
        tablePagination.current = 1;
        query();
    };

    /*  重置搜索 */
    const reset = () => {
        Object.assign(where, {
            username: '',
            sex: undefined
        });
        reload();
    };

    /* 清空选择 */
    const clearChoose = () => {
        rowSelection.selectedRowKeys = [];
        selection.value = [];
    };

    /* 删除单个 */
    const remove = (row: User) => {
        removeUser(row.userId).then((msg) => {
            message.success(msg);
            reload();
        }).catch((e: Error) => {
            message.error(e.message);
        });
    };

    query();
</script>
```

@tab JavaScript

```vue
<template>
    <div class="ele-body">
        <a-card :bordered="false">
            <!-- 搜索表单 -->
            <a-form
                :model="where"
                :label-col="{ md: { span: 6 }, sm: { span: 24 } }"
                :wrapper-col="{ md: { span: 18 }, sm: { span: 24 } }"
            >
                <a-row>
                    <a-col :lg="6" :md="12" :sm="24" :xs="24">
                        <a-form-item label="用户账号:">
                            <a-input
                                v-model:value.trim="where.username"
                                placeholder="请输入"
                                allow-clear
                            />
                        </a-form-item>
                    </a-col>
                    <a-col :lg="6" :md="12" :sm="24" :xs="24">
                        <a-form-item label="性别:">
                            <a-select
                                v-model:value="where.sex"
                                placeholder="请选择"
                                allow-clear
                            >
                                <a-select-option value="1">男</a-select-option>
                                <a-select-option value="2">女</a-select-option>
                            </a-select>
                        </a-form-item>
                    </a-col>
                    <a-col :lg="6" :md="12" :sm="24" :xs="24">
                        <a-form-item class="ele-text-right" :wrapper-col="{ span: 24 }">
                            <a-space>
                                <a-button type="primary" @click="reload">查询</a-button>
                                <a-button @click="reset">重置</a-button>
                            </a-space>
                        </a-form-item>
                    </a-col>
                </a-row>
            </a-form>
            <!-- 表格 -->
            <a-table
                row-key="userId"
                :columns="columns"
                :data-source="data"
                :loading="loading"
                :pagination="tablePagination"
                :row-selection="rowSelection"
                :scroll="{ x: 800 }"
                @change="onTableChange"
            >
                <template #action="{ record }">
                    <a-popconfirm title="确定要删除此用户吗？" @confirm="remove(record)">
                        <a class="ele-text-danger">删除</a>
                    </a-popconfirm>
                </template>
            </a-table>
        </a-card>
    </div>
</template>

<script setup>
    import { ref, reactive } from 'vue';
    import { message } from 'ant-design-vue';
    import { pageUsers, removeUser } from '@/api/system/user';

    // 表格列配置
    const columns = ref([
        {
            key: 'index',
            customRender: ({ index }) => ((tablePagination.current ?? 1) - 1) * (tablePagination.pageSize ?? 0) + index + 1
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
            title: '性别',
            dataIndex: 'sexName',
            sorter: true
        },
        {
            title: '操作',
            key: 'action'
        }
    ]);

    // 表格数据
    const data = ref([]);

    // 表格加载状态
    const loading = ref(true);

    // 表格分页配置
    const tablePagination = reactive({
        current: 1,
        pageSize: 10,
        total: 0,
        showSizeChanger: true,
        showTotal: (total) => `共 ${total} 条`
    });

    // 表格复选框列配置
    const rowSelection = reactive({
        selectedRowKeys: [],
        onChange: (selectedRowKeys, selectedRows) => {
            rowSelection.selectedRowKeys = selectedRowKeys;
            selection.value = selectedRows;
        }
    });

    // 表格选中数据
    const selection = ref([]);

    // 表格搜索条件
    const where = reactive({
        username: '',
        sex: undefined
    });

    // 表格排序条件
    const tableSorter = ref(null);

    /* 表格查询数据 */
    const query = () => {
        clearChoose();
        loading.value = true;
        pageUsers({
            page: tablePagination.current,
            limit: tablePagination.pageSize,
            sort: tableSorter.value?.field,
            order: { ascend: 'asc', descend: 'desc' }[tableSorter.value?.order ?? ''],
            ...where
        }).then(({ list, count }) => {
            loading.value = false;
            data.value = list;
            tablePagination.total = count;
        }).catch((e) => {
            loading.value = false;
            message.error(e.message);
        });
    };

    /* 表格分页、排序、筛选改变回调 */
    const onTableChange = (pagination, _filters, sorter) => {
        if (Array.isArray(sorter)) {
            tableSorter.value = sorter[0];
        } else {
            tableSorter.value = sorter;
        }
        Object.assign(tablePagination, pagination);
        query();
    };

    /* 搜索 */
    const reload = () => {
        tablePagination.current = 1;
        query();
    };

    /*  重置搜索 */
    const reset = () => {
        Object.assign(where, {
            username: '',
            sex: undefined
        });
        reload();
    };

    /* 清空选择 */
    const clearChoose = () => {
        rowSelection.selectedRowKeys = [];
        selection.value = [];
    };

    /* 删除单个 */
    const remove = (row) => {
        removeUser(row.userId).then((msg) => {
            message.success(msg);
            reload();
        }).catch((e) => {
            message.error(e.message);
        });
    };

    query();
</script>
```

:::

<br/>

## 11.6. ATable 前端分页  

使用 a-table 实现接口查询全部数据，前端进行分页、排序、模糊搜索：

::: code-tabs

@tab TypeScript

```vue
<template>
    <div class="ele-body">
        <a-card :bordered="false">
            <!-- 搜索表单 -->
            <a-form
                :model="where"
                :label-col="{ md: { span: 6 }, sm: { span: 24 } }"
                :wrapper-col="{ md: { span: 18 }, sm: { span: 24 } }"
            >
                <a-row>
                    <a-col :lg="6" :md="12" :sm="24" :xs="24">
                        <a-form-item label="用户账号:">
                            <a-input
                                v-model:value.trim="where.username"
                                placeholder="请输入"
                                allow-clear
                            />
                        </a-form-item>
                    </a-col>
                    <a-col :lg="6" :md="12" :sm="24" :xs="24">
                        <a-form-item label="性别:">
                            <a-select
                                v-model:value="where.sex"
                                placeholder="请选择"
                                allow-clear
                            >
                                <a-select-option value="1">男</a-select-option>
                                <a-select-option value="2">女</a-select-option>
                            </a-select>
                        </a-form-item>
                    </a-col>
                    <a-col :lg="6" :md="12" :sm="24" :xs="24">
                        <a-form-item class="ele-text-right" :wrapper-col="{ span: 24 }">
                            <a-space>
                                <a-button type="primary" @click="reload">查询</a-button>
                                <a-button @click="reset">重置</a-button>
                            </a-space>
                        </a-form-item>
                    </a-col>
                </a-row>
            </a-form>
            <!-- 表格 -->
            <a-table
                row-key="userId"
                :columns="columns"
                :data-source="data"
                :loading="loading"
                :pagination="tablePagination"
                :row-selection="rowSelection"
                :scroll="{ x: 800 }"
                @change="onTableChange"
            >
                <template #bodyCell="{ column, record }">
                    <template v-if="column.key === 'action'">
                        <a-popconfirm
                            title="确定要删除此用户吗？"
                            @confirm="remove(record)"
                        >
                            <a class="ele-text-danger">删除</a>
                        </a-popconfirm>
                    </template>
                </template>
            </a-table>
        </a-card>
    </div>
</template>

<script lang="ts" setup>
    import { ref, reactive } from 'vue';
    import { message } from 'ant-design-vue';
    import { listUsers, removeUser } from '@/api/system/user';
    import type { User, UserParam } from '@/api/system/user/model';
    import useSearch from '@/utils/use-search';
    import type {
        DefaultRecordType as RecordType,
        Key
    } from 'ant-design-vue/es/vc-table/interface';
    import type {
        ColumnsType,
        TableRowSelection,
        TablePaginationConfig,
        FilterValue,
        SorterResult
    } from 'ant-design-vue/es/table/interface';

    // 表格列配置
    const columns = ref<ColumnsType<RecordType>>([
        {
            key: 'index',
            customRender: ({ index }) => ((tablePagination.current ?? 1) - 1) * (tablePagination.pageSize ?? 0) + index + 1
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
            title: '性别',
            dataIndex: 'sexName',
            sorter: true
        },
        {
            title: '操作',
            key: 'action'
        }
    ]);

    // 表格数据
    const data = ref<User[]>([]);

    // 表格加载状态
    const loading = ref(true);

    // 表格分页配置
    const tablePagination = reactive<TablePaginationConfig>({
        current: 1,
        pageSize: 10,
        total: 0,
        showSizeChanger: true,
        showTotal: (total: number) => `共 ${total} 条`
    });

    // 表格复选框列配置
    const rowSelection = reactive<TableRowSelection>({
        selectedRowKeys: [],
        onChange: (selectedRowKeys: Key[], selectedRows: RecordType[]) => {
            rowSelection.selectedRowKeys = selectedRowKeys;
            selection.value = selectedRows;
        }
    });

    // 表格选中数据
    const selection = ref<User[]>([]);

    // 表格搜索条件
    const { where, resetFields } = useSearch<UserParam>({
        username: '',
        sex: undefined
    });

    // 表格排序条件
    const tableSorter = ref<SorterResult<RecordType>>();

    // 全部的用户数据
    const userList = ref<User[]>([]);

    /* 表格查询全部数据 */
    const query = () => {
        loading.value = true;
        listUsers({}).then((data) => {
            loading.value = false;
            userList.value = data ?? [];
            reload();
        }).catch((e) => {
            loading.value = false;
            message.error(e.message);
        });
    };

    /* 表格分页、排序、筛选改变回调 */
    const onTableChange = (
        pagination: TablePaginationConfig,
        _filters: Record<string, FilterValue | null | undefined>,
        sorter: SorterResult<RecordType> | SorterResult<RecordType>[]
    ) => {
        if (Array.isArray(sorter)) {
            tableSorter.value = sorter[0];
        } else {
            tableSorter.value = sorter;
        }
        Object.assign(tablePagination, pagination);
        refresh();
    };

    /* 获取表格当前显示的数据 */
    const refresh = () => {
        // 前端搜索
        const result = userList.value.filter((d) => {
            if (
                where.username &&
                (!d.username || d.username.indexOf(where.username) === -1)
            ) {
                return false;
            }
            if (where.sex && d.sex !== where.sex) {
                return false;
            }
            return true;
        });
        // 页码超出修正
        tablePagination.total = result.length;
        const maxPage = Math.ceil(tablePagination.total / (tablePagination.pageSize ?? 1));
        if (
            maxPage &&
            tablePagination.current &&
            tablePagination.current > maxPage
        ) {
            tablePagination.current = maxPage;
        }
        // 前端排序
        const field = tableSorter.value?.field as string;
        if (field) {
            result.sort((a, b) => {
                if (b[field] == a[field]) {
                    return 0;
                }
                if (tableSorter.value?.order === 'descend') {
                    return a[field] < b[field] ? 1 : -1;
                }
                return a[field] < b[field] ? -1 : 1;
            });
        }
        // 前端分页
        const start = ((tablePagination.current ?? 1) - 1) * (tablePagination.pageSize ?? 0);
        const end = start + (tablePagination.pageSize ?? 0);
        data.value = result.slice(start, end > result.length ? result.length : end);
    };

    /* 获取表格当前显示的数据，回到第一页 */
    const reload = () => {
        clearChoose();
        tablePagination.current = 1;
        refresh();
    };

    /*  重置搜索 */
    const reset = () => {
        resetFields();
        reload();
    };

    /* 清空选择 */
    const clearChoose = () => {
        rowSelection.selectedRowKeys = [];
        selection.value = [];
    };

    /* 删除单个 */
    const remove = (row: User) => {
        removeUser(row.userId).then(() => {
            userList.value.splice(userList.value.indexOf(row), 1);
            refresh();
            message.success('删除成功');
        }).catch((e) => {
            message.error(e.message);
        });
    };

    query();
</script>
```

@tab JavaScript

```vue
<template>
    <div class="ele-body">
        <a-card :bordered="false">
            <!-- 搜索表单 -->
            <a-form
                :model="where"
                :label-col="{ md: { span: 6 }, sm: { span: 24 } }"
                :wrapper-col="{ md: { span: 18 }, sm: { span: 24 } }"
            >
                <a-row>
                    <a-col :lg="6" :md="12" :sm="24" :xs="24">
                        <a-form-item label="用户账号:">
                            <a-input
                                v-model:value.trim="where.username"
                                placeholder="请输入"
                                allow-clear
                            />
                        </a-form-item>
                    </a-col>
                    <a-col :lg="6" :md="12" :sm="24" :xs="24">
                        <a-form-item label="性别:">
                            <a-select
                                v-model:value="where.sex"
                                placeholder="请选择"
                                allow-clear
                            >
                                <a-select-option value="1">男</a-select-option>
                                <a-select-option value="2">女</a-select-option>
                            </a-select>
                        </a-form-item>
                    </a-col>
                    <a-col :lg="6" :md="12" :sm="24" :xs="24">
                        <a-form-item class="ele-text-right" :wrapper-col="{ span: 24 }">
                            <a-space>
                                <a-button type="primary" @click="reload">查询</a-button>
                                <a-button @click="reset">重置</a-button>
                            </a-space>
                        </a-form-item>
                    </a-col>
                </a-row>
            </a-form>
            <!-- 表格 -->
            <a-table
                row-key="userId"
                :columns="columns"
                :data-source="data"
                :loading="loading"
                :pagination="tablePagination"
                :row-selection="rowSelection"
                :scroll="{ x: 800 }"
                @change="onTableChange"
            >
                <template #bodyCell="{ column, record }">
                    <template v-if="column.key === 'action'">
                        <a-popconfirm
                            title="确定要删除此用户吗？"
                            @confirm="remove(record)"
                        >
                            <a class="ele-text-danger">删除</a>
                        </a-popconfirm>
                    </template>
                </template>
            </a-table>
        </a-card>
    </div>
</template>

<script setup>
    import { ref, reactive } from 'vue';
    import { message } from 'ant-design-vue';
    import { listUsers, removeUser } from '@/api/system/user';
    import useSearch from '@/utils/use-search';

    // 表格列配置
    const columns = ref([
        {
            key: 'index',
            customRender: ({ index }) => ((tablePagination.current ?? 1) - 1) * (tablePagination.pageSize ?? 0) + index + 1
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
            title: '性别',
            dataIndex: 'sexName',
            sorter: true
        },
        {
            title: '操作',
            key: 'action'
        }
    ]);

    // 表格数据
    const data = ref([]);

    // 表格加载状态
    const loading = ref(true);

    // 表格分页配置
    const tablePagination = reactive({
        current: 1,
        pageSize: 10,
        total: 0,
        showSizeChanger: true,
        showTotal: (total) => `共 ${total} 条`
    });

    // 表格复选框列配置
    const rowSelection = reactive({
        selectedRowKeys: [],
        onChange: (selectedRowKeys, selectedRows) => {
            rowSelection.selectedRowKeys = selectedRowKeys;
            selection.value = selectedRows;
        }
    });

    // 表格选中数据
    const selection = ref([]);

    // 表格搜索条件
    const { where, resetFields } = useSearch({
        username: '',
        sex: undefined
    });

    // 表格排序条件
    const tableSorter = ref();

    // 全部的用户数据
    const userList = ref([]);

    /* 表格查询全部数据 */
    const query = () => {
        loading.value = true;
        listUsers({}).then((data) => {
            loading.value = false;
            userList.value = data ?? [];
            reload();
        }).catch((e) => {
            loading.value = false;
            message.error(e.message);
        });
    };

    /* 表格分页、排序、筛选改变回调 */
    const onTableChange = (pagination, _filters, sorter) => {
        if (Array.isArray(sorter)) {
            tableSorter.value = sorter[0];
        } else {
            tableSorter.value = sorter;
        }
        Object.assign(tablePagination, pagination);
        refresh();
    };

    /* 获取表格当前显示的数据 */
    const refresh = () => {
        // 前端搜索
        const result = userList.value.filter((d) => {
            if (
                where.username &&
                (!d.username || d.username.indexOf(where.username) === -1)
            ) {
                return false;
            }
            if (where.sex && d.sex !== where.sex) {
                return false;
            }
            return true;
        });
        // 页码超出修正
        tablePagination.total = result.length;
        const maxPage = Math.ceil(tablePagination.total / (tablePagination.pageSize ?? 1));
        if (
            maxPage &&
            tablePagination.current &&
            tablePagination.current > maxPage
        ) {
            tablePagination.current = maxPage;
        }
        // 前端排序
        const field = tableSorter.value?.field;
        if (field) {
            result.sort((a, b) => {
                if (b[field] == a[field]) {
                    return 0;
                }
                if (tableSorter.value?.order === 'descend') {
                    return a[field] < b[field] ? 1 : -1;
                }
                return a[field] < b[field] ? -1 : 1;
            });
        }
        // 前端分页
        const start = ((tablePagination.current ?? 1) - 1) * (tablePagination.pageSize ?? 0);
        const end = start + (tablePagination.pageSize ?? 0);
        data.value = result.slice(start, end > result.length ? result.length : end);
    };

    /* 获取表格当前显示的数据，回到第一页 */
    const reload = () => {
        clearChoose();
        tablePagination.current = 1;
        refresh();
    };

    /*  重置搜索 */
    const reset = () => {
        resetFields();
        reload();
    };

    /* 清空选择 */
    const clearChoose = () => {
        rowSelection.selectedRowKeys = [];
        selection.value = [];
    };

    /* 删除单个 */
    const remove = (row) => {
        removeUser(row.userId).then(() => {
            userList.value.splice(userList.value.indexOf(row), 1);
            refresh();
            message.success('删除成功');
        }).catch((e) => {
            message.error(e.message);
        });
    };

    query();
</script>
```

:::