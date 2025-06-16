---
title: 文件列表
createTime: 2025/01/10 09:30:40
permalink: /EleAdminPro/yp7q6chn/
---

## 6.7.1. 快速使用

组件名 `ele-file-list` ，使用方式：

::: code-tabs

@tab TypeScript

```vue
<template>
  <ele-file-list :data="data" v-model:checked="checked"></ele-file-list>
</template>

<script lang="ts" setup>
import { ref } from "vue";
import type { FileItem } from "ele-admin-pro/es/ele-file-list/types";

// 文件列表数据
const data = ref<FileItem[]>([
  {
    name: "20200708230820.png", // 文件名称
    url: "https://v1.eleadmin.com/api/file/20200610/20200708230820.png", // 文件地址
    thumbnail:
      "https://v1.eleadmin.com/api/file/thumbnail/20200610/20200708230820.png", // 缩略图地址
    length: "20KB", // 文件大小
    updateTime: "2020-09-29 09:22:16", // 修改日期
    isDirectory: false, // 是否是文件夹
  },
]);

// 选中数据
const checked = ref<FileItem[]>([]);
</script>
```

@tab JavaScript

```vue
<template>
  <ele-file-list :data="data" v-model:checked="checked"></ele-file-list>
</template>

<script setup>
import { ref } from "vue";

// 文件列表数据
const data = ref([
  {
    name: "20200708230820.png", // 文件名称
    url: "https://v1.eleadmin.com/api/file/20200610/20200708230820.png", // 文件地址
    thumbnail:
      "https://v1.eleadmin.com/api/file/thumbnail/20200610/20200708230820.png", // 缩略图地址
    length: "20KB", // 文件大小
    updateTime: "2020-09-29 09:22:16", // 修改日期
    isDirectory: false, // 是否是文件夹
  },
]);

// 选中数据
const checked = ref([]);
</script>
```

:::

![](/images/6-7-1.png)

`ele-file-list` 只负责展示数据，文件夹点进去、返回上一级、文件打开等交互逻辑需要自己实现，可参考模板页面 `扩展组件/文件列表` 。

注意，v1.5.0 以及之前的版本需要手动引入：

```vue
<script>
import EleFileList from "ele-admin-pro/packages/ele-file-list";
// v1.0.0 版本的引入方式为如下写法:
//import EleFileList from '@/components/EleFileList';

export default {
  components: { EleFileList },
};
</script>
```

<br/>

## 6.7.2. 属性列表

| 属性名  | 类型              | 说明                                            | 默认值 |
| :------ | :---------------- | :---------------------------------------------- | :----- |
| data    | `Array<FileItem>` | 数据                                            |        |
| checked | `Array<FileItem>` | 选中数据(v-model), 不设置这个属性不会显示复选框 |        |
| grid    | Boolean           | 是否网格展示                                    | true   |
| sort    | String/Boolean    | 排序字段                                        |        |
| order   | String            | 排序方式                                        |        |
| icons   | Array             | 网格模式后缀对应图标                            |        |
| smIcons | Array             | 表格模式后缀对应图标                            |        |
| columns | Array             | 表格模式自定义列配置, `v1.10.0 新增`            |        |

表格模式自定义列配置, `v1.10.0 新增`：

::: code-tabs

@tab TypeScript

```vue
<template>
  <ele-file-list
    :data="data"
    :grid="false"
    :checked="checked"
    :columns="columns"
  >
    <template #nickname="{ item }">
      <a-tag>{{ item.nickname }}</a-tag>
    </template>
  </ele-file-list>
</template>

<script lang="ts" setup>
import { ref } from "vue";
import type {
  FileItem,
  ColumnItem,
} from "ele-admin-pro/es/ele-file-list/types";

// 数据
const data = ref<FileItem[]>([
  {
    name: "20200708230820.png",
    url: "https://v1.eleadmin.com/api/file/20200610/20200708230820.png",
    thumbnail:
      "https://v1.eleadmin.com/api/file/thumbnail/20200610/20200708230820.png",
    length: "20KB",
    updateTime: "2020-09-29 09:22:16",
    isDirectory: false,
    nickname: "管理员",
  },
]);

// 列配置
const columns = ref<ColumnItem[]>([
  {
    title: "上传人",
    prop: "nickname",
    slot: "nickname",
    style: {
      width: "120px",
      flexShrink: 0,
    },
  },
  {
    title: "文件大小",
    prop: "length",
    style: {
      width: "120px",
      flexShrink: 0,
    },
  },
  {
    title: "上传时间",
    prop: "updateTime",
    style: {
      width: "180px",
      flexShrink: 0,
    },
  },
]);

// 选中数据
const checked = ref<FileItem[]>([]);
</script>
```

@tab JavaScript

```vue
<template>
  <ele-file-list
    :data="data"
    :grid="false"
    :checked="checked"
    :columns="columns"
  >
    <template #nickname="{ item }">
      <a-tag>{{ item.nickname }}</a-tag>
    </template>
  </ele-file-list>
</template>

<script setup>
import { ref } from "vue";

// 数据
const data = ref([
  {
    name: "20200708230820.png",
    url: "https://v1.eleadmin.com/api/file/20200610/20200708230820.png",
    thumbnail:
      "https://v1.eleadmin.com/api/file/thumbnail/20200610/20200708230820.png",
    length: "20KB",
    updateTime: "2020-09-29 09:22:16",
    isDirectory: false,
    nickname: "管理员",
  },
]);

// 列配置
const columns = ref([
  {
    title: "上传人",
    prop: "nickname",
    slot: "nickname",
    style: {
      width: "120px",
      flexShrink: 0,
    },
  },
  {
    title: "文件大小",
    prop: "length",
    style: {
      width: "120px",
      flexShrink: 0,
    },
  },
  {
    title: "上传时间",
    prop: "updateTime",
    style: {
      width: "180px",
      flexShrink: 0,
    },
  },
]);

// 选中数据
const checked = ref([]);
</script>
```

:::

表格列配置参数的属性：

| 属性名     | 类型   | 说明                 |
| :--------- | :----- | :------------------- |
| title      | String | 表头标题             |
| prop       | String | 字段名               |
| slot       | String | 自定义列的插槽名称   |
| headerSlot | String | 自定义表头的插槽名称 |
| style      | Object | 自定义样式           |
| headStyle  | Object | 自定义表头样式       |

自定义文件后缀对应的图标属性 `icons` 和 `smIcons` 需要的数据格式为：

```javascript
[
  {
    icon: "https://cdn.eleadmin.com/20200609/ic_file_misc_sm.png",
    type: "file",
  }, // 文件通用图标
  {
    icon: "https://cdn.eleadmin.com/20200609/ic_file_folder_sm.png",
    type: "dir",
  }, // 文件夹图标
  {
    icon: "https://cdn.eleadmin.com/20200609/ic_file_htm_sm.png",
    types: [".html", ".htm"],
  }, // 不同后缀对应的图标
  {
    icon: "https://cdn.eleadmin.com/20200609/ic_file_text_sm.png",
    types: [".txt"],
  }, // 不同后缀对应的图标
];
```

<br/>

## 6.7.3. 事件列表

| 事件名称       | 说明                      | 回调参数                                |
| :------------- | :------------------------ | :-------------------------------------- |
| item-click     | item 点击事件             | item                                    |
| sort-change    | 排序点击事件              | obj, 示例 sort: 'name', order: 'desc' |
| update:checked | 用于 checked 属性 v-model |                                         |

<br/>

## 6.7.4. 插槽列表

| 插槽名       | 说明                                   | 参数 |
| :----------- | :------------------------------------- | :--- |
| tool         | 自定义表格模式工具按钮图标             | item |
| context-menu | 自定义 item 鼠标右键菜单 `v1.10.0新增` | item |

自定义 item 鼠标右键菜单：

::: code-tabs

@tab TypeScript

```vue
<template>
  <ele-file-list :data="data">
    <template #context-menu="{ item }">
      <a-menu
        :selectable="false"
        @click="({ key }) => onCtxMenuClick(key, item)"
      >
        <a-menu-item key="download">下载</a-menu-item>
        <a-menu-item key="remove">删除</a-menu-item>
      </a-menu>
    </template>
  </ele-file-list>
</template>

<script lang="ts" setup>
import { ref } from "vue";
import type { FileItem } from "ele-admin-pro/es/ele-file-list/types";

// 数据
const data = ref<FileItem[]>([
  {
    name: "20200708230820.png",
    url: "https://v1.eleadmin.com/api/file/20200610/20200708230820.png",
    thumbnail:
      "https://v1.eleadmin.com/api/file/thumbnail/20200610/20200708230820.png",
    length: "20KB",
    updateTime: "2020-09-29 09:22:16",
    isDirectory: false,
    nickname: "管理员",
  },
]);

/* 右键菜单点击事件 */
const onCtxMenuClick = (key: any, item: FileItem) => {
  console.log(item);
  if (key === "download") {
    console.log("点击了下载");
  } else if (key === "remove") {
    console.log("点击了删除");
  }
};
</script>
```

@tab JavaScript

```vue
<template>
  <ele-file-list :data="data">
    <template #context-menu="{ item }">
      <a-menu
        :selectable="false"
        @click="({ key }) => onCtxMenuClick(key, item)"
      >
        <a-menu-item key="download">下载</a-menu-item>
        <a-menu-item key="remove">删除</a-menu-item>
      </a-menu>
    </template>
  </ele-file-list>
</template>

<script setup>
import { ref } from "vue";

// 数据
const data = ref([
  {
    name: "20200708230820.png",
    url: "https://v1.eleadmin.com/api/file/20200610/20200708230820.png",
    thumbnail:
      "https://v1.eleadmin.com/api/file/thumbnail/20200610/20200708230820.png",
    length: "20KB",
    updateTime: "2020-09-29 09:22:16",
    isDirectory: false,
    nickname: "管理员",
  },
]);

/* 右键菜单点击事件 */
const onCtxMenuClick = (key, item) => {
  console.log(item);
  if (key === "download") {
    console.log("点击了下载");
  } else if (key === "remove") {
    console.log("点击了删除");
  }
};
</script>
```

:::

表格模式下使用插槽 `tool` 自定义操作按钮：

```vue
<template>
  <ele-file-list :data="data">
    <template #tool="{ item }">
      <ele-file-list-tool @click="view(item)">查看</ele-file-list-tool>
      <ele-file-list-tool>分享</ele-file-list-tool>
    </template>
  </ele-file-list>
</template>
```

注意，v1.6.0 之前的版本没有 ele-file-list-tool 组件可以使用：

```vue
<template>
  <ele-file-list :data="data">
    <template #tool="{ item }">
      <i
        class="ele-file-list-item-tool ele-text-primary"
        @click.stop="view(item)"
        >查看</i
      >
      <i class="ele-file-list-item-tool ele-text-primary">分享</i>
    </template>
  </ele-file-list>
</template>
```

![](/images/6-7-2.png)
