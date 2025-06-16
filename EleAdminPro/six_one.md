---
title: 框架布局
createTime: 2025/01/10 09:30:40
permalink: /EleAdminPro/ly6jpx3q/
---

## 6.1.1. 快速使用

组件名 `ele-pro-layout` ，是框架外层布局的组件，支持多种布局风格切换，使用方式：

::: code-tabs

@tab TypeScript

```vue
<template>
  <ele-pro-layout :menus="menus" v-model:collapse="collapse">
    <router-view />
  </ele-pro-layout>
</template>

<script lang="ts" setup>
import { ref } from "vue";
import type { MenuItem } from "ele-admin-pro";

const collapse = ref(false);

const menus = ref<MenuItem[]>([
  {
    path: "/system",
    meta: {
      title: "系统管理",
      icon: "setting-outlined",
    },
    children: [
      {
        path: "/system/user",
        meta: {
          title: "用户管理",
          icon: "team-outlined",
        },
      },
    ],
  },
]);
</script>
```

@tab JavaScript

```vue
<template>
  <ele-pro-layout :menus="menus" v-model:collapse="collapse">
    <router-view />
  </ele-pro-layout>
</template>

<script setup>
import { ref } from "vue";

const collapse = ref(false);

const menus = ref([
  {
    path: "/system",
    meta: {
      title: "系统管理",
      icon: "setting-outlined",
    },
    children: [
      {
        path: "/system/user",
        meta: {
          title: "用户管理",
          icon: "team-outlined",
        },
      },
    ],
  },
]);
</script>
```

:::

上面只是一个简单的使用示例，在 `src/layout/index.vue` 中是完整的使用方式，包括整合状态管理、设置抽屉、国际化等。

<br/>

## 6.1.2. 属性列表

| 属性名                  | 类型              | 说明                                               | 默认值      |
| :---------------------- | :---------------- | :------------------------------------------------- | :---------- |
| menus                   | Array: MenuItem[] | 菜单数据                                           | []          |
| tabs                    | Array: TabItem[]  | 标签页数据                                         | []          |
| collapse                | Boolean           | 是否折叠侧栏                                       | false       |
| sideNavCollapse         | Boolean           | 侧栏双菜单是否折叠一级菜单 `v1.4.0新增`            | false       |
| bodyFullscreen          | Boolean           | 内容区域是否全屏 `v1.6.0新增`                      | false       |
| showCollapse            | Boolean           | 是否显示顶栏折叠按钮                               | true        |
| showRefresh             | Boolean           | 是否显示顶栏刷新按钮                               | true        |
| showBreadcrumb          | Boolean           | 是否显示顶栏面包屑导航 `v1.2.0新增`                | true        |
| showNavCollapse         | Boolean           | 侧栏双菜单是否显示一级的折叠按钮 `v1.4.0新增`      | true        |
| showTabs                | Boolean           | 是否开启页签栏                                     | true        |
| showFooter              | Boolean           | 是否显示页脚                                       | false       |
| showBackTop             | Boolean           | 是否需要返回顶部组件 `v1.4.0新增`                  | true        |
| headStyle               | String            | 顶栏风格: light(亮色), dark(暗色), primary(主色)   | light       |
| sideStyle               | String            | 侧栏风格: light(亮色), dark(暗色)                  | light       |
| layoutStyle             | String            | 布局风格: side(默认), top(顶栏菜单), mix(混合菜单) | side        |
| sideMenuStyle           | String            | 侧栏菜单模式, mix(双菜单) `v1.3.0新增`             | 'default'   |
| tabStyle                | String            | 页签栏风格: default(默认), dot(圆点), card(卡片)   | default     |
| fixedHeader             | Boolean           | 是否固定顶栏                                       | false       |
| fixedSidebar            | Boolean           | 是否固定侧栏                                       | true        |
| fixedBody               | Boolean           | 是否固定主体                                       | false       |
| bodyFull                | Boolean           | 内容区域宽度是否铺满                               | true        |
| logoAutoSize            | Boolean           | logo 是否自适应宽度                                | false       |
| colorfulIcon            | Boolean           | 侧栏是否彩色图标                                   | false       |
| sideUniqueOpen          | Boolean           | 侧栏是否只保持一个子菜单展开                       | true        |
| inlineIndent            | Number            | 菜单缩进量 `v1.6.0新增`                            | 16          |
| breadcrumbSeparator     | String            | 顶栏面包屑导航分隔符                               | '/'         |
| backTopVisibilityHeight | Number            | 返回顶部可见时的滚动高度 `v1.4.0新增`              | 200         |
| contentFullscreen       | Boolean           | 内容区域全屏时是否不显示页签栏 `v1.4.0新增`        | false       |
| fullscreenExitOnEsc     | Boolean           | 内容区域全屏时是否按返回键退出 `v1.4.0新增`        | false       |
| clickReload             | Boolean           | 是否再次点击选中页签刷新 `v1.2.0新增`              | false       |
| projectName             | String            | 项目名称，显示在 logo 中                           |             |
| homeTitle               | String            | 主页标题，页签和面包屑会显示                       | '主页'      |
| homePath                | String            | 主页路由的 path                                    | '/'         |
| redirectPath            | String            | 刷新路由的 path                                    | '/redirect' |
| layoutPath              | String            | 外层布局的路由地址                                 | '/'         |
| hideFooters             | Array: string[]   | 不显示全局页脚的路由地址 `v1.2.0新增`              | []          |
| hideSidebars            | Array: string[]   | 不显示侧栏的路由地址 `v1.2.0新增`                  | []          |
| repeatableTabs          | Array: string[]   | 可重复打开页签的路由地址 `v1.3.0新增`              | []          |
| locale                  | String            | 国际化的当前语言 `v1.6.0新增`                      |             |
| i18n                    | Function          | 自定义菜单标题的多语言处理 `v1.2.0新增`            |             |
| sideDefaultOpeneds      | Array: string[]   | 侧栏默认展开的菜单 key `v1.6.0新增`                |             |
| autoScrollTop           | Boolean           | 固定主体时切换路由自动滚动到顶部 `v1.8.0新增`      | true        |
| tabContextMenu          | Boolean           | 是否开启页签右键菜单 `v1.9.0新增`                  | true        |
| styleResponsive         | Boolean           | 是否开启响应式 `v1.10.0新增`                       | true        |
| tabSortable             | Boolean           | 页签是否支持拖动排序 `v1.11.0新增`                 | false       |
| tabArrow                | Boolean           | 页签栏是否显示左右滚动箭头 `v1.11.0新增`           | false       |
| needSetting             | Boolean           | 是否需要主题设置按钮 `v1.6.0移除`                  | true        |
| showSetting             | Boolean           | 是否打开主题设置抽屉 `v1.6.0移除`                  | false       |
| tips                    | String            | 主题抽屉底部提示文字 `v1.6.0移除`                  |             |
| themes                  | Array             | 主题列表 `v1.6.0移除`                              |             |
| color                   | String            | 当前主题色 `v1.6.0移除`                            | null        |
| darkMode                | Boolean           | 是否暗黑模式 `v1.6.0移除`                          | false       |
| weakMode                | Boolean           | 是否色弱模式 `v1.6.0移除`                          | false       |
| showContent             | Boolean           | 是否显示主体部分 `v1.6.0移除`                      | true        |
| useDeep                 | Boolean           | 是否使用递归方式渲染菜单 `v1.6.0移除`              | false       |
| colorPicker             | Boolean           | 主题设置抽屉是否显示颜色选择器 `v1.6.0移除`        | true        |
| predefineColors         | Array             | 主题设置抽屉颜色选择器预设颜色 `v1.6.0移除`        |             |

**tabs** ，已打开的多页签的数据，数据格式为：

```javascript
[
  {
    key: "/system/user", // 页签的唯一 key
    title: "用户管理", // 页签的标题
    path: "/system/user", // 页签对应的路由地址(不含参数)
    fullPath: "/system/user", // 页签对应的完整路由地址
    closable: true, // 页签是否可关闭
    meta: {}, // 路由元信息
  },
];
```

**menus** ，菜单数据，不同布局风格会自动分割菜单，数据格式为：

```javascript
[
  {
    path: "/system", // 路由地址，也可以为 `http://` 、`https://` 、`//` 开头的外链
    meta: { title: "系统管理", icon: "setting-outlined" }, // 路由元信息
    children: [
      // 子级菜单
      {
        path: "/system/user",
        meta: {
          title: "用户管理", // 菜单标题
          icon: "team-outlined", // 非必须，菜单图标
          hide: false, // 非必须，是否不显示
        },
      },
    ],
  },
];
```

**redirectPath** ，避免刷新时做无用的混合导航菜单分割和计算顶栏、侧栏菜单的选中；

**locale** ，当前的国际化语言，改变时会重新调用 i18n 方法；

**i18n** ，自定义菜单标题的多语言处理：

```javascript
import { useI18n } from "vue-i18n";

const { t } = useI18n();

const i18n = (path, key, menu) => {
  // 如果需要菜单标题多语言数据从接口返回可用 menu 参数获取对应的多语言标题
  return t("route." + key + "._name");
};
```

- path   菜单的路由地址
- key   对 path 处理后的数据，如 `/system/user-info` 会处理成 `system.userInfo`
- menu   原始菜单数据，也可能为空注意判断

**showContent** ，是否显示主体部分 `v1.6.0移除` ：

比如权限控制指令，如果权限还没有请求到，页面就渲染出来，页面渲染的时候就会认为没有权限， 所以应该在权限还没有请求完成的时候不渲染页面，通过此参数控制。

<br/>

## 6.1.3. 图标插槽

通过插槽 `icon` 来自定义菜单图标的渲染 `v1.6.0新增`：

```vue
<template>
  <ele-pro-layout>
    <!-- 之前版本只能使用 antd 的图标，v1.6.0 开始通过插槽自定义图标渲染 -->
    <template #icon="{ icon }">
      <component :is="icon" class="ant-menu-item-icon" />
    </template>
  </ele-pro-layout>
</template>

<script>
import {
  SettingOutlined,
  TeamOutlined,
  IdcardOutlined,
} from "@ant-design/icons-vue";

export default {
  name: "EleLayout",
  components: {
    SettingOutlined,
    TeamOutlined,
    IdcardOutlined,
  },
};
</script>
```

为了减少打包体积上面代码示例是只引入菜单用到的图标，如果觉得这样写麻烦，可以改为全部引入：

```vue
<template>
  <ele-pro-layout>
    <template #icon="{ icon }">
      <component :is="icon" class="ant-menu-item-icon" />
    </template>
  </ele-pro-layout>
</template>

<script>
import * as icons from "@ant-design/icons-vue";

export default {
  name: "EleLayout",
  components: icons,
};
</script>
```

<br/>

## 6.1.4. 插槽列表

| 插槽名       | 说明                                        | 参数            |
| :----------- | :------------------------------------------ | :-------------- |
| default      | 默认插槽                                    | 无              |
| logo         | 自定义 logo 图标                            | 无              |
| right        | 自定义顶栏右侧区域                          | 无              |
| left         | 自定义顶栏左侧区域                          | 无              |
| center       | 自定义顶栏面包屑导航后面的区域 `v1.2.0新增` | 无              |
| top          | 自定义侧栏顶部                              | 无              |
| bottom       | 自定义侧栏底部                              | 无              |
| nav-top      | 自定义双侧栏一级的顶部 `v1.3.0新增`         | 无              |
| nav-bottom   | 自定义双侧栏一级的底部 `v1.3.0新增`         | 无              |
| footer       | 自定义全局页脚                              | 无              |
| dropdown     | 自定义页签栏右侧下拉菜单 `v1.6.0新增`       | refresh, active |
| icon         | 自定义菜单图标的渲染 `v1.6.0新增`           | icon, item      |
| title        | 自定义菜单标题的渲染 `v1.7.0新增`           | title, item     |
| nav-title    | 自定义双侧栏一级菜单标题的渲染 `v1.7.0新增` | title, item     |
| top-title    | 自定义顶栏菜单标题的渲染 `v1.7.0新增`       | title, item     |
| tab-title    | 自定义页签标题的渲染 `v1.7.0新增`           | title, item     |
| context-menu | 自定义页签右键菜单 `v1.8.0新增`             | tabKey, item    |

通过具名插槽 logo 自定义 logo：

```vue
<template>
  <ele-pro-layout>
    <template #logo>
      <img src="@/assets/logo.svg" alt="logo" />
    </template>
  </ele-pro-layout>
</template>
```

通过具名插槽 right 自定义顶栏右侧区域：

```vue
<template>
  <ele-pro-layout>
    <template #right>
      <div class="ele-admin-header-tool">
        <div class="ele-admin-header-tool-item">欢迎你，管理员！</div>
      </div>
    </template>
  </ele-pro-layout>
</template>
```

通过具名插槽 left 自定义顶栏左侧区域：

```vue
<template>
  <ele-pro-layout>
    <template #left>
      <div class="ele-admin-header-tool-item">
        <plus-outlined />
      </div>
    </template>
  </ele-pro-layout>
</template>

<script setup>
import { PlusOutlined } from "@ant-design/icons-vue";
</script>
```

通过具名插槽 center 自定义顶栏面包屑导航后面的区域 `v1.2.0新增` ：

```vue
<template>
  <ele-pro-layout>
    <template #center>
      <div class="ele-admin-header-tool-item">Hello</div>
    </template>
  </ele-pro-layout>
</template>
```

通过具名插槽 top 自定义侧栏顶部：

```vue
<template>
  <ele-pro-layout>
    <template #top>
      <div>Hello</div>
    </template>
    <!-- 开启双侧栏时一级的顶部，v1.3.0新增 -->
    <template #nav-top>
      <div>Hello</div>
    </template>
  </ele-pro-layout>
</template>
```

通过具名插槽 bottom 自定义侧栏底部：

```vue
<template>
  <ele-pro-layout>
    <template #bottom>
      <div>Hello</div>
    </template>
    <!-- 开启双排菜单时一级菜单的底部，v1.3.0新增 -->
    <template #nav-bottom>
      <div>Hello</div>
    </template>
  </ele-pro-layout>
</template>
```

通过具名插槽 footer 自定义全局页脚：

```vue
<template>
  <ele-pro-layout>
    <template #footer>
      <div>Copyright©2022 eleadmin.com all rights reserved.</div>
    </template>
  </ele-pro-layout>
</template>
```

也可以通过默认插槽添加其它自定义内容：

```vue
<template>
  <ele-pro-layout>
    <!-- 比如路由出口 router-view ，注意 v1.5.0 以及之前版本内部已写了路由出口，不要重复写 -->
    <router-view />
    <!-- 比如放修改密码的弹窗、一些悬浮在右侧的按钮等 -->
    <div style="position: fixed;right: 0;top: 50%;transform: translateY(-50%);">
      <div
        style="color: #fff;padding: 10px;cursor: pointer;"
        class="ele-bg-primary"
      >
        设置
      </div>
    </div>
  </ele-pro-layout>
</template>
```

自定义页签栏右侧下拉菜单：

::: code-tabs

@tab TypeScript

```vue
<template>
  <ele-pro-layout>
    <template #dropdown="{ active, refresh }">
      <a-menu
        :selectable="false"
        @click="({ key }) => onDropMenuClick(key, active)"
      >
        <a-menu-item key="m1">菜单一</a-menu-item>
        <a-menu-item key="m2">菜单二</a-menu-item>
        <a-menu-item key="m3" :disabled="active === '/'">关闭全部</a-menu-item>
        <a-menu-item key="m4" v-if="refresh">刷新当前</a-menu-item>
      </a-menu>
    </template>
  </ele-pro-layout>
</template>

<script lang="ts" setup>
import { removeAllPageTab } from "@/utils/page-tab-util";

const onDropMenuClick = (key: string | number, active: string) => {
  console.log("key:", key);
  if (key === "m3") {
    removeAllPageTab(active);
  }
};
</script>
```

@tab JavaScript

```vue
<template>
  <ele-pro-layout>
    <template #dropdown="{ active, refresh }">
      <a-menu
        :selectable="false"
        @click="({ key }) => onDropMenuClick(key, active)"
      >
        <a-menu-item key="m1">菜单一</a-menu-item>
        <a-menu-item key="m2">菜单二</a-menu-item>
        <a-menu-item key="m3" :disabled="active === '/'">关闭全部</a-menu-item>
        <a-menu-item key="m4" v-if="refresh">刷新当前</a-menu-item>
      </a-menu>
    </template>
  </ele-pro-layout>
</template>

<script setup>
import { removeAllPageTab } from "@/utils/page-tab-util";

const onDropMenuClick = (key, active) => {
  console.log("key:", key);
  if (key === "m3") {
    removeAllPageTab(active);
  }
};
</script>
```

:::

自定义页签鼠标右键菜单 `v1.8.0新增` ：

::: code-tabs

@tab TypeScript

```vue
<template>
  <ele-pro-layout>
    <template #context-menu="{ tabKey, item, active }">
      <a-menu
        :selectable="false"
        @click="({ key }) => onCtxMenuClick(key, tabKey, item, active)"
      >
        <a-menu-item key="m1">菜单一</a-menu-item>
        <a-menu-item key="m2">菜单二</a-menu-item>
        <a-menu-item key="m3" :disabled="tabKey === '/'">关闭当前</a-menu-item>
      </a-menu>
    </template>
  </ele-pro-layout>
</template>

<script lang="ts" setup>
import { removePageTab } from "@/utils/page-tab-util";
import type { TabItem } from "ele-admin-pro";

const onCtxMenuClick = (
  key: string | number,
  tabKey: string,
  item: TabItem,
  active: string
) => {
  console.log("key:", key); // 菜单 item key
  console.log("tabKey:", tabKey); // 页签 key
  console.log("item:", item); // 页签数据
  if (key === "m3") {
    removePageTab({ key: tabKey, active });
  }
};
</script>
```

@tab JavaScript

```vue
<template>
  <ele-pro-layout>
    <template #context-menu="{ tabKey, item, active }">
      <a-menu
        :selectable="false"
        @click="({ key }) => onCtxMenuClick(key, tabKey, item, active)"
      >
        <a-menu-item key="m1">菜单一</a-menu-item>
        <a-menu-item key="m2">菜单二</a-menu-item>
        <a-menu-item key="m3" :disabled="tabKey === '/'">关闭当前</a-menu-item>
      </a-menu>
    </template>
  </ele-pro-layout>
</template>

<script setup>
import { removePageTab } from "@/utils/page-tab-util";

const onCtxMenuClick = (key, tabKey, item, active) => {
  console.log("key:", key); // 菜单 item key
  console.log("tabKey:", tabKey); // 页签 key
  console.log("item:", item); // 页签数据
  if (key === "m3") {
    removePageTab({ key: tabKey, active });
  }
};
</script>
```

混合导航布局模式下在顶栏菜单后面添加自定义元素，在 `src/layout/components/header-tools.vue` 中添加即可：

```vue
<!-- 顶栏右侧区域 -->
<template>
  <!-- 这里要加样式 flex: 1 -->
  <div class="ele-admin-header-tool" style="flex: 1">
    <div style="flex: 1">
      <div>Hello</div>
    </div>
    <!-- ......省略其它代码 -->
    <div
      class="ele-admin-header-tool-item hidden-sm-and-down"
      @click="toggleFullscreen"
    >
      <fullscreen-exit-outlined v-if="fullscreen" />
      <fullscreen-outlined v-else />
    </div>
  </div>
</template>
```

:::

然后还需要在 `src/styles/index.less` 的最下面添加样式取消顶栏菜单原来设置的 `flex: 1` ：

```less
// ......省略其它代码
.ele-admin-header .ele-admin-header-nav {
  flex: none;
}
```

<br/>

## 6.1.5. 标题插槽

自定义菜单标题的插槽 `title`、`nav-title`、`top-title`，v1.7.0 新增，例如实现徽章、小红点展示等：

::: code-tabs

@tab TypeScript

```vue
<template>
  <ele-pro-layout>
    <!-- 自定义侧栏菜单标题插槽 -->
    <template #title="{ title, item }">
      <span>{{ title }}</span>
      <div class="ele-menu-badge" v-if="item && item.meta && item.meta.badge">
        <a-badge
          :count="item.meta.badge"
          :number-style="{ background: item.meta.badgeColor as string }"
        />
      </div>
    </template>
    <!-- 自定义顶栏菜单标题插槽 -->
    <template #top-title="{ title, item }">
      <span>{{ title }}</span>
      <div class="ele-menu-badge" v-if="item && item.meta && item.meta.badge">
        <a-badge
          :count="item.meta.badge"
          :number-style="{ background: item.meta.badgeColor as string }"
        />
      </div>
    </template>
    <!-- 自定义双侧栏时一级侧栏菜单标题插槽 -->
    <template #nav-title="{ title, item }">
      <span>{{ title }}</span>
      <div class="ele-menu-badge" v-if="item && item.meta && item.meta.badge">
        <a-badge
          :count="item.meta.badge"
          :number-style="{ background: item.meta.badgeColor as string }"
        />
      </div>
    </template>
  </ele-pro-layout>
</template>
<!-- ......样式省略 -->
```

@tab JavaScript

```vue
<template>
  <ele-pro-layout>
    <!-- 自定义侧栏菜单标题插槽 -->
    <template #title="{ title, item }">
      <span>{{ title }}</span>
      <div class="ele-menu-badge" v-if="item && item.meta && item.meta.badge">
        <a-badge
          :count="item.meta.badge"
          :number-style="{ background: item.meta.badgeColor }"
        />
      </div>
    </template>
    <!-- 自定义顶栏菜单标题插槽 -->
    <template #top-title="{ title, item }">
      <span>{{ title }}</span>
      <div class="ele-menu-badge" v-if="item && item.meta && item.meta.badge">
        <a-badge
          :count="item.meta.badge"
          :number-style="{ background: item.meta.badgeColor }"
        />
      </div>
    </template>
    <!-- 自定义双侧栏时一级侧栏菜单标题插槽 -->
    <template #nav-title="{ title, item }">
      <span>{{ title }}</span>
      <div class="ele-menu-badge" v-if="item && item.meta && item.meta.badge">
        <a-badge
          :count="item.meta.badge"
          :number-style="{ background: item.meta.badgeColor }"
        />
      </div>
    </template>
  </ele-pro-layout>
</template>
<!-- ......样式省略 -->
```

:::

面的示例是把徽章的数据放在菜单的 `meta.badge` 中，可以根据自己的接口修改，然后在 `store/modules/user.ts` 中封装更新徽章数据的方法：

::: code-tabs

@tab TypeScript

```typescript
import { formatTreeData } from "ele-admin-pro";

export const useUserStore = defineStore({
  actions: {
    /**
     * 更新菜单的 badge
     * @param path 菜单的 path
     * @param value 徽章的值
     * @param color 徽章的背景颜色
     */
    setMenuBadge(path: string, value?: number | string, color?: string) {
      this.menus = formatTreeData(this.menus, (m) => {
        if (path === m.path) {
          return Object.assign({}, m, {
            meta: Object.assign({}, m.meta, {
              badge: value,
              badgeColor: color,
            }),
          });
        }
        return m;
      });
    },
  },
});
```

@tab JavaScript

```javascript
import { formatTreeData } from "ele-admin-pro";

export const useUserStore = defineStore({
  actions: {
    /**
     * 更新菜单的 badge
     * @param path 菜单的 path
     * @param value 徽章的值
     * @param color 徽章的背景颜色
     */
    setMenuBadge(path, value, color) {
      this.menus = formatTreeData(this.menus, (m) => {
        if (path === m.path) {
          return Object.assign({}, m, {
            meta: Object.assign({}, m.meta, {
              badge: value,
              badgeColor: color,
            }),
          });
        }
        return m;
      });
    },
  },
});
```

:::

在其它地方调用方法更新徽章数据示例：

```vue
<template>
  <a-button @click="setBadge">更新徽章数据</a-button>
</template>

<script setup>
import { useUserStore } from "@/store/modules/user";

const userStore = useUserStore();

const setBadge = () => {
  userStore.setMenuBadge("/system", "New", "#f90");
};
</script>
```

<br/>

## 6.1.6. 页签插槽

自定义页签标题的插槽 `tab-title` ， v1.7.0 新增，例如实现在标题文字前面增加图标：

```vue
<template>
  <ele-pro-layout>
    <template #tab-title="{ title, item }">
      <!-- item 是对应的页签数据，主页为空 -->
      <HomeOutlined v-if="!item" style="margin-right: 4px" />
      <component
        v-else-if="item.meta && item.meta.icon"
        :is="item.meta.icon"
        style="margin-right: 4px"
      />
      <span>{{ title }}</span>
    </template>
  </ele-pro-layout>
</template>

<script setup>
import { HomeOutlined } from "@ant-design/icons-vue";
</script>

<script>
import * as MenuIcons from "./menu-icons";

export default {
  name: "EleLayout",
  components: MenuIcons,
};
</script>
```

**如果需要去掉页签栏的左右箭头和右边下拉箭头在 src/index.less 中加样式隐藏即可：**

```less
// 去掉左右箭头
.ele-admin-tabs-arrow {
  display: none;
}

// 去掉右边下拉箭头
.ele-admin-tabs-drop {
  display: none;
}

// 让页签栏左右留点间距
.ele-admin-tabs {
  padding-left: 16px;
  padding-right: 16px;
}
```

<br/>

## 6.1.7. 事件列表

因为 vue 的数据是从父到子组件单流向，`ele-pro-layout` 不能直接改变传递的属性的值，应该监听下面的事件去改变传递的对应属性的值。

| 事件名称                 | 说明                                      | 回调参数                                       |
| :----------------------- | :---------------------------------------- | :--------------------------------------------- |
| update:collapse          | 更新 collapse 属性 `v1.6.0新增`           | collapse: boolean                              |
| update:side-nav-collapse | 更新 sideNavCollapse 属性 `v1.4.0新增`    | sideNavCollapse: boolean                       |
| update:body-fullscreen   | 更新 bodyFullscreen 属性 `v1.6.0新增`     | bodyFullscreen: boolean                        |
| tab-add                  | 添加 tab                                  | data: TabItem                                  |
| tab-remove               | 移除 tab                                  | option: TabRemoveOption                        |
| tab-remove-left          | 移除左边 tab                              | option: TabRemoveOption                        |
| tab-remove-right         | 移除右边 tab                              | option: TabRemoveOption                        |
| tab-remove-other         | 移除其它 tab                              | option: TabRemoveOption                        |
| tab-remove-all           | 移除全部 tab                              | active: string                                 |
| reload-page              | 刷新路由 `v1.5.0新增`                     | 无                                             |
| logo-click               | logo 点击事件 `v1.2.0新增`                | isHome: boolean                                |
| screen-size-change       | 屏幕尺寸改变事件 `v1.6.0新增`             | 无                                             |
| set-home-components      | 设置主页路由的组件名 `v1.6.0新增`         | components: string[]                           |
| tab-context-menu         | 页签右键菜单点击事件 `v1.8.0新增`         | {key, tabKey, item, active} : TabCtxMenuOption |
| side-menu-title-click    | 侧栏父级菜单点击事件 `v1.8.0新增`         | key: string                                    |
| head-menu-title-click    | 顶栏父级菜单点击事件 `v1.8.0新增`         | key: string                                    |
| side-nav-title-click     | 双侧栏一级的父级菜单点击事件 `v1.8.0新增` | key: string                                    |
| tabSortChange            | 页签拖动排序改变事件 `v1.11.0新增`        | data(排序后的数据)                             |
| change-color             | 切换主题色 `v1.6.0移除`                   | theme: string                                  |
| change-style             | 切换主题风格 `v1.6.0移除`                 | obj(包含{key: string, value: boolean/number})  |
| change-dark-mode         | 切换暗黑模式 `v1.6.0移除`                 | darkMode: boolean                              |
| change-weak-mode         | 切换色弱模式 `v1.6.0移除`                 | weakMode: boolean                              |
| update-collapse          | 更新侧栏折叠状态 `v1.6.0移除`             | collapse: boolean                              |
| update-screen            | 屏幕尺寸改变事件 `v1.6.0移除`             | width, height                                  |

添加页签事件参数 TabItem 类型定义，每个字段的含义详见文档 8.2 ：

```typescript
interface TabItem {
  // 页签的唯一 key
  key?: string;
  // 页签的标题
  title?: string;
  // 页签对应的路由地址(不含参数)
  path?: string;
  // 页签对应的完整路由地址(含参数)
  fullPath?: string;
  // 页签是否可以关闭
  closable?: boolean;
  // 页签对应的组件名称，用于 keep-alive
  components?: string[];
  // 页签对应的路由元数据
  meta?: MenuMeta;
}
```

页签删除事件参数 TabRemoveOption 类型定义：

```typescript
interface TabRemoveOption {
  // 页签的 key
  key: string;
  // 当前选中的页签的 key
  active?: string;
}
```

页签右键菜单点击事件参数 TabCtxMenuOption 类型定义：

```typescript
interface TabCtxMenuOption {
  // 菜单 key
  key: string;
  // 页签的 key
  tabKey: string;
  // 页签数据
  item?: TabItem;
  // 当前选中的页签的 key
  active?: string;
}
```

screen-size-change 事件：

`ele-pro-layout` 会监听浏览器尺寸变化用于做布局的自适应，同时也会回调这个事件， 像 echarts 等也需要监听屏幕尺寸变化做自适应处理， 所以就在 `src/store/modules/theme.ts` 状态管理中用 `screenWidth` 和 `screenHeight` 记录屏幕当前尺寸， 就不需要每个界面都去监听了。

<br/>

## 6.1.8. 默认展开

通过 `sideDefaultOpeneds` 属性设置侧栏菜单的默认展开，v1.6.0 新增：

```vue
<template>
  <ele-pro-layout
    :side-default-openeds="sideDefaultOpeneds"
    :menus="menus"
  ></ele-pro-layout>
</template>

<script setup>
import { ref, computed } from "vue";
import { storeToRefs } from "pinia";
import { useUserStore } from "@/store/modules/user";
//import { eachTreeData } from 'ele-admin-pro';

const userStore = useUserStore();

const { menus } = storeToRefs(userStore);

const sideDefaultOpeneds = computed(() => {
  return menus.value.filter((d) => !!d.children?.length).map((d) => d.path);
  // 上面代码只能展开第一层级菜单，需要展开所有层级要使用 eachTreeData 遍历
  /* const openeds = [];
        eachTreeData(menus.value, (d) => {
            if(d.children?.length) {
                openeds.push(d.path);
            }
        });
        return openeds; */
});
</script>
```

默认展开所有菜单再配合关闭侧栏排他展开体验效果才会更好，可以到主题设置抽屉关闭，或这里加 `:side-unique-open="false"`。

<br/>

## 6.1.9. 主题抽屉

位于 `src/layout/components/setting-drawer.vue` 中，v1.6.0 开始放在项目中便于自定义，之前版本是在依赖中。

如果需要修改抽屉中的主题色列表直接修改源码中的 themes 的值，框架里的 name 用的是字母，是集成了国际化的：

```javascript
[
  { name: "拂晓蓝", value: undefined, color: "#409eff" },
  { name: "薄暮", value: "#5f80c7" },
  { name: "日暮", value: "#faad14" },
  { name: "火山", value: "#f5686f" },
  { name: "酱紫", value: "#9266f9" },
  { name: "明青", value: "#2bccce" },
  { name: "极光绿", value: "#33cc99" },
  { name: "极客蓝", value: "#32a2d4" },
  // 也可以增加主题, 前面默认的几个也可以删除
  { name: "我的主题", value: "#fb7299" },
  //{ name: 'my1', value: '#fb7299' }
];
```

- name   主题色名称
- value   主题颜色值，默认主题可以设为 undefined
- color   用于显示设置抽屉界面主题色的小方块，value 为 undefined 时使用

如果是 v1.6.0 之前的版本，themes 是在 ele-pro-layout 的属性中配置，值与上面是一样的方式。

如果需要主题的名字适配国际化，最好写字母，然后在 `src/i18n/lang/zh_CN.ts` 及其它语言文件中增加对应的语言：

```javascript
export default {
  //......省略其它代码
  layout: {
    setting: {
      colors: {
        default: "拂晓蓝",
        dust: "薄暮",
        sunset: "日暮",
        volcano: "火山",
        purple: "酱紫",
        cyan: "明青",
        green: "极光绿",
        geekblue: "极客蓝",
        // 也可以增加主题, 前面默认的几个也可以删除
        my1: "我的主题",
      },
    },
  },
};
```
