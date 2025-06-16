---
title: 公共样式
createTime: 2025/01/10 09:30:40
permalink: /EleAdminPro/hd9wg6wy/
---
## 4.1. 文字辅助类  

  关于辅助类说明，现在的 UI 框架都是作为组件库，不像传统框架那样提供一些辅助类 class， 但在实际项目中，一些简单的样式封装成组件过于浪费，比如趋势标记红色和绿色的上下箭头， 还比如情景色的文字，封装一个组件或自己去引入 less 变量写样式都没有加个 class 方便。

**文字颜色控制：**

| class                | 描述           |
| :------------------- | :------------- |
| ele-text             | 常规文字颜色   |
| ele-text-heading     | 标题级文字颜色 |
| ele-text-secondary   | 二级文字颜色   |
| ele-text-placeholder | 提示文字颜色   |

**文字情景色控制：**

| class            | 描述         |
| :--------------- | :----------- |
| ele-text-primary | 主色         |
| ele-text-success | 成功色(绿色) |
| ele-text-warning | 警告色(黄色) |
| ele-text-danger  | 危险色(红色) |
| ele-text-info    | 信息色(灰色) |

最常见的用法就是用在显示状态文字上：

```vue
<template>
    <div>
        <span v-if="status===0" class="ele-text-warning">待审核</span>
        <span v-else-if="status===1" class="ele-text-success">已通过</span>
        <span v-else class="ele-text-danger">未通过</span>
    </div>
</template>
```

还比如实现分析页中的红色和绿色的趋势标记的上下箭头，使用图标加上情景色的辅助类即可：

```vue
<template>
    <div>
        <span>周同比上升12% <caret-up-outlined class="ele-text-danger"/></span>
        <span>日同比下降11% <caret-down-outlined class="ele-text-success"/></span>
    </div>
</template>

<script setup>
    import { CaretUpOutlined, CaretDownOutlined } from '@ant-design/icons-vue';
</script>
```

在 a 标签上加情景色辅助类还会有 hover 和 active 的处理：

```vue
<template>
    <div>
        <a class="ele-text-success">同意</a>
        <a class="ele-text-danger">删除</a>
    </div>
</template>
```

**更多文字辅助类：**

| class           | 描述             |
| :-------------- | :--------------- |
| ele-text-center | 文字居中         |
| ele-text-left   | 文字居左         |
| ele-text-right  | 文字居右         |
| ele-text-delete | 文字加删除线     |
| ele-text-small  | 超小型文字(12px) |

**此外还有一些 html 标签也可以用来控制字体样式：**

- `h1`-`h6`，标题由大到小(26px、24px、22px、20px、18px、16px)
- `<b>Hello</b>`，字体加粗`<h1><b>Hello</b></h1>`
- `<i>Hello</i>`，斜体
- `<del>Hello</del>`，删除线
- `<ins>Hello</ins>`，下划线
- `<sup>2</sup>`，文字上标
- `<sub>2</sub>`，文字下标

<br/>

## 4.2. 背景和边框  

**背景色控制：**

| class          | 描述                     |
| :------------- | :----------------------- |
| ele-bg-primary | 主色背景                 |
| ele-bg-success | 成功色背景(绿色)         |
| ele-bg-warning | 警告色背景(黄色)         |
| ele-bg-danger  | 危险色背景(红色)         |
| ele-bg-info    | 信息色背景(灰色)         |
| ele-bg-white   | 白色背景(夜间主题是黑色) |
| ele-bg-base    | 底色背景(body背景颜色)   |

**边框色控制：**

| class              | 描述             |
| :----------------- | :--------------- |
| ele-border-primary | 主色边框         |
| ele-border-success | 成功色边框(绿色) |
| ele-border-warning | 警告色边框(黄色) |
| ele-border-danger  | 危险色边框(红色) |
| ele-border-info    | 信息色边框(灰色) |

这些只控制边框颜色，还需要加些 css 才能看到边框：

```vue
<template>
    <div class="ele-border-primary" style="border-width: 1px;border-style: solid;">Hello</div>
</template>
```

<br/>

## 4.3. flex 辅助类  

`ele-cell` 是对 flex 布局常用方式的简化，可以实现各种常见的布局，是非常实用的一个辅助类。

| class                 | 描述                                 |
| :-------------------- | :----------------------------------- |
| ele-cell              | 设置为 flex 布局并垂直居中对齐       |
| ele-cell-align-top    | 设置为顶部对齐                       |
| ele-cell-align-bottom | 设置为底部对齐                       |
| ele-cell-content      | 设置为 `flex: 1;` (占剩下内容的一份) |
| ele-cell-title        | 设置为媒体标题                       |
| ele-cell-desc         | 设置为媒体内容                       |

实现头像媒体效果：

```vue
<template>
    <div class="ele-cell">
        <img src="a.png" width="80px" height="80px"/>
        <div class="ele-cell-content">
            <div class="ele-cell-title">SunSmile</div>
            <div class="ele-cell-desc">2020-08-23 21:14:00</div>
        </div>
        <a-tag color="green">在线</a-tag>
    </div>
</template>
```

![](/images/4-1.png)

  `ele-cell` 下面的 `img` 和 `tag` 没有设置 `flex` 就会自适应宽度，`ele-cell-content` 会占剩下的空间的一份， 只有一个就是占剩下的空间的全部，所以不用浮动就达到了头像在左边，tag 在右边的效果了。

实现平分效果：

```vue
<template>
    <div class="ele-cell">
        <div class="ele-cell-content">图标</div>
        <a-divider type="vertical"/>
        <div class="ele-cell-content">图标</div>
        <a-divider type="vertical"/>
        <div class="ele-cell-content">图标</div>
        <a-divider type="vertical"/>
        <div class="ele-cell-content">图标</div>
    </div>
</template>
```

![](/images/4-2.png)

  flex 是一个非常实用 css 布局方式，如果还没有掌握，看看这篇文章 [flex 布局教程 - 阮一峰](http://www.ruanyifeng.com/blog/2015/07/flex-grammar.html) ， 如果当宽度较小的时候没有设置 flex 的元素被压缩了可以设置 `flex-shrink: 0` 解决，设置 `flex: 1` 中的表格无法自适应宽度应该加 `overflow: auto;` 解决。

<br/>

## 4.4. 其它辅助类  

**浮动控制：**

| class           | 描述     |
| :-------------- | :------- |
| ele-pull-left   | 左浮动   |
| ele-pull-right  | 右浮动   |
| ele-clear       | 清除浮动 |
| ele-clear-after | 清除浮动 |

```vue
<template>
    <div>
        <div>
            <div class="ele-pull-left"></div>
            <div class="ele-pull-right"></div>
            <div class="ele-clear"></div>
        </div>

        <div class="ele-clear-after">
            <div class="ele-pull-left"></div>
            <div class="ele-pull-right"></div>
        </div>
    </div>
</template>
```

`ele-clear` 写一个空的 div 加浮动元素后面，`ele-clear-after` 加父元素上，因为是利用伪元素实现的。

**占位符：**

| 元素 | 描述           |
| :--- | :------------- |
| em   | 占一个字符     |
| s    | 占 0.25 个字符 |

vue 中用不了连续的 ` &emsp;` 和 ` &nbsp;`，在 ele-admin-pro 中可以使用 `<em/>` 和 `<s/>` 替代。

**更多辅助类：**

| class               | 描述                                                   |
| :------------------ | :----------------------------------------------------- |
| ele-elip            | 设置文字单行显示，超出显示省略号                       |
| ele-block           | 设置为块级元素                                         |
| ele-inline-block    | 设置为 inline-block                                    |
| ele-inline          | 设置为行内元素                                         |
| ele-fluid           | 设置元素宽度为 100%                                    |
| ele-body            | 外层 body，有 16px 内间距                              |
| ele-body-card       | 外层 body 下面有多个 card 时加，调整多个card之间的间距 |
| ele-btn-icon        | 带 icon 的按钮调整间距                                 |
| ele-pop-wrap-higher | 级联选择器 popper 变高一点                             |
| ele-scrollbar-mini  | 小型滚动条                                             |
| ele-scrollbar-hide  | 隐藏滚动条，可以使用鼠标滚轮滚动                       |
| ele-scrollbar-hover | 鼠标进入才显示滚动条                                   |
| ele-form-detail     | 用于表单详情减少表单项上下间距，加在 form 上           |

- `ele-body`，用于外层容器，如果主题配置了内容不铺满，`ele-body` 会有固定宽度，并且水平居中。
- `ele-fluid`，有些下拉框、日期选择框宽度不是 100% ，而输入框宽度是 100% ，所以加这个来对齐。
- `ele-form-detail`，可以在 `系统管理/操作日志/详情` 弹窗中查看表单详情弹窗的例子。

<br/>

## 4.5. 底部工具栏  

| class                   | 描述         |
| :---------------------- | :----------- |
| ele-bottom-tool         | 底部工具栏   |
| ele-bottom-tool-actions | 右侧操作按钮 |

```vue
<template>
    <div class="ele-bottom-tool">
        <div>这是一段描述文字</div>
        <div class="ele-bottom-tool-actions">
            <a-button type="primary">提交</a-button>
        </div>
    </div>
</template>
```

![](/images/4-3.png)

用于固定在界面底部的操作栏，并且会自适应左边侧栏的宽度及折叠、展开状态的宽度变化。

<br/>

## 4.6. 静态表格  

如果需要使用原生的 table 加个 class 就能有与 ele-pro-table 一样的样式了，非常方便，当然也可以用 v-for 循环 tr 展示动态数据 `v1.9.0新增`。

| class            | 描述                                        |
| :--------------- | :------------------------------------------ |
| ele-table        | 与数据表格样式一样的静态表格，加在 table 上 |
| ele-table-stripe | 斑马线样式，加在 table 上                   |
| ele-table-border | 边框样式，加在 table 上                     |
| ele-table-medium | 中等尺寸，加在 table 上                     |
| ele-table-small  | 小型尺寸，加在 table 上                     |
| ele-table-mini   | 超小型尺寸，加在 table 上                   |
| ele-table-active | 行选中样式，加在 tr 上                      |

```vue
<template>
    <table class="ele-table ele-table-border ele-table-stripe ele-table-medium">
        <colgroup>
            <col width="180" />
            <col width="120" />
            <col />
        </colgroup>
        <thead>
            <tr>
                <th>产品</th>
                <th style="text-align: center;">发布时间</th>
                <th>描述</th>
            </tr>
        </thead>
        <tbody>
            <tr>
                <td>EasyWeb</td>
                <td style="text-align: center;">2018-10-11</td>
                <td>基于 jQuery、Layui 的后台管理模板</td>
            </tr>
            <tr class="ele-table-active">
                <td>EleAdmin</td>
                <td style="text-align: center;">2020-10-20</td>
                <td>基于 Vue2、ElementUI 的后台管理模板</td>
            </tr>
            <tr>
                <td>EleAdminPro</td>
                <td style="text-align: center;">2021-01-01</td>
                <td>基于 Vue3、AntDesignVue 的后台管理模板</td>
            </tr>
        </tbody>
    </table>
</template>
```

<br/>

## 4.7. 断点隐藏类  

断点隐藏类用于在某些条件下隐藏元素，可以添加在任何 DOM 元素或自定义组件上，v1.5.0 新增：

| class               | 描述                                       |
| :------------------ | :----------------------------------------- |
| hidden-xs-only      | 当视口在 xs 尺寸时隐藏                     |
| hidden-sm-only      | 当视口在 sm 尺寸时隐藏                     |
| hidden-sm-and-down  | 当视口在 sm 及以下尺寸时隐藏               |
| hidden-sm-and-up    | 当视口在 sm 及以上尺寸时隐藏               |
| hidden-md-only      | 当视口在 md 尺寸时隐藏                     |
| hidden-md-and-down  | 当视口在 md 及以下尺寸时隐藏               |
| hidden-md-and-up    | 当视口在 md 及以上尺寸时隐藏               |
| hidden-lg-only      | 当视口在 lg 尺寸时隐藏                     |
| hidden-lg-and-down  | 当视口在 lg 及以下尺寸时隐藏               |
| hidden-lg-and-up    | 当视口在 lg 及以上尺寸时隐藏               |
| hidden-xl-only      | 当视口在 xl 尺寸时隐藏                     |
| hidden-xl-and-down  | 当视口在 xl 及以下尺寸时隐藏               |
| hidden-xl-and-up    | 当视口在 xl 及以上尺寸时隐藏               |
| hidden-xxl-only     | 当视口在 xxl 尺寸时隐藏                    |
| hidden-xxl-and-down | 当视口在 xxl 及以下尺寸时隐藏 `v1.7.0新增` |
| hidden-xxl-and-up   | 当视口在 xxl 及以上尺寸时隐藏 `v1.7.0新增` |
| hidden-xxxl-only    | 当视口在 xxxl 尺寸时隐藏 `v1.7.0新增`      |

- xs - `<576px`
- sm - `≥576px`
- md - `≥768px`
- lg - `≥992px`
- xl - `≥1200px`
- xxl - `≥1600px`
- xxxl - `≥2000px`