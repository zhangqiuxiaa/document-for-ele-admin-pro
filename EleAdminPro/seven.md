---
title: 主题功能
createTime: 2025/01/10 09:30:40
permalink: /EleAdminPro/f3z4yksx/
---
## 7.1. 自定义主题  

当前实现在线切换主题是在 `src/styles/index.less` 中引入 dynamic 主题，原理是把颜色相关的 less 变量转为了 css 变量：

```less
@import "ele-admin-pro/es/style/themes/dynamic.less";
```

dynamic.less 依赖 less 插件来修改 less 变量为 css 变量，需要在 `vite.config.ts` 中配置插件，下载的代码中已经做好了配置：

```javascript
import { defineConfig } from 'vite';
import { DynamicAntdLess } from 'ele-admin-pro/lib/utils/dynamic-theme';
//......省略其它代码
export default defineConfig(({ command }) => {
    return {
        css: {
            preprocessorOptions: {
                less: {
                    javascriptEnabled: true,
                    plugins: [new DynamicAntdLess()]
                }
            }
        }
    };
});
```

如果需要修改主题抽屉的预设主题色，在 `src/layout/components/setting-drawer.vue` 中的 themes 增加即可 `v1.4.0 新增` ：

::: code-tabs

@tab TypeScript

```vue
<script lang="ts" setup>
    // .....省略其它代码
    const themes = ref<ThemeItem[]>([
        { name: '拂晓蓝', value: undefined, color: '#1890ff' },
        { name: '薄暮', value: '#5f80c7' },
        { name: '日暮', value: '#faad14' },
        { name: '火山', value: '#f5686f' },
        { name: '酱紫', value: '#9266f9' },
        { name: '明青', value: '#2bccce' },
        { name: '极光绿', value: '#33cc99' },
        { name: '极客蓝', value: '#32a2d4' },
        // 配置新加的主题, 前面默认的几个也可以删除
        { name: '我的主题', value: '#fb7299' }
        // value 为 undefined 表示是默认的主题色, 没有 value 时 color 用于显示在抽屉中的色块
    ]);
    // 注意如果你没有去掉国际化功能, name 建议写字母, 然后在 src/i18n/lang 里面写对应的名称
</script>
```

@tab JavaScript

```vue
<script setup>
    // .....省略其它代码
    const themes = ref([
        { name: '拂晓蓝', value: undefined, color: '#1890ff' },
        { name: '薄暮', value: '#5f80c7' },
        { name: '日暮', value: '#faad14' },
        { name: '火山', value: '#f5686f' },
        { name: '酱紫', value: '#9266f9' },
        { name: '明青', value: '#2bccce' },
        { name: '极光绿', value: '#33cc99' },
        { name: '极客蓝', value: '#32a2d4' },
        // 配置新加的主题, 前面默认的几个也可以删除
        { name: '我的主题', value: '#fb7299' }
        // value 为 undefined 表示是默认的主题色, 没有 value 时 color 用于显示在抽屉中的色块
    ]);
    // 注意如果你没有去掉国际化功能, name 建议写字母, 然后在 src/i18n/lang 里面写对应的名称
</script>
```

:::

然后就可以在抽屉中切换你配置的主题了，主题抽屉详细说明查看文档 6.1.9 ，需要修改默认主题色查看文档 7.5，覆盖 less 变量请查看文档 7.2。

<br/>

## 7.2. 覆盖主题变量  

如果要覆盖框架主题变量，写在 `src/styles/index.less` 中的最下面，下面是 ele-admin-pro 的全部样式变量：

```less
// 请务必全部看完本节文档后再去修改 src/styles/index.less (因为如果使用 dynamic.less 会略有不同)

/* 滚动条, v1.9.1新增 */
@scrollbar-thumb-color: #cfcfcf; // 滑块颜色
@scrollbar-thumb-hover-color: #b6b6b6; // 滑块 hover 颜色
@scrollbar-thumb-size: 12px; // 滑块大小
@scrollbar-thumb-border-size: 2px; // 滑块边框大小
@scrollbar-mini-thumb-size: 8px; // 小型滚动条滑块大小
@scrollbar-track-color: transparent; // 轨道颜色

/* Layout */
@layout-z-index: 99;  // 框架布局最小 z-index (页签栏[最小] -> 「顶栏|侧栏」[+2|+3|+1] -> 移动端遮罩层[+3])
@body-limit-width: 1200px; // 主体内容定宽
@body-max-width: 1400px;  // 关闭响应式布局后的最大宽度, v1.10.0新增
@dark-class: ele-admin-theme-dark;  // 暗黑模式 class

/* 侧栏 */
@sidebar-width: 210px; // 侧栏宽度
@sidebar-collapse-width: 48px; // 侧栏折叠后宽度
@sidebar-background: @layout-sider-background; // 侧栏背景, v1.9.1新增
@sidebar-light-background: @component-background; // 侧栏亮色背景, v1.9.1新增
@sidebar-light-shadow: 1px 3px 3px rgba(0, 21, 41, .08); // 侧栏亮色阴影
@sidebar-dark-shadow: 0 4px 4px rgba(0, 0, 0, .35); // 侧栏暗色阴影
@sidebar-transition-anim: .3s cubic-bezier(0.2, 0, 0, 1) 0s; // 侧栏过渡动画
@sidebar-transition: all @sidebar-transition-anim; // 侧边栏过渡效果
@sidebar-nav-width: 80px; // 双侧栏一级宽度
@sidebar-nav-padding: 0 @padding-xs; // 双侧栏一级内间距
@sidebar-collapse-nav-padding: 0 @padding-xss; // 双侧栏一级折叠后内间距
@sidebar-nav-pop-menu-margin: @padding-xs; // 双侧栏一级菜单 popover 的左右 margin
@sidebar-nav-font-size: @font-size-sm; // 双侧栏一级菜单 item 字体大小
@sidebar-nav-icon-font-size: @font-size-lg; // 双侧栏一级菜单 item 图标字体大小
@sidebar-nav-item-padding: @padding-sm 0; // 双侧栏一级菜单 item 内间距
@sidebar-collapse-nav-item-padding: @padding-sm 0; // 双侧栏一级菜单折叠后 item 内间距
@sidebar-nav-item-title-margin: (@padding-sm / 2) 0 0 0; // 双侧栏一级菜单 item 标题外间距
@sidebar-nav-item-margin: @padding-xss 0 @padding-xs 0; // 双侧栏一级菜单 item 外间距
@sidebar-collapse-nav-item-margin: @padding-xss 0 @padding-xs 0; // 双侧栏一级折叠后菜单 item 外间距
@sidebar-fixed-z-index: (@zindex-modal + 1001); // 侧栏固定时的 z-index, v1.7.0新增
@sidebar-scrollbar-thumb-color: tint(@layout-sider-background, 30%); // 侧栏滚动条滑块颜色, v1.9.1新增
@sidebar-scrollbar-thumb-hover-color: tint(@layout-sider-background, 30%); // 侧栏滚动条滑块 hover 颜色, v1.9.1新增
@sidebar-scrollbar-track-color: transparent; // 侧栏滚动条轨道颜色, v1.9.1新增
@sidebar-light-scrollbar-thumb-color: @scrollbar-thumb-color; // 侧栏亮色滚动条滑块颜色, v1.9.1新增
@sidebar-light-scrollbar-thumb-hover-color: @scrollbar-thumb-hover-color; // 侧栏亮色滚动条滑块 hover 颜色, v1.9.1新增
@sidebar-light-scrollbar-track-color: transparent; // 侧栏亮色滚动条轨道颜色, v1.9.1新增
@sidebar-colorful-icon-width: 26px; // 侧栏彩色图标宽高
@sidebar-colorful-icon-size: @font-size-base; // 侧栏彩色图标字体大小
@sidebar-colorful-color-1: #61b2fc; // 侧栏彩色图标颜色1, v1.9.1新增
@sidebar-colorful-color-2: #7dd733; // 侧栏彩色图标颜色2, v1.9.1新增
@sidebar-colorful-color-3: #32a2d4; // 侧栏彩色图标颜色3, v1.9.1新增
@sidebar-colorful-color-4: #7383cf; // 侧栏彩色图标颜色4, v1.9.1新增
@sidebar-colorful-color-5: #f5686f; // 侧栏彩色图标颜色5, v1.9.1新增
@sidebar-colorful-color-6: #2bccce; // 侧栏彩色图标颜色6, v1.9.1新增
@sidebar-colorful-color-7: #7dd733; // 侧栏彩色图标颜色7, v1.9.1新增
@sidebar-colorful-color-8: #faad14; // 侧栏彩色图标颜色8, v1.9.1新增

/* 顶栏 */
@header-height: 48px; // 顶栏高度
@header-background: @layout-sider-background; // 顶栏背景, v1.9.1新增
@header-light-background: @component-background; // 顶栏亮色背景, v1.9.1新增
@header-light-shadow: 0 1px 4px rgba(0, 21, 41, .08); // 顶栏亮色阴影
@header-dark-shadow: 0 1px 4px rgba(0, 0, 0, .1); // 顶栏暗色阴影
@header-avatar-size: 28px; // 顶栏头像大小
@header-tool-hover-bg: rgba(0, 0, 0, .025); // 顶栏工具按钮 hover 背景
@header-dark-tool-hover-bg: rgba(255, 255, 255, .05); // 顶栏暗色工具按钮 hover 背景

// logo
@logo-size: 30px; // logo 大小
@logo-font-size: @font-size-lg + 2px; // logo 文字大小
@logo-light-color: @heading-color; // logo 亮色文字颜色
@logo-dark-color: @menu-dark-selected-item-text-color; // logo 暗色文字颜色
@logo-light-shadow: 1px 2px 3px rgba(0, 21, 41, .08); // logo 亮色阴影
@logo-dark-shadow: 0 3px 4px rgba(0, 0, 0, .35); // logo 暗色阴影
@logo-font-family: -apple-system, Arial, 'Segoe UI Symbol', 'Noto Color Emoji'; // logo 字体

/* 页签栏 */
@tabs-height: 40px; // 页签栏高度
@tabs-card-padding: @padding-xs; // 页签栏卡片式间距

/* 返回顶部位置 */
@layout-back-top-right: 30px;
@layout-back-top-bottom: 30px;

/* Modal */
@modal-header-padding: 16px 24px;
@modal-close-x-height: 56px;
@modal-close-x-width: 56px;

/* FileList, v1.10.0新增 */
@file-list-selector-z-index: 999;
@file-list-selector-border-color: #0078d7;
@file-list-selector-background: rgba(0, 120, 215, 0.2);
@file-list-item-active-border-color: @primary-3;
@file-list-item-active-background: @item-active-bg;
@file-list-table-sort-color: hsla(0, 0%, 60%, 0.6);
@file-list-table-sort-active-color: @primary-color;
```

还可以覆盖 ant-design-vue 的样式变量，到这里查看 [themes/default.less](https://github.com/vueComponent/ant-design-vue/blob/master/components/style/themes/default.less) ，请务必先看完下面的文档。

注意 v1.6.0 之后组件样式默认是按需加载，在 index.less 覆盖变量是无效的，需要在 vite.config.ts 中使用 `modifyVars`：

```javascript
//......省略其它代码
export default defineConfig(({ command }) => {
    return {
        css: {
            preprocessorOptions: {
                less: {
                    javascriptEnabled: true,
                    plugins: [new DynamicAntdLess()],
                    // 在这里增加 modifyVars
                    modifyVars: {
                        'sidebar-width': '180px'  // 变量名不要带 @
                    }
                }
            }
        }
    };
});
```

需要注意 dynamic.less 为了方便在线切换主题把颜色相关的一些 less 变量修改为 css 变量，覆盖这些变量要这样写：

```less
@import "ele-admin-pro/es/style/themes/dynamic.less";

// 非颜色相关的变量还是使用 less 的覆盖方式写在最下面，如：
@border-radius-base: 4px;
// 按需加载非颜色相关的变量还是要写在 vite.config.ts 中

// 覆盖 css 变量也是写在最下面，按需加载也是写在这里
// 例如覆盖默认的蓝色主色，要写 10 个色阶
:root {
    --primary-1: #f0faff;
    --primary-2: #e3f5ff;
    --primary-3: #bae3ff;
    --primary-4: #91cfff;
    --primary-5: #69b9ff;
    --primary-6: #409eff;
    --primary-7: #2b7cd9;
    --primary-8: #1b5db3;
    --primary-9: #0e418c;
    --primary-10: #092b66;
}

// 还需要写一份暗黑主题的
.ele-admin-theme-dark {
    --primary-1: #111d2c;
    --primary-2: #112a45;
    --primary-3: #15395b;
    --primary-4: #164c7e;
    --primary-5: #1765ad;
    --primary-6: #177ddc;
    --primary-7: #3c9ae8;
    --primary-8: #65b7f3;
    --primary-9: #8dcff8;
    --primary-10: #b7e3fa;
}
```

推荐使用 ant-design 网站中的 [色板生成工具](https://ant-design.gitee.io/docs/spec/colors-cn) 生成色阶，以及 [暗黑模式色板色板生成工具](https://ant-design.gitee.io/docs/spec/dark-cn) 。

下面这些变量都被改成了 css 变量 var，这些变量不能使用 less 的方式覆盖，要按上面方式覆盖，注意覆盖时还要写一份暗黑主题的：

```less
// 每个 -- 开头的 css 变量都对应 @ 开头的同名 less 变量
:root {
    --blue-1: #e6f7ff;
    --blue-2: #bae7ff;
    --blue-3: #91d5ff;
    --blue-4: #69c0ff;
    --blue-5: #40a9ff;
    --blue-6: #1890ff;
    --blue-7: #096dd9;
    --blue-8: #0050b3;
    --blue-9: #003a8c;
    --blue-10: #002766;

    --purple-1: #f9f0ff;
    --purple-2: #efdbff;
    --purple-3: #d3adf7;
    --purple-4: #b37feb;
    --purple-5: #9254de;
    --purple-6: #722ed1;
    --purple-7: #531dab;
    --purple-8: #391085;
    --purple-9: #22075e;
    --purple-10: #120338;

    --cyan-1: #e6fffb;
    --cyan-2: #b5f5ec;
    --cyan-3: #87e8de;
    --cyan-4: #5cdbd3;
    --cyan-5: #36cfc9;
    --cyan-6: #13c2c2;
    --cyan-7: #08979c;
    --cyan-8: #006d75;
    --cyan-9: #00474f;
    --cyan-10: #002329;

    --green-1: #f6ffed;
    --green-2: #d9f7be;
    --green-3: #b7eb8f;
    --green-4: #95de64;
    --green-5: #73d13d;
    --green-6: #52c41a;
    --green-7: #389e0d;
    --green-8: #237804;
    --green-9: #135200;
    --green-10: #092b00;

    --pink-1: #fff0f6;
    --pink-2: #ffd6e7;
    --pink-3: #ffadd2;
    --pink-4: #ff85c0;
    --pink-5: #f759ab;
    --pink-6: #eb2f96;
    --pink-7: #c41d7f;
    --pink-8: #9e1068;
    --pink-9: #780650;
    --pink-10: #520339;

    --red-1: #fff1f0;
    --red-2: #ffccc7;
    --red-3: #ffa39e;
    --red-4: #ff7875;
    --red-5: #ff4d4f;
    --red-6: #f5222d;
    --red-7: #cf1322;
    --red-8: #a8071a;
    --red-9: #820014;
    --red-10: #5c0011;

    --orange-1: #fff7e6;
    --orange-2: #ffe7ba;
    --orange-3: #ffd591;
    --orange-4: #ffc069;
    --orange-5: #ffa940;
    --orange-6: #fa8c16;
    --orange-7: #d46b08;
    --orange-8: #ad4e00;
    --orange-9: #873800;
    --orange-10: #612500;

    --gold-1: #fffbe6;
    --gold-2: #fff1b8;
    --gold-3: #ffe58f;
    --gold-4: #ffd666;
    --gold-5: #ffc53d;
    --gold-6: #faad14;
    --gold-7: #d48806;
    --gold-8: #ad6800;
    --gold-9: #874d00;
    --gold-10: #613400;

    --primary-1: var(--blue-1);
    --primary-2: var(--blue-2);
    --primary-3: var(--blue-3);
    --primary-4: var(--blue-4);
    --primary-5: var(--blue-5);
    --primary-6: var(--blue-6);
    --primary-7: var(--blue-7);
    --primary-8: var(--blue-8);
    --primary-9: var(--blue-9);
    --primary-10: var(--blue-10);

    --primary-color: var(--primary-6);
    --primary-color-hover: var(--primary-5);
    --primary-color-active: var(--primary-7);
    --primary-color-outline: var(--primary-2);

    --info-color: var(--primary-color);

    --success-color: var(--green-6);
    --success-color-hover: var(--green-5);
    --success-color-active: var(--green-7);
    --success-color-outline: var(--green-2);

    --warning-color: var(--gold-6);
    --warning-color-hover: var(--gold-5);
    --warning-color-active: var(--gold-7);
    --warning-color-outline: var(--gold-2);

    --error-color: var(--red-5);
    --error-color-hover: var(--red-4);
    --error-color-active: var(--red-7);
    --error-color-outline: var(--red-2);

    --highlight-color: var(--red-5);
    --processing-color: var(--blue-6);

    --body-background: #fff;
    --component-background: #fff;
    --popover-background: @component-background;
    --popover-customize-border-color: @border-color-split;

    --text-color: fade(@black, 85%);
    --text-color-secondary: fade(@black, 45%);
    --text-color-inverse: @white;
    --icon-color-hover: fade(@black, 75%);
    --heading-color: fade(@black, 85%);

    --item-hover-bg: #f5f5f5;

    // Border color
    --border-color-base: hsv(0, 0, 85%);
    --border-color-split: hsv(0, 0, 94%);

    //
    --background-color-light: hsv(0, 0, 98%);
    --background-color-base: hsv(0, 0, 96%);

    // Disabled states
    --disabled-color: fade(#000, 25%);
    --disabled-bg: @background-color-base;
    --disabled-color-dark: fade(#fff, 35%);

    // Shadow
    --shadow-color: rgba(0, 0, 0, 0.15);
    --shadow-color-inverse: @component-background;
    --box-shadow-base: @shadow-2;
    --shadow-1-up: 0 -6px 16px -8px rgba(0, 0, 0, 0.08), 0 -9px 28px 0 rgba(0, 0, 0, 0.05), 0 -12px 48px 16px rgba(0, 0, 0, 0.03);
    --shadow-1-down: 0 6px 16px -8px rgba(0, 0, 0, 0.08), 0 9px 28px 0 rgba(0, 0, 0, 0.05), 0 12px 48px 16px rgba(0, 0, 0, 0.03);
    --shadow-1-left: -6px 0 16px -8px rgba(0, 0, 0, 0.08), -9px 0 28px 0 rgba(0, 0, 0, 0.05), -12px 0 48px 16px rgba(0, 0, 0, 0.03);
    --shadow-1-right: 6px 0 16px -8px rgba(0, 0, 0, 0.08), 9px 0 28px 0 rgba(0, 0, 0, 0.05), 12px 0 48px 16px rgba(0, 0, 0, 0.03);
    --shadow-2: 0 3px 6px -4px rgba(0, 0, 0, 0.12), 0 6px 16px 0 rgba(0, 0, 0, 0.08), 0 9px 28px 8px rgba(0, 0, 0, 0.05);

    // Buttons
    --btn-shadow: 0 2px 0 rgba(0, 0, 0, 0.015);
    --btn-primary-shadow: 0 2px 0 rgba(0, 0, 0, 0.045);
    --btn-text-shadow: 0 -1px 0 rgba(0, 0, 0, 0.12);
    --btn-default-bg: @component-background;
    --btn-default-ghost-color: @component-background;
    --btn-default-ghost-border: @component-background;
    --btn-text-hover-bg: rgba(0, 0, 0, 0.018);
    --btn-text-active-bg: rgba(0, 0, 0, 0.028);

    // Checkbox
    --checkbox-check-bg: @checkbox-check-color;

    // Descriptions
    --descriptions-bg: #fafafa;

    // Divider
    --divider-color: rgba(0, 0, 0, 6%);

    // Dropdown
    --dropdown-menu-submenu-disabled-bg: @component-background;

    // Radio
    --radio-dot-disabled-color: fade(@black, 20%);
    --radio-solid-checked-color: @component-background;

    // Radio buttons
    --radio-disabled-button-checked-bg: @disabled-active-bg;
    --radio-disabled-button-checked-color: @disabled-color;

    // Layout
    --layout-body-background: #f0f2f5;
    --layout-header-background: #001529;
    --layout-trigger-background: #002140;

    // Dropdown
    --dropdown-menu-bg: @component-background;

    // Input
    --input-placeholder-color: hsv(0, 0, 75%);
    --input-icon-color: @input-color;
    --input-bg: @component-background;
    --input-number-handler-active-bg: #f4f4f4;
    --input-icon-hover-color: fade(@black, 85%);

    // Mentions
    --mentions-dropdown-bg: @component-background;

    // Select
    --select-dropdown-bg: @component-background;
    --select-background: @component-background;
    --select-clear-background: @select-background;
    --select-selection-item-bg: @background-color-base;
    --select-selection-item-border-color: @border-color-split;
    --select-multiple-disabled-background: @input-disabled-bg;
    --select-multiple-item-disabled-color: #bfbfbf;
    --select-multiple-item-disabled-border-color: @select-border-color;

    // Cascader
    --cascader-bg: @component-background;
    --cascader-menu-bg: @component-background;
    --cascader-menu-border-color-split: @border-color-split;

    // Tooltip
    --tooltip-bg: rgba(0, 0, 0, 0.75);

    // Popover
    --popover-bg: @component-background;

    // Modal
    --modal-header-bg: @component-background;
    --modal-header-border-color-split: @border-color-split;
    --modal-content-bg: @component-background;
    --modal-footer-border-color-split: @border-color-split;

    // Progress
    --progress-steps-item-bg: #f3f3f3;

    // Menu
    --menu-popup-bg: @component-background;
    --menu-dark-bg: @layout-header-background;
    --menu-dark-inline-submenu-bg: #000c17;

    // Table
    --table-header-bg: @background-color-light;
    --table-header-sort-bg: @background-color-base;
    --table-body-sort-bg: #fafafa;
    --table-row-hover-bg: @background-color-light;
    --table-expanded-row-bg: #fbfbfb;
    --table-header-cell-split-color: rgba(0, 0, 0, 0.06);
    --table-header-sort-active-bg: rgba(0, 0, 0, 0.04);
    --table-fixed-header-sort-active-bg: hsv(0, 0, 96%);
    --table-header-filter-active-bg: rgba(0, 0, 0, 0.04);
    --table-filter-btns-bg: inherit;
    --table-filter-dropdown-bg: @component-background;
    --table-expand-icon-bg: @component-background;

    // TimePicker
    --picker-bg: @component-background;
    --picker-basic-cell-disabled-bg: @disabled-bg;
    --picker-border-color: @border-color-split;

    // Calendar
    --calendar-bg: @component-background;
    --calendar-input-bg: @input-bg;
    --calendar-border-color: @border-color-inverse;
    --calendar-full-bg: @calendar-bg;

    // Badge
    --badge-text-color: @component-background;

    // Rate
    --rate-star-bg: @border-color-split;

    // Card
    --card-actions-background: @component-background;
    --card-skeleton-bg: #cfd8dc;
    --card-shadow: 0 1px 2px -2px rgba(0, 0, 0, 0.16),
        0 3px 6px 0 rgba(0, 0, 0, 0.12), 0 5px 12px 4px rgba(0, 0, 0, 0.09);

    // Comment
    --comment-bg: inherit;
    --comment-author-time-color: #ccc;
    --comment-action-hover-color: #595959;

    // BackTop
    --back-top-bg: @text-color-secondary;
    --back-top-hover-bg: @text-color;

    // Avatar
    --avatar-bg: #ccc;

    // Switch
    --switch-bg: @component-background;

    // Pagination
    --pagination-item-bg: @component-background;
    --pagination-item-bg-active: @component-background;
    --pagination-item-link-bg: @component-background;
    --pagination-item-disabled-color-active: @white;
    --pagination-item-disabled-bg-active: @disabled-active-bg;
    --pagination-item-input-bg: @component-background;

    // PageHeader
    --page-header-back-color: #000;
    --page-header-ghost-bg: inherit;

    // Slider
    --slider-rail-background-color: @background-color-base;
    --slider-rail-background-color-hover: #e1e1e1;
    --slider-dot-border-color: @border-color-split;
    --slider-dot-border-color-active: @primary-4;

    // Tree
    --tree-bg: @component-background;

    // Skeleton
    --skeleton-to-color: shade(@skeleton-color, 5%);

    // Transfer
    --transfer-item-hover-bg: @item-hover-bg;

    // Message
    --message-notice-content-bg: @component-background;

    // List
    --list-customize-card-bg: @component-background;

    // Drawer
    --drawer-bg: @component-background;

    // Timeline
    --timeline-color: @border-color-split;
    --timeline-dot-color: @primary-color;

    // Steps
    --steps-nav-arrow-color: fade(@black, 25%);
    --steps-background: @component-background;

    // Notification
    --notification-bg: @component-background;

    // Image
    --image-preview-operation-disabled-color: rgba(255, 255, 255, 0.45);

    //
    --gradient-min: fade(#cfd8dc, 20%);
    --gradient-max: fade(#cfd8dc, 40%);

    // 滚动条
    --scrollbar-thumb-color: #cfcfcf;
    --scrollbar-thumb-hover-color: #b6b6b6;
    --scrollbar-track-color: transparent;

    // 侧栏
    --sidebar-background: @layout-sider-background;
    --sidebar-light-background: @component-background;
    --sidebar-light-shadow: 1px 3px 3px rgba(0, 21, 41, 0.08);
    --sidebar-dark-shadow: 0 4px 4px rgba(0, 0, 0, 0.35);
    --sidebar-scrollbar-thumb-color: tint(#001529, 30%);
    --sidebar-scrollbar-thumb-hover-color: tint(#001529, 40%);
    --sidebar-scrollbar-track-color: transparent;
    --sidebar-light-scrollbar-thumb-color: @scrollbar-thumb-color;
    --sidebar-light-scrollbar-thumb-hover-color: @scrollbar-thumb-hover-color;
    --sidebar-light-scrollbar-track-color: transparent;

    // 顶栏
    --header-background: @layout-sider-background;
    --header-light-background: @component-background;
    --header-light-shadow: 0 1px 4px rgba(0, 21, 41, 0.08);
    --header-dark-shadow: 0 1px 4px rgba(0, 0, 0, 0.1);
    --header-tool-hover-bg: rgba(0, 0, 0, 0.025);
    --header-dark-tool-hover-bg: rgba(255, 255, 255, 0.05);

    // logo
    --logo-light-shadow: 1px 2px 3px rgba(0, 21, 41, 0.08);
    --logo-dark-shadow: 0 3px 4px rgba(0, 0, 0, 0.35);
}
```

<br/>

## 7.3. 固定一套主题  

如果不需要在线切换主题修改 `src/styles/index.less`，去掉 dynamic.less：

```less
// 不需要在线切换主题请去掉这个
//@import "ele-admin-pro/es/style/themes/dynamic.less";

// 如果要固定为夜间主题可以增加这个
@import "ele-admin-pro/es/style/themes/dark.less";

// 如果需要覆盖框架样式变量写这里, 例如修改默认的主题色
@primary-color: #5f80c7;
```

然后还需要在 `vite.config.ts` 中去掉 `DynamicAntdLess` 的 plugin ：

```javascript
import { defineConfig } from 'vite';
//import { DynamicAntdLess } from 'ele-admin-pro/lib/utils/dynamic-theme';
//......省略其它代码
export default defineConfig(({ command }) => {
    return {
        css: {
            preprocessorOptions: {
                less: {
                    javascriptEnabled: true
                    //plugins: [new DynamicAntdLess()]
                }
            }
        }
    };
});
```

然后建议把主题抽屉中切换主题色的功能都删掉，不使用 `dynamic.less` 就可以完全按照 less 的规则来覆盖变量。

<br/>

## 7.4. 组件支持主题  
如果需要自定义组件的样式跟随主题切换，组件的样式不推荐写在组件的 vue 里面，新建一个 `src/styles/component.less` ：

```less
@import "ele-admin-pro/es/style/themes/default.less";  // 这个文件是所有的 less 变量

// 这里写组件的样式, 如：
.my-component {
    color: @primary-color;
}

// 所有自定义组件的样式都写在这里, 或者每个单独建 less 文件在这里 import
//@import "./my-component-1.less";
//@import "./my-component-2.less";
```

然后在 `src/styles/index.less` 中的最上面或者覆盖的变量之前引入这个文件：

```less
// 一定要在最上面或覆盖的变量之前引入
@import "./component.less";
// ......省略下面的代码
```

<br/>

## 7.5. 修改默认主题  

如果使用的是 dynamic.less 最简单的方法在 src/store/modules/theme.ts 中设置：

::: code-tabs

@tab TypeScript

```typescript
//......省略其它代码
const DEFAULT_STATE: ThemeState = Object.freeze({
    // 主题色
    //color: null,
    color: '#9266f9',
    // 是否暗黑模式
    darkMode: true,
});
```

@tab JavaScript

```javascript
//......省略其它代码
const DEFAULT_STATE = Object.freeze({
    // 主题色
    //color: null,
    color: '#9266f9',
    // 是否暗黑模式
    darkMode: true,
});
```

:::

上面这种方法是在程序加载后通过 js 切换的，如果不怕麻烦也可以在 src/styles/index.less 中的最下面增加代码来覆盖默认的 css 变量：

```less
@import "ele-admin-pro/es/style/dynamic.less";

// 需要覆盖完整的色阶变量，色阶生成方式查看文档 7.2
:root {
    --primary-1: #f9f0ff;
    --primary-2: #efdbff;
    --primary-3: #d3adf7;
    --primary-4: #b37feb;
    --primary-5: #9254de;
    --primary-6: #722ed1;
    --primary-7: #531dab;
    --primary-8: #391085;
    --primary-9: #22075e;
    --primary-10: #120338;
}

// 还需要写一份暗黑主题的
.ele-admin-theme-dark {
    --primary-1: #1a1325;
    --primary-2: #24163a;
    --primary-3: #301c4d;
    --primary-4: #3e2069;
    --primary-5: #51258f;
    --primary-6: #642ab5;
    --primary-7: #854eca;
    --primary-8: #ab7ae0;
    --primary-9: #cda8f0;
    --primary-10: #ebd7fa;
}
```

还要修改主题抽屉中的色块颜色，在 setting-drawer.vue 中，参考文档 7.1 ，如果没有使用 dynamic.less 按照 less 的方式覆盖即可，参考文档 7.3 。