---
title: 其他功能
createTime: 2025/01/10 09:30:40
permalink: /EleAdminPro/oqfl8jc0/
---
## 8.1. 主题状态管理

  主题状态管理位于 src/store/modules/theme.ts，如果需要修改默认值直接修改源码中的 `DEFAULT_STATE` 即可， 需要注意在设置抽屉中切换后会缓存在本地，修改默认配置后应该手动清缓存或者点击设置抽屉中的重置按钮才会生效。

例如修改默认主题色：

::: code-tabs

@tab TypeScript

```typescript
//......省略其它代码
const DEFAULT_STATE: ThemeState = Object.freeze({
    // 主题色
    //color: null,
    color: '#9266f9',
    // 是否开启多页签
    showTabs: false,
});
```

@tab JavaScript

```javascript
//......省略其它代码
const DEFAULT_STATE = Object.freeze({
    // 主题色
    //color: null,
    color: '#9266f9',
    // 是否开启多页签
    showTabs: false,
});
```

:::

theme.ts 里面的 actions 调用示例：

::: code-tabs

@tab TypeScript

```vue
<template>
    <div>
        <a-button @click="closeFooter">关闭全局页脚</a-button>
        <a-button @click="openDarkModel">开启暗黑模式</a-button>
        <a-button @click="changeThemeColor">切换主题色</a-button>
    </div>
</template>

<script lang="ts" setup>
    import { message } from 'ant-design-vue';
    import { useThemeStore } from '@/store/modules/theme';

    const themeStore = useThemeStore();

    /* 设置不显示全局页脚 */
    const closeFooter = () => {
        themeStore.setShowFooter(false);
    }

    /* 设置暗黑模式 */
    const openDarkModel = () => {
        themeStore.setDarkMode(true);
    }

    /* 切换主题色 */
    const changeThemeColor = () => {
        const hide = message.loading('正在加载主题..', 0);
        themeStore.setColor('#9266f9').then(() => {
            hide();
        }).catch((e) => {
            hide();
            console.error(e);
            message.error('主题加载失败');
        });
    }
</script>
```

@tab JavaScript

```vue
<template>
    <div>
        <a-button @click="closeFooter">关闭全局页脚</a-button>
        <a-button @click="openDarkModel">开启暗黑模式</a-button>
        <a-button @click="changeThemeColor">切换主题色</a-button>
    </div>
</template>

<script setup>
    import { message } from 'ant-design-vue';
    import { useThemeStore } from '@/store/modules/theme';

    const themeStore = useThemeStore();

    /* 设置不显示全局页脚 */
    const closeFooter = () => {
        themeStore.setShowFooter(false);
    }

    /* 设置暗黑模式 */
    const openDarkModel = () => {
        themeStore.setDarkMode(true);
    }

    /* 切换主题色 */
    const changeThemeColor = () => {
        const hide = message.loading('正在加载主题..', 0);
        themeStore.setColor('#9266f9').then(() => {
            hide();
        }).catch((e) => {
            hide();
            console.error(e);
            message.error('主题加载失败');
        });
    }
</script>
```

:::

如果要实现刷新页面保留上次打开的页签，修改 `setTabs` 方法 增加 cacheSetting 即可：

::: code-tabs

@tab TypeScript

```typescript
// ......省略其它代码
export const useThemeStore = defineStore({
    // ......省略其它代码
    actions: {
        setTabs(value: TabItem[]) {
            this.tabs = value;
            // 增加下面这行代码
            cacheSetting('tabs', value);
        }
    }
});
```

@tab JavaScript

```typescript
// ......省略其它代码
export const useThemeStore = defineStore({
    // ......省略其它代码
    actions: {
        setTabs(value) {
            this.tabs = value;
            // 增加下面这行代码
            cacheSetting('tabs', value);
        }
    }
});
```

:::

<br/>

## 8.2. 页签数据管理

多页签的数据在 `src/store/modules/theme.ts` 的 `tabs` 中管理，数据格式为：

```javascript
[
    {
        key: '/system/user',
        title: '用户管理', 
        path: '/system/user', 
        fullPath: '/system/user', 
        closable: true,
        components: ['SystemUser', 'RouterLayout', 'EleLayout']
    },
    {
        key: '/system/user-info?id=1',
        title: '用户一的信息', 
        path: '/system/user-info', 
        fullPath: '/system/user-info?id=1', 
        closable: true,
        components: []
    },
    {
        key: '/user/message',
        title: '我的消息', 
        path: '/user/message', 
        fullPath: '/user/message?type=notice', 
        closable: true,
        components: []
    }
]
```

- key   string   页签的唯一 key，v1.3.0 新增，必须
- title   string   页签的标题，必须
- path   string   页签对应的路由地址(不含参数)，必须
- fullPath   string   页签对应的完整路由地址(含参数)，v1.3.0新增，必须
- closable   boolean   页签是否可关闭，v1.2.0新增，必须
- components   string[]   页签对应的组件名称，用于 keep-alive，v1.5.0新增，必须

  切换路由会自动添加页签，如果页签同路由不同参数可重复打开，key 就会取 fullPath 的值，不能重复打开 key 就会取 path 的值， 如果需要默认打开几个不可关闭的多页签，直接在 `tabs` 中写完整数据即可，components 可以先写空数组，切换路由时会自动更新的。

<br/>

## 8.3. 使用全局配置

组件名 `ele-config-provider` ，v1.7.0 新增，使用 Vue 的 provide / inject 特性，在 `src/App.vue` 中已使用：

::: code-tabs

@tab TypeScript

```vue
<template>
    <ele-config-provider :license="LICENSE_CODE">
        <router-view />
    </ele-config-provider>
</template>

<script lang="ts" setup>
    import { LICENSE_CODE } from '@/config/setting';
</script>
```

@tab JavaScript

```vue
<template>
    <ele-config-provider :license="LICENSE_CODE">
        <router-view />
    </ele-config-provider>
</template>

<script setup>
    import { LICENSE_CODE } from '@/config/setting';
</script>
```

:::

属性列表：

| 属性名    | 类型                      | 说明                                            | 默认值 |
| :-------- | :------------------------ | :---------------------------------------------- | :----- |
| license   | String                    | 授权码                                          |        |
| locale    | Object: EleLocale         | 国际化配置                                      |        |
| mapKey    | String                    | 高德地图 key                                    |        |
| request   | Object: RequestOption     | 表格全局 request                                |        |
| response  | Object: ResponseOption    | 表格全局 response                               |        |
| table     | Object: TableGlobalConfig | 表格全局配置 `v1.8.0新增`                       |        |
| keepAlive | Boolean                   | 是否开启 keep-alive ，用于内链缓存 `v1.8.0新增` |        |

类型定义：

```typescript
interface RequestOption {
    // 排序字段参数名称
    sortName?: string;
    // 排序方式的参数名称
    orderName?: string;
}

interface ResponseOption {
    // 数据列表的字段名称
    dataName?: string;
    // 数据总数的字段名称
    countName?: string;
}

interface TableGlobalConfig {
    // 升序值
    ascendValue?: string;
    // 降序值
    descendValue?: string;
    // 表格尺寸
    size?: TableSizeType;
}
```

<br/>

## 8.4. 开启 KeepAlive

把 src/config/setting.ts 中的 `TAB_KEEP_ALIVE` 值改为 true 即可开启 KeepAlive，需要注意 vue 中要写 name 才可生效：

::: code-tabs

@tab TypeScript

```vue
<template>
</template>

<script lang="ts" setup>
</script>

<script lang="ts">
    export default {
        name: 'SystemUser'
    };
</script>
```

@tab JavaScript

```vue
<template>
</template>

<script setup>
</script>

<script>
    export default {
        name: 'SystemUser'
    };
</script>
```

:::

  实现 KeepAlive 功能主要在 src/components 下的 RouterLayout 和 RedirectLayout 组件， RouterLayout 是用于结合 router-view 和 keep-alive 组件， 当打开新 Tab 时会获取当前路由对应的组件名称更新到 tabs 数据中， RedirectLayout 用于实现刷新路由的功能，刷新时会把当前路由信息放在 theme 状态管理的 routeReload 中， theme 状态管理的 keepAliveInclude 是通过 getter 实现的，根据 tabs、homeComponents、routeReload 三个数据计算当前要缓存的组件名称。

  v1.8.0 版本开始在 src/router/routes.ts 中的 `getMenuRoutes` 方法中把菜单数据转成路由数据时进行了铺平处理，铺平后就不存在嵌套路由了，路由铺平可以解决多级路由下 keep-alive 和 transition 组件的很多奇怪问题，其它主流的模板框架也都是采用铺平处理。

例如同一个路由传递不同的参数刷新数据，如查看用户信息的路由 `/system/user-info?id=1` ：

::: code-tabs

@tab TypeScript

```vue
<template>
    <div>
        <div class="ele-text-secondary">账号: {{ user.username }}</div>
        <div class="ele-text-secondary">用户名: {{ user.nickname }}</div>
    </div>
</template>

<script lang="ts" setup>
    import { reactive, watch, unref } from 'vue';
    import { useRouter } from 'vue-router';
    import { setPageTab } from '@/utils/page-tab-util';
    import { getUser } from '@/api/system/user';
    import type { User } from '@/api/system/user/model';

    const { currentRoute } = useRouter();

    const user = reactive<User>({
        username: '',
        nickname: ''
    });

    // 监听路由变化
    watch(currentRoute, (route) => {
        const { query, path, fullPath } = unref(route);
        const id = query.id;
        if (path !== '/system/user-info' || !id) {  // 再做一下路由地址判断
            return;
        }
        getUser(Number(id)).then((data) => {
            Object.assign(user, data);
            // 修改页签标题
            setPageTab({
                fullPath: fullPath,
                title: data.nickname + '的信息'
            });
        }).catch((e) => {
            console.error(e.message);
        });
    }, {
        immediate: true
    });
</script>
```

@tab JavaScript

```vue
<template>
    <div>
        <div class="ele-text-secondary">账号: {{ user.username }}</div>
        <div class="ele-text-secondary">用户名: {{ user.nickname }}</div>
    </div>
</template>

<script setup>
    import { reactive, watch, unref } from 'vue';
    import { useRouter } from 'vue-router';
    import { setPageTab } from '@/utils/page-tab-util';
    import { getUser } from '@/api/system/user';

    const { currentRoute } = useRouter();

    const user = reactive({
        username: '',
        nickname: ''
    });

    // 监听路由变化
    watch(currentRoute, (route) => {
        const { query, path, fullPath } = unref(route);
        const id = query.id;
        if (path !== '/system/user-info' || !id) {  // 再做一下路由地址判断
            return;
        }
        getUser(Number(id)).then((data) => {
            Object.assign(user, data);
            // 修改页签标题
            setPageTab({
                fullPath: fullPath,
                title: data.nickname + '的信息'
            });
        }).catch((e) => {
            console.error(e.message);
        });
    }, {
        immediate: true
    });
</script>
```

:::

也可以跳转路由的时候先关闭页签再打开页签，这种方式最好设置此路由为同路由不同参数不可重复打开的类型：

::: code-tabs

@tab TypeScript

```vue
<template>
    <a-button @click="goUserInfo">跳转</a-button>
</template>

<script lang="ts" setup>
    import { nextTick } from 'vue';
    import { useRouter } from 'vue-router';
    import { removePageTab } from '@/utils/page-tab-util';

    const { push } = useRouter();

    const goUserInfo = () => {
        removePageTab({ key: '/system/user-info' });
        nextTick(() => {
            push({
                path: '/system/user-info',
                query: { id: 1 }
            });
        });
    };
</script>
```

@tab JavaScript

```vue
<template>
    <a-button @click="goUserInfo">跳转</a-button>
</template>

<script setup>
    import { nextTick } from 'vue';
    import { useRouter } from 'vue-router';
    import { removePageTab } from '@/utils/page-tab-util';

    const { push } = useRouter();

    const goUserInfo = () => {
        removePageTab({ key: '/system/user-info' });
        nextTick(() => {
            push({
                path: '/system/user-info',
                query: { id: 1 }
            });
        });
    };
</script>
```

:::

<br/>

## 8.5. 国际化多语言

v1.8.0 版本开始 ele-admin-pro 的国际化自己实现，不共用 ant-design-vue 的国际化，在 `ele-config-provider` 中通过 `locale` 配置：

::: code-tabs

@tab TypeScript

```vue
<template>
    <ele-config-provider :locale="eleLocale">
        <router-view />
    </ele-config-provider>
</template>

<script lang="ts" setup>
    import { useI18n } from 'vue-i18n';
    import type { EleLocale } from 'ele-admin-pro';
    import eleZh_CN from 'ele-admin-pro/es/lang/zh_CN';
    import eleEn from 'ele-admin-pro/es/lang/en_US';
    const eleLocales = { zh_CN: eleZh_CN, en: eleEn };

    const { locale } = useI18n();
    const eleLocale = ref<EleLocale | null>(null);

    watch(locale, () => {
        eleLocale.value = eleLocales[locale.value];
    }, {
        immediate: true
    });
</script>
```

@tab JavaScript

```vue
<template>
    <ele-config-provider :locale="eleLocale">
        <router-view />
    </ele-config-provider>
</template>

<script setup>
    import { useI18n } from 'vue-i18n';
    import eleZh_CN from 'ele-admin-pro/es/lang/zh_CN';
    import eleEn from 'ele-admin-pro/es/lang/en_US';
    const eleLocales = { zh_CN: eleZh_CN, en: eleEn };

    const { locale } = useI18n();
    const eleLocale = ref(null);

    watch(locale, () => {
        eleLocale.value = eleLocales[locale.value];
    }, {
        immediate: true
    });
</script>
```

:::

以上是 ele-admin-pro 集成 i18n 的示例，在下载的模板中已经集成好 ele-admin-pro 、ant-design-vue 、 dayjs 三者与 i18n 的结合了。

如果不需要在线切换语言，只需要固定某一种语言，修改 `src/App.vue`：

::: code-tabs

@tab TypeScript

```vue
<template>
    <!-- 删除 locale 属性，ele-admin-pro 默认就是中文 -->
    <ele-config-provider :map-key="MAP_KEY" :license="LICENSE_CODE" :keep-alive="keepAlive">
        <a-config-provider :locale="antLocale">
            <router-view />
        </a-config-provider>
    </ele-config-provider>
</template>

<script lang="ts" setup>
    import { computed } from 'vue';
    import { storeToRefs } from 'pinia';
    import { useThemeStore } from '@/store/modules/theme';
    import { MAP_KEY, LICENSE_CODE, TAB_KEEP_ALIVE } from '@/config/setting';
    //import { useLocale } from '@/i18n/use-locale';
    // ant-design-vue 和 dayjs 默认是英文，要引入中文
    import antLocale from 'ant-design-vue/es/locale/zh_CN';
    import dayjs from 'dayjs';
    import 'dayjs/locale/zh-cn';

    const themeStore = useThemeStore();
    //const { antLocale, eleLocale } = useLocale();
    dayjs.locale('zh-cn');

    // 用于内链 iframe 组件获取 KeepAlive
    const { showTabs } = storeToRefs(themeStore);
    const keepAlive = computed(() => TAB_KEEP_ALIVE && showTabs.value);

    // 恢复主题
    themeStore.recoverTheme();
</script>
```

@tab JavaScript

```vue
<template>
    <!-- 删除 locale 属性，ele-admin-pro 默认就是中文 -->
    <ele-config-provider :map-key="MAP_KEY" :license="LICENSE_CODE" :keep-alive="keepAlive">
        <a-config-provider :locale="antLocale">
            <router-view />
        </a-config-provider>
    </ele-config-provider>
</template>

<script setup>
    import { computed } from 'vue';
    import { storeToRefs } from 'pinia';
    import { useThemeStore } from '@/store/modules/theme';
    import { MAP_KEY, LICENSE_CODE, TAB_KEEP_ALIVE } from '@/config/setting';
    //import { useLocale } from '@/i18n/use-locale';
    // ant-design-vue 和 dayjs 默认是英文，要引入中文
    import antLocale from 'ant-design-vue/es/locale/zh_CN';
    import dayjs from 'dayjs';
    import 'dayjs/locale/zh-cn';

    const themeStore = useThemeStore();
    //const { antLocale, eleLocale } = useLocale();
    dayjs.locale('zh-cn');

    // 用于内链 iframe 组件获取 KeepAlive
    const { showTabs } = storeToRefs(themeStore);
    const keepAlive = computed(() => TAB_KEEP_ALIVE && showTabs.value);

    // 恢复主题
    themeStore.recoverTheme();
</script>
```

:::

然后还需要去掉模板中所有用到国际化相关的地方：

- src/layout/index.vue
  - `:home-title="homeTitle"` 改成 `:home-title="HOME_TITLE"` 并去掉 `homeTitle` 计算属性
  - 去掉 `:i18n="i18n"`、`:locale="locale"` 及 `i18n` 方法，去掉 `vue-i18n` 引入及 `useI18n`
- src/layout/components/page-footer.vue 去掉 t 相关的直接写文字
- src/layout/components/header-tools.vue 去掉 t 相关的直接写文字，去掉 `i18n-icon` 组件
- src/layout/components/setting-drawer.vue 去掉 t 相关的直接写文字
- src/views/login/index.vue 去掉 t 相关的直接写文字，去掉 `i18n-icon` 组件
- src/main.ts 中去掉 i18n 的引入
- 删除 src/i18n 目录，删除 src/layout/components/i18n-icon.vue
- src/views/list/basic/index.vue 去掉 t 相关的直接写文字

<br/>

## 8.6. 封装公用 iframe

在菜单路由配置中介绍的内链形式只能打开配置的地址，如果需要用 js 打开任意的内链，可以封装一个公用的路由，`src/router/routes.ts` 中增加：

```javascript
// v1.7.0 以及之前的版本静态路由在 src/router/index.ts 中配置
// 静态路由
const routes = [
    {
        path: '/login',
        component: () => import('@/views/login/index.vue'),
        meta: { title: '登录' }
    },
    //......省略其它代码
    {
        path: '/common',
        component: EleLayout,
        children: [
            {
                path: '/common/iframe',
                component: () => import('@/views/iframe/index.vue'),
                meta: { title: 'iframe', tabUnique: false }
            }
        ]
    },
    {
        path: '/:path(.*)*',
        component: () => import('@/views/exception/404/index.vue')
    }
];
```

然后在 views 下创建 `/iframe/index.vue` 文件：

::: code-tabs

@tab TypeScript

```vue
<template>
    <iframe :src="url" class="ele-admin-iframe"></iframe>
</template>

<script lang="ts" setup>
    import { ref, watch, unref } from 'vue';
    import { useRouter } from 'vue-router';
    import { setPageTab } from '@/utils/page-tab-util';
    
    const { currentRoute } = useRouter();
    const url = ref('');
    
    watch(
        currentRoute,
        (route) => {
            const { fullPath, query } = unref(route);
            url.value = query.url;
            setPageTab({
                fullPath: fullPath,
                title: query.title || '无标题'
            });
        },
        { immediate: true }
    );
</script>
```

@tab JavaScript

```vue
<template>
    <iframe :src="url" class="ele-admin-iframe"></iframe>
</template>

<script setup>
    import { ref, watch, unref } from 'vue';
    import { useRouter } from 'vue-router';
    import { setPageTab } from '@/utils/page-tab-util';
    
    const { currentRoute } = useRouter();
    const url = ref('');
    
    watch(
        currentRoute,
        (route) => {
            const { fullPath, query } = unref(route);
            url.value = query.url;
            setPageTab({
                fullPath: fullPath,
                title: query.title || '无标题'
            });
        },
        { immediate: true }
    );
</script>
```

:::

然后就可以通过路由打开任意的内链了：

::: code-tabs

@tab TypeScript

```vue
<template>
    <a-button @click="openEleAdmin">打开EleAdmin</a-button>
</template>

<script lang="ts" setup>
    import { useRouter } from 'vue-router';

    const { push } = useRouter();

    push({
        path: '/common/iframe',
        query: {
            url: 'https://eleadmin.com',
            title: 'EleAdmin官网'
        }
    });
</script>
```

@tab JavaScript

```vue
<template>
    <a-button @click="openEleAdmin">打开EleAdmin</a-button>
</template>

<script setup>
    import { useRouter } from 'vue-router';

    const { push } = useRouter();

    push({
        path: '/common/iframe',
        query: {
            url: 'https://eleadmin.com',
            title: 'EleAdmin官网'
        }
    });
</script>
```

:::

v1.5.0 的版本没有用到 ts 及 setup 语法的可以按照下面代码写：

```vue
<template>
    <iframe :src="url" class="ele-admin-iframe"></iframe>
</template>

<script>
    import { setPageTab } from '@/utils/page-tab-util';

    export default {
        data() {
            return {
                url: ''
            };
        },
        watch: {
            $route: {
                handler(route) {
                    const { fullPath, query } = route;
                    this.url = query.url;
                    setPageTab({
                        fullPath,
                        title: query.title || '无标题'
                    });
                },
                immediate: true
            }
        }
    };
</script>
```

其它页面调用示例：

```vue
<template>
    <a-button @click="openEleAdmin">打开EleAdmin</a-button>
</template>

<script>
    export default {
        methods: {
            openEleAdmin() {
                this.$router.push({
                    path: '/common/iframe',
                    query: {
                        url: 'https://eleadmin.com',
                        title: 'EleAdmin官网'
                    }
                });
            }
        }
    }
</script>
```

<br/>

## 8.7. 按需引入说明

为了减少打包体积，并没有在 main.ts 中全局引入 Antd 和 EleAdminPro，所以使用这两个库的组件按道理要手动引入：

::: code-tabs

@tab TypeScript

```vue
<template>
    <div>
        <a-button type="primary">按钮</a-button>
        <ele-pro-table :datasource="datasource"></ele-pro-table>
    </div>
</template>

<script lang="ts" setup>
    import { Button as AButton } from 'ant-design-vue';
    import { EleProTable } from 'ele-admin-pro';
</script>
```

@tab JavaScript

```vue
<template>
    <div>
        <a-button type="primary">按钮</a-button>
        <ele-pro-table :datasource="datasource"></ele-pro-table>
    </div>
</template>

<script setup>
    import { Button as AButton } from 'ant-design-vue';
    import { EleProTable } from 'ele-admin-pro';
</script>
```

:::

手动引入过于麻烦，所以在 vite.config.ts 中配置了 `unplugin-vue-components` 的 vite 插件自动进行按需引入，无需手动引入。

原理就是 `AntDesignVueResolver` 会识别所有 `a` 开头的组件，`EleAdminResolver` 会识别所有 `ele` 开头组件并自动添加 `import` 的代码。

没手动引入编辑器无法识别组件类型进行提示及检查，所以根目录 `components.d.ts` 就是告诉编辑器组件的类型配置在 `tsconfig.json` 的 `include` 中。

```vue
<template>
    <div>
        <a-button type="primary">按钮</a-button>
        <!-- 编辑器可以识别出组件类型，如果瞎写就会给出错误提示 -->
        <a-button type="xxx">按钮</a-button>
    </div>
</template>
```

按需引入后 `vite` 启动超快，但访问时每当遇到新依赖都会重新编译并刷新页面，导致访问很慢要自动刷新多次，可在 `vite.config.ts` 配置依赖预构建：

```javascript
//......省略其它代码
export default defineConfig(({ command }) => {
    return {
        optimizeDeps: {
            include: [
                'ant-design-vue',
                'ant-design-vue/es',
                'ele-admin-pro',
                'ele-admin-pro/es',
                '@ant-design/icons-vue',
                'vuedraggable',
                'dayjs',
                'xlsx'
            ]
        }
    };
});
```

配置的依赖会在启动的时候先构建好，所以启动会慢一点点，例如富文本 `tinymce` 依赖没有配置，访问富文本页面时会提示构建依赖然后刷新页面。

<br/>

## 8.8. 改为全局引入

如果需要改为全局引入，修改 `main.ts` 为：

```javascript
//......省略其它代码
// 全局引入 Antd 和 EleAdminPro
import Antd from 'ant-design-vue';
import EleAdminPro from 'ele-admin-pro';

const app = createApp(App);

// 然后 use
app.use(Antd);
app.use(EleAdminPro);

app.mount('#app');
```

然后修改 `src/style/index.less` 为：

```less
/** 全局样式 */
@import 'ant-design-vue/dist/antd.less';
@import 'ele-admin-pro/es/style/index.less';

// 主题
@import 'ele-admin-pro/es/style/themes/dynamic.less';
```

然后去掉 `vite.config.ts` 中按需引入插件的配置，`plugins` 下删除按需引入的插件 `ViteComponents` 。

**仅把样式改为全局引入，组件 js 还是按需引入**

按上面修改 `src/style/index.less`，然后 `vite.config.ts` 中按需引入插件的 `importStyle` 配置为 `false` ：

```javascript
// ......省略其它代码
export default defineConfig(({ command }) => {
    const isBuild = command === 'build';
    return {
        plugins: [
            vue(),
            // 组件按需引入
            ViteComponents({
                resolvers: [
                    AntDesignVueResolver({
                        importStyle: false
                    }),
                    EleAdminResolver({
                        importStyle: false
                    })
                ]
            })
        ]
        // ......省略其它代码
    };
});
```

<br/>

## 8.9. 关闭布局响应

在 `src/layout/index.vue` 中使用 `style-responsive` 属性关闭布局样式的响应式，关闭后移动端会整体缩放显示：

```vue
<template>
    <ele-pro-layout :style-responsive="false"></ele-pro-layout>
</template>
```

这个属性会对 EleAdminPro 的所有组件都生效，但是 AntDesignVue 的一些组件需要自己避免用到一些响应式的属性，如：

```vue
<template>
    <!-- a-col 不要使用 lg、md、sm 等属性 -->
    <a-row :gutter="16">
        <a-col :md="12" :sm="12" :xs="24">
        </a-col>
        <a-col :md="12" :sm="12" :xs="24">
        </a-col>
    </a-row>
    <!-- 应该使用 span 属性 -->
    <a-row :gutter="16">
        <a-col :span="12">
        </a-col>
        <a-col :span="12">
        </a-col>
    </a-row>

    <!-- a-form 的 label-col 和 wrapper-col 也是同理 -->
    <a-form :label-col="{ md: 7, sm: 4, xs: 24 }" :wrapper-col="{ md: 17, sm: 20, xs: 24 }">
        <a-form-item label="用户账号">
            <a-input placeholder="请输入用户账号" v-model:value="form.username" />
        </a-form-item>
        <a-form-item label="用户名">
            <a-input placeholder="请输入用户名" v-model:value="form.nickname" />
        </a-form-item>
    </a-form>
    <!-- 可以换成 flex 属性 -->
    <a-form :label-col="{ flex: '90px' }" :wrapper-col="{ flex: '1' }">
        <a-form-item label="用户账号">
            <a-input placeholder="请输入用户账号" v-model:value="form.username" />
        </a-form-item>
        <a-form-item label="用户名">
            <a-input placeholder="请输入用户名" v-model:value="form.nickname" />
        </a-form-item>
    </a-form>
</template>
```

然后就可以给 body 设置最小宽度了，可以在 `public.html` 中给 `body` 加 `class="ele-body-limit-width"` ：

```html
<html lang="zh">
    <head>
        <!-- viewport 也设置宽度为 body 的宽度可解决小屏幕底部大片空白的问题 -->
        <meta name="viewport" content="width=1400, initial-scale=1.0" />
        <!-- ......省略其它代码 -->
    </head>
    <!-- 加这个 class 会给 body 设置最小宽度为 `1400px` -->
    <body class="ele-body-limit-width">
        <!-- ......省略其它代码 -->
    </body>
</html>
```

并且 AntDesignVue 的表单小屏幕下 label 会上下排列，这个 class 也会进行修正，如果要修改宽度值在 `src/styles/index.less` 中加：

```less
@body-max-width: 1400px;
```