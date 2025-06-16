---
title: 打包部署
createTime: 2025/01/10 09:30:40
permalink: /EleAdminPro/wqdb913y/
---
## 9.1. 生产环境打包  

在项目根目录执行打包命令：

```bash
pnpm run build
```

打包完成后会生成 `dist` 目录，里面就是编译完成的代码，可以部署在你的服务器上，在打包之前也可以通过如下命令模拟生成环境运行：

```bash
pnpm run 
```

启动完成后浏览器访问 [http://localhost:5000](http://localhost:5000/) ，这种方式启动项目浏览器访问请求的文件与打包后是一样的，详见 vite 文档，1.6.0 之前版本可忽略。

<br/>

## 9.2. 部署到服务器  

  最简单的办法就是把 `dist` 里面打包好的文件放在 `nginx` 的 `html` 目录里面就能访问了，但是这样 nginx 只能部署一个项目， 可以配置 nginx 给项目分配一个端口或者绑定域名。

打开 `conf/nginx.conf` ，增加配置：

```bash
http {
    # 在http下面增加这一段
    server {
        # 这里配置端口
        listen       8080;
        server_name  localhost;

        location / {
            # 加这个是支持history的路由模式，很重要，不然刷新会404
            try_files $uri $uri/ /index.html;
            # 这里配置项目位置
            root   C:/webapp/eleadminpro;
            index  index.html index.htm;
        }
    }
}
```

把打包后的项目放在 `C:/webapp/eleadminpro/` 目录下，然后重启 nginx 在浏览器输入 `http://服务器ip:8080` 就能访问了。

<br/>

## 9.3. 绑定域名地址  

打开 `conf/nginx.conf` ，增加配置：

```bash
http {
    # 在http下面增加这一段
    server {
        # 端口要使用80
        listen       80;
        # 这里配置域名
        server_name  pro.eleadmin.com;

        location / {
            # 加这个是支持history的路由模式
            try_files $uri $uri/ /index.html;
            # 这里配置项目位置
            root   C:/webapp/eleadminpro;
            index  index.html index.htm;
        }
    }
}
```

把你的域名解析到你的服务器ip，然后就能用域名访问了。

**配置 https 的访问方式：**

```bash
http {
    # 在http下面增加这两段
    server {
        listen       80;
        # 这里配置域名
        server_name  pro.eleadmin.com;

        rewrite ^ https://$http_host$request_uri? permanent;
    }
    
    server {
        listen       443 ssl;
        # 这里配置域名
        server_name  pro.eleadmin.com;

        # 这里配置ssl证书
        ssl_certificate      cert/xxxx.pem;
        ssl_certificate_key  cert/xxxx.key;

        ssl_session_cache    shared:SSL:1m;
        ssl_session_timeout  5m;

        ssl_ciphers  HIGH:!aNULL:!MD5;
        ssl_prefer_server_ciphers  on;

        location / {
            # 加这个是支持history的路由模式
            try_files $uri $uri/ /index.html;
            # 这里配置项目位置
            root   C:/webapp/eleadminpro;
            index  index.html index.htm;
        }
    }
}
```

ssl 证书放在 `conf/cert` 目录下(在 conf 下新建一个 cert 目录)，确认名称配置的是否一致。

<br/>

## 9.4. 开启 gzip 功能  

  项目打包后一般都有一两个核心的 js 体积会很大，推荐 nginx 服务器开启 gzip 功能，gzip 可以压缩 3-5 倍左右， 能够大幅度优化首屏加载的速度，ele-admin-pro 已经配置了打包生成 gzip 文件，只需要给 nginx 增加如下配置：

```bash
server {
    listen       80;
    server_name  pro.eleadmin.com;

    # 开启gzip功能
    gzip on;
    gzip_min_length 10k;
    gzip_comp_level 9;
    gzip_types text/plain text/css application/javascript application/x-javascript text/javascript application/xml;
    gzip_vary on;
    gzip_disable "MSIE [1-6]\.";

    location / {
        try_files $uri $uri/ /index.html;
        root   C:/webapp/eleadminpro;
        index  index.html index.htm;
    }
}
```

<br/>

## 9.5. 部署在子路径  

项目打包后默认只能部署在服务器根路径，如果想 `http://localhost:3000/eleadmin/` 这种形式，可以根据 vite、vue-router 文档介绍进行配置。

1. 在 `vite.config.ts` 中配置：

```javascript
// ......省略其它代码
export default defineConfig(({ command }) => {
    return {
        // 在这里增加 base 写子路径
        base: '/eleadmin/',  // 注意这里前后都要有斜杠
        // ......省略其它代码
        resolve: { /*......省略*/ }
        // 如果要修改打包后的输出目录可以加这个配置
        build: {
            outDir: 'eleadmin'  // 默认是 dist
        }
    };
});
```

2. 然后在 `src/router/index.ts` 中增加：

```javascript
// ......省略其它代码
const router = createRouter({
    routes,
    // 给 createWebHistory 方法传参数配置子路径
    history: createWebHistory(import.meta.env.BASE_URL)
});
```

3. 修改 `src/layout/components/header-tools.vue` 中退出登录跳转的代码：

```javascript
// v1.8.0 以及之后的版本无需操作这一步
location.replace('/login');
// 替换为
location.replace(import.meta.env.BASE_URL);
```

4. 然后重新运行(run dev)或者打包(run build)部署后再访问就可以带上配置的子路径了，也可以在打包的时候动态设置子路径：

```bash
pnpm run build --base=/eleadmin/
```

**如果需要部署在任意路径，换路径不用重新打包，按照下面步骤配置**

1. 把 `vite.config.ts` 中的 `base` 配置值为空或者 `./` ：

```javascript
// ......省略其它代码
export default defineConfig(({ command }) => {
    return {
        base: '',
        //base: './',
    };
});
```

2. 把 `src/router/index.ts` 中路由 `history` 模式改为 `hash` 模式：

```javascript
import { createRouter, createWebHashHistory } from 'vue-router';

// ......省略其它代码
const router = createRouter({
    routes,
    history: createWebHashHistory()
});
```

3. 修改 `src/utils/page-tab-util.js` 中退出登录的方法 `logout` ：

```javascript
// 找到下面这行代码
location.replace(BASE_URL + 'login' + (from ? '?from=' + from : ''));
// v1.7.0 以及之前的版本是修改 src/layout/components/header-tools.vue 中下面这行代码
location.replace('/login');

// 替换为
location.reload();
```

4. 然后重新打包就可以部署在任意子路径，如 nginx 的 `www/demo/`、tomcat 的 `webapps/demo/`

打包后的目录名字默认是 `dist` 部署的时候要把目录名字改成自己想要的子路径的名字，如 `demo`。

注意，v1.5.0 以及之前的版本使用的不是 vite ，要按照如下方式配置：

1. 在 `vue.config.js` 中增加 `publicPath` 配置值为空或者 `./` ：

```javascript
module.exports = {
    publicPath: '',
    //publicPath: './',
    //......省略其它代码
}
```

2. 把 `src/router/index.js` 中路由 `history` 模式改为 `hash` 模式：

```javascript
import { createRouter, createWebHashHistory } from 'vue-router';
// ......省略其它代码
const router = createRouter({
    routes,
    history: createWebHashHistory()
});
```

3. 修改 `src/layout/header-right.vue` 和 `src/config/axios-config.js`：

```javascript
location.replace('/login');
// 搜索上面这行代码，替换为
location.reload();
```

4. 然后重新打包就可以部署在任意子路径，如 nginx 的 `www/demo/`、tomcat 的 `webapps/demo/`