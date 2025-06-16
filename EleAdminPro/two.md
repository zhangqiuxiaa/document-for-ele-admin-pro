---
title: 框架配置
createTime: 2025/01/10 09:30:40
permalink: /EleAdminPro/8gdvbykx/
---
## 2.1. 环境变量配置 

项目的环境变量配置位于根目录下的 `.env` 、`.env.development` 、`.env.production` 几个文件，具体可以参考 [Vite 文档](https://cn.vitejs.dev/guide/env-and-mode.html) ：

- .env - 在所有的环境下都会加载
- .env.development - 开发环境下会加载，对应 `pnpm run dev`
- .env.production - 生产环境下会加载，对应 `pnpm run build`

目前配置了两个变量，如果要增加变量必须以 `VITE_` 开头，代码中可以使用如 `import.meta.env.VITE_APP_NAME` 的方式读取：

```bash
# 项目名称，会显示在顶栏 logo 区及浏览器标题中
VITE_APP_NAME=Ele Admin Pro
# axios 请求的默认地址前缀
VITE_API_URL=https://v2.eleadmin.com/api
```

<br/>

## 2.2. 全局配置常量 

在 `src/config/setting.ts` 中定义了一些全局配置常量，可以根据自己的需要修改，v1.5.0 以及之前的版本请查看章节 12 ：

| 参数                | 说明                                               | 示例值                        |
| :------------------ | :------------------------------------------------- | :---------------------------- |
| HIDE_FOOTERS        | 不显示全局页脚的路由地址                           | `['/system/dictionary']`      |
| HIDE_SIDEBARS       | 不显示侧边栏的路由地址                             | `[]`                          |
| REPEATABLE_TABS     | 页签同路由不同参数可重复打开的路由                 | `['/system/user-info']`       |
| WHITE_LIST          | 不需要登录可访问的路由                             | `['/login', '/forget']`       |
| KEEP_ALIVE_EXCLUDES | 开启 KeepAlive 后仍然不需要缓存的路由 `v1.9.0新增` | `['/system/role']`            |
| USER_MENUS          | 直接指定菜单数据                                   | null                          |
| HOME_TITLE          | 首页名称, 为空则取第一个菜单的名称                 | null                          |
| HOME_PATH           | 首页路径, 为空则取第一个菜单的地址                 | null                          |
| TAB_KEEP_ALIVE      | 开启多页签后是否缓存组件                           | !import.meta.env.DEV          |
| TOKEN_HEADER_NAME   | token 传递的 header 名称                           | 'Authorization'               |
| TOKEN_STORE_NAME    | token 在 storage 中存储的名称                      | 'access_token'                |
| THEME_STORE_NAME    | 主题配置在 storage 中存储的名称                    | 'theme'                       |
| I18N_CACHE_NAME     | i18n 缓存的名称                                    | 'i18n-lang'                   |
| LAYOUT_PATH         | 外层布局的路由地址 `v1.8.0新增`                    | '/'                           |
| REDIRECT_PATH       | 刷新路由的路由地址 `v1.8.0新增`                    | '/redirect'                   |
| API_BASE_URL        | 对应 env 中的配置，简化调用 `v1.8.0新增`           | import.meta.env.VITE_API_URL  |
| PROJECT_NAME        | 对应 env 中的配置，简化调用 `v1.8.0新增`           | import.meta.env.VITE_APP_NAME |
| MAP_KEY             | 高德地图 key `v1.8.0新增`                          |                               |
| LICENSE_CODE        | EleAdminPro 授权码 `v1.8.0新增`                    |                               |

其它地方调用示例：

```javascript
import { MAP_KEY } from '@/config/setting';
```

<br/>

## 2.3. 菜单路由配置  

登录界面在 `src/views/login/index.vue` 中，登录接口需要的数据格式为如下，如果格式不一致直接在 api 或 vue 中修改：

```json
{
    "code": 0,
    "message": "登录成功",
    "data": {
        "access_token": "eyJhbGciOiJIUzI1NiJ9.eyJzdWIiOiJhZ"
    }
}
```

登录后通过 `src/utils/token-util.ts` 的 `setToken` 方法缓存 token， 如果勾选记住密码存在 localStorage 中，否则存在 sessionStorage 中。

**登录认证流程图：**

![](/images/2-1.svg)

这里请求菜单数据的接口与请求用户信息、按钮权限、角色等信息的接口是同一个，如果你是分多个接口参考文档 2.8 的示例。

<br/>

## 2.4. 菜单路由配置  

  在 `src/router/index.ts` 中配置有路由守卫，如果没有登录(没有缓存的 token)会跳转到登录界面，如果已登录会再判断是否已经获取到当前登录用户的菜单， 如果没有获取就调用 `src/store/modules/user.ts` 中的 `fetchUserInfo` 方法获取登录用户的信息及菜单数据，然后根据菜单数据注册动态路由， `fetchUserInfo` 方法是调用 `src/api/layout/index.ts` 中的 `getUserInfo` 方法请求后端接口，与后端对接时可修改这两个方法调整数据格式， 默认需要返回的数据格式为：

```json
{
    "code": 0,
    "data": {
        "nickname": "管理员",
        "avatar": "https://cdn.eleadmin.com/20200610/avatar.jpg",
        "authorities": [
            {
                "title": "系统管理",
                "icon": "setting-outlined",
                "path": "/system",
                "hide": false,
                "children": [
                    {
                        "title": "用户管理",
                        "icon": "team-outlined",
                        "path": "/system/user",
                        "component": "/system/user",
                        "hide": false,
                        "active": null,
                        "hideFooter": false,
                        "hideSidebar": false,
                        "closable": true,
                        "tabUnique": true
                    }, 
                    {
                        "title": "我是内链",
                        "icon": "team-outlined",
                        "path": "/system/baidu",
                        "component": "https://baidu.com"
                    }, 
                    {
                        "title": "我是外链",
                        "icon": "team-outlined",
                        "path": "https://eleadmin.com"
                    }
                ]
            }
        ],
        "roles": [
            { "roleCode": "admin" },
            { "roleCode": "user" }
        ]
    }
}
```

data 是用户信息数据，里面的 authorities 是用户的菜单数据，菜单的数据格式说明：

- path   路由关键字，必须，没指定 fullPath 的时候建议都以 / 开头
- component   组件地址，组件要放在 views 目录下，父级非必须
- redirect   路由的 redirect，非必须，默认会处理为父级路由重定向到第一个子级的 path
- name   路由的 name，非必须，也支持放在 meta 中，下面几个字段都可放在 meta 中
- title   菜单标题，必须
- icon   菜单图标，非必须
- hide   为 true 只注册路由不显示在左侧菜单，比如非弹窗形式的添加页面，非必须
- active   比如添加页面不在侧栏，打开后侧栏就没有选中，这个配置选中地址，非必须
- hideFooter   是否隐藏全局页脚，非必须，也可在 setting.ts 中配置 (v1.2.0新增)
- hideSidebar   是否隐藏侧栏，非必须，也可在 setting.ts 中配置 (v1.2.0新增)
- closable   是否可以在多页签中关闭，非必须，不写默认为 true (v1.2.0新增)
- tabUnique   同路由不同参数是否只显示一个页签，不写默认为 true (v1.3.0新增)
- fullPath   当路由关键字与菜单展示的地址不一致时这个配置菜单地址 (v1.7.0新增)
- keepAlive   页签是否可缓存，默认 true ，也可在 setting.ts 中配置 (v1.9.0新增)
- meta   Object 类型或 json 字符串，非必须，对应路由元数据 meta
- children   子级，非必须

  ( fullPath || path ) 会在渲染菜单的时候当作循环的 key ，所以需要必填且唯一，混合导航布局父级也会需要 path 跳转路由， 父级一般都是重定向到第一个子级，path 如果以 `https://`、`http://`、`//` 开头是外链形式， 外链不注册路由，`component` 如果是网址则是内链形式(v1.1.0 新增)，否则就是路由组件的形式， 其实标准的数据格式是除了 path、component 等路由需要的字段外其它都要放在 meta 中，考虑到正常后端数据库菜单表的字段大都是平铺式的设计， 所以是在前端处理放在 meta 下面，也支持接口返回 meta 字段，会合并在一起，注意菜单在注册路由的时候会铺平，所以父级不需要写 component ，写了也不会有嵌套路由的效果。

  这里的菜单数据 authorities 不一定要是 children 形式的数据， 也可以是 parentId 形式的数据在 `fetchUserInfo` 里面使用 toTreeData 转换， 默认的代码里面就是 parentId 的形式前端转成 children 形式，此外默认的配套后端的设计是菜单里面包含按钮级权限， 所以 authorities 里面是菜单和按钮权限混在一起的，`fetchUserInfo` 方法里面进行筛选， 把按钮权限标识放在 user 状态管理 state 的 authorities 中，去掉按钮后的菜单转成 children 形式数据放在 state 的 menus 中。

如果不使用接口返回菜单，直接前端配置使用 src/config/setting.ts 中的 `USER_MENUS` 参数来指定菜单：

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
                meta: {title: '用户管理', icon: 'team-outlined', hide: false}
            }
        ]
    }
]
```

这里的数据格式也可与上面的接口方式完全一致，同样支持外链和内链，如果菜单和用户信息是分两个接口可以看文档 2.8 。

要注意组件 component 要放在 src/views 目录下且要为 index.vue 命名，因为 vite 要支持路由动态导入组件要使用 Glob 导入：

```javascript
// 在 src/router/routes.ts 中是这样写的 Glob 导入, v1.7.0 以及之前的版本在 src/router/index.ts
const modules = import.meta.glob('/src/views/**/index.vue');
```

不建议修改上面规则改为 `/src/views/**/*.vue` ，因为 `modules` 会被编译为如下格式：

```javascript
const modules = {
    '/src/views/system/user/index.vue': () => import('/src/views/system/user/index.vue'),
    '/src/views/system/role/index.vue': () => import('/src/views/system/role/index.vue'),
    // 如果改为 *.vue 一些无关的组件也会扫描进去
    '/src/views/system/user/components/user-edit.vue': () => import('/src/views/system/user/components/user-edit.vue'),
    '/src/views/system/role/components/role-edit.vue': () => import('/src/views/system/role/components/role-edit.vue'),
}
```

`modules` 是只用于获取路由对应的组件的，如果改为 `*.vue` 编译后这个 `modules` 会多很多无关的字段增大打包体积。

**formatMenus 方法说明：**

```javascript
// 在 src/store/modules/user.ts 中有用到
import { formatMenus } from 'ele-admin-pro';

const { menus, homePath, homeTitle } = formatMenus(data, childrenField);
```

  此方法是用于把接口格式的菜单数据转成标准格式的数据，会把 icon、title、hide 等非路由需要的字段放在 meta 下，component 会去掉前面的 '/' ， 父级增加 redirect 字段指向第一个子级的 path， 返回的 homePath 是第一个不为目录的菜单的 path，homeTitle 是第一个不为目录的菜单的 title，childrenField 非必须，默认为 'children' 。

**menuToRoutes 方法说明：**

```javascript
// 在 src/router/index.ts 中有用到
import { menuToRoutes } from 'ele-admin-pro';
const modules = import.meta.glob('/src/views/**/index.vue');

const routes = menuToRoutes(menus, (component) => modules[`/src/views/${component}.vue`], []);
```

此方法是用于把菜单数据转成路由数据，包括过滤掉重复的路由 path、过滤掉外链、内链的数据处理等：

- 参数一   Array，菜单数据，最好先经过 formatMenus 方法处理
- 参数二   Function，加载组件的方法
- 参数三   Array，已添加的路由地址，如有些菜单已配置静态路由不要再动态注册 `['/system/user']`
- 参数四   String，刷新路由的地址，如果不平铺路由最好在每级都加刷新路由
- 参数五   Object，刷新路由的组件，如果不平铺路由最好在每级都加刷新路由
- 参数六   String，刷新路由的匹配表达式

**如果接口返回的数据已经是 children 形式：**

```javascript
// src/store/modules/user.ts
export const useUserStore = defineStore({
    //......省略其它代码
    actions: {
        async fetchUserInfo() {
            // ......省略其它代码
            // 只需要去掉 toTreeData 方法，切忌不要去掉 formatMenus 方法
            // 如果接口返回的菜单数据没有按钮类型要去掉 ?.filter((d) => d.menuType === 0) 这句代码
            const { menus, homePath } = formatMenus(USER_MENUS ?? result.authorities);
            /*const { menus, homePath } = formatMenus(
                USER_MENUS ?? toTreeData({
                    data: result.authorities?.filter((d) => d.menuType === 0),
                    idField: 'menuId',
                    parentIdField: 'parentId'
                })
            );*/
            this.menus = menus;
            return { menus, homePath };
        }
    }
});
```

**如果接口返回的数据字段名等与要求的不一致可在前端处理：**

```javascript
// src/store/modules/user.ts
export const useUserStore = defineStore({
    //......省略其它代码
    actions: {
        async fetchUserInfo() {
            // ......省略其它代码
            const { menus, homePath } = formatMenus(
                USER_MENUS ?? toTreeData({
                    // 例如菜单数据的字段名不一致可以在这后面使用 map 方法处理
                    data: result.authorities?.filter((d) => d.menuType === 0).map(d => {
                        return {
                            path: d.menuUrl,  // 例如你的菜单路由字段是 menuUrl
                            title: d.menuName  // 例如你的菜单名称字段是 menuName
                        };
                    }),
                    idField: 'menuId',
                    parentIdField: 'parentId'
                })
            );
            this.menus = menus;
            return { menus, homePath };
        }
    }
});
```

<br/>

## 2.5. 用户信息接口  

在进入系统之前会先调用 `store/modules/user.ts` 中的 `fetchUserInfo` 方法获取登录用户信息然后注册动态路由，接口的数据格式：

```json
{
    "code": 0,
    "data": {
        "nickname": "管理员",
        "avatar": "http://abc.com/a.png",
        "authorities": [
            { "authority": "user:add" },
            { "authority": "role:add" }
        ],
        "roles": [
            { "roleCode": "admin" },
            { "roleCode": "user" }
        ]
    }
}
```

- nickname   用户昵称，会显示在顶栏
- avatar   用户头像，会显示在顶栏
- authorities   用户所拥有的权限，在权限控制组件中用到
- roles   用户所拥有的角色，在权限控制组件中用到

  之所以要在路由守卫中获取用户信息一是为了获取菜单注册动态路由， 二是为了要在页面渲染出来之前就获取到用户的权限和角色数据，以用于权限控制指令(见文档 5.3)， 用户信息、权限数据、角色数据、菜单数据等都使用 pinia 进行状态管理，以便在任何地方都能获取到。

在 vue 模板中获取用户信息：

```vue
<template>
    <h4>欢迎 {{ loginUser.nickname }} ！</h4>
</template>

<script lang="ts" setup>
    import { computed } from 'vue';
    import { useUserStore } from '@/store/modules/user';

    const userStore = useUserStore();

    const loginUser = computed(() => userStore.info ?? {});
</script>
```

<br/>

## 2.6. 静态路由配置  

可以在 `src/router/routes.ts` 中的 `routes` 里配置一些不需要顶栏、侧栏的静态路由，如登录、注册、忘记密码页面等:

```javascript
// v1.7.0 以及之前的版本在 src/router/index.ts
const routes = [
    {
        path: '/login',
        component: () => import('@/views/login/index.vue'),
        meta: { title: '登录' }
    },
    {
        path: '/forget',
        component: () => import('@/views/forget/index.vue'),
        meta: { title: '忘记密码' }
    },
    // 没有匹配的路由显示 404 页面
    {
        path: '/:path(.*)*',
        component: () => import('@/views/exception/404/index.vue')
    }
];
```

  注意 path 不要为 `/`，因为接口返回的动态路由会注册到 `/` 下面，meta 下面的 `title` 是必填的，会显示在浏览器页签的标题上， 静态路由不会显示在侧栏菜单中，如果你是想纯前端配置菜单应该在 src/config/setting.ts 中的 `USER_MENUS` 中配置，参考文档 2.4 ， 如果是想在后端接口返回的菜单数据基础上加一些菜单可修改 `src/store/modules/user.ts` 中的 `fetchUserInfo` 方法：

```javascript
// 额外的菜单数据，数据格式与文档 2.4 中的介绍保持一致
const EXTRA_MENUS = [{
    path: '/demo',
    meta: { title: 'XX管理' },
    children: [
        {
            path: '/demo/test1',
            component: '/demo/test1',
            meta: { title: 'test1' }
        },
        {
            path: '/demo/test2',
            component: '/demo/test2',
            meta: { title: 'test2' }
        }
    ]
}];
//......省略其它代码
export const useUserStore = defineStore({
    actions: {
        async fetchUserInfo() {
            //......省略其它代码
            const { menus, homePath } = formatMenus(
                USER_MENUS ??
                toTreeData({
                    data: result.authorities?.filter((d) => d.menuType === 0),
                    idField: 'menuId',
                    parentIdField: 'parentId'
                }).concat(EXTRA_MENUS)  // 在这里使用 concat 添加额外的菜单数据
            );
            this.menus = menus;
            return { menus, homePath };
        }
    }
});
```

<br/>

## 2.7. 编写 api 模块 

本框架使用 axios 工具库操作 http 请求，`src/utils/request.ts` 是创建的 axios 实例，在里面可以配置拦截器等：

```javascript
import axios from 'axios';
// ...代码省略
const service = axios.create({
    baseURL: import.meta.env.VITE_API_URL
});

/* 请求拦截器 */
service.interceptors.request.use('...代码省略');

/* 响应拦截器 */
service.interceptors.response.use(
    (res) => {
        // 统一处理登录过期状态等
        return res;
    }, 
    (error) => {
        // 如果接口 http 状态码不是 200 可在这里处理
        console.log(error.response);
        return Promise.reject(error);
    }
);

// ...代码省略
```

- **baseURL** 说明：

  用于配置接口统一的前缀地址，`import.meta.env.VITE_API_URL` 这个是取的 `.env` 里面的环境变量， 在项目的根目录下有好几个以 `.env` 开头的文件，分别对应不同的运行环境。

- **token** 自动续期：

  这里设计的是后端在 token 将要过期的时候返回新的 token 在 response 的 header 中，然后前端获取后覆盖当前缓存的 token， 下次再请求就会用新的 token 了，这样可以让用户无感知的续期 token，防止用户正在操作的时候 token 过期需要重新登录， 也有些后端可能需要单独请求续期 token 的接口来续期 token，这种前端如果要实现无感知的续期 token 比较麻烦，可以自行根据后端要求修改。

**编写 api 模块**

api 模块都写在 `/src/api/` 下，例如编写一个模块的增删改查 api ：

```javascript
import request from '@/utils/request';

/** 查询 */
export async function listUsers(params) {
    const res = await request.get('/system/user', { params });
    if (res.data.code === 0 && res.data.data) {
        return res.data.data;
    }
    return Promise.reject(new Error(res.data.message));
}

/** 根据 id 查询 */
export async function getUser(id) {
    const res = await request.get('/system/user/' + id);
    if (res.data.code === 0 && res.data.data) {
        return res.data.data;
    }
    return Promise.reject(new Error(res.data.message));
}

/** 添加 */
export async function addUser(data) {
    const res = await request.post('/system/user', data);
    if (res.data.code === 0) {
        return res.data.message;
    }
    return Promise.reject(new Error(res.data.message));
}

/** 修改 */
export async function updateUser(data) {
    const res = await request.put('/system/user', data);
    if (res.data.code === 0) {
        return res.data.message;
    }
    return Promise.reject(new Error(res.data.message));
}

/** 删除 */
export async function removeUser(id) {
    const res = await request.delete('/system/user/' + id);
    if (res.data.code === 0) {
        return res.data.message;
    }
    return Promise.reject(new Error(res.data.message));
}
```

使用 api 模块：

```javascript
import { listUsers, getUser, addUser } from '@/api/system/user';
import { message } from 'ant-design-vue';

// 根据 id 查询
getUser(1).then(data => {
    console.log(data);
}).catch(e => {
    message.error(e.message);
});

// 查询
listUsers().then((data) => {
    console.log(data);
}).catch(e => {
    message.error(e.message);
});

// 添加
addUser({
    username: 'admin',
    password: '123456'
}).then((message) => {
    message.success(message);
}).catch(e => {
    message.error(e.message);
});
```

  上面展示的都把判断接口返回的数据里面的业务状态码的操作也封装进 api， 这样调用 api 时 then 就是请求成功，并且 then 里面一定有且只有需要返回的数据， catch 就是请求失败，并返回错误信息，就不需要在复用接口的时候每次判断业务状态码了， 这里的 async、await 相当于 Promise 的简写：

```javascript
import request from '@/utils/request';

export function listUsers(params) {
    return new Promise((resolve, reject) => {
        request.get('/system/user', { params }).then((res) => {
            if (res.data.code === 0 && res.data.data) {
                resolve(res.data.data);
            } else {
                reject(new Error(res.data.message));
            }
        }).catch((e) => {
            reject(e);
        });
    });
}
```

**Typescript 的支持**

在 `src/api/index.ts` 中定义了一些基本的数据类型，可以根据自己的后端接口进行修改：

```typescript
/** 接口统一返回结果 */
export interface ApiResult<T> {
    // 状态码
    code: number;
    // 状态信息
    message?: string;
    // 返回数据, T 是泛型
    data?: T;
}

/** 分页查询统一结果 */
export interface PageResult<T> {
    // 返回数据
    list: T[];
    // 总数量
    count: number;
}
```

在使用 axios 编写 api 模块的时候可以指定类型：

```typescript
import request from '@/utils/request';
import type { ApiResult, PageResult } from '@/api';
import type { User, UserParam } from './model';

/** 查询, 查询用户的时候就可以给 ApiResult 指定 User 类型 */
export async function listUsers(params: UserParam) {
    const res = await request.get<ApiResult<User[]>>('/system/user', { params });
    if (res.data.code === 0 && res.data.data) {
        return res.data.data;
    }
    return Promise.reject(new Error(res.data.message));
}

/** 分页查询, 分页查询 ApiResult 结合 PageResult 使用 */
export async function pageUsers(params: UserParam) {
    const res = await request.get<ApiResult<PageResult<User>>>('/system/user', { params });
    if (res.data.code === 0 && res.data.data) {
        return res.data.data;
    }
    return Promise.reject(new Error(res.data.message));
}

/** 根据 id 查询 */
export async function getUser(id: number) {
    const res = await request.get<ApiResult<User>>('/system/user/' + id);
    if (res.data.code === 0 && res.data.data) {
        return res.data.data;
    }
    return Promise.reject(new Error(res.data.message));
}

/** 添加, 添加、修改、删除这种 ApiResult 没有 data 可以指定 unknown */
export async function addUser(data: User) {
    const res = await request.post<ApiResult<unknown>>('/system/user', data);
    if (res.data.code === 0) {
        return res.data.message;
    }
    return Promise.reject(new Error(res.data.message));
}
```

<br/>

## 2.8. 进阶配置示例  

**请求用户信息和菜单数据分两个接口**

默认用户信息接口里面权限是包含菜单和按钮权限的，如果菜单和用户信息需要分两个接口可以修改用户状态管理的 `fetchUserInfo` 方法：

```javascript
// src/store/modules/user.ts
import { getUserInfo, getUserMenu } from '@/api/layout';

// 省略其它代码......
export const useUserStore = defineStore({
    actions: {
        // 使用 Promise.all 同时调用两个 api 请求
        fetchUserInfo() {
            return new Promise((resolve, reject) => {
                Promise.all([getUserInfo(), getUserMenu()]).then(([userRes, menuRes]) => {
                    // 用户信息
                    this.info = userRes;
                    // 用户权限
                    this.authorities = userRes.authorities?.map((d) => d.authority) ?? [];
                    // 用户角色
                    this.roles = userRes.roles?.map((d) => d.roleCode) ?? [];
                    // 用户菜单, 转为 children 形式
                    const { menus, homePath } = formatMenus(
                        toTreeData({
                            data: menuRes,
                            idField: 'menuId',
                            parentIdField: 'parentId'
                        })
                    );
                    this.menus = menus;
                    resolve({ menus, homePath });
                }).catch((e) => {
                    reject(e);
                });
            });
        }
    }
});
```

src/api/layout/index.ts 中的写法：

::: code-tabs

@tab TypeScript

```typescript
import request from '@/utils/request';
import type { ApiResult } from '@/api';
import type { User } from '@/api/system/user/model';
import type { Menu } from '@/api/system/menu/model';

/** 获取当前登录的用户信息、权限、角色 */
export async function getUserInfo(): Promise<User> {
    const res = await request.get<ApiResult<User>>('/auth/user');
    if (res.data.code === 0 && res.data.data) {
        return res.data.data;
    }
    return Promise.reject(new Error(res.data.message));
}

/** 获取当前登录的用户菜单 */
export async function getUserMenu(): Promise<Menu[]> {
    const res = await request.get<ApiResult<Menu[]>>('/auth/menu');
    if (res.data.code === 0 && res.data.data) {
        return res.data.data;
    }
    return Promise.reject(new Error(res.data.message));
}
```

@tab JavaScript

```javascript
import request from '@/utils/request';

/** 获取当前登录的用户信息、权限、角色 */
export async function getUserInfo() {
    const res = await request.get('/auth/user');
    if (res.data.code === 0 && res.data.data) {
        return res.data.data;
    }
    return Promise.reject(new Error(res.data.message));
}

/** 获取当前登录的用户菜单 */
export async function getUserMenu() {
    const res = await request.get('/auth/menu');
    if (res.data.code === 0 && res.data.data) {
        return res.data.data;
    }
    return Promise.reject(new Error(res.data.message));
}

```

:::

**取消请求用户信息、角色、权限、菜单等数据的接口**

当登录成功后如果还未注册动态路由会先调用请求用户信息等的接口，如果不需要可以修改用户状态管理的 `fetchUserInfo` 方法：

```javascript
// src/store/modules/user.ts
export const useUserStore = defineStore({
    actions: {
        //......省略其它代码
        async fetchUserInfo() {
            // 用户信息
            this.info = {};
            // 用户菜单
            const { menus, homePath } = formatMenus(USER_MENUS);
            this.menus = menus;
            return { menus, homePath };
        }
    }
});
```

然后在 `src/config/setting.ts` 中的 `USER_MENUS` 直接配置菜单数据即可，这样进入系统就不会调用任何接口了。

**不需要登录界面**

当没有缓存 token 时访问非白名单的路由会自动跳转到登录界面，如果不需要登录界面修改 `src/router/index.ts` ：

```javascript
/** 将路由守卫中判断是否登录相关的代码注释掉即可 */
router.beforeEach(async (to, from) => {
    // ......省略其它代码
    /*if (!getToken()) {
        // 未登录跳转登录界面
        if (!WHITE_LIST.includes(to.path)) {
            return {
                path: '/login',
                query: to.path === '/' ? {} : { from: to.path }
            };
        }
        return;
    }*/
    // 注册动态路由
    const userStore = useUserStore();
    if (!userStore.menus) {
        const { menus, homePath } = await userStore.fetchUserInfo();
        if (menus) {
            router.addRoute(getMenuRoutes(menus, homePath));
            return { ...to, replace: true };
        }
    }
});
```