---
title: 打印组件
createTime: 2025/01/10 09:30:40
permalink: /EleAdminPro/qr4kkl10/
---
## 6.10.1. 打印当前页面  

```vue
<template>
    <a-button @click="doPrint">打印</a-button>
</template>

<script setup>
    import { printThis } from 'ele-admin-pro';

    const doPrint = () => {
        printThis();  // 打印当前页面

        // 也可以设置一些参数
        /*printThis({
            horizontal: false, // 是否横向打印
            blank: false // 是否打开新页面打印
        });*/
    };
</script>
```

类型定义：

```typescript
declare function printThis(option?: PrintOption): Window | null;

interface PrintOption {
    hide?: string | HTMLElement | Array<string | HTMLElement>;
    horizontal?: boolean;
    iePreview?: boolean;
    blank?: boolean;
    close?: boolean;
    margin?: string | number;
    title?: string;
}
```

| 参数       | 类型                    | 说明                                                        | 示例值                   |
| :--------- | :---------------------- | :---------------------------------------------------------- | :----------------------- |
| hide       | String, `Array<String>` | 打印时隐藏的元素                                            | `['.ant-btn', '#btn01']` |
| horizontal | Boolean                 | 是否横向打印                                                | false                    |
| blank      | Boolean                 | 是否打开新页面打印                                          | false                    |
| close      | Boolean                 | 如果是打开新页面，打印完是否关闭                            | true                     |
| iePreview  | Boolean                 | 是否兼容 ie 打印预览                                        | true                     |
| margin     | String, NUmber          | 页面间距                                                    | 0                        |
| title      | String                  | 页面标题，浏览器打印设置显示页眉会显示页面标题 `v1.6.0新增` |                          |

参数都是可选参数，如果不需要修改默认值就不要写，比如 `horizontal` 不写打印时可以选择纸张方向，写了就会固定横向或纵向不能选择。

注意，v1.5.0 以及之前的版本请按照如下方式使用：

```javascript
import printer from 'ele-admin-pro/packages/printer.js';
// v1.0.0 的版本使用下面方式引入
//import printer from '@/utils/printer.js';

printer.print();

printer.print({ horizontal: false });
```

<br/>

## 6.10.2. 设置排除元素  

```vue
<template>
    <div>
        <div class="ele-printer-hide">非打印内容</div>
        <div class="ele-printer-hide-none">非打印内容</div>
    </div>
</template>
```

加 class 即可，可以跟 `hide` 参数叠加使用，`ele-printer-hide` 只是隐藏但位置保留不变，`ele-printer-hide-none` 是隐藏且不占位置。

<br/>

## 6.10.3. 打印指定区域  

通过方法 `printElement` 可以打印指定区域，传入区域对应的 `el` 即可，`v1.8.0新增`：

::: code-tabs

@tab TypeScript

```vue
<template>
    <div>
        <div ref="printRef" class="demo-print-group">
            <div class="demo-print-div">示例示例示例示例示例</div>
            <div class="demo-print-right ele-bg-white">
                <div>
                    <ele-tag size="mini" color="blue">示例</ele-tag>
                    <ele-tag size="mini" color="green">示例</ele-tag>
                    <ele-tag size="mini" color="orange">示例</ele-tag>
                </div>
                <div style="margin-top: 12px">
                    <a-input v-model:value="text" />
                </div>
            </div>
        </div>
        <div style="margin-top: 20px">
            <a-button @click="print">打印</a-button>
        </div>
    </div>
</template>

<script lang="ts" setup>
    import { ref } from 'vue';
    import { printElement } from 'ele-admin-pro';

    const text = ref('示例示例示例');

    const printRef = ref<HTMLElement | null>(null);

    const print = () => {
        // 如果 ref 是组件可以用 printRef.value.$el
        printElement(printRef.value);
        //printElement(printRef.value, { });  // 参数二加更多参数同 printThis 方法
    }
</script>

<style lang="less" scoped>
    .demo-print-group {
        display: flex;
        align-items: center;
    }

    .demo-print-div {
        background: #096dd9;
        color: #fff;
        font-size: 18px;
        text-align: center;
        padding: 40px 0;
        flex: 1;
        border: 2px solid #096dd9;
        height: 120px;
        box-sizing: border-box;
        border-top-left-radius: 8px;
        border-bottom-left-radius: 8px;
    }

    .demo-print-right {
        flex: 1;
        padding: 20px;
        border: 2px solid #096dd9;
        height: 120px;
        box-sizing: border-box;
        border-top-right-radius: 8px;
        border-bottom-right-radius: 8px;
    }
</style>
```

@tab JavaScript

```vue
<template>
    <div>
        <div ref="printRef" class="demo-print-group">
            <div class="demo-print-div">示例示例示例示例示例</div>
            <div class="demo-print-right ele-bg-white">
                <div>
                    <ele-tag size="mini" color="blue">示例</ele-tag>
                    <ele-tag size="mini" color="green">示例</ele-tag>
                    <ele-tag size="mini" color="orange">示例</ele-tag>
                </div>
                <div style="margin-top: 12px">
                    <a-input v-model:value="text" />
                </div>
            </div>
        </div>
        <div style="margin-top: 20px">
            <a-button @click="print">打印</a-button>
        </div>
    </div>
</template>

<script setup>
    import { ref } from 'vue';
    import { printElement } from 'ele-admin-pro';

    const text = ref('示例示例示例');

    const printRef = ref(null);

    const print = () => {
        // 如果 ref 是组件可以用 printRef.value.$el
        printElement(printRef.value);
        //printElement(printRef.value, { });  // 参数二加更多参数同 printThis 方法
    }
</script>

<style lang="less" scoped>
    .demo-print-group {
        display: flex;
        align-items: center;
    }

    .demo-print-div {
        background: #096dd9;
        color: #fff;
        font-size: 18px;
        text-align: center;
        padding: 40px 0;
        flex: 1;
        border: 2px solid #096dd9;
        height: 120px;
        box-sizing: border-box;
        border-top-left-radius: 8px;
        border-bottom-left-radius: 8px;
    }

    .demo-print-right {
        flex: 1;
        padding: 20px;
        border: 2px solid #096dd9;
        height: 120px;
        box-sizing: border-box;
        border-top-right-radius: 8px;
        border-bottom-right-radius: 8px;
    }
</style>
```

:::

<br/>

## 6.10.4. 打印任意内容  

```vue
<template>
    <a-button @click="doPrint">打印</a-button>
</template>

<script setup>
    import { printHtml } from 'ele-admin-pro';

    const doPrint = () => {
        const pWindow = printHtml({
            html: '<span>xxxx</span>', // 要打印的内容
            style: '<style>span { color: red; }</style>',  // 页面样式
            title: '', // 页面标题，浏览器打印设置显示页眉会显示页面标题
            horizontal: true, // 是否横向打印
            blank: true, // 是否打开新页面打印
            close: true, // 如果是打开新页面，打印完是否关闭
            print: true, // 如果是打开新窗口是否自动打印
            iePreview: true, // 是否兼容 ie 打印预览
            loading: true, // 是否显示 loading
            before: () => { // 打印开始的回调
            },
            done: () => { // 打印完成的回调
            },
            margin: 0, // 页间距
            header: '', // 页眉 html
            footer: '' // 页脚 html
        });

        // 如果 print 为 false 关闭自动打印，需要手动触发打印
        //pWindow.print();
    };
</script>
```

类型定义：

```typescript
declare function printHtml(option: PrintHtmlOption): Window | null;

interface PrintHtmlOption extends PrintOption {
    html?: string;
    print?: boolean;
    loading?: boolean;
    header?: string;
    footer?: string;
    before?: () => void;
    done?: () => void;
    style?: string;
}
// PrintOption 的类型定义请在文档 6.10.1 中查看
```

| 参数    | 类型     | 说明               | 示例值 |
| :------ | :------- | :----------------- | :----- |
| html    | String   | 要打印的 html 内容 |        |
| print   | Boolean  | 是否立即打印       | true   |
| loading | Boolean  | 是否显示加载层     | true   |
| header  | String   | 页眉               |        |
| footer  | String   | 页脚               |        |
| before  | Function | 打印开始的回调     |        |
| done    | Function | 打印完成的回调     |        |
| style   | String   | 打印的自定义样式   |        |

打印表格时可以给 table 加 `ele-printer-table` 这个 class 设置表格的样式：`<table class="ele-printer-table">` 。

**实用小技巧**

使用单引号拼接 html 可能不优雅，可以使用模板字符串(反引号)：

```javascript
import { escape } from 'ele-admin-pro';

const url = 'https://eleadmin.com';
const html = `
    <div>
        <div class="title">EleAdmin后台管理模板</div>
        <div>
            <!-- 可以用表达式取js变量 -->
            <p>网站 ${ url }</p>
            <!-- 如果要拼接来自后端的内容要注意避免 xss 攻击 -->
            <p>网站 ${ escape(url) }</p>
        </div>
    </div>
    <!-- 可以直接写样式 -->
    <style>
        .title {
            color: #000;
        }
    </style>
    <!-- 也可以引用外部样式 -->
    <link rel="stylesheet" href="/xxx.css">
`;

printHtml({
    html: html
});
```

引用外部 css 样式需要放在项目的 `/public` 目录下面，注意不能是 scss、less 等。

<br/>

## 6.10.5. 分页打印内容  

```javascript
import { printPage } from 'ele-admin-pro';

const pWindow = printPage({
    pages: [ // 要打印的内容
        '<span>xxxx</span>',
        '<span>xxxx</span>'
    ],
    style: '<style>span{ color: red; }</style>', // 页面样式
    horizontal: true, // 是否横向打印
    blank: true, // 是否打开新页面打印
    close: true, // 如果是打开新页面，打印完是否关闭
    iePreview: true, // 是否兼容 ie 打印预览
    print: true, // 如果是打开新窗口是否自动打印
    padding: undefined, // 页面间距
    isDebug: false, // 调试模式
    height: undefined, // 每一页高度
    width: undefined, // 每一页宽度
    loading: true, // 是否显示 loading
    before: () => { // 打印开始的回调
    },
    done: () => { // 打印完成的回调
    },
    margin: 0, // 打印机页间距
    header: '', // 页眉 html
    footer: '', // 页脚 html
    title: '' // 页面标题，浏览器打印设置显示页眉会显示页面标题，不要标题还应该设置 blank: true
});
```

类型定义：

```typescript
declare function printPage(option: PrintPageOption): Window | null;

interface PrintPageOption extends PrintHtmlOption {
    pages?: string[];
    padding?: string | number;
    width?: string | number;
    height?: string | number;
    isDebug?: boolean;
}
// PrintHtmlOption 的类型定义在文档 6.10.4 中查看
```

| 参数    | 类型    | 说明                                                  | 示例值 |
| :------ | :------ | :---------------------------------------------------- | :----- |
| pages   | String  | 数组，代表每一页，(v1.6.0 之前的版本是 htmls)         |        |
| style   | String  | 样式，也可以写`<link rel="stylesheet" href="">`的形式 |        |
| padding | String  | 页面间距                                              | '10px' |
| isDebug | Boolean | 调试模式，每一页会加红色边框，便于调试大小            | true   |
| width   | String  | 每一页宽度                                            |        |
| height  | String  | 每一页高度                                            |        |

<br/>

## 6.10.6. 打印任意 pdf  

```javascript
import { printPdf } from 'ele-admin-pro';

const pWindow = printPdf({
    url: 'http://xxx.pdf', // pdf 地址
    //arraybuffer: arraybuffer,  // 直接指定 arraybuffer 数据, 和 url 二选一
    loading: true,  // 是否显示 loading
    before: () => {  // 打印开始的回调
    },
    done: () => {  // 打印完成的回调
    }
});
```

类型定义：

```typescript
declare function printPdf(option: PrintPdfOption): Window | null;

interface PrintPdfOption {
    url?: string;
    arraybuffer?: ArrayBuffer;
    loading?: boolean;
    before?: () => void;
    done?: () => void;
    error?: (status: number, result: string) => void;
}
```

<br/>

## 6.10.7. 打印数据表格  

```vue
<template>
    <a-button @click="printDataTable">打印</a-button>
</template>

<script setup>
    import { ref } from 'vue';
    import { printHtml, makeTable } from 'ele-admin-pro';

    const users = ref([{
        "username": "张小三",
        "amount": 18,
        "province": "浙江",
        "city": "杭州",
        "zone": "西湖区",
        "street": "西溪街道",
        "address": "西溪花园",
        "house": "30栋1单元"
    }, {
        "username": "李小四",
        "amount": 39,
        "province": "江苏",
        "city": "苏州",
        "zone": "姑苏区",
        "street": "丝绸路",
        "address": "天墅之城",
        "house": "9幢2单元"
    }]);

    /* 打印数据表格 */
    const printDataTable = () => {
        const html = makeTable(users.value, [
            [
                {field: 'username', width: 150, rowspan: 2, title: '联系人'},
                {align: 'center', colspan: 3, title: '地址'},
                {field: 'amount', width: 120, rowspan: 2, title: '金额', align: 'center'}
            ],
            [
                {field: 'province', width: 120, title: '省'},
                {field: 'city', width: 120, title: '市'},
                {
                    width: 200, title: '区', templet: (d) => {
                        return `<span style="color:red;">${d.zone}</span>`;
                    }
                }
            ]
        ]);

        printHtml({
            html: '<div>可以拼接点内容</div>' + html
        });
    };
</script>
```

打印数据表格用到了 `makeTable(data, cols)` 方法生成表格，参数一是数据(Array 类型)，参数二是列配置(二维数组)，列参数列表：

| 参数    | 类型          | 说明                             | 示例值                      |
| :------ | :------------ | :------------------------------- | :-------------------------- |
| field   | String        | 设定字段名                       | 'username'                  |
| title   | String        | 设定标题名称                     | '用户名'                    |
| width   | Number/String | 设定列宽，若不填写，则自动分配   | 200、'30%'                  |
| align   | String        | 单元格排列方式                   | 'center'、'left'、'right'   |
| thAlign | String        | 表头单元格排列方式               | 'center'、'left'、'right'   |
| colspan | Number        | 单元格所占列数，一般用于多级表头 | 3                           |
| rowspan | Number        | 单元格所占行数，一般用于多级表头 | 2                           |
| style   | String        | 自定义单元格样式                 | 'background-color: red;'    |
| thStyle | String        | 自定义表头单元格样式             | 'color: red;'               |
| templet | Function      | 自定义列模板                     | (d) => { return '${d.xx}' } |

类型定义：

```typescript
declare function makeTable<T>(data: T[], cols: Array<ColOption<T>[]>): string;

interface ColOption<T> {
    INIT_OK?: boolean;
    key?: string;
    parentKey?: string;
    colGroup?: boolean;
    HAS_PARENT?: boolean;
    PARENT_COL_INDEX?: number;
    CHILD_COLS?: ColOption<T>[];
    field?: string;
    title?: string;
    width?: number;
    align?: string;
    thAlign?: string;
    style?: string;
    thStyle?: string;
    colspan?: number;
    rowspan?: number;
    templet?: (d: T, index?: number, colIndex?: number) => any;
}
```

最后打印的效果：

![](/images/6-10-1.png)