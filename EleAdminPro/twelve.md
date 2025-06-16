---
title: 旧版配置
createTime: 2025/01/10 09:30:40
permalink: /EleAdminPro/d6e81tkx/
---
## 12.1. 环境变量配置  

此部分文档适用于 v1.5.0 及之前的版本

在项目根目录有`.env`开头的几个文件，用于定义环境变量，不同打包方式会读取对应的文件，环境变量修改后需要重新运行项目：

- .env - 生产环境配置，所有环境都会先读取该文件
- .env.development - 开发环境配置，对应`npm run serve`
- .env.preview - 测试(预览)环境配置，对应`npm build:preview`

例如使用`npm run serve`会先读取`.env`再读取`.env.development`, 后读取的配置会覆盖前面读取的，目前配置了三个变量：

- VUE_APP_VERSION - 版本号
- VUE_APP_NAME - 项目名称，会显示在顶栏logo以及浏览器标题中
- VUE_APP_API_BASE_URL - axios请求的默认地址前缀

<br/>

## 12.2. 项目全局配置  

在`src/config/setting.js`中定义了许多全局配置参数：

| 参数            | 说明                                               | 默认值                   |
| :-------------- | :------------------------------------------------- | :----------------------- |
| whiteList       | 路由白名单(不需要登录的)                           | `['/login', '/forget']`  |
| hideFooters     | 不显示全局页脚的路由地址(v1.2.0新增)               | `['/system/dictionary']` |
| hideSidebars    | 不显示侧边栏的路由地址(v1.2.0新增)                 | `[]`                     |
| repeatableTabs  | 可重复打开页签的路由地址(v1.3.0新增)               | `['/system/user/info']`  |
| menuUrl         | 菜单数据接口                                       | '/main/menu'             |
| parseMenu       | 自定义解析接口菜单数据                             | null                     |
| parseMenuItem   | 自定义解析接口菜单每一个数据格式                   | null                     |
| menus           | 直接指定菜单数据                                   | null                     |
| userUrl         | 用户信息接口                                       | '/main/user'             |
| parseUser       | 自定义解析接口用户信息                             | null                     |
| tokenHeaderName | token传递的header名称                              | 'Authorization'          |
| tokenStoreName  | token存储的名称                                    | 'access_token'           |
| takeToken       | 获取缓存的token(v1.1.0新增)                        |                          |
| cacheToken      | 缓存token(v1.1.0新增)                              |                          |
| userStoreName   | 用户信息存储的名称                                 | 'user'                   |
| takeUser        | 获取缓存的用户信息(v1.1.0新增)                     |                          |
| cacheUser       | 缓存用户信息(v1.1.0新增)                           |                          |
| themeStoreName  | 主题配置存储的名称                                 | 'theme'                  |
| homeTitle       | 首页tab显示标题, null会根据菜单自动获取            | '主页'                   |
| homePath        | 首页路径, null会自动获取                           | null                     |
| showSetting     | 顶栏是否显示主题设置按钮                           | true                     |
| tabKeepAlive    | 开启多页签是否缓存组件(v1.5.0新增)                 | true                     |
| collapse        | 是否默认折叠侧边栏                                 | false                    |
| sideStyle       | light(亮色), dark(暗色)                            | dark                     |
| headStyle       | 顶栏风格: light(亮色), dark(暗色), primary(主色)   | light                    |
| tabStyle        | 标签页风格: default(默认), dot(圆点), card(卡片)   | default                  |
| layoutStyle     | 布局风格: side(默认), top(顶栏菜单), mix(混合菜单) | side                     |
| fixedSidebar    | 是否固定侧栏                                       | true                     |
| fixedHeader     | 是否固定顶栏                                       | false                    |
| fixedBody       | 是否固定主体                                       | false                    |
| logoAutoSize    | logo是否自适应宽度                                 | false                    |
| bodyFull        | 内容区域宽度是否铺满                               | true                     |
| showTabs        | 是否开启多标签                                     | true                     |
| colorfulIcon    | 侧栏是否多彩图标                                   | false                    |
| sideUniqueOpen  | 侧边栏是否只保持一个子菜单展开                     | true                     |
| showFooter      | 是否开启页脚(v1.1.0新增)                           | true                     |
| weakMode        | 是否是色弱模式                                     | false                    |
| darkMode        | 是否是暗黑模式                                     | false                    |
| color           | 默认主题色，如：#009688                            | null                     |

获取缓存token方法：

```javascript
import setting from '@/config/setting';

let access_token = setting.takeToken();
```

<br/>

## 12.3. 菜单路由配置  

如果使用后端返回菜单数据在`src/config/setting.js`中的`menuUrl`参数配置后端接口地址，接口返回的数据格式需要为：

```json
{
    "code": 0,
    "data": [{
        "title": "系统管理",
        "icon": "setting-outlined",
        "path": "/system",
        "hide": false,
        "children": [{
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
        }, {
            "title": "我是内链",
            "icon": "team-outlined",
            "path": "/system/baidu",
            "component": "https://baidu.com"
        }, {
            "title": "我是外链",
            "icon": "team-outlined",
            "path": "https://eleadmin.com"
        }]
    }],
    "user": {
        "nickname": "管理员",
        "avatar": "http://abc.com/a.png",
        "authorities": ["user:add", "role:add"],
        "roles": ["admin", "user"]
    }
}
```

data是菜单数据，user是用户信息(非必须，v1.3.0新增)，如果用户信息、角色、权限、菜单是一个接口需要有这个字段，菜单的格式说明：

- title   菜单的标题，必须
- icon   菜单的图标，非必须
- path   路由地址(要以/开头)，必须
- component   组件地址(组件要放在views目录下)，父级非必须
- hide   为true只注册路由不显示在左侧菜单(比如独立的添加页面)，非必须
- active   比如修改页面不在侧栏，打开后侧栏就没有选中，这个配置选中地址，非必须
- hideFooter   是否隐藏全局页脚，非必须，也可在setting.js中配置(v1.2.0新增)
- hideSidebar   是否隐藏侧边栏，非必须，也可在setting.js中配置(v1.2.0新增)
- closable   是否可以在多页签中关闭，非必须，不写默认为true(v1.2.0新增)
- tabUnique   同路由不同参数页签是否只显示一个，不写默认为true(v1.3.0新增)
- meta   Object类型，非必须，对应路由元数据meta
- children   子级，非必须

 path需要必填，父级也要有path，因为path会在渲染菜单的时候当作循环的key，并且混合导航布局父级也会需要path跳转路由， 父级一般都是重定向到第一个子级，这里的菜单会动态注册路由，path如果以`https://`、`http://`、`//`开头是外链的形式， 外链不注册路由，`component`如果以上面三个开头是内链的形式(v1.1.0新增)，否则就是路由组件的形式， 其实标准的数据格式是除了path、component、children其它字段都要放在meta中，考虑到正常后端数据库菜单表的字段设计都在一级， 所以是在前端处理放在meta下面，也支持接口返回meta字段，会合并在一起。

如果接口返回的数据不是这个格式，可以在src/config/setting.js中使用 `parseMenu` 或 `parseMenuItem` 自定义解析：

```javascript
// import {toTreeData} from 'ele-admin-pro/packages/util';

export default {
    parseMenu(res) {
        // res即接口原始数据，注意菜单的path必填，如果接口是url对应path，那就是url必填
        // 接口需要返回树形的数据，如果接口是pid形式的数据可以在这里转换
        /*return {
            code: 0, 
            data: toTreeData(res.data, 'menuId', 'parentId'),
            // user可以不需要，角色和权限需要是String数组，可以在这里转换
            user: Object.assign({}, res.user, {
                roles: res.user.roles.map(d => d.roleCode),
                authorities: res.user.authorities.map(d => d.authority)
            })
        };*/
        return res;
    },
    parseMenuItem(item) {
        // item即接口原始数据菜单的每一个，如果字段名称不对应可以这样处理
        /*return Object.assign({}, item, {
            title: item.menuTitle
        });*/
        return Object.assign({}, item);
    }
}
```

如果不使用接口返回菜单，直接前端配置，把src/config/setting.js中`menuUrl`参数设为null，并配置`menus`参数：

```javascript
export default {
    // 菜单数据接口
    menuUrl: null,
    // 直接指定菜单数据
    menus: [
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
}
```

在这里配置的菜单会动态注册路由，数据格式与上面介绍的一直，`path` 和 `component` 同样支持外链和内链的规则。

**formatMenus方法说明：**

```javascript
// 在src/store/modules/user.js中有用到
import {formatMenus} from 'ele-admin-pro';

const {menus, homePath, homeTitle} = formatMenus(result.data, setting.parseMenuItem);
```

 此方法是用于把接口格式的菜单数据转成标准格式的数据，会把icon、title、hide等放在meta字段下，增加redirect字段指向第一个子级的path， 返回的homePath是第一个不为目录的菜单的path，homeTitle是第一个不为目录的菜单的title。

**menuToRoutes方法说明：**

```javascript
// 在src/router/index.js中有用到
import {menuToRoutes} from 'ele-admin-pro';

const routes = menuToRoutes(menus, (component) => import('@/views' + component), []);
```

  此方法是用于把菜单数据转成路由数据，包括过滤掉重复的路由path、过滤掉外链、内链的数据处理等， 参数三是配置需要去掉的路由path，比如有些菜单已经配置了静态路由不要再动态注册，例如写 `['/system/user']` 。

<br/>

## 12.4. 用户信息接口  

在src/config/setting.js中`userUrl`参数是用来配置获取登录用户信息的接口地址，数据需要的格式为：

```json
{
    "code": 0,
    "data": {
        "nickname": "管理员",
        "avatar": "http://abc.com/a.png",
        "authorities": ["user:add", "role:add"],
        "roles": ["admin", "user"]
    },
    "msg": ""
}
```

用户信息是在 `src/layout/index.vue` 中去获取的，数据的各字段含义：

- nickname   用户昵称，会显示在顶栏
- avatar   用户头像，会显示在顶栏
- authorities   用户所拥有的权限，在权限控制组件中用到
- roles   用户所拥有的角色，在权限控制组件中用到

如果接口返回的数据不是这个格式，需要使用`parseUser`参数自定义解析：

```javascript
export default {
    parseUser(res) {
        // code为0是成功, 不一样可以处理如: {code: res.code === 200 ? 0 : res.code, msg: res.message}
        let result = {code: res.code, msg: res.msg};
        if (res.data) {
            result.data = Object.assign({}, res.data, {
                // 姓名和头像会显示在顶栏, 字段不一样可以在这处理, 如:
                //avatar: res.data.avatarUrl,
                //nickname: res.data.userName,
                // 角色和权限信息, 需要为string数组类型
                roles: res.data.roles ? res.data.roles.map(d => d.roleCode) : [],
                authorities: res.data.authorities ? res.data.authorities.map(d => d.authority) : []
            });
        }
        return result;
    }
}
```

如果获取用户信息、角色、权限和获取用户的菜单是一个接口，把`userUrl`设置为`null`，菜单的接口返回如下格式：

```json
{
    "code": 0,
    "data": [{
        "title": "系统管理",
        "icon": "setting-outlined",
        "path": "/system"
    }],
    "user": {
        "nickname": "管理员",
        "avatar": "http://abc.com/a.png",
        "authorities": ["user:add", "role:add"],
        "roles": ["admin", "user"]
    }
}
```

data是菜单数据，详细格式说明到上面菜单配置中查看，user是用户信息，需要对接口数据格式化使用`parseMenu`方法。

<br/>

## 12.5. 登录状态管理  

登录界面在`src/views/login/login.vue`中，登录接口需要的数据格式为，如果格式不一致直接在login.vue中修改：

```json
{
    "code": 0,
    "msg": "登录成功",
    "access_token": "eyJhbGciOiJIUzI1NiJ9.eyJzdWIiOiJhZ"
}
```

登录状态管理在`src/store/modules/user.js`中，使用vuex管理当前用户信息、权限数据、角色数据、菜单数据等。

```javascript
this.$store.state.user.user;  // 获取用户的信息
this.$store.state.user.authorities;  // 获取用户的权限列表
this.$store.state.user.roles;  // 获取用户的角色列表
this.$store.state.user.menus;  // 获取用户的菜单数据

// 缓存用户token
this.$store.dispatch('user/setToken', {
    token: 'Bearer eyJhbGciOiJIUzI1NiJ9.eyJzdWIiOiJhZ',
    remember: false  // remember默认为true是localStorage存储，为false是sessionStorage
});

// 移除当前token
this.$store.dispatch('user/removeToken');

// 设置当前登录的用户信息
this.$store.dispatch('user/setUser', res.data.data);

// 设置当前登录的用户权限列表
this.$store.dispatch('user/setAuthorities', ['user:add', 'user:update']);

// 设置当前登录的用户角色列表
this.$store.dispatch('user/setRoles', ['admin', 'user']);
```

例如在模板中获取用户信息：

```vue
<template>
    <div>早安，{{ $store.state.user.user.nickname }}，开始您一天的工作吧！</div>
</template>
```

也可以使用`mapGetters`：

```vue
<template>
    <template>
        <div>早安，{{ user.user.nickname }}，开始您一天的工作吧！</div>
    </template>
</template>

<script>
    import { mapGetters } from 'vuex';

    export default {
        computed: {
            ...mapGetters(['user'])
        }
    }
</script>
```

也可以使用`computed`：

```vue
<template>
    <template>
        <div>早安，{{ loginUser.nickname }}，开始您一天的工作吧！</div>
    </template>
</template>

<script>
    export default {
        computed: {
            loginUser() {
                return this.$store.state.user.user;
            }
        }
    }
</script>
```

<br/>

## 12.6. 静态路由配置  

路由的主要配置位于`src/router/index.js`中:

```javascript
import {createRouter, createWebHistory} from 'vue-router';
// ...省略其它代码

// 静态路由
const routes = [
    {
        path: '/login',
        component: () => import('@/views/login/login'),
        meta: {title: '登录'}
    },
    {
        path: '/forget',
        component: () => import('@/views/login/forget'),
        meta: {title: '忘记密码'}
    },
    // 如果需要静态路由显示顶栏和侧边栏需要这样配置
    {
        path: '/payment',
        component: EleLayout,  // 这里要使用EleLayout
        meta: {title: '支付'},
        children: [
            {
                path: '/payment/success',
                component: () => import('@/views/payment/success'),
                meta: {title: '支付成功'}
            },
            {
                path: '/payment/failure',
                component: () => import('@/views/payment/failure'),
                meta: {title: '支付失败'}
            }
        ]
    },
    // 没有匹配的路由显示404页面
    {
        path: '/:pathMatch(.*)*',
        component: () => import('@/views/exception/404')
    }
];

const router = createRouter({
    history: createWebHistory(),
    routes
});

// 路由守卫
router.beforeEach((to, from, next) => {
    //......省略
});

export default router;
```

 可以在这里配置一些静态路由、一些不需要顶栏、侧栏的路由，需要注意的是path不要为`/`，因为接口返回的动态路由会注册到`/`下面， meta下面的`title`是必填的，会用于切换路由后更新浏览器页签上的标题文字，以及显示在面包屑导航、多页签栏中。 静态路由不会显示在侧边栏菜单中，要注意静态路由的path不要跟动态注册的路由存在重复的。

<br/>

## 12.7. 网络请求axios  

axios是一个非常强大、方便的http请求库，axios的配置文件位于`src/config/axios-config.js`中：

```javascript
import axios from 'axios';
// ...代码省略

// 设置统一的url
axios.defaults.baseURL = process.env.VUE_APP_API_BASE_URL;

/* 请求拦截器 */
axios.interceptors.request.use('...代码省略');

/* 响应拦截器 */
axios.interceptors.response.use('...代码省略');

// ...代码省略
```

axios.defaults.baseURL：

  用于配置接口统一的前缀地址，`process.env.VUE_APP_API_BASE_URL` 这个是取的 `.env` 里面的环境变量， 在项目的根目录下有好几个以 `.env` 开头的文件，分别对应不同的运行环境。

token自动续期：

  这里设计的是后端在token将要过期的时候返回新的token在response的header中，然后前端获取后覆盖当前缓存的token， 下次再请求就会用新的token了，这样可以让用户无感知的续期token，防止用户正在操作的时候token过期需要重新登录， 也有些后端可能需要单独请求续期token的接口来续期token，这种前端如果要实现无感知的续期token比较麻烦，可以自行根据后端要求修改。