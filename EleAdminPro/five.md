---
title: 公共方法
createTime: 2025/01/10 09:30:40
permalink: /EleAdminPro/lohny0e3/
---
## 5.1. token 操作  

token 操作方法位于 `src/utils/token-util.ts` 中（v1.6.0新增），使用示例：

```javascript
import { getToken, setToken, removeToken } from '@/utils/token-util';

const token = getToken();  // 获取缓存的 token

setToken('token', true);  // 缓存 token，参数二为 true 使用 localStorage 否则使用 sessionStorage

removeToken();  // 移除缓存的 token
```

<br/>

## 5.2. 页签 tab 操作  

页签操作方法位于 `src/utils/page-tab-util.ts` 中（v1.5.0新增），使用示例：

```javascript
import { reloadPageTab, finishPageTab } from '@/utils/page-tab-util';

reloadPageTab();  // 刷新当前页签
finishPageTab();  // 关闭当前页签
```

全部方法及参数说明：

| 方法               | 说明                            | 参数                      | 参数说明                           |
| :----------------- | :------------------------------ | :------------------------ | :--------------------------------- |
| setPageTabTitle    | 修改当前页签标题 `v1.10.0新增`  | title: string             |                                    |
| reloadPageTab      | 刷新当前页签                    | option: TabReloadOptions  | 可为空                             |
| finishPageTab      | 关闭当前页签                    | 无                        |                                    |
| removePageTab      | 关闭指定页签                    | option: TabRemoveOption   |                                    |
| removeLeftPageTab  | 移除左侧页签                    | option: TabRemoveOption   |                                    |
| removeRightPageTab | 移除右侧页签                    | option: TabRemoveOption   |                                    |
| removeOtherPageTab | 移除其他页签                    | option: TabRemoveOption   |                                    |
| removeAllPageTab   | 移除全部页签                    | active: string            |                                    |
| addPageTab         | 添加页签                        | data: TabItem/TabItem[]   | 页签数据，在文档 8.2 中查看        |
| setPageTab         | 修改指定页签                    | data: TabItem             | 见下面示例                         |
| setHomeComponents  | 设置主页的组件名称 `v1.7.0新增` | components: string[]      | 组件名称                           |
| setRouteReload     | 设置路由刷新信息 `v1.8.0新增`   | option: RouteReloadOption | 路由刷新参数                       |
| isHomeRoute        | 判断路由是否是主页 `v1.8.0新增` | route                     | 路由信息                           |
| goHomeRoute        | 登录成功后跳转首页 `v1.8.0新增` | from: string              | 登录前的地址                       |
| cleanPageTabs      | 登录成功后清空页签 `v1.8.0新增` | 无                        |                                    |
| logout             | 退出登录封装 `v1.8.0新增`       | route, from               | 是否使用路由跳转, 登录后跳转的地址 |

修改当前页签标题的使用示例，v1.10.0 新增：

::: code-tabs

@tab TypeScript

```vue
<template>
    <a-button @click="updateTab">修改当前页签标题</a-button>
</template>

<script lang="ts" setup>
    import { setPageTabTitle } from '@/utils/page-tab-util';
    import { useRouter } from 'vue-router';

    const { currentRoute } = useRouter();

    const updateTab = () => {
        // 此方法只能修改当前页签标题，不能指定某个页签，所以调用的时候最好自己判断下当前路由
        if (unref(currentRoute).path === '/system/user/details') {
            setPageTabTitle('新的标题');
        }
    };
</script>
```

@tab JavaScript

```vue
<template>
    <a-button @click="updateTab">修改当前页签标题</a-button>
</template>

<script setup>
    import { setPageTabTitle } from '@/utils/page-tab-util';
    import { useRouter } from 'vue-router';

    const { currentRoute } = useRouter();

    const updateTab = () => {
        // 此方法只能修改当前页签标题，不能指定某个页签，所以调用的时候最好自己判断下当前路由
        if (unref(currentRoute).path === '/system/user/details') {
            setPageTabTitle('新的标题');
        }
    };
</script>
```

:::

修改指定页签的使用示例：

::: code-tabs

@tab TypeScript

```vue
<template>
    <a-button @click="updateTab">修改页签</a-button>
</template>

<script lang="ts" setup>
    import { setPageTab } from '@/utils/page-tab-util';
    import { useRouter } from 'vue-router';
    import { unref } from 'vue';

    const { currentRoute } = useRouter();
    const { path, fullPath } = unref(currentRoute);

    const updateTab = () => {
        // 可以根据 fullPath 或 path 修改 title 和 closable
        setPageTab({
            fullPath: fullPath,  // 可重复打开的页签用 fullPath
            //path: path,  // 不可重复打开的页签用 path 或 fullPath 都可以
            title: '新的标题',
            //closable: false
        });
        // v1.10.0 版本之后修改页签标题推荐使用 setPageTabTitle 方法
    };
</script>
```

@tab JavaScript

```vue
<template>
    <a-button @click="updateTab">修改页签</a-button>
</template>

<script setup>
    import { setPageTab } from '@/utils/page-tab-util';
    import { useRouter } from 'vue-router';
    import { unref } from 'vue';

    const { currentRoute } = useRouter();
    const { path, fullPath } = unref(currentRoute);

    const updateTab = () => {
        // 可以根据 fullPath 或 path 修改 title 和 closable
        setPageTab({
            fullPath: fullPath,  // 可重复打开的页签用 fullPath
            //path: path,  // 不可重复打开的页签用 path 或 fullPath 都可以
            title: '新的标题',
            //closable: false
        });
        // v1.10.0 版本之后修改页签标题推荐使用 setPageTabTitle 方法
    };
</script>
```

:::

添加页签的方法基本用不到，需要打开或切换某个页签，使用路由跳转即可：

::: code-tabs

@tab TypeScript

```vue
<template>
    <a-button @click="navigateTo">跳转页面</a-button>
</template>

<script lang="ts" setup>
    import { useRouter } from 'vue-router';

    const { push } = useRouter();

    const navigateTo = () => {
        push('/system/user');

        // 带参数跳转
        push({
            path: '/system/user',
            query: { id: 10 }  // 传参数建议用 query 显示在 url 上，不建议用 params 刷新页面就没了
        });
    };
</script>
```

@tab JavaScript

```vue
<template>
    <a-button @click="navigateTo">跳转页面</a-button>
</template>

<script setup>
    import { useRouter } from 'vue-router';

    const { push } = useRouter();

    const navigateTo = () => {
        push('/system/user');

        // 带参数跳转
        push({
            path: '/system/user',
            query: { id: 10 }  // 传参数建议用 query 显示在 url 上，不建议用 params 刷新页面就没了
        });
    };
</script>
```

:::

被跳转的页面接收参数：

::: code-tabs

@tab TypeScript

```vue
<script lang="ts" setup>
    import { unref } from 'vue';
    import { useRouter } from 'vue-router';

    const { currentRoute } = useRouter();
    const { query } = unref(currentRoute);
    console.log(query);
</script>
```

@tab JavaScript

```vue
<script setup>
    import { unref } from 'vue';
    import { useRouter } from 'vue-router';

    const { currentRoute } = useRouter();
    const { query } = unref(currentRoute);
    console.log(query);
</script>
```

:::

移除页签的各种方法的参数说明 TabRemoveOption ，页签 key 的含义在文档 8.2 中查看：

```typescript
interface TabRemoveOption {
    // 页签 key
    key: string;
    // 当前选中的页签 key
    active?: string;
}
```

注意 v1.8.0 以及之前版本 removeLeftPageTab、removeRightPageTab、removeOtherPageTab、removeAllPageTab 参数是 string 类型的 active 。

刷新页签方法的参数 TabReloadOptions 说明，不传参数就是刷新当前页签，传参数就是刷新指定页签：

```typescript
interface TabReloadOptions {
    // 是否是主页
    isHome: boolean;
    // 路由地址
    fullPath?: string;
}
```

<br/>

## 5.3. 权限控制指令  

权限控制指令位于 `src/utils/permission.ts`，已在 main.ts 中全局安装，可以直接使用。

指令方式使用，注意不能用在 `template` 上：

```vue
<template>
    <div>
        <!-- 有某个权限才会显示按钮，注意值是字符串，有单引号 -->
        <a-button v-permission="'sys:user:add'">添加</a-button>
        <!-- 有某些权限(全部都有)才会显示按钮 -->
        <a-button v-permission="['sys:user:add','sys:user:edit']">添加</a-button>
        <!-- 有任意一个权限才会显示按钮 -->
        <a-button v-any-permission="['sys:user:add','sys:user:edit']">添加</a-button>

        <!-- 有某个角色才会显示按钮 -->
        <a-button v-role="'admin'">添加</a-button>
        <!-- 有某些角色才会显示按钮 -->
        <a-button v-role="['admin','user']">添加</a-button>
        <!-- 有任意一个角色才会显示按钮 -->
        <a-button v-any-role="['admin','user']">添加</a-button>
    </div>
</template>
```

服务的方式使用：

::: code-tabs

@tab TypeScript

```vue
<template>
    <div>
        <a-button @click="add">添加</a-button>
        <!-- 也可以在 v-if 中使用 -->
        <a-button v-if="hasPermission('sys:user:add')">添加</a-button>
    </div>
</template>

<script lang="ts" setup>
    import { hasPermission, hasAnyPermission, hasRole, hasAnyRole } from '@/utils/permission';
    import { message } from 'ant-design-vue';
    
    const add = () => {
        // 判断是否有某个权限
        if (!hasPermission('sys:user:add')) {
            message.error('没有权限');
        }
        // 判断是否有某些权限
        if (!hasPermission(['sys:user:add', 'sys:user:edit'])) {
            message.error('没有权限');
        }
        // 判断是否有任意一个权限
        if (!hasAnyPermission(['sys:user:add', 'sys:user:edit'])) {
            message.error('没有权限');
        }

        // 判断是否有某个角色
        if (!hasRole('admin')) {
            message.error('没有权限');
        }
        // 判断是否有某些角色
        if (!hasRole(['admin', 'user'])) {
            message.error('没有权限');
        }
        // 判断是否有任意一个角色
        if (!hasAnyRole(['admin', 'user'])) {
            message.error('没有权限');
        }
    }
</script>
```

@tab JavaScript

```vue
<template>
    <div>
        <a-button @click="add">添加</a-button>
        <!-- 也可以在 v-if 中使用 -->
        <a-button v-if="hasPermission('sys:user:add')">添加</a-button>
    </div>
</template>

<script setup>
    import { hasPermission, hasAnyPermission, hasRole, hasAnyRole } from '@/utils/permission';
    import { message } from 'ant-design-vue';
    
    const add = () => {
        // 判断是否有某个权限
        if (!hasPermission('sys:user:add')) {
            message.error('没有权限');
        }
        // 判断是否有某些权限
        if (!hasPermission(['sys:user:add', 'sys:user:edit'])) {
            message.error('没有权限');
        }
        // 判断是否有任意一个权限
        if (!hasAnyPermission(['sys:user:add', 'sys:user:edit'])) {
            message.error('没有权限');
        }

        // 判断是否有某个角色
        if (!hasRole('admin')) {
            message.error('没有权限');
        }
        // 判断是否有某些角色
        if (!hasRole(['admin', 'user'])) {
            message.error('没有权限');
        }
        // 判断是否有任意一个角色
        if (!hasAnyRole(['admin', 'user'])) {
            message.error('没有权限');
        }
    }
</script>
```

:::

用户的角色和权限列表是在动态路由注册之前获取登录用户信息的时候从登录用户信息中获取的，详细介绍请查看文档 2.5。

<br/>

## 5.4. 常用工具方法  

ele-admin-pro 依赖里面封装了一些常用的工具方法，使用示例及参数说明：

| 方法             | 说明                           | 参数                           | 参数说明               |
| :--------------- | :----------------------------- | :----------------------------- | :--------------------- |
| digit            | 数字位数不够前置补0            | num, length                    | 数字, 位数             |
| toDateString     | 日期格式化                     | time, format                   | 时间, 日期格式         |
| timeAgo          | 时间语义化                     | time, onlyDate                 | 时间, 是否只显示日期   |
| toTreeData       | parentId形式数据转children形式 | option                         | 见下方单独说明         |
| eachTreeData     | 遍历children形式数据           | data, callback, childrenField  | 见下方单独说明         |
| formatTreeData   | 处理children形式数据           | data, formatter, childrenField | 见下方单独说明         |
| uuid             | 生成随机字符串                 | length, radix                  | 长度(默认32), 基数     |
| random           | 生成m到n随机整数               | m, n                           | 最小值, 最大值(不含)   |
| formatNumber     | 数字千分位, 每3位加逗号        | num                            | 需要格式的数字         |
| escape           | html转义, 标签使用转义字符     | html                           | html内容               |
| htmlToText       | 获取html纯文本, 去掉标签       | html                           | html内容               |
| deepClone        | 深度克隆对象                   | obj                            | 源对象                 |
| assignObject     | 赋值不改变原字段 `1.6.0新增`   | target, source, excludes       | 见下方单独说明         |
| bd09ToGcj02      | 百度转高德地图坐标             | point                          | lng: 经度, lat: 纬度 |
| gcj02ToBd09      | 高德转百度地图坐标             | point                          | lng: 经度, lat: 纬度 |
| play             | 播放音频                       | url                            | 音频地址               |
| device           | 获取设备信息                   | key(可选)                      | 自定义userAgent标识    |
| countdown        | 倒计时                         | endTime, serverTime, callback  | 见下方单独说明         |
| toggleFullscreen | 全屏/退出全屏                  | el, fullscreen                 | dom, 是否全屏          |
| isFullscreen     | 获取当前是否是全屏状态         | 无                             |                        |
| screenWidth      | 获取屏幕宽度                   | 无                             | 无                     |
| screenHeight     | 获取屏幕高度                   | 无                             | 无                     |
| contentWidth     | 获取内容区域宽度               | 无                             | 无                     |
| contentHeight    | 获取内容区域高度               | 无                             | 无                     |
| exportSheet      | 导出excel                      | XLSX, sheet, name, type        | 见文档6.13             |
| combineArrays    | 排列组合二维数组 `1.6.0新增`   | arrays, separator              | 见下方单独说明         |

使用示例：

```javascript
import { digit, toDateString } from 'ele-admin-pro';

console.log(digit(1, 3));  // 返回001
console.log(toDateString(new Date()));
console.log(toDateString(new Date(), 'yyyy-MM-dd HH:mm:ss'));
```

**时间语义化:**

```javascript
import { timeAgo } from 'ele-admin-pro';

timeAgo(new Date());  // 刚刚
timeAgo('2020-08-24 20:43:00', true);  // 2020-08-24
```

  如果在 3 分钟以内，返回 “刚刚” ，如果在 30 天以内，返回 “若干分钟前”、“若干小时前”、“若干天前” ，如：5 分钟前， 如果在 30 天以上，返回日期字符，如：`2017-01-01 17:33:12`，参数二 `onlyDate` 为 `true` 不显示时分秒。

**parentId 形式数据转 children 形式:**

```javascript
import { toTreeData } from 'ele-admin-pro';

const pidData = [
    {name: 'aa', id: 1, pid: null},
    {name: 'bb', id: 2, pid: 1},
    {name: 'cc', id: 3, pid: null},
    {name: 'dd', id: 4, pid: 3},
    {name: 'ee', id: 5, pid: 3}
];
const result = toTreeData({
    data: pidData,   // 需要转换的数组，必须
    idField: 'id',   // id字段名，默认id，非必须
    parentIdField: 'pid',   // parentId字段名，默认parentId，非必须
    childrenField: 'children',  // 生成的children字段名，默认children，非必须
    parentId: null,  // 顶级的parentId，默认null会自动获取顶级parentId，非必须
    addParentIds: true,  // 是否添加所有父级id的字段，默认false，非必须
    parentIdsField: 'parentIds'  // 添加的所有父级id的字段名称，默认parentIds，非必须
});
console.log(result);
```

addParentIds 为 true 会在每条数据里面加一个字段 parentIds 值为该条数据所有的父级及祖先的 id。

**遍历 children 形式数据:**

```javascript
import { eachTreeData } from 'ele-admin-pro';

const treeData = [];  // 任意树形结构数据，这里省略
eachTreeData(treeData, (item, index, parent) => {
    console.log(item);
    // index 是当前循环索引, parent 是父级数据
}, 'children');
// 参数三是子级的字段名称，可选, 默认是'children'
```

**处理 children 形式的数据格式：**

```javascript
import { formatTreeData } from 'ele-admin-pro';

// 比如想把树形数据里面的 name 字段变成 title，id 字段变成 key
const treeData = [];  // 任意树形结构数据，这里省略
const result = formatTreeData(treeData, (item, index, parent) => {
    return Object.assign({}, item, {
        title: item.name,
        key: item.id
    });
}, 'children');
// 参数三是子级的字段名称，可选, 默认是'children'
console.log(result);
```

**生成随机字符串：**

```javascript
import { uuid } from 'ele-admin-pro';

console.log(uuid());  // 随机32位字符串
console.log(uuid(8));  // 随机8位字符串
// 生成的字符串由以下字符组成
// 0123456789ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz
// 参数二表示只取上面前多少位的基字符来生成
console.log(uuid(32, 10));  // 生成的字符串是全数字组成
console.log(uuid(32, 2));  // 生成的字符串由0和1组成
```

**赋值不改变原字段：**

```javascript
import { assignObject } from 'ele-admin-pro';

const form = {username: '', nickname: ''};
const data = {username: 'admin', nickname: '管理员', createTime: '2021-10-16 08:35'};
// 用 data 赋值 form
assignObject(form, data);
console.log(form);  // 结果 {username: 'admin', nickname: '管理员'}
```

原生的 Object.assign(form, data) 会把 data 所有字段都赋值给 form，在用 form 提交数据到后端时会多一些没用的字段。

**获取设备信息:**

```javascript
import { device } from 'ele-admin-pro';

const result = device();
// result 包含的字段：
/*
{
    os: 'windows', // 底层操作系统，windows、linux、mac 等
    ie: false, // ie6-11 的版本，如果不是 ie 浏览器，则为 false
    weixin: false, // 是否微信环境
    android: false, // 是否安卓系统
    ios: false // 是否 ios 系统
}
*/

// 有时你的 App 可能会对 userAgent 插入一段特定的标识，可用如下方法获取
const result = device('myapp');
if (result.myapp) {
    console.log('在我的App环境');
}
```

**倒计时：**

```javascript
import { countdown } from 'ele-admin-pro';

const endTime = new Date(2099, 1, 1).getTime();  // 结束时间戳或 Date 对象
const serverTime = null; // 当前服务器时间戳或 Date 对象，实际使用是从服务端取，可省略
countdown(endTime, serverTime, (date, serverTime, timer) => {
    // 在倒计时未结束前每秒都会进入该回调
    // date - 包含 天/时/分/秒 的对象
    // serverTime - 当前服务器时间戳或 Date 对象
    // timer - 计时器的 ID 值，用于 clearTimeout
    const str = date[0] + '天' + date[1] + '时' + date[2] + '分' + date[3] + '秒';
    console.log(str);
});
```

**排列组合二维数组：**

```javascript
import { combineArrays } from 'ele-admin-pro';

const array = [
    ['张', '李'],
    ['三', '飞', '四'],
];
console.log(combineArrays(array));
// 结果 ['张三', '张飞', '张四', '李三', '李飞', '李四']
// 还可以加参数二用于设置连接的分隔符
```

**全屏/退出全屏：**

::: code-tabs

@tab TypeScript

```vue
<template>
    <div>
        <a-button @click="toggleFullscreen()">全屏切换</a-button>
        <div ref="divRef" class="ele-bg-white">Hello</div>
        <a-button @click="toggleDivFullscreen">让div全屏切换</a-button>
    </div>
</template>

<script lang="ts" setup>
    import { ref } from 'vue';
    import { toggleFullscreen } from 'ele-admin-pro';

    const divRef = ref<HTMLElement | null>(null);

    const toggleDivFullscreen = () => {
        toggleFullscreen(divRef.value);
    };
</script>
```

@tab JavaScript

```vue
<template>
    <div>
        <a-button @click="toggleFullscreen()">全屏切换</a-button>
        <div ref="divRef" class="ele-bg-white">Hello</div>
        <a-button @click="toggleDivFullscreen">让div全屏切换</a-button>
    </div>
</template>

<script setup>
    import { ref } from 'vue';
    import { toggleFullscreen } from 'ele-admin-pro';

    const divRef = ref(null);

    const toggleDivFullscreen = () => {
        toggleFullscreen(divRef.value);
    };
</script>
```

:::

<br/>

## 5.5. 格式校验方法  

ele-admin-pro 依赖里面提供了一些常用的格式校验方法：

| 方法                 | 描述                                               | 参数                        |
| :------------------- | :------------------------------------------------- | :-------------------------- |
| isPhone              | 是否是手机号                                       | value                       |
| isPhoneStrong        | 是否是手机号(强校验) `v1.6.0新增`                  | value                       |
| isTel                | 是否为固话                                         | value                       |
| isEmail              | 是否是邮箱                                         | value                       |
| isUrl                | 是否是网址                                         | value                       |
| isIdentity           | 是否是身份证                                       | value                       |
| isDate               | 是否是日期                                         | value                       |
| isNumber             | 是否是数字                                         | value                       |
| isInteger            | 是否是整数(v1.6.0 前是 isDigits)                   | value                       |
| isPositiveInteger    | 是否是正整数(v1.6.0 前是 isDigitsP)                | value                       |
| isNegativeInteger    | 是否是负整数(v1.6.0 前是 isDigitsN)                | value                       |
| isNonNegativeInteger | 是否是非负整数(正整数或 0)(v1.6.0 前是 isDigitsPZ) | value                       |
| isNonPositiveInteger | 是否是非正整数(负整数或 0)(v1.6.0 前是 isDigitsNZ) | value                       |
| isChinese            | 是否是中文(v1.0.0 是 isChina)                      | value                       |
| isPort               | 是否是端口号                                       | value                       |
| isIP                 | 是否是IP                                           | value                       |
| isLongitude          | 是否是经度                                         | value                       |
| isLatitude           | 是否是纬度                                         | value                       |
| maxMinLength         | 验证最小、最大长度是否在范围内                     | value, minLength, maxLength |
| maxMin               | 验证最小值、最大值是否在范围内                     | value, min, max             |
| isIdentityStrong     | 是否是身份证(强校验)                               | value                       |

常用的正则表达式， `v1.6.0` 新增：

| 字段                  | 描述                          |
| :-------------------- | :---------------------------- |
| phoneReg              | 手机号正则表达式              |
| phoneStrongReg        | 手机号正则表达式              |
| telReg                | 固话正则表达式                |
| emailReg              | 邮箱正则表达式                |
| urlReg                | 网址正则表达式                |
| identityReg           | 身份证号正则表达式            |
| dateReg               | 日期正则表达式                |
| numberReg             | 数字正则表达式                |
| integerReg            | 整数正则表达式                |
| positiveIntegerReg    | 正整数正则表达式              |
| negativeIntegerReg    | 负整数正则表达式              |
| nonNegativeIntegerReg | 非负整数(正整数或0)正则表达式 |
| nonPositiveIntegerReg | 非正整数(负整数或0)正则表达式 |
| chineseReg            | 中文正则表达式                |
| portReg               | 端口号正则表达式              |
| ipReg                 | IP正则表达式                  |
| longitudeReg          | 经度正则表达式                |
| latitudeReg           | 纬度正则表达式                |

使用示例：

```javascript
import { isPhone, isIdentityStrong } from 'ele-admin-pro';

if(isPhone('12345678901')) {
    console.log('ok');
}

// 要注意是否是身份证(强校验)，会返回错误信息(String)而不是Boolean，如果返回空则说明是正确的身份证号
const msg = isIdentityStrong('12345678901');
console.log(msg);  // 返回具体的错误信息，如地区码无效、出生日期无效、最后一位无效等
if(!msg) {
    console.log('是正确的身份证格式');
}
```

结合表单验证使用：

::: code-tabs

@tab TypeScript

```typescript
import { reactive } from 'vue';
import { phoneReg, identityReg, isIdentityStrong } from 'ele-admin-pro';
import type { RuleObject } from 'ant-design-vue/es/form';

// 表单验证规则
const rules = reactive({
    // 手机号校验并且必填
    phone: [
        {
            required: true,
            message: '请输入手机号',
            trigger: 'blur'
        },
        {
            pattern: phoneReg,
            message: '手机号格式不正确',
            type: 'string'
        }
    ],
    // 身份证号并且非必填
    identity: [
        {
            pattern: identityReg,
            message: '身份证号格式不正确',
            type: 'string'
        }
    ],
    // 身份证号强校验
    identity2: [
        {
            validator: async (_rule: RuleObject, value: string) => {
                if (!value) {
                    return Promise.reject('请输入身份证号');
                } else if (isIdentityStrong(value)) {
                    return Promise.reject('身份证号格式不正确');
                } else {
                    return Promise.resolve();
                }
            }
        }
    ]
});
```

@tab JavaScript

```javascript
import { reactive } from 'vue';
import { phoneReg, identityReg, isIdentityStrong } from 'ele-admin-pro';

// 表单验证规则
const rules = reactive({
    // 手机号校验并且必填
    phone: [
        {
            required: true,
            message: '请输入手机号',
            trigger: 'blur'
        },
        {
            pattern: phoneReg,
            message: '手机号格式不正确',
            type: 'string'
        }
    ],
    // 身份证号并且非必填
    identity: [
        {
            pattern: identityReg,
            message: '身份证号格式不正确',
            type: 'string'
        }
    ],
    // 身份证号强校验
    identity2: [
        {
            validator: async (_rule, value) => {
                if (!value) {
                    return Promise.reject('请输入身份证号');
                } else if (isIdentityStrong(value)) {
                    return Promise.reject('身份证号格式不正确');
                } else {
                    return Promise.resolve();
                }
            }
        }
    ]
});
```

:::

注意，v1.6.0 以及之前的版本需要使用如下方式引入：

```javascript
import validate from 'ele-admin-pro/packages/validate';

if(validate.isPhone('12345678901')) {
    console.log('ok');
}

// 正则表达式使用如下方式，版本至少需要v1.1.0
validate.phone;  // 手机号正则表达式
validate.tel;  // 固话正则表达式
validate.email;  // 邮箱正则表达式
validate.url;  // 网址正则表达式
validate.number;  // 数字正则表达式
validate.date;  // 日期正则表达式
validate.identity;  // 身份证号正则表达式
validate.digits;  // 整数正则表达式
validate.digitsP;  // 正整数正则表达式
validate.digitsN;  // 负整数正则表达式
validate.digitsPZ;  // 非负整数(正整数或0)正则表达式
validate.digitsNZ;  // 非正整数(负整数或0)正则表达式
validate.chinese;  // 中文正则表达式
validate.port;  // 端口号正则表达式
validate.ip;  // IP正则表达式
validate.longitude;  // 经度正则表达式
validate.latitude;  // 纬度正则表达式
```

<br/>

## 5.6. messageLoading  

antdv 的 message.loading 没有遮罩层覆盖界面，所以进行了扩展(v1.4.0新增)，使用方式：

::: code-tabs

@tab TypeScript

```vue
<template>
    <a-button @click="showLoading">loading</a-button>
</template>

<script lang="ts" setup>
    import { messageLoading } from 'ele-admin-pro';

    const showLoading = () => {
        // 快速使用, 参数为 string 或 object 类型
        const hide = messageLoading('正在请求中..');
        // 关闭方法
        hide();
    }
</script>
```

@tab JavaScript

```vue
<template>
    <a-button @click="showLoading">loading</a-button>
</template>

<script setup>
    import { messageLoading } from 'ele-admin-pro';

    const showLoading = () => {
        // 快速使用, 参数为 string 或 object 类型
        const hide = messageLoading('正在请求中..');
        // 关闭方法
        hide();
    }
</script>
```

:::

完整参数：

```javascript
const hide = messageLoading({
    content: '正在请求中..',  // 提示内容
    duration: 0,  // 自动关闭的延时，单位秒。设为 0 时不自动关闭
    onClose: () => {},  // 关闭时触发的回调函数
    mask: false,  // 是否需要半透明遮罩, 默认是全透明, 但也会覆盖全界面
    center: false  // 是否垂直水平居中显示, v1.11.0新增
});
```

<br/>

## 5.7. useFormData hook  

位于 `src/utils/use-form-data.ts` ，封装了表单重置、赋值不改变原字段的方法 `v1.10.1新增` ，使用示例：

::: code-tabs

@tab TypeScript

```vue
<template>
    <div>
        <pre>{{ JSON.stringify(form, null, 4) }}</pre>
        <div>
            <a-button @click="onReset">重置</a-button>
            <a-button @click="onAssign">赋值</a-button>
        </div>
    </div>
</template>

<script lang="ts" setup>
    import useFormData from '@/utils/use-form-data';
    import type { User } from '@/api/system/user/model';

    // 返回的 form 是已经进行 reactive 包装的了
    const { form, resetFields, assignFields } = useFormData<User>({
        userId: undefined,
        username: '',
        nickname: ''
    });

    /*  resetFields 用于把 form 的每个字段重置为初始值 */
    const onReset = () => {
        resetFields();
    };

    /*  assignFields 用于给 form 的每个字段赋新的值，不会改变 form 的字段数量 */
    const onAssign = () => {
        assignFields({
            userId: 1,
            username: 'admin',
            nickname: '管理员',
            createTime: '2022/09/03 10:35:00'
        });
        // 例如上面的 createTime 字段就不会赋值给 form
    };
</script>
```

@tab JavaScript

```vue
<template>
    <div>
        <pre>{{ JSON.stringify(form, null, 4) }}</pre>
        <div>
            <a-button @click="onReset">重置</a-button>
            <a-button @click="onAssign">赋值</a-button>
        </div>
    </div>
</template>

<script setup>
    import useFormData from '@/utils/use-form-data';

    // 返回的 form 是已经进行 reactive 包装的了
    const { form, resetFields, assignFields } = useFormData({
        userId: undefined,
        username: '',
        nickname: ''
    });

    /*  resetFields 用于把 form 的每个字段重置为初始值 */
    const onReset = () => {
        resetFields();
    };

    /*  assignFields 用于给 form 的每个字段赋新的值，不会改变 form 的字段数量 */
    const onAssign = () => {
        assignFields({
            userId: 1,
            username: 'admin',
            nickname: '管理员',
            createTime: '2022/09/03 10:35:00'
        });
        // 例如上面的 createTime 字段就不会赋值给 form
    };
</script>
```

:::

结合表单组件及表单验证使用的完整例子，例如实现一个用户添加和修改的表单弹窗组件：

::: code-tabs

@tab TypeScript

```vue
<template>
    <ele-modal
        :width="460"
        :visible="visible"
        :confirm-loading="loading"
        :title="isUpdate ? '修改用户' : '添加用户'"
        @update:visible="updateVisible"
        @ok="save"
    >
        <a-form
            ref="formRef"
            :model="form"
            :rules="rules"
            :label-col="{ flex: '90px' }"
            :wrapper-col="{ flex: '1' }"
        >
            <a-form-item label="用户账号" name="username">
                <a-input v-model:value="form.username" placeholder="请输入用户账号" />
            </a-form-item>
            <a-form-item label="用户名" name="nickname">
                <a-input v-model:value="form.nickname" placeholder="请输入用户名" />
            </a-form-item>
        </a-form>
    </ele-modal>
</template>

<script lang="ts" setup>
    import { ref, reactive, watch } from 'vue';
    import { message } from 'ant-design-vue/es';
    import type { FormInstance, Rule } from 'ant-design-vue/es/form';
    import useFormData from '@/utils/use-form-data';
    import { addUser, updateUser } from '@/api/system/user';
    import type { User } from '@/api/system/user/model';

    const props = defineProps<{
        // 弹窗是否打开
        visible: boolean;
        // 修改时回显的数据
        data?: User | null;
    }>();

    const emit = defineEmits<{
        (e: 'done'): void;
        (e: 'update:visible', visible: boolean): void;
    }>();

    // 表单ref
    const formRef = ref<FormInstance | null>(null);

    // 表单数据
    const { form, resetFields, assignFields } = useFormData<User>({
        userId: undefined,
        username: '',
        nickname: ''
    });

    // 表单验证规则
    const rules = reactive<Record<string, Rule[]>>({
        username: [
            { required: true, message: '请输入用户账号', type: 'string', trigger: 'blur' }
        ],
        nickname: [
            { required: true, message: '请输入用户名', type: 'string', trigger: 'blur' }
        ]
    });

    // 是否是修改
    const isUpdate = ref(false);

    // 提交状态
    const loading = ref(false);

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
                updateVisible(false);
                emit('done');
            }).catch((e) => {
                loading.value = false;
                message.error(e.message);
            });
        }).catch(() => {});
    };

    /* 更新visible */
    const updateVisible = (value: boolean) => {
        emit('update:visible', value);
    };

    /* 弹窗关闭后重置表单，打开后赋值数据 */
    watch(() => props.visible, (visible) => {
        if (visible) {
            if (props.data) {
                assignFields(props.data);
                isUpdate.value = true;
            } else {
                isUpdate.value = false;
            }
        } else {
            resetFields();
            formRef.value?.clearValidate();
            // 这里也可以使用下面一行代替上面两行，但需要注意没有加 name 的 a-form-item 不会被重置
            //formRef.value?.resetFields();
            // 实际使用一般都是需要表单验证的 a-form-item 才加 name 所以建议最好用上面两行的形式
        }
    });
</script>
```

@tab JavaScript

```vue
<template>
    <ele-modal
        :width="460"
        :visible="visible"
        :confirm-loading="loading"
        :title="isUpdate ? '修改用户' : '添加用户'"
        @update:visible="updateVisible"
        @ok="save"
    >
        <a-form
            ref="formRef"
            :model="form"
            :rules="rules"
            :label-col="{ flex: '90px' }"
            :wrapper-col="{ flex: '1' }"
        >
            <a-form-item label="用户账号" name="username">
                <a-input v-model:value="form.username" placeholder="请输入用户账号" />
            </a-form-item>
            <a-form-item label="用户名" name="nickname">
                <a-input v-model:value="form.nickname" placeholder="请输入用户名" />
            </a-form-item>
        </a-form>
    </ele-modal>
</template>

<script setup>
    import { ref, reactive, watch } from 'vue';
    import { message } from 'ant-design-vue/es';
    import useFormData from '@/utils/use-form-data';
    import { addUser, updateUser } from '@/api/system/user';

    const props = defineProps({
        // 弹窗是否打开
        visible: Boolean,
        // 修改时回显的数据
        data: Object
    });

    const emit = defineEmits(['done', 'update:visible']);

    // 表单ref
    const formRef = ref(null);

    // 表单数据
    const { form, resetFields, assignFields } = useFormData({
        userId: undefined,
        username: '',
        nickname: ''
    });

    // 表单验证规则
    const rules = reactive({
        username: [
            { required: true, message: '请输入用户账号', type: 'string', trigger: 'blur' }
        ],
        nickname: [
            { required: true, message: '请输入用户名', type: 'string', trigger: 'blur' }
        ]
    });

    // 是否是修改
    const isUpdate = ref(false);

    // 提交状态
    const loading = ref(false);

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
                updateVisible(false);
                emit('done');
            }).catch((e) => {
                loading.value = false;
                message.error(e.message);
            });
        }).catch(() => {});
    };

    /* 更新visible */
    const updateVisible = (value) => {
        emit('update:visible', value);
    };

    /* 弹窗关闭后重置表单，打开后赋值数据 */
    watch(() => props.visible, (visible) => {
        if (visible) {
            if (props.data) {
                assignFields(props.data);
                isUpdate.value = true;
            } else {
                isUpdate.value = false;
            }
        } else {
            resetFields();
            formRef.value?.clearValidate();
            // 这里也可以使用下面一行代替上面两行，但需要注意没有加 name 的 a-form-item 不会被重置
            //formRef.value?.resetFields();
            // 实际使用一般都是需要表单验证的 a-form-item 才加 name 所以建议最好用上面两行的形式
        }
    });
</script>
```

:::

位于`src/utils/use-search.ts`，用于简化搜索和重置搜索操作，重置搜索就不用自己给每个字段都重新赋值了，v1.6.0新增，v1.10.1移除：

::: code-tabs

@tab TypeScript

```vue
<template>
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
                <a-form-item label="用户名">
                    <a-input
                        v-model:value.trim="where.nickname"
                        placeholder="请输入"
                        allow-clear
                    />
                </a-form-item>
            </a-col>
            <a-col :lg="6" :md="12" :sm="24" :xs="24">
                <a-form-item class="ele-text-right" :wrapper-col="{ span: 24 }">
                    <a-space>
                        <a-button type="primary" @click="search">查询</a-button>
                        <a-button @click="reset">重置</a-button>
                    </a-space>
                </a-form-item>
            </a-col>
        </a-row>
    </a-form>
</template>

<script lang="ts" setup>
    import useSearch from '@/utils/use-search';
    import type { UserParam } from '@/api/system/user/model';

    const emit = defineEmits<{
        (e: 'search', where?: UserParam): void;
    }>();

    // 表单数据, where 是已经进行 reactive 包装的对象了, resetFields 用于把 where 的每个字段重置为初始值
    const { where, resetFields } = useSearch<UserParam>({
        username: '',
        nickname: ''
    });

    /* 搜索 */
    const search = () => {
        emit('search', where);
    };

    /*  重置 */
    const reset = () => {
        resetFields();
        search();
    };
</script>
```

@tab JavaScript

```vue
<template>
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
                <a-form-item label="用户名">
                    <a-input
                        v-model:value.trim="where.nickname"
                        placeholder="请输入"
                        allow-clear
                    />
                </a-form-item>
            </a-col>
            <a-col :lg="6" :md="12" :sm="24" :xs="24">
                <a-form-item class="ele-text-right" :wrapper-col="{ span: 24 }">
                    <a-space>
                        <a-button type="primary" @click="search">查询</a-button>
                        <a-button @click="reset">重置</a-button>
                    </a-space>
                </a-form-item>
            </a-col>
        </a-row>
    </a-form>
</template>

<script setup>
    import useSearch from '@/utils/use-search';

    const emit = defineEmits(['search']);

    // 表单数据, where 是已经进行 reactive 包装的对象了, resetFields 用于把 where 的每个字段重置为初始值
    const { where, resetFields } = useSearch({
        username: '',
        nickname: ''
    });

    /* 搜索 */
    const search = () => {
        emit('search', where);
    };

    /*  重置 */
    const reset = () => {
        resetFields();
        search();
    };
</script>
```

:::

<br/>

## 5.8. 窗口尺寸监听  

在 `src/utils/on-size-change.ts` 中是封装的监听窗口尺寸变化的方法，使用方式：

```javascript
import { onSizeChange } from '@/utils/on-size-change';

onSizeChange(() => {
    console.log('onSizeChange');
});
```

核心的实现方式就是监听状态管理 `src/store/modules/theme.ts` 中的 `contentWidth` 字段变化：

```javascript
import { storeToRefs } from 'pinia';
import { useThemeStore } from '@/store/modules/theme';

const themeStore = useThemeStore();
const { contentWidth } = storeToRefs(themeStore);

// 使用 watch 监听 contentWidth 改变
watch(contentWidth, () => {
    resizeCharts();
});
```

`screenWidth` 、`contentWidth` 等字段会在窗口大小改变后、侧栏折叠展开等操作后同步更新值，在 `src/layout/index.vue` 中触发更新的。