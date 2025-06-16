---
title: 常见问题
createTime: 2025/01/10 09:30:40
permalink: /EleAdminPro/shbl1yw6/
---
## 10.1. 修改项目端口  

在 `vite.config.ts` 中增加配置，然后重新运行就可以用配置的端口访问：

```javascript
// ......省略其它代码
export default defineConfig(({ command }) => {
    return {
        server: {
            port: 8080  // 这里写要改的端口
        },
        // ......省略其它代码
        resolve: {}
    };
});
```

v1.5.0 以及之前的版本是在 **vue.config.js** 中配置端口：

```javascript
module.exports = {
    devServer: {
        port: 8888
    },
    //......省略其它代码
}
```

<br/>

## 10.2. 解决跨域问题  

如果后端接口没有设置 cors (跨域资源共享)，可在前端解决跨域问题，在 `vite.config.ts` 中配置代理：

```javascript
// ......省略其它代码
export default defineConfig(({ command }) => {
    return {
        server: {
            proxy: {
                '/api': 'https://v2.eleadmin.com'
                // 这里可以写你自己的后端接口地址，如：
                //'/api': 'http://localhost:8081'
            }
        },
        // ......省略其它代码
        resolve: {}
    };
});
// 注意，这种方式需要接口都有一个统一的前缀，这里 /api 就是所有接口都会有的前缀复制代码
```

然后修改 `.env` 、 `.env.development` 文件中的下面这行代码，修改后需要重新运行项目：

```bash
VITE_API_URL=/
```

  在 api 中请求 `/api/system/user` ，`/api` 是设置为 axios 的 baseURL，api 里面使用 axios 请求路径只用写 `/system/user`， 因为没有带其它的网址前缀，所以浏览器发起的请求是在同域名下，就不存在跨域，然后 vite 会代理到实际接口 `https://v2.eleadmin.com/api/system/user` ， 需要注意这只是解决开发时候的跨域问题，生产环境需要用 nginx 配置代理，原理同 vite，在 `nginx/conf/nginx.conf` 中配置：

```bash
http {
    server {
        # 这里配置端口
        listen       8080;
        server_name  localhost;

        location / {
            # 加这个是支持history的路由模式
            try_files $uri $uri/ /index.html;
            # 这里配置项目位置
            root   C:/webapp/eleadminpro;
            index  index.html index.htm;
        }

        # 加入以下配置，之前的配置全部不要动，这个 location 是新加入的，注意前后都有 /
        location /api/ {
            proxy_pass  https://v2.eleadmin.com/api/; # 这个是后台接口所在的地址
            #proxy_pass  http://localhost:8081/api/; # 这个是后台接口所在的地址
        }
    }
}
```

注意，v1.5.0 以及之前的版本使用的不是 vite，前两步要这样配置：

在 `vue.config.js` 中配置代理：

```javascript
module.exports = {
    devServer: {
        proxy: {
            '/api': {
                target: 'https://v1.eleadmin.com'
                //target: 'http://localhost:8081'
            }
        }
    },
    //......省略其它代码
}
```

然后修改 `.env` 、 `.env.development` 文件：

```bash
VUE_APP_API_BASE_URL=/api
```

<br/>

## 10.3. 数据无法更新  

例如在使用自定义组件传数组类型的参数时，往数组 push 数据后界面没有自动更新，通常是因为组件没有深度监听参数变化:

::: code-tabs

@tab TypeScript

```vue
<template>
    <my-component :data="data" />
    <a-button @click="add">添加数据</a-button>
</template>

<script lang="ts" setup>
    import { ref } from 'vue';

    const data = ref<string[]>(['a', 'b', 'c', 'd']);

    const add = () => {
        data.value.push('e');
    };
</script>
```

@tab JavaScript

```vue
<template>
    <my-component :data="data" />
    <a-button @click="add">添加数据</a-button>
</template>

<script setup>
    import { ref } from 'vue';

    const data = ref(['a', 'b', 'c', 'd']);

    const add = () => {
        data.value.push('e');
    };
</script>
```

:::

这种情况应该给整个数据重新赋值，比如这样修改：

```javascript
const add = () => {
    //data.value.push('e');
    data.value = [...data.value, 'e'];
};
```

也可以换成下面这种方式：

```javascript
// 添加可以这样操作
//data.value.push('e');
data.value = data.value.concat(['e']);

// 删除可以这样修改
const index = 2;
//data.value.splice(index, 1);
data.value = data.value.slice(0, index).concat(data.value.slice(index + 1));

// 或者
data.value = data.value.filter((_d, i) => i !== index);
```

同理 object 类型的参数也会存在同样的问题，解决方式也是类似的：

::: code-tabs

@tab TypeScript

```vue
<template>
    <my-component :data="data" />
    <a-button @click="update">修改数据</a-button>
</template>

<script lang="ts" setup>
    import { ref } from 'vue';

    const data = ref({
        username: 'admin',
        nickname: '管理员'
    });

    const update = () => {
        // 只给 object 类型参数的某个字段赋值界面可能不会更新，应该整个重新赋值
        //data.value.nickname = '超级管理员';
        data.value = {
            ...data.value,
            nickname: '超级管理员'
        };
    };
</script>
```

@tab JavaScript

```vue
<template>
    <my-component :data="data" />
    <a-button @click="update">修改数据</a-button>
</template>

<script setup>
    import { ref } from 'vue';

    const data = ref({
        username: 'admin',
        nickname: '管理员'
    });

    const update = () => {
        // 只给 object 类型参数的某个字段赋值界面可能不会更新，应该整个重新赋值
        //data.value.nickname = '超级管理员';
        data.value = {
            ...data.value,
            nickname: '超级管理员'
        };
    };
</script>
```

:::

<br/>

## 10.4. 移除 eslint 检查  

把 eslint 相关的依赖全部卸载即可，通过如下命令卸载，或者把 package.json 中 eslint 相关依赖全部删除再重新 install ：

```bash
pnpm uninstall @typescript-eslint/eslint-plugin @typescript-eslint/parser

pnpm uninstall eslint eslint-config-prettier eslint-define-config

pnpm uninstall eslint-plugin-prettier eslint-plugin-vue vue-eslint-parser
```

然后删除根目录下的 `.eslintrc.js` 和 `.eslintignore` 文件。

<br/>

## 10.5. 登录跳转 404  

登录成功后跳转到 404 界面基本都是后端接口没有返回正确的菜单路由数据，可在 `src/router/routes.ts` 中把数据打印出来看看：

::: code-tabs

@tab TypeScript

```typescript
// 版本需要至少为 v1.8.1 才有此文件
export function getMenuRoutes(menus?: MenuItem[], homePath?: string) {
    //......省略其它代码
    console.log('menus', menus);
    console.log('routes', routes);
    console.log('homePath', homePath);
    return {
        path: LAYOUT_PATH,
        component: EleLayout,
        redirect: HOME_PATH ?? homePath,
        children: routes
    };
}
```

@tab JavaScript

```javascript
// 版本需要至少为 v1.8.1 才有此文件
export function getMenuRoutes(menus, homePath) {
    //......省略其它代码
    console.log('menus', menus);
    console.log('routes', routes);
    console.log('homePath', homePath);
    return {
        path: LAYOUT_PATH,
        component: EleLayout,
        redirect: HOME_PATH ?? homePath,
        children: routes
    };
}
```

:::

再看浏览器控制台调用请求用户信息、菜单数据的接口有没有请求成功及返回正确的数据，数据格式按照文档 2.4 介绍的返回。

<br/>

## 10.6. 页面打开空白  

  如果是所有页面空白看控制台有没有警告信息是不是授权码没配置正确导致的，如果是部分页面空白或一个页面切换到另一个页面空白刷新又是好的是路由切换动画 Transition 导致的，路由切换动画需要路由页面对应的组件的根节点只有一个，注释也不行：

```vue
<!-- template 下面要只有一个根节点，template 外面加注释是可以的 -->
<template>
    <div>
        <!-- 这里加注释也是可以的 -->
        <div>Hello</div>
        <div>Xxx</div>
    </div>
</template>
```

错误示范：

```vue
<template>
    <!-- 这里不能有注释 -->
    <div>
        <div>Hello</div>
    </div>
    <div>Xxx</div>
    <user-edit />
</template>
```

<br/>

## 10.7. 组件样式丢失 

例如页面的表格没有样式或刷新后没有样式一般是使用按需引入后又自己在页面上引入了组件：

::: code-tabs

@tab TypeScript

```vue
<template>
    <ele-pro-table ref="tableRef"></ele-pro-table>
</template>

<script lang="ts" setup>
    // 例如要引入 EleProTable 组件的类型，这样写就是错误的写法
    //import { EleProTable } from 'ele-admin-pro';
    // 应该使用 import type 而不是 import
    import type { EleProTable } from 'ele-admin-pro';

    // 表格 ref
    const tableRef = ref<InstanceType<typeof EleProTable> | null>(null);
</script>
```

@tab JavaScript

```vue
<template>
    <ele-pro-table ref="tableRef"></ele-pro-table>
</template>

<script setup>
    // 请不要自己引入组件，按需引入插件会自动帮你引入
    //import { EleProTable } from 'ele-admin-pro';

    // 表格 ref
    const tableRef = ref(null);
</script>
```

:::

因为自己在页面上引入了组件，按需加载的插件就不会再自动引入组件以及组件的样式了。

## 10.8. less 变量报错  
v1.7.0 的版本自行升级到 `ant-design-vue3.1.x` 最新版 less 变量报错解决，在 `vite.config.ts` 中的 `DynamicAntdLess` 插件中增加参数：

```javascript
export default defineConfig(({ command }) => {
    return {
        // ......省略其它代码
        css: {
            preprocessorOptions: {
                less: {
                    plugins: [
                        new DynamicAntdLess({
                            variables: {
                                '@info-color-deprecated-bg': 'var(--primary-1)',
                                '@info-color-deprecated-border': 'var(--primary-3)',
                                '@success-color-hover': 'var(--green-5)',
                                '@success-color-active': 'var(--green-7)',
                                '@success-color-outline': 'var(--green-2)',
                                '@success-color-deprecated-bg': 'var(--green-1)',
                                '@success-color-deprecated-border': 'var(--green-3)'
                            },
                            replaces: {
                                'box-shadow: 0 0 0 2px fade(@disabled-color, 10%)':
                                    'box-shadow: 0 0 0 2px var(--primary-fade-20)'
                            }
                        })
                    ]
                }
            }
        }
    };
});
```

v1.6.0 的版本自行升级到 `ant-design-vue3.0.x` 最新版 less 变量报错解决，在 `vite.config.ts` 中的 `DynamicAntdLess` 插件中增加参数：

```javascript
export default defineConfig(({ command }) => {
    return {
        // ......省略其它代码
        css: {
            preprocessorOptions: {
                less: {
                    plugins: [
                        new DynamicAntdLess({
                            variables: {
                                '@primary-color-hover': 'var(--primary-5)',
                                '@primary-color-active': 'var(--primary-7)',
                                '@primary-color-outline': 'var(--primary-2)',
                                '@warning-color-hover': 'var(--gold-5)',
                                '@warning-color-active': 'var(--warning-color-active)',
                                '@warning-color-outline': 'var(--warning-color-outline)',
                                '@warning-color-deprecated-bg': 'var(--gold-1)',
                                '@warning-color-deprecated-border': 'var(--gold-3)',
                                '@error-color-hover': 'var(--red-4)',
                                '@error-color-active': 'var(--red-7)',
                                '@error-color-outline': 'var(--red-2)',
                                '@error-color-deprecated-bg': 'var(--red-1)',
                                '@error-color-deprecated-border': 'var(--gold-5)'
                            },
                            replaces: {
                                'fade(@borderColor, @outline-fade)': 'ele-fade(@borderColor, @outline-fade)'
                            }
                        })
                    ]
                }
            }
        }
    };
});
```

v1.5.0 的版本自行升级到 `ant-design-vue2.x` 最新版 less 变量报错解决，在 `vue.config.js` 中的 `DynamicAntdLess` 插件中增加参数：

```javascript
module.exports = {
    // ......省略其它代码
    css: {
        loaderOptions: {
            less: {
                lessOptions: {
                    javascriptEnabled: true,
                    plugins: [
                        new DynamicAntdLess({
                            replaces: {
                                'darken(@shadow-color, 5%)': '@shadow-color'
                            }
                        })
                    ]
                }
            }
        }
    }
}
```

  换主题功能是通过插件把 less 变量转成 css 变量实现的，升级版本如果新增了插件没有考虑到的变量转换不彻底就会报错， 可以通过参数自行添加需要转换和替换的 less 内容，需要对 ant-design-vue 的样式源码非常熟悉才能操作， 不会操作又需要升级到最新版本可以参考文档 7.3 去掉切换主题的功能也不会有 less 变量报错的问题。