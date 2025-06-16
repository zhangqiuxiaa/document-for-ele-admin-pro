---
title: 地图组件
createTime: 2025/01/10 09:30:40
permalink: /EleAdminPro/2q5kaij4/
---
## 6.6.1. 快速使用  

组件名 `ele-map-picker` ，用于弹窗选择地图位置，使用方式：

::: code-tabs

@tab TypeScript

```vue
<template>
    <ele-map-picker
        v-model:visible="visible"
        :need-city="true" 
        map-key="请先到高德地图官网申请 key 并填写"
        @done="onDone" />
</template>

<script lang="ts" setup>
    import { ref } from 'vue';
    import type { CenterPoint } from 'ele-admin-pro/es/ele-map-picker/types';

    // 是否显示地图选择弹窗
    const visible = ref(false);
    
    /* 地图选择后回调 */
    const onDone = (location: CenterPoint) => {
        console.log(location);
    };
</script>
```

@tab JavaScript

```vue
<template>
    <ele-map-picker
        v-model:visible="visible"
        :need-city="true" 
        map-key="请先到高德地图官网申请 key 并填写"
        @done="onDone" />
</template>

<script setup>
    import { ref } from 'vue';

    // 是否显示地图选择弹窗
    const visible = ref(false);
    
    /* 地图选择后回调 */
    const onDone = (location) => {
        console.log(location);
    };
</script>
```

:::

`mapKey` 支持全局配置见文档 6.6.4 ，选择后返回的数据如下，`lat` 是纬度，`lng` 是经度，`city` 只有设置 `:need-city="true"` 才会返回：

```json
{
    "name": "光谷广场",
    "address": "珞喻路889号",
    "lat": 30.50756,
    "lng": 114.398107,
    "city": {
        "province": "湖北省",
        "city": "武汉市",
        "district": "洪山区",
        "citycode": "027"
    }
}
```

![](/images/6-6-1.png)

注意，v1.5.0 以及之前的版本需要手动引入：

```vue
<script>
    import EleMapPicker from 'ele-admin-pro/packages/ele-map-picker';
    // v1.0.0 版本的引入方式为如下写法:
    //import EleMapPicker from '@/components/EleMapPicker';

    export default {
        components: { EleMapPicker }
    }
</script>
```

<br/>

## 6.6.2. 属性列表  

| 属性名            | 类型            | 说明                                            | 默认值                  |
| :---------------- | :-------------- | :---------------------------------------------- | :---------------------- |
| visible           | Boolean         | 是否显示(v-model)                               |                         |
| height            | String          | 地图的高度                                      | '450px'                 |
| center            | Array           | 地图默认中心点，例如 `[114.398107, 30.50756]`   |                         |
| zoom              | Number          | 地图初始缩放级别                                | 11                      |
| chooseZoom        | Number          | 地图选中后缩放级别                              | 17                      |
| poiSize           | Number          | poi 检索每页数量                                | 30                      |
| poiType           | String          | poi 检索兴趣点类别                              | 参考值 '酒店'、'电影院' |
| poiKeywords       | String          | poi 检索关键字                                  | 参考值 '大学'、'栋'     |
| poiRadius         | Number          | poi 检索半径                                    | 1000                    |
| needCity          | Boolean         | 是否返回行政区                                  | false                   |
| forceChoose       | Boolean         | 是否强制选择                                    | true                    |
| suggestionCity    | String          | 搜索建议的城市范围                              | '全国'                  |
| searchType        | Number          | 地点检索类型, 0: POI 检索, 1: 关键字检索        | 0                       |
| searchPlaceholder | String          | 搜索建议框提示文字                              | '输入关键字搜索'        |
| markerSrc         | String          | marker 图标地址                                 |                         |
| mapKey            | String          | 地图默认的 key                                  |                         |
| mapVersion        | String          | 地图默认版本号                                  | 2.0                     |
| mapStyle          | String          | 地图风格                                        | 'amap://styles/normal'  |
| title             | String          | 弹窗的标题                                      | '选择位置'              |
| width             | String          | 弹窗的宽度                                      | '780px'                 |
| dialogStyle       | Object          | 弹窗样式                                        |                         |
| centered          | Boolean         | 垂直居中展示 Modal                              | false                   |
| closable          | Boolean         | 是否显示右上角的关闭按钮                        | true                    |
| destroyOnClose    | Boolean         | 关闭时销毁 Modal 里的子元素                     | false                   |
| forceRender       | Boolean         | 强制渲染 Modal                                  | false                   |
| keyboard          | Boolean         | 是否支持键盘 esc 关闭                           | true                    |
| mask              | Boolean         | 是否展示遮罩                                    | true                    |
| maskClosable      | Boolean         | 点击蒙层是否允许关闭                            | true                    |
| maskStyle         | Object          | 遮罩样式                                        | {}                      |
| wrapClassName     | String          | 对话框外层容器的类名                            |                         |
| zIndex            | Number          | 设置 Modal 的 z-index                           | 1000                    |
| dialogClass       | String          | 可用于设置浮层的类名                            |                         |
| movable           | Boolean         | 是否可以拖动 `v1.7.0新增`                       | true                    |
| moveOut           | Boolean         | 是否可以拖出边界 `v1.7.0新增`                   | false                   |
| moveOutPositive   | Boolean         | 如果可以拖出边界是否只允许下右拖出 `v1.7.0新增` | false                   |
| resizable         | Boolean, String | 是否可以拉伸 `v1.7.0新增`                       | false                   |
| maxable           | Boolean         | 是否显示最大化切换按钮 `v1.7.0新增`             | false                   |
| multiple          | Boolean         | 是否支持打开多个 `v1.7.0新增`                   | false                   |
| fullscreen        | Boolean         | 是否默认全屏 `v1.7.0新增`                       | false                   |
| inner             | Boolean         | 是否限制在主体内部 `v1.7.0新增`                 | false                   |
| resetOnClose      | Boolean         | 是否在弹窗关闭后重置位置和大小 `v1.7.0新增`     | true                    |
| maskClass         | String          | 遮罩的类名 `v1.9.0新增`                         |                         |

  参数 `searchType` 地点检索类型，0 是 POI 检索，是检索地图中心点周围的兴趣点，1 是关键字检索，是根据输入的关键字检索匹配的地点， 弹窗相关的几个参数同 ant-design-vue 的 a-modal 组件的参数及弹窗扩展 ele-modal 组件。

<br/>

## 6.6.3. 事件列表  

| 事件名称       | 说明                             | 回调参数         |
| :------------- | :------------------------------- | :--------------- |
| open           | dialog 打开的回调                | 无               |
| closed         | dialog 关闭的回调                | 无               |
| done           | 选择完成的回调                   | 选择的结果       |
| update:visible | 用于 visible 的 v-model          | visible: boolean |
| map-done       | 地图渲染完成的事件 `v1.10.0新增` | ins 地图实例     |

<br/>

## 6.6.4. 设置地图 key  

使用地图组件需要先到 [高德地图官网](https://console.amap.com/dev/key/app) 创建应用获取 Key ，然后通过属性 `mapKey` 指定或者在 App.vue 中全局配置：

```vue
<template>
    <!-- 注意 ele-config-provider 组件是 v1.7.0新增，之前版本不支持全局配置 mapKey -->
    <ele-config-provider map-key="申请好的Web端开发者Key">
        <router-view />
    </ele-config-provider>
</template>
```

需要注意在 2021 年 12 月 02 日以后申请的 Key 需要配合安全密钥一起使用 [见文档](https://lbs.amap.com/api/jsapi-v2/guide/abc/load) ， 最简单的在 App.vue 中增加：

```vue
<script setup>
    window._AMapSecurityConfig = {
        securityJsCode: '您申请的安全密钥'
    }
</script>
```

<br/>

## 6.6.5. 直接使用地图  

高德地图 JSAPI 文档地址：https://lbs.amap.com/api/jsapi-v2/summary ，实现官网底部地图：

::: code-tabs

@tab TypeScript

```vue
<template>
    <div ref="mapRef" style="height: 300px;max-width: 800px;"></div>
</template>

<script lang="ts" setup>
    import { ref, onMounted, onUnmounted } from 'vue';
    import AMapLoader from '@amap/amap-jsapi-loader';

    //
    const mapRef = ref<HTMLDivElement | null>(null);

    // 地图实例
    let mapIns: any;

    /* 渲染地图 */
    const renderMap = () => {
        AMapLoader.load({
            key: '申请好的Web端开发者Key',
            version: '2.0',
            plugins: ['AMap.InfoWindow', 'AMap.Marker']
        }).then((AMap) => {
            // 渲染地图
            mapIns = new AMap.Map(mapRef.value, {
                zoom: 13, // 初缩放级别
                center: [114.346084, 30.511215] // 初始中心点
            });
            // 创建信息窗体
            const infoWindow = new AMap.InfoWindow({
                content: `
                    <div style="color: #333;">
                        <div style="padding: 5px;font-size: 16px;">武汉易云智科技有限公司</div>
                        <div style="padding: 0 5px;">地址：湖北省武汉市洪山区雄楚大道222号</div>
                        <div style="padding: 0 5px;">电话：020-123456789</div>
                    </div>
                    <a class="ele-text-primary"
                       style="padding: 8px 5px 0;text-decoration: none;display: inline-block;"
                       href="//uri.amap.com/marker?position=114.346084,30.511215&name=武汉易云智科技有限公司"
                       target="_blank">到这里去→
                    </a>
                    `
            });
            infoWindow.open(mapIns, [114.346084, 30.511215]);
            // 添加标记点
            const icon = new AMap.Icon({
                size: new AMap.Size(25, 34),
                image: '//a.amap.com/jsapi_demos/static/demo-center/icons/poi-marker-red.png',
                imageSize: new AMap.Size(25, 34)
            });
            const marker = new AMap.Marker({
                icon: icon,
                position: [114.346084, 30.511215],
                offset: new AMap.Pixel(-12, -28)
            });
            marker.setMap(mapIns);
            marker.on('click', () => {
                infoWindow.open(mapIns);
            });
        }).catch(e => {
            console.error(e);
        });
    };
    
    /* 渲染地图 */
    onMounted(() => {
        renderMap();
    });
    
    /* 销毁地图 */
    onUnmounted(() => {
        if (mapIns) {
            mapIns.destroy();
        }
    });
</script>
```

@tab JavaScript

```vue
<template>
    <div ref="mapRef" style="height: 300px;max-width: 800px;"></div>
</template>

<script setup>
    import { ref, onMounted, onUnmounted } from 'vue';
    import AMapLoader from '@amap/amap-jsapi-loader';

    //
    const mapRef = ref(null);

    // 地图实例
    let mapIns;

    /* 渲染地图 */
    const renderMap = () => {
        AMapLoader.load({
            key: '申请好的Web端开发者Key',
            version: '2.0',
            plugins: ['AMap.InfoWindow', 'AMap.Marker']
        }).then((AMap) => {
            // 渲染地图
            mapIns = new AMap.Map(mapRef.value, {
                zoom: 13, // 初缩放级别
                center: [114.346084, 30.511215] // 初始中心点
            });
            // 创建信息窗体
            const infoWindow = new AMap.InfoWindow({
                content: `
                    <div style="color: #333;">
                        <div style="padding: 5px;font-size: 16px;">武汉易云智科技有限公司</div>
                        <div style="padding: 0 5px;">地址：湖北省武汉市洪山区雄楚大道222号</div>
                        <div style="padding: 0 5px;">电话：020-123456789</div>
                    </div>
                    <a class="ele-text-primary"
                       style="padding: 8px 5px 0;text-decoration: none;display: inline-block;"
                       href="//uri.amap.com/marker?position=114.346084,30.511215&name=武汉易云智科技有限公司"
                       target="_blank">到这里去→
                    </a>
                    `
            });
            infoWindow.open(mapIns, [114.346084, 30.511215]);
            // 添加标记点
            const icon = new AMap.Icon({
                size: new AMap.Size(25, 34),
                image: '//a.amap.com/jsapi_demos/static/demo-center/icons/poi-marker-red.png',
                imageSize: new AMap.Size(25, 34)
            });
            const marker = new AMap.Marker({
                icon: icon,
                position: [114.346084, 30.511215],
                offset: new AMap.Pixel(-12, -28)
            });
            marker.setMap(mapIns);
            marker.on('click', () => {
                infoWindow.open(mapIns);
            });
        }).catch(e => {
            console.error(e);
        });
    };
    
    /* 渲染地图 */
    onMounted(() => {
        renderMap();
    });
    
    /* 销毁地图 */
    onUnmounted(() => {
        if (mapIns) {
            mapIns.destroy();
        }
    });
</script>
```

:::

![](/images/6-6-2.png)

<br/>

## 6.6.6. 实现地图轨迹回放 

::: code-tabs

@tab TypeScript

```vue
<template>
    <div>
        <div ref="mapRef" style="height: 300px;max-width: 800px;"></div>
        <a-button @click="startTrackAnim">开始动画</a-button>
        <a-button @click="pauseTrackAnim">暂停动画</a-button>
        <a-button @click="resumeTrackAnim">继续动画</a-button>
    </div>
</template>

<script lang="ts" setup>
    import { ref, onMounted, onUnmounted } from 'vue';
    import AMapLoader from '@amap/amap-jsapi-loader';

    //
    const mapRef = ref<HTMLDivElement | null>(null);

    // 地图实例
    let mapIns: any;
    
    // 小车 marker
    let carMarker: any;
    
    // 轨迹路线
    const lineData = [
        [116.478935, 39.997761],
        [116.478939, 39.997825],
        [116.478912, 39.998549],
        [116.478912, 39.998549],
        [116.478998, 39.998555],
        [116.478998, 39.998555],
        [116.479282, 39.99856],
        [116.479658, 39.998528],
        [116.480151, 39.998453],
        [116.480784, 39.998302],
        [116.480784, 39.998302],
        [116.481149, 39.998184],
        [116.481573, 39.997997],
        [116.481863, 39.997846],
        [116.482072, 39.997718],
        [116.482362, 39.997718],
        [116.483633, 39.998935],
        [116.48367, 39.998968],
        [116.484648, 39.999861]
    ];
    
    /* 渲染地图 */
    const renderMap = () => {
        AMapLoader.load({
            key: '申请好的Web端开发者Key',
            version: '2.0',
            plugins: ['AMap.MoveAnimation', 'AMap.Marker', 'AMap.Polyline']
        }).then((AMap) => {
            // 渲染地图
            mapIns = new AMap.Map(trackMapRef.value, {
                zoom: 17,
                center: [116.478935, 39.997761]
            });
            // 创建小车 marker
            carMarker = new AMap.Marker({
                map: mapIns,
                position: [116.478935, 39.997761],
                icon: 'https://a.amap.com/jsapi_demos/static/demo-center-v2/car.png',
                offset: new AMap.Pixel(-13, -26)
            });
            // 绘制轨迹
            new AMap.Polyline({
                map: mapIns,
                path: lineData,
                showDir: true,
                strokeColor: '#2288FF', // 线颜色
                strokeOpacity: 1, // 线透明度
                strokeWeight: 6 // 线宽
                //strokeStyle: 'solid'  // 线样式
            });
            // 通过的轨迹
            const passedPolyline = new AMap.Polyline({
                map: mapInsTrack,
                showDir: true,
                strokeColor: '#44BB55', // 线颜色
                strokeOpacity: 1, // 线透明度
                strokeWeight: 6 // 线宽
            });
            // 小车移动回调
            carMarker.on('moving', (e) => {
                passedPolyline.setPath(e.passedPath);
            });
            // 地图自适应
            mapIns.setFitView();
        }).catch(e => {
            console.error(e);
        });
    };
    
    /* 开始轨迹回放动画 */
    const startTrackAnim = () => {
        if (carMarker) {
            carMarker.stopMove();
            carMarker.moveAlong(lineData, {
                duration: 200,
                autoRotation: true
            });
        }
    };
    
    /* 暂停轨迹回放动画 */
    const pauseTrackAnim = () => {
        if (carMarker) {
            carMarker.pauseMove();
        }
    };
    
    /* 继续开始轨迹回放动画 */
    const resumeTrackAnim = () => {
        if (carMarker) {
            carMarker.resumeMove();
        }
    };
    
    /* 渲染地图 */
    onMounted(() => {
        renderMap();
    });
    
    /* 销毁地图 */
    onUnmounted(() => {
        if (mapIns) {
            mapIns.destroy();
        }
    });
</script>
```

@tab JavaScript

```vue
<template>
    <div>
        <div ref="mapRef" style="height: 300px;max-width: 800px;"></div>
        <a-button @click="startTrackAnim">开始动画</a-button>
        <a-button @click="pauseTrackAnim">暂停动画</a-button>
        <a-button @click="resumeTrackAnim">继续动画</a-button>
    </div>
</template>

<script setup>
    import { ref, onMounted, onUnmounted } from 'vue';
    import AMapLoader from '@amap/amap-jsapi-loader';

    //
    const mapRef = ref(null);

    // 地图实例
    let mapIns;
    
    // 小车 marker
    let carMarker;
    
    // 轨迹路线
    const lineData = [
        [116.478935, 39.997761],
        [116.478939, 39.997825],
        [116.478912, 39.998549],
        [116.478912, 39.998549],
        [116.478998, 39.998555],
        [116.478998, 39.998555],
        [116.479282, 39.99856],
        [116.479658, 39.998528],
        [116.480151, 39.998453],
        [116.480784, 39.998302],
        [116.480784, 39.998302],
        [116.481149, 39.998184],
        [116.481573, 39.997997],
        [116.481863, 39.997846],
        [116.482072, 39.997718],
        [116.482362, 39.997718],
        [116.483633, 39.998935],
        [116.48367, 39.998968],
        [116.484648, 39.999861]
    ];
    
    /* 渲染地图 */
    const renderMap = () => {
        AMapLoader.load({
            key: '申请好的Web端开发者Key',
            version: '2.0',
            plugins: ['AMap.MoveAnimation', 'AMap.Marker', 'AMap.Polyline']
        }).then((AMap) => {
            // 渲染地图
            mapIns = new AMap.Map(trackMapRef.value, {
                zoom: 17,
                center: [116.478935, 39.997761]
            });
            // 创建小车 marker
            carMarker = new AMap.Marker({
                map: mapIns,
                position: [116.478935, 39.997761],
                icon: 'https://a.amap.com/jsapi_demos/static/demo-center-v2/car.png',
                offset: new AMap.Pixel(-13, -26)
            });
            // 绘制轨迹
            new AMap.Polyline({
                map: mapIns,
                path: lineData,
                showDir: true,
                strokeColor: '#2288FF', // 线颜色
                strokeOpacity: 1, // 线透明度
                strokeWeight: 6 // 线宽
                //strokeStyle: 'solid'  // 线样式
            });
            // 通过的轨迹
            const passedPolyline = new AMap.Polyline({
                map: mapInsTrack,
                showDir: true,
                strokeColor: '#44BB55', // 线颜色
                strokeOpacity: 1, // 线透明度
                strokeWeight: 6 // 线宽
            });
            // 小车移动回调
            carMarker.on('moving', (e) => {
                passedPolyline.setPath(e.passedPath);
            });
            // 地图自适应
            mapIns.setFitView();
        }).catch(e => {
            console.error(e);
        });
    };
    
    /* 开始轨迹回放动画 */
    const startTrackAnim = () => {
        if (carMarker) {
            carMarker.stopMove();
            carMarker.moveAlong(lineData, {
                duration: 200,
                autoRotation: true
            });
        }
    };
    
    /* 暂停轨迹回放动画 */
    const pauseTrackAnim = () => {
        if (carMarker) {
            carMarker.pauseMove();
        }
    };
    
    /* 继续开始轨迹回放动画 */
    const resumeTrackAnim = () => {
        if (carMarker) {
            carMarker.resumeMove();
        }
    };
    
    /* 渲染地图 */
    onMounted(() => {
        renderMap();
    });
    
    /* 销毁地图 */
    onUnmounted(() => {
        if (mapIns) {
            mapIns.destroy();
        }
    });
</script>
```

:::

![](/images/6-6-3.png)