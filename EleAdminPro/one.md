---
title: 开始使用
createTime: 2025/01/10 09:30:40
permalink: /EleAdminPro/iidfqfe8/
---
## 1.1. 技术栈  

以下是 EleAdminPro 所使用的前端技术栈，在上手之前应该先了解它们，Node.js 和 NPM 环境需要提前安装好，建议使用最新的稳定版本：

| 名称         | 简介                                | 文档                                                         |
| :----------- | :---------------------------------- | :----------------------------------------------------------- |
| Node.js      | JavaScript 运行环境(建议版本16.x.x) | [https://nodejs.org](https://nodejs.org/)                    |
| NPM          | 包管理工具(建议版本8.x.x)           | [https://www.npmjs.com.cn](https://www.npmjs.com.cn/)        |
| Vite         | 下一代前端开发与构建工具            | [https://cn.vitejs.dev](https://cn.vitejs.dev/)              |
| ES6          | ECMAScript 6                        | [https://es6.ruanyifeng.com](https://es6.ruanyifeng.com/)    |
| Vue.js       | 渐进式 JavaScript 框架              | [https://staging-cn.vuejs.org](https://staging-cn.vuejs.org/) |
| Vue Router   | Vue.js 官方路由管理器               | [https://next.router.vuejs.org](https://next.router.vuejs.org/) |
| Pinia        | 用于 Vue 的状态管理库               | [https://pinia.esm.dev](https://pinia.esm.dev/)              |
| Axios        | 简洁、易用且高效的 http 库          | [http://www.axios-js.com](http://www.axios-js.com/)          |
| Less         | CSS 预处理、扩展语言                | [https://lesscss.org](https://lesscss.org/)                  |
| AntDesignVue | AntDesign 的 Vue 实现               | [https://antdv.com](https://antdv.com/)                      |

<br/>

## 1.2. 项目结构  

从官网 [我的订单/产品下载](https://eleadmin.com/user/order) 中下载代码并解压后在项目的根目录执行如下命令进行安装依赖和运行：

```bash
# 先安装依赖
npm install

# 访问国外网站比较慢的用户可以在后面加--registry
npm install --registry=https://registry.npmmirror.com

# 运行项目
npm run dev

# 清除 vite 的缓存
npm run clean:cache
```

如果使用 yarn 或 pnpm 由于没有对应的 lock 文件如果运行出错把 package.json 中的版本号都去掉 `^` 然后重新安装再运行。

启动后在浏览器输入 [http://localhost:5173](http://localhost:5173/) 访问， 开发工具建议使用 VSCode 并安装 Volar(卸载Vetur)、Prettier、ESLint、DotENV 等插件。

下载产品时可查看自己的产品授权码并复制，然后需要在 `src/config/setting.ts` 中配置，永久授权用户的授权码没有域名和时长的限制：

```javascript
// EleAdminPro 授权码
export const LICENSE_CODE = '这里换成你的授权码';  // 在 App.vue 的 ele-config-provider 中引用了此配置
```

  授权码与版本号对应，升级版本也要更换授权码，一个大版本一个授权码， 例如 1.8.x -> 1.9.x 授权码不一样， 1.9.0 -> 1.9.1 是一样的，授权码纯前端校验不联网，不用担心隐私安全问题，无域名限制的授权码可用于 Electron、IP 内网。

<br/>

## 1.3. 项目结构  

js 版与 ts 版结构是一样的，后缀 `.js` 对应后缀 `.ts` 的同名文件：

```bash
|-public                               # 静态资源
|   |-tinymce                          # tinymce 编辑器静态资源
|   |-json
|   |   |-regions-data.json            # 省市区数据
|   |   |-china-provinces.geo.json     # 地图轮廓数据
|   |-favicon.ico                      # favicon 图标
|-src
|   |-api                              # 数据接口
|   |-assets                           # 图片等静态资源
|   |-components                       # 公共组件
|   |-config
|   |   |-setting.ts                   # 项目全局配置
|   |-i18n                             # 多语言配置
|   |   |-lang
|   |   |   |-en.ts                    # 英文语言
|   |   |   |-zh_CN.ts                 # 简体中文语言
|   |   |   |-zh_TW.ts                 # 繁体中文语言
|   |   |-index.ts
|   |   |-use-locale.ts
|   |-layout                           # 外层布局
|   |   |-components
|   |   |   |-header-tools.vue         # 顶栏右侧区域
|   |   |   |-header-notice.vue        # 顶栏消息通知
|   |   |   |-password-modal.vue       # 修改密码弹窗
|   |   |   |-page-footer.vue          # 全局页脚
|   |   |   |-setting-drawer.vue       # 主题设置抽屉
|   |   |   |-i18n-icon.vue            # 多语言切换组件
|   |   |   |-menu-title.vue           # 菜单标题加徽章组件
|   |   |-index.vue
|   |-router                           # 路由配置
|   |-store                            # 状态管理
|   |   |-modules
|   |   |   |-user.ts                  # 登录状态管理，用户信息、菜单、权限、角色
|   |   |   |-theme.ts                 # 主题风格相关的状态管理
|   |   |-index.ts
|   |-styles
|   |   |-transition
|   |   |-index.less                   # 全局样式
|   |-utils
|   |   |-page-tab-util.ts             # 页签 tab 操作方法
|   |   |-permission.ts                # 按钮级权限控制
|   |   |-request.ts                   # axios 实例
|   |   |-token-util.ts                # token 操作方法
|   |   |-use-search.ts                # 搜索表单 hook
|   |   |-on-size-change.ts            # 监听屏幕尺寸改变封装
|   |-views                            # 页面
|   |   |-dashboard                    # 控制台
|   |   |-system                       # 系统管理
|   |   |   |-user                     # 用户管理
|   |   |   |   |-components           # 页面组件拆分
|   |   |   |   |   |-user-search.vue  # 表格搜索表单
|   |   |   |   |   |-user-edit.vue    # 用户添加、修改弹窗
|   |   |   |   |   |-role-select.vue  # 编辑弹窗中的角色选择下拉框
|   |   |   |   |   |-user-import.vue  # 用户导入弹窗
|   |   |   |   |-index.vue
|   |   |   |-xxxx                     # 其它模块
|   |   |-exception                    # 异常页面
|   |   |-xxxxxx                       # 其它模板页面，不一一列举
|   |-App.vue                          # 入口页面
|   |-main.ts                          # 入口 js
|-.env                                 # 多环境配置
|-.env.development                     # 开发环境配置
|-.env.production                      # 生产环境配置
|-.eslintignore                        # eslint 忽略配置
|-.eslintrc.js                         # eslint 配置
|-.prettierignore                      # prettier 忽略配置
|-prettier.config.js                   # prettier 配置
|-package.json
|-tsconfig.json
|-vite.config.ts
|-components.d.ts
|-index.html
```

<br/>

## 1.4. 升级指南  

升级 ele-admin-pro 依赖通过以下命令：

```bash
npm install ele-admin-pro@1.11.0 -S --registry=https://registry.npmmirror.com
```

  升级指南只针对上个版本升到下个版本，所以最好及时更新，以免跨多个版本不便升级，没有说明的版本只按上面升级依赖， 如果很久没有升级跨版本较多需要升级到最新版建议直接把 `src/views/` 下的所有页面都复制到最新的模板中，再运行根据报错修改，以及修改自己改动过的其它文件。

**v1.11.0**

需要页签拖动排序布局中加新增的属性 `tabSortable` 和事件 `tabSortChange` 和 `page-tab-util.js` 加更新页签数据方法。

**v1.10.1**

只用按上面升级依赖，如果 `vue-i18n` 升级到最新版需要在 `src/i18n/index.ts` 中的 `createI18n` 时增加 `legacy: false` 。

**v1.10.0**

升级依赖后还可以根据需要替换以下文件(非必须)，js 版本为对应的同名文件：

1. src/utils/page-tab-util.ts 新增 setPageTabTitle 方法修改页签标题时支持同时修改浏览器页签
2. src/utils/document-title-util.ts 封装的用于修改浏览器页签标题的方法
3. src/utils/use-echarts.ts 新增的 echarts 自动切换主题、重置尺寸封装
4. src/utils/on-size-change.ts 进行简化以便给 use-echarts.ts 引入使用
5. src/components/TinymceEditor 重新封装此组件，用法不变，新组件不再依赖于 @tinymce/tinymce-vue
6. src/store/modules/theme.js 关闭页签 tabRemove 方法优化，关闭不存在的页签不弹出提示

如果用到了国际化还需要替换以下两个文件：

1. src/App.vue 路由切换自动更新浏览器标题的功能移到此以便于支持国际化
2. src/router/index.js 移除路由切换自动更新浏览器标题的相关代码

**v1.9.0**

还需要替换以下 4 个文件，js 版本为对应的同名文件：

- src/utils/page-tab-util.ts 优化 finishPageTab 方法以解决页签关闭后可能没有选中的 bug
- src/store/modules/theme.ts 修改 keepAliveInclude 方法以支持对单个路由配置 keepAlive
- src/config/setting.ts 增加 KEEP_ALIVE_EXCLUDES 用于配置不需要缓存的路由地址
- src/App.vue 给 ele-config-provider 增加 keep-alive 属性以支持 iframe 内链缓存

还需要添加 jsbarcode 依赖：

```bash
pnpm install jsbarcode -S
```

**v1.8.1**

需要更新以下几个文件，js 版本为对应的同名文件：

- src/store/modules/theme.ts
- src/utils/page-tab-util.ts
- src/layout/index.vue
- src/layout/components/menu-title.vue 菜单标题加徽章抽取到组件中

**v1.8.0**

需要同时升级 ant-design-vue 的版本：

```bash
yarn upgrade ant-design-vue@3.1.1 --registry=https://registry.npmmirror.com
```

新版组件库的国际化不共用 ant-design-vue 的 LocaleReceiver ，因为这不是公开的组件，经常变更较大，需要更新下面三个文件：

- src/i18n/use-locale.ts 新增的封装 ele-admin-pro、ant-design-vue、dayjs 国际化的配置
- src/App.vue 改为 setup 语法，内容改简单了，以及配置授权码
- src/config/setting.ts 增加 MAP_KEY、LICENSE_CODE、HOME_PATH、LAYOUT_PATH、REDIRECT_PATH 等

页签右键菜单、路由缓存刷新优化、路由切换动画等要更新以下几个文件：

- src/store/modules/theme.ts 增加 transitionName 相关及 keep-alive 相关的优化
- src/utils/page-tab-util.ts 刷新路由方法优化及各个方法对标签页右键菜单的支持
- src/styles/index.less 及 transition 目录，增加路由切换动画的样式
- src/router/index.ts 、routes.ts 动态路由注册时铺平以解决多级路由缓存问题
- src/components/RouterLayout/index.vue 增加路由切换动画
- src/components/RedirectLayout/index.ts 优化路由刷新的方法
- src/layout/index.vue 处理页签右键菜单事件

上面列的文件更新后即可成功升级，需要注意路由切换动画需要组件只有一个根节点，弹窗都要放到 div 里面去。

特别要注意新版开始增加了授权码校验，请到 [产品下载](https://eleadmin.com/user/order) 中获取授权码在 src/config/setting.ts 中配置：

```javascript
// EleAdminPro 授权码, 自带的只能用于演示, 正式项目请更换为自己的授权码
export const LICENSE_CODE = '这里换成你的授权码';
```

授权码纯前端校验，不联网，不用担心隐私安全问题，无域名限制的授权码可用于 Electron、IP 内网。

还可以升级 vite 版本启动会更快，vue 版本最高到 3.2.30 再高 ant-design-vue 会报警告，还可升级 bytemd、tinymce 等版本。

图标选择器需要配置图标数据，增加 data 属性，新版默认不包含数据：

```vue
<template>
    <ele-icon-picker :data="iconData" v-model:value="icon">
</template>

<script lang="ts" setup>
    import iconData from 'ele-admin-pro/es/ele-icon-picker/icons';
</script>
```

所有组件的类型定义为了规范都放在组件的 types 目录下，要注意修改：

```typescript
// 原来组件的类型引入
import type { DatasourceFunction, ColumnItem } from 'ele-admin-pro/es/ele-pro-table';
// 新版的引入，后面加 /types
import type { DatasourceFunction, ColumnItem } from 'ele-admin-pro/es/ele-pro-table/types';
```

**v1.7.0**

两处不兼容的更新：

- 弹窗拖动移除了之前插件式的形式，新版本使用组件式的形式，main.ts中要移除下面两行代码

  ```javascript
  import { ModalPlugin } from 'ele-admin-pro';
  app.use(ModalPlugin);
  // 新版本的弹窗组件使用方式及如何兼容旧版使用方式查看文档6.18
  ```

- 地图选择组件不再内置key，请前往高德地图官网自行申请key并配置，参考文档6.6.4

template中的更新：

- src/components/ByteMdEditor、ByteMdViewer是新增的markdown编辑器组件
- src/components/RegionsSelect城市选择组件优化了数据请求方式，只做一次数据请求
- src/store/modules/theme.ts、src/utils/page-tab-util.ts修复页签关闭的方法

如果需要菜单显示徽章更新下面的文件：

- src/layout/index.vue增加菜单标题插槽以及样式
- src/store/modules/user.ts增加更新菜单徽章数据的方法

其它更新：

- src/main.ts、vite.config.ts开发环境样式也改为按需加载
- components.d.ts简化代码直接引入组件库提供的global.d.ts

还要升级ant-design-vue到最新以及引入markdown组件需要的依赖：

```bash
yarn upgrade ant-design-vue@3.0.0-beta.9
yarn add bytemd @bytemd/plugin-gfm github-markdown-css
```

**v1.6.0**

  此版本改动较大，建议已开发的项目不要升级，新项目使用新的 ele-admin-pro-template 开发， 能力较强的用户如果要在已开发的项目中升级要注意 ant-design-vue 要至少升级至 3.0.0-alpha.12。

**v1.5.0**

除了升级ele-admin-pro依赖版本以下几个文件也需要替换：

- src/config/setting.js - 增加tabKeepAlive移除keepAliveList配置
- src/utils/page-tab-util.js - 新增的文件, 操作页签方法的封装
- src/layout/index.vue - 改动较大, 支持页签缓存, 代码优化
- src/store/modules/theme.js - 改动较大, 支持页签缓存, 代码优化
- src/store/modules/user.js - 页签tabs相关移到theme.js中, 便于实现页签缓存

由于页签tabs相关的代码从user.js移到theme.js, 要全局替换以下代码以确保兼容:

```javascript
this.$store.dispatch('user/tab
//替换为
this.$store.dispatch('theme/tab
// 更推荐替换为新增的page-tab-util.js封装的方法，查看文档5.1
```

开启页签keep-alive后有echarts的页面应该在`activated`生命周期中resize图表：

```vue
<template>
    <ele-chart ref="visitChart" style="height: 40px;" :option="visitChartOption" />
</template>

<script>
    export default {
        name: 'DashboardAnalysis',
        activated() {
            this.$refs['visitChart'].resize();
        }
    }
</script>
```

**v1.4.0**

除了升级ele-admin-pro依赖版本以下几个文件也需要替换：

- src/store/modules/theme.js - 切换主题方式优化, 改简单了
- src/style/index.less - 使用新提供的dynamic.less实现切换主题
- vue.config.js - 增加用于把less变量转成css变量的Less插件
- package.json中的所有依赖版本号都与最新的保持一致, 然后重新安装
- src/layout/notice.vue - 适配antd-vue最新版

src/styles/theme目录还有var.less可以删除，'vue@3.1.1'、'ant-design-vue@2.2.0-beta.4'这两个依赖版本号不要随意改。

notice.vue的改动：

```vue
<template>
    <a-dropdown v-model:visible="visible" :trigger="['click']">
        <!-- 这一块 padding 原来写在 badge 上现在写在下面图标上 -->
        <a-badge :count="allNum" class="ele-notice-trigger">
            <bell-outlined style="padding: 6px;"/>
        </a-badge>
        <!-- ......省略代码 -->
    </a-dropdown>
</template>
<!-- ......省略代码 -->
<style>
/* 增加这个样式 */
.ele-notice-trigger.ant-badge {
    color: inherit;
}
</style>
```

**v1.3.0**

除了升级ele-admin-pro依赖版本以下几个文件也需要替换：

- src/config/setting.js - 增加了repeatableTabs、sideMenuStyle两个配置
- src/layout/index.vue - 增加了repeatableTabs、sideMenuStyle配置, i18n方法优化
- src/store/modules/theme.js - 增加sideMenuStyle(侧边栏菜单布局)字段
- src/store/modules/user.js - 操作tab的方法都进行了修改, 格式化菜单数据封装了formatMenus方法
- src/router/index.js - 菜单转路由使用封装的menuToRoutes方法
- src/components/TinymceEditor/index.vue - css、js等路径增加BASE_URL以支持部署在子路径下
- src/views/system/user/info/index.vue - 新增的同路由不同参数页签展示多个的例子
- src/views/system/user/index.vue - 同上
- src/layout/notice.vue - 顶栏消息通知点击查看更多跳到消息页面并选中对应的数据
- src/views/user/message.vue - 同上, 是同路由不同参数页签只展示一个的例子

  新增的同路由不同参数页签展示一个还是多个的具体使用可在文档2.2、2.3中查看, 同路由不同参数只打开一个页签的例子是在顶栏的消息中， 点击通知、私信、待办中的查看更多会跳到消息列表页面，并会选中对应的数据，页签只会打开一个，同路由不同参数打开多个页签的例子是在系统管理用户管理中， 点击用户名会跳转到用户信息页面，点不同的用户会打开多个页签。   菜单数据格式化、菜单转路由格式的方法封装在ele-admin中主要是为了方便后续更新, 因为meta中会时常增加一些配置, 比如hideFooter、hideSidebar、closable以及本次的tabUnique都是陆续增加的。   src/layout/index.vue中i18n方法做了无匹配多语言数据返回null就会使用原有标题， 并且新增了参数三返回原始的menu数据，如果多语言从接口返回，可从此参数中取。

**v1.2.0**

除了升级ele-admin-pro依赖版本以下几个文件也需要替换：

- src/lang - 整个目录是新增的多语言功能
- src/main.js - 增加了多语言插件的集成
- src/layout/index.vue - 增加多语言、侧边栏隐藏、页脚隐藏等配置
- src/layout/header-right.vue - 增加多语言切换功能
- src/layout/footer.vue - 文字调多语言插件方法取
- src/config/setting.js - 增加侧边栏隐藏配置、页脚隐藏配置等字段
- src/store/modules/user.js - 修改了getMenus方法和tabSetTitle方法
- src/App.vue - 增加了国际化多语言切换的功能
- src/router/index.js - 内嵌页面配置不显示页脚，去掉404的标题
- src/views/user/info.vue - 重命名为profile.vue
- packages.json 中 vue 保持 3.0.5，ant-design-vue 保持 2.1.0，不要升级到最新版。

**v1.1.0**

 注意`package.json`里面的`name`不能为`ele-admin-pro`，请先修改，最新下载的项目已改成`ele-admin-pro-template`了， 因为组件都放到了`npm`中，所以升级要做些修改，如果是开发新项目使用最新的框架，已经开发的项目可按照下面步骤升级。

1.全局替换组件的引入方式：

```javascript
import EleAvatarList from '@/components/EleAvatarList'
// 全局替换为
import EleAvatarList from 'ele-admin-pro/packages/ele-avatar-list'

import EleChart from '@/components/EleChart'
// 全局替换为
import EleChart from 'ele-admin-pro/packages/ele-chart'

import EleWordCloud from '@/components/EleChart/EleWordCloud'
// 全局替换为
import EleWordCloud from 'ele-admin-pro/packages/ele-word-cloud'

import EleCountUp from '@/components/EleCountUp'
// 全局替换为
import EleCountUp from 'ele-admin-pro/packages/ele-count-up'

import EleCropperModal from '@/components/EleCropperModal'
// 全局替换为
import EleCropperModal from 'ele-admin-pro/packages/ele-cropper-modal'

import EleFileList from '@/components/EleFileList'
// 全局替换为
import EleFileList from 'ele-admin-pro/packages/ele-file-list'

import EleIconPicker from '@/components/EleIconPicker'
// 全局替换为
import EleIconPicker from 'ele-admin-pro/packages/ele-icon-picker'

import EleMapPicker from '@/components/EleMapPicker'
// 全局替换为
import EleMapPicker from 'ele-admin-pro/packages/ele-map-picker'

import EleTag from '@/components/EleTag'
// 全局替换为
import EleTag from 'ele-admin-pro/packages/ele-tag'

import EleEditTag from '@/components/EleTag/EleEditTag'
// 全局替换为
import EleEditTag from 'ele-admin-pro/packages/ele-edit-tag'

import EleToolbar from '@/components/EleToolbar'
// 全局替换为
import EleToolbar from 'ele-admin-pro/packages/ele-toolbar'

import util from '@/utils/util'
// 全局替换为
import util from 'ele-admin-pro/packages/util.js'

import printer from '@/utils/printer'
// 全局替换为
import printer from 'ele-admin-pro/packages/printer.js'

import cityData from '@/utils/cityData'
// 全局替换为
import cityData from 'ele-admin-pro/packages/regions.js'

import validate from '@/utils/validate'
// 全局替换为
import validate from 'ele-admin-pro/packages/validate.js'
```

2.把新项目中的`src/layout`目录复制到项目中

3.替换以下几个文件为新项目中的对应文件

- src/main.js
- src/config/setting.js
- src/config/axios.js已改为axios-config.js
- src/router/index.js
- src/store/modules/user.js、theme.js
- src/styles整个目录
- src/views/common/exception移到views下面
- src/views/login/forget.vue
- src/views/login/login.vue
- src/components/TinymceEditor/index.vue

4.修改package.json中所有依赖版本，然后`npm install`

5.然后运行项目，成功运行就完成升级了，以下文件可以删除

- src/components下除TinymceEditor都删除
- src/styles/eleadmin目录删除
- src/utils下除permission.js都删除
- src/views/common目录删除

不兼容的写法：

```javascript
// 在上个版本关闭当前tab需要这样写
this.$store.dispatch('user/tabRemove', this.$route.fullPath).then(last => {
    this.$router.push(last === -1 ? '/' : this.$store.state.user.tabs[last].path);
});

// 新版本进行了简化，方法如下
this.$store.dispatch('user/tabRemove', this.$route.fullPath).then(({lastPath}) => {
    this.$router.push(lastPath || '/');
});
```

<br/>

## 1.5. 其他项目中使用  

EleAdminPro 分为 `ele-admin-pro` (组件库) 和 `ele-admin-pro-template` (后台模板), 组件库发布在 `npm` 上，可以很方便的升级和用于其它项目。

如果需要在已有的 vue3.x 、 ant-design-vue3.x 、 less 的项目中使用，可通过如下命令安装 `ele-admin-pro` 组件库及相关的其它依赖：

```bash
# 建议用 1.6.0 及之后的版本，对 tree-shaking 、按需加载支持更友好
npm install ele-admin-pro -S

# 如果使用 ele-pro-table 、 ele-image-upload 组件需要再安装此依赖
npm install vuedraggable -S

# 如果使用 ele-xg-player 组件需要再安装此依赖
npm install xgplayer -S

# 如果使用 ele-bar-code 组件需要再安装此依赖
npm install jsbarcode -S

# 如果使用 ele-cropper-modal 组件需要再安装此依赖
npm install cropperjs -S

# 如果使用 ele-count-up 组件需要再安装此依赖
npm install countup.js -S

# 如果使用 ele-map-picker 组件需要再安装此依赖
npm install @amap/amap-jsapi-loader -S
```

在使用之前要通过 `ele-config-provider` 组件全局配置授权码，一般写在 `App.vue` 中：

```vue
<template>
    <ele-config-provider license="这里放你自己的授权码">
        <router-view />
    </ele-config-provider>
</template>

<script lang="ts" setup>
    import { EleConfigProvider } from 'ele-admin-pro';
</script>
```

然后就可以使用 EleAdminPro 的所有组件了，例如使用 `ele-pro-table` 组件：

```vue
<template>
    <ele-pro-table
        ref="tableRef"
        row-key="userId"
        :columns="columns"
        :datasource="datasource"
    >
        <template #bodyCell="{ column, record }">
            <template v-if="column.key === 'action'">
                <a @click="onEdit(record)">修改</a>
            </template>
        </template>
    </ele-pro-table>
</template>

<script setup>
    import { ref } from 'vue';
    import { EleProTable } from 'ele-admin-pro';
    import 'ele-admin-pro/es/ele-pro-table/style';

    // 表格ref
    const tableRef = ref(null);

    // 表格列
    const columns = ref([
        {
            key: 'index',
            width: 48,
            align: 'center',
            hideInSetting: true,
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
            key: 'action',
            title: '操作'
        }
    ]);

    // 表格数据源
    const datasource = ({ page, limit, orders }) => {
        console.log({ page, limit, orders });
        return new Promise((resolve, reject) => {
            setTimeout(() => {
                resolve({
                    list: [
                        { userId: 1, username: 'admin', nickname: '管理员' },
                        { userId: 2, username: 'user01', nickname: '用户一' }
                    ],
                    count: 2 
                });
            }, 1500);
        });
    };

    const onEdit = (row) => {
        console.log(row);
    };
</script>
```

如果需要使用章节 4 介绍的公共样式需要全局引入对应的样式文件：

```less
// 公共样式
@import 'ele-admin-pro/es/style/common.less';
// 断点隐藏类
@import 'ele-admin-pro/es/style/display.less';
```