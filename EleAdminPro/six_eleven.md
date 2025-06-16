---
title: excel 插件
createTime: 2025/01/10 09:30:40
permalink: /EleAdminPro/51xhqxrn/
---
## 6.11.1. 导出 excel  

excel 操作使用的是 `sheetjs` ，github 地址：[SheetJS/sheetjs](https://github.com/SheetJS/sheetjs) ，使用示例：

```vue
<template>
    <a-button @click="onExport">导出</a-button>
</template>

<script setup>
    import { ref } from 'vue';
    import { utils, writeFile } from 'xlsx';

    const users = ref([{
        "username": "张小三",
        "amount": 18,
        "province": "浙江",
        "city": "杭州",
        "zone": "西湖区",
        "street": "西溪街道",
        "address": "西溪花园30栋1单元",
    }, {
        "username": "李小四",
        "amount": 39,
        "province": "江苏",
        "city": "苏州",
        "zone": "姑苏区",
        "street": "丝绸路",
        "address": "天墅之城9幢2单元",
    }]);

    /* 导出excel */
    const onExport = () => {
        const array = [
            ['序号', '用户名', '省', '市', '区', '街道', '详细地址', '金额']
        ];
        users.value.forEach((d, i) => {
            array.push([i + 1, d.username, d.province, d.city, d.zone, d.street, d.address, d.amount]);
        });
        const sheet = utils.aoa_to_sheet(array);
        // 如果需要设置列宽可以这样写
        sheet["!cols"] = [
            { wch: 10 },
            { wch: 60 },
            { wch: 20 },
            { wch: 20 },
            { wch: 20 },
            { wch: 20 },
            { wch: 20 }
        ];
        writeFile(
            {
                SheetNames: ['Sheet1'],
                Sheets: {
                    Sheet1: sheet
                }
            },
            '用户数据.xlsx'
        );
    }
</script>
```

xlsx 较老的版本用法不一样，v1.7.0 以及之前的版本使用是较老的 xlsx ：

```vue
<template>
    <a-button @click="onExport">导出</a-button>
</template>

<script setup>
    import { ref } from 'vue';
    import XLSX from 'xlsx';
    import { exportSheet } from 'ele-admin-pro';

    const users = ref([{
        "username": "张小三",
        "amount": 18,
        "province": "浙江",
        "city": "杭州",
        "zone": "西湖区",
        "street": "西溪街道",
        "address": "西溪花园30栋1单元",
    }, {
        "username": "李小四",
        "amount": 39,
        "province": "江苏",
        "city": "苏州",
        "zone": "姑苏区",
        "street": "丝绸路",
        "address": "天墅之城9幢2单元",
    }]);

    /* 导出excel */
    const onExport = () => {
        const array = [
            ['用户名', '省', '市', '区', '街道', '详细地址', '金额']
        ];
        users.value.forEach(d => {
            array.push([d.username, d.province, d.city, d.zone, d.street, d.address, d.amount]);
        });
        exportSheet(XLSX, array, '用户数据');
    }
</script>
```

`exportSheet` 方法类型定义及参数说明：

```typescript
function exportSheet(XLSX: any, sheet: any, sheetName?: string, type?: string): void;
```

- 参数一   传入 XLSX 插件
- 参数二   传入二维数组或者 Sheet 实例
- 参数三   导出的文件名称，不用带后缀
- 参数四   文件格式，默认 'xlsx' ，可选 'csv'

<br/>

## 6.11.2. 导出多个 sheet  

```vue
<template>
    <a-button @click="onExport">导出</a-button>
</template>

<script setup>
    import { utils, writeFile } from 'xlsx';

    const onExport = () => {
        const array1 = [
            ['账号', '用户名', '性别'],
            ['000001', 'aa', '男'],
            ['000002', 'bb', '女']
        ];
        const array2 = [
            ['账号', '用户名', '性别'],
            ['000003', 'cc', '女'],
            ['000004', 'dd', '女']
        ];
        const sheet1 = utils.aoa_to_sheet(array1);
        const sheet2 = utils.aoa_to_sheet(array2);
        const workbook = {
            SheetNames: ['sheet01', 'sheet02'],
            Sheets: {
                sheet01: sheet1,
                sheet02: sheet2
            }
        };
        writeFile(workbook, '用户数据.xlsx');
    };
</script>
```

xlsx 较老的版本用法不一样，v1.7.0 以及之前的版本使用是较老的 xlsx ：

```vue
<template>
    <a-button @click="onExport">导出</a-button>
</template>

<script setup>
    import { ref } from 'vue';
    import XLSX from 'xlsx';

    const array1 = ref([
        ['账号', '用户名', '性别'],
        ['000001', 'aa', '男'],
        ['000002', 'bb', '女']
    ]);

    const array2 = ref([
        ['账号', '用户名', '性别'],
        ['000003', 'cc', '女'],
        ['000004', 'dd', '女']
    ]);

    const onExport = () => {
        const sheet1 = XLSX.utils.aoa_to_sheet(array1.value);
        const sheet2 = XLSX.utils.aoa_to_sheet(array2.value);
        const workbook = {
            SheetNames: ['sheet01', 'sheet02'],
            Sheets: {
                sheet01: sheet1,
                sheet02: sheet2
            }
        };
        XLSX.writeFile(workbook, '用户数据.xlsx');
    };
</script>
```

<br/>

## 6.11.3. 导出合并单元格  

```vue
<template>
    <a-button @click="onExport">导出</a-button>
</template>

<script setup>
    import { ref } from 'vue';
    import { utils, writeFile } from 'xlsx';

    const users = ref([{
        "username": "张小三",
        "amount": 18,
        "province": "浙江",
        "city": "杭州",
        "zone": "西湖区",
        "street": "西溪街道",
        "address": "西溪花园30栋1单元",
    }, {
        "username": "李小四",
        "amount": 39,
        "province": "江苏",
        "city": "苏州",
        "zone": "姑苏区",
        "street": "丝绸路",
        "address": "天墅之城9幢2单元",
    }]);

    /* 导出excel */
    const onExport = () => {
        const array = [
            ['用户名', '地址', null, null, null, null, '金额'],
            [null, '省', '市', '区', '街道', '详细地址', null]
        ];
        users.value.forEach(d => {
            array.push([d.username, d.province, d.city, d.zone, d.street, d.address, d.amount]);
        });
        const sheet = utils.aoa_to_sheet(array);
        sheet['!merges'] = [
            { s: { r: 0, c: 1 }, e: { r: 0, c: 5 } }, // 合并第 0 行第 1 列到第 0 行第 5 列(地址)
            { s: { r: 0, c: 0 }, e: { r: 1, c: 0 } }, // 合并第 0 行第 0 列到第 1 行第 0 列(用户名)
            { s: { r: 0, c: 6 }, e: { r: 1, c: 6 } } // 合并第 0 行第 6 列到第 1 行第 6 列(金额)
        ];
        writeFile(
            {
                SheetNames: ['Sheet1'],
                Sheets: {
                    Sheet1: sheet
                }
            },
            '用户数据.xlsx'
        );
    };
</script>
```

  合并单元格需要设置 sheet 的 `!merges` 参数，是一个数组，`s` 表示开始合并的位置，`e` 表示结束合并的位置，`r` 表示第几行，`c` 表示第几列，合并单元格的写法虽然繁琐，但是很容易理解。

最后导出的效果：

![img](https://cdn.eleadmin.com/20210217/DGLj0A.png)

合并主体部分是一样的写法，指定从第几行第几列合并到第几行第几列，被合并的单元格值最好为 null 。

xlsx 较老的版本用法不一样，v1.7.0 以及之前的版本使用是较老的 xlsx ：

```vue
<template>
    <a-button @click="onExport">导出</a-button>
</template>

<script setup>
    import { ref } from 'vue';
    import XLSX from 'xlsx';
    import { exportSheet } from 'ele-admin-pro';

    const users = ref([{
        "username": "张小三",
        "amount": 18,
        "province": "浙江",
        "city": "杭州",
        "zone": "西湖区",
        "street": "西溪街道",
        "address": "西溪花园30栋1单元",
    }, {
        "username": "李小四",
        "amount": 39,
        "province": "江苏",
        "city": "苏州",
        "zone": "姑苏区",
        "street": "丝绸路",
        "address": "天墅之城9幢2单元",
    }]);

    /* 导出excel */
    const onExport = () => {
        const array = [
            ['用户名', '地址', null, null, null, null, '金额'],
            [null, '省', '市', '区', '街道', '详细地址', null]
        ];
        users.value.forEach(d => {
            array.push([d.username, d.province, d.city, d.zone, d.street, d.address, d.amount]);
        });
        const sheet = XLSX.utils.aoa_to_sheet(array);
        sheet['!merges'] = [
            { s: { r: 0, c: 1 }, e: { r: 0, c: 5 } }, // 合并第 0 行第 1 列到第 0 行第 5 列(地址)
            { s: { r: 0, c: 0 }, e: { r: 1, c: 0 } }, // 合并第 0 行第 0 列到第 1 行第 0 列(用户名)
            { s: { r: 0, c: 6 }, e: { r: 1, c: 6 } } // 合并第 0 行第 6 列到第 1 行第 6 列(金额)
        ];
        exportSheet(XLSX, sheet, '用户数据');
    };
</script>
```

<br/>

## 6.11.4. 导入 excel  

```vue
<template>
    <a-upload :before-upload="importFile" :show-file-list="false" accept=".xls,.xlsx,.csv">
        <a-button>导入 excel</a-button>
    </a-upload>
</template>

<script setup>
    import { utils, read } from 'xlsx';

    const importFile = ({ file }) => {
        const reader = new FileReader();
        reader.onload = (e) => {
            const data = new Uint8Array(e.target.result);
            const workbook = read(data, {type: 'array'});
            const sheetNames = workbook.SheetNames;
            const worksheet = workbook.Sheets[sheetNames[0]];
            // 解析成二维数组
            const aoa = utils.sheet_to_json(worksheet, { header: 1 });
            console.log(aoa);
        };
        reader.readAsArrayBuffer(file);
        return false;
    }
</script>
```

  使用 `a-upload` 来选择文件，在 `before-upload` 中获取到选择的文件，并且 `return flase` 阻止默认的上传行为， 然后再使用 `FileReader` 读取文件为 `arrayBuffer` 数据提供给 `XLSX` 解析，最后读取的是二维数组，如果要转成对象的格式可以继续这样操作：

```javascript
const list = aoa.slice(1).map(d => {
    return { username: d[0], sex: d[1], phone: d[2] };
});
// slice 是去掉第一行(一般第一行是表头)
```

**导入拆分合并单元格：**

如果有合并单元格，二维数组里面对于合并的单元格只有第一格有数据，合并的几个为 `null`，一般都是希望合并的单元格拆开后值都是一样的。

```javascript
import { utils, read } from 'xlsx';

const workbook = read(data, { type: 'array' });
const sheetNames = workbook.SheetNames;
const worksheet = workbook.Sheets[sheetNames[0]];
// 解析成二维数组
const aoa = utils.sheet_to_json(worksheet, { header: 1 });
// 拆分合并单元格
if (worksheet['!merges']) {
    worksheet['!merges'].forEach(m => {
        for (let r = m.s.r; r <= m.e.r; r++) {
            for (let c = m.s.c; c <= m.e.c; c++) {
                aoa[r][c] = aoa[m.s.r][m.s.c];
            }
        }
    });
}
// 下面你也可以继续把二维数组转为对象的 list
```

xlsx 较老的版本用法不一样，v1.7.0 以及之前的版本使用是较老的 xlsx ：

```vue
<template>
    <a-upload :before-upload="importFile" :show-file-list="false" accept=".xls,.xlsx,.csv">
        <a-button>导入excel</a-button>
    </a-upload>
</template>

<script setup>
    import XLSX from 'xlsx';

    const importFile = ({ file }) => {
        const reader = new FileReader();
        reader.onload = (e) => {
            const data = new Uint8Array(e.target.result);
            const workbook = XLSX.read(data, { type: 'array' });
            const sheetNames = workbook.SheetNames;
            const worksheet = workbook.Sheets[sheetNames[0]];
            // 解析成二维数组
            const aoa = XLSX.utils.sheet_to_json(worksheet, { header: 1 });
            console.log(aoa);
        };
        reader.readAsArrayBuffer(file);
        return false;
    }
</script>
```