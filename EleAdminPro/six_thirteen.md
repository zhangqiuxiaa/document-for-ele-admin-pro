---
title: 统计图表
createTime: 2025/01/10 09:30:40
permalink: /EleAdminPro/16zpfigy/
---
## 6.13.1. 快速使用  

图表使用的是 [echarts 5.x](https://echarts.apache.org/) ，以及 vue 组件 [ecomfe/vue-echarts](https://github.com/ecomfe/vue-echarts) ，使用示例：

::: code-tabs
@tab TypeScript

```vue
<template>
    <v-chart
        ref="saleChartRef"
        style="height: 320px;"
        :option="saleChartOption"/>
</template>

<script lang="ts" setup>
    import { ref, reactive } from 'vue';
    import { use } from 'echarts/core';
    import type { EChartsCoreOption } from 'echarts/core';
    import { CanvasRenderer } from 'echarts/renderers';
    import { BarChart } from 'echarts/charts';
    import { GridComponent, TooltipComponent } from 'echarts/components';
    import VChart from 'vue-echarts';
    import { getSaleroomList } from '@/api/dashboard/analysis';

    // 按需加载 echarts
    use([
        CanvasRenderer,
        BarChart,
        GridComponent,
        TooltipComponent
    ]);

    // 图表 ref
    const saleChartRef = ref<InstanceType<typeof VChart> | null>(null);

    // 图表配置
    const saleChartOption = reactive<EChartsCoreOption>({});

    /* 获取数据 */
    const getSaleroomData = () => {
        getSaleroomList().then((data) => {
            Object.assign(saleChartOption, {
                tooltip: {
                    trigger: 'axis'
                },
                xAxis: [
                    {
                        type: 'category',
                        data: data.map((d) => d.month)
                    }
                ],
                yAxis: [
                    {
                        type: 'value'
                    }
                ],
                series: [
                    {
                        type: 'bar',
                        data: data.map((d) => d.value)
                    }
                ]
            });
        }).catch((e) => {
            console.error(e);
        });
    };

    getSaleroomData();
</script>
```

@tab JavaScript

```vue
<template>
    <v-chart
        ref="saleChartRef"
        style="height: 320px;"
        :option="saleChartOption"/>
</template>

<script setup>
    import { ref, reactive } from 'vue';
    import { use } from 'echarts/core';
    import { CanvasRenderer } from 'echarts/renderers';
    import { BarChart } from 'echarts/charts';
    import { GridComponent, TooltipComponent } from 'echarts/components';
    import VChart from 'vue-echarts';
    import { getSaleroomList } from '@/api/dashboard/analysis';

    // 按需加载 echarts
    use([
        CanvasRenderer,
        BarChart,
        GridComponent,
        TooltipComponent
    ]);

    // 图表 ref
    const saleChartRef = ref(null);

    // 图表配置
    const saleChartOption = reactive({});

    /* 获取数据 */
    const getSaleroomData = () => {
        getSaleroomList().then((data) => {
            Object.assign(saleChartOption, {
                tooltip: {
                    trigger: 'axis'
                },
                xAxis: [
                    {
                        type: 'category',
                        data: data.map((d) => d.month)
                    }
                ],
                yAxis: [
                    {
                        type: 'value'
                    }
                ],
                series: [
                    {
                        type: 'bar',
                        data: data.map((d) => d.value)
                    }
                ]
            });
        }).catch((e) => {
            console.error(e);
        });
    };

    getSaleroomData();
</script>
```

:::

::: code-tabs
@tab TypeScript

```typescript
import request from '@/utils/request';
import type { ApiResult } from '@/api';
import type { SaleroomResult } from './model';

/** 获取销售量数据 */
export async function getSaleroomList() {
    const res = await request.get<ApiResult<SaleroomResult>>(
        'https://cdn.eleadmin.com/20200610/analysis-saleroom.json'
    );
    if (res.data.code === 0 && res.data.data) {
        return res.data.data;
    }
    return Promise.reject(new Error(res.data.message));
}
```

@tab JavaScript

```javascript
import request from '@/utils/request';

/** 获取销售量数据 */
export async function getSaleroomList() {
    const res = await request.get(
        'https://cdn.eleadmin.com/20200610/analysis-saleroom.json'
    );
    if (res.data.code === 0 && res.data.data) {
        return res.data.data;
    }
    return Promise.reject(new Error(res.data.message));
}
```

:::

![](/images/6-13-1.png)

<br/>

## 6.13.2. 设置主题  

ele-admin-pro 内置了一套好看的 echarts 主题，可以通过 provide 给 VChart 组件设置主题：

```vue
<script setup>
    import { provide } from 'vue';
    import VChart, { THEME_KEY } from 'vue-echarts';
    import { ChartTheme } from 'ele-admin-pro';

    provide(THEME_KEY, ChartTheme);
</script>
```

v1.10.0 版本开始提供了 `src/utils/use-echarts.ts` 封装了自动切换暗黑主题、重置尺寸等操作，使用方式：

```vue
<template>
    <div>
        <v-chart
            ref="visitChartRef"
            :option="visitChartOption"
            style="height: 40px"
        />
        <v-chart
            ref="payNumChartRef"
            :option="payNumChartOption"
            style="height: 40px"
        />
    </div>
</template>

<script lang="ts" setup>
    import { ref } from 'vue';
    import type { EChartsCoreOption } from 'echarts/core';
    import type VChart from 'vue-echarts';
    import useEcharts from '@/utils/use-echarts';

    //
    const visitChartRef = ref<InstanceType<typeof VChart> | null>(null);
    const payNumChartRef = ref<InstanceType<typeof VChart> | null>(null);

    // useEcharts 使用传递图表的 ref，数组形式，可传多个
    useEcharts([visitChartRef, payNumChartRef]);

    // 图表配置, 省略......
    const visitChartOption: EChartsCoreOption = reactive({});

    // 图表配置, 省略......
    const payNumChartOption: EChartsCoreOption = reactive({});
</script>
```

<br/>

## 6.13.3. 使用词云  

echarts 词云是使用的 [echarts-wordcloud](https://github.com/ecomfe/echarts-wordcloud) 插件，使用示例：

::: code-tabs

@tab TypeScript

```vue
<template>
    <v-chart
        ref="hotSearchChartRef"
        style="height: 330px"
        :option="hotSearchChartOption"/>
</template>

<script lang="ts" setup>
    import { ref, reactive } from 'vue';
    import { use } from 'echarts/core';
    import type { EChartsCoreOption } from 'echarts/core';
    import { CanvasRenderer } from 'echarts/renderers';
    import { TooltipComponent } from 'echarts/components';
    import VChart from 'vue-echarts';
    import 'echarts-wordcloud';
    import { wordCloudColor } from 'ele-admin-pro';
    import { getWordCloudList } from '@/api/dashboard/analysis';

    use([ CanvasRenderer, TooltipComponent ]);

    // 词云图表 ref
    const hotSearchChartRef = ref<InstanceType<typeof VChart> | null>(null);

    // 词云图表配置
    const hotSearchChartOption = reactive<EChartsCoreOption>({});

    /* 获取词云数据 */
    const getWordCloudData = () => {
        getWordCloudList().then((data) => {
            Object.assign(hotSearchChartOption, {
                tooltip: {
                    show: true
                },
                series: [
                    {
                        type: 'wordCloud',
                        width: '100%',
                        height: '100%',
                        sizeRange: [12, 24],
                        gridSize: 6,
                        textStyle: {
                            // ele-admin-pro 提供的好看的词云文字配色
                            color: wordCloudColor
                        },
                        emphasis: {
                            textStyle: {
                                shadowBlur: 8,
                                shadowColor: 'rgba(0, 0, 0, .15)'
                            }
                        },
                        data: data
                    }
                ]
            });
        }).catch((e) => {
            console.error(e);
        });
    };

    getWordCloudData();
</script>
```

@tab JavaScript

```vue
<template>
    <v-chart
        ref="hotSearchChartRef"
        style="height: 330px"
        :option="hotSearchChartOption"/>
</template>

<script setup>
    import { ref, reactive } from 'vue';
    import { use } from 'echarts/core';
    import { CanvasRenderer } from 'echarts/renderers';
    import { TooltipComponent } from 'echarts/components';
    import VChart from 'vue-echarts';
    import 'echarts-wordcloud';
    import { wordCloudColor } from 'ele-admin-pro';
    import { getWordCloudList } from '@/api/dashboard/analysis';

    use([ CanvasRenderer, TooltipComponent ]);

    // 词云图表 ref
    const hotSearchChartRef = ref(null);

    // 词云图表配置
    const hotSearchChartOption = reactive({});

    /* 获取词云数据 */
    const getWordCloudData = () => {
        getWordCloudList().then((data) => {
            Object.assign(hotSearchChartOption, {
                tooltip: {
                    show: true
                },
                series: [
                    {
                        type: 'wordCloud',
                        width: '100%',
                        height: '100%',
                        sizeRange: [12, 24],
                        gridSize: 6,
                        textStyle: {
                            // ele-admin-pro 提供的好看的词云文字配色
                            color: wordCloudColor
                        },
                        emphasis: {
                            textStyle: {
                                shadowBlur: 8,
                                shadowColor: 'rgba(0, 0, 0, .15)'
                            }
                        },
                        data: data
                    }
                ]
            });
        }).catch((e) => {
            console.error(e);
        });
    };

    getWordCloudData();
</script>
```

:::

::: code-tabs

@tab TypeScript

```typescript
import request from '@/utils/request';
import type { ApiResult } from '@/api';
import type { CloudData } from './model';

/** 获取词云数据 */
export async function getWordCloudList() {
    const res = await request.get<ApiResult<CloudData[]>>(
        'https://cdn.eleadmin.com/20200610/analysis-hot-search.json'
    );
    if (res.data.code === 0 && res.data.data) {
        return res.data.data;
    }
    return Promise.reject(new Error(res.data.message));
}
```

@tab JavaScript

```javascript
import request from '@/utils/request';

/** 获取词云数据 */
export async function getWordCloudList() {
    const res = await request.get(
        'https://cdn.eleadmin.com/20200610/analysis-hot-search.json'
    );
    if (res.data.code === 0 && res.data.data) {
        return res.data.data;
    }
    return Promise.reject(new Error(res.data.message));
}
```

:::

![](/images/6-13-2.png)

<br/>

## 6.13.4. 使用地图  

echarts 使用地图需要先用地图的轮廓数据注册地图(轮廓数据放在 public 下)，使用示例：

::: code-tabs

@tab TypeScript

```vue
<template>
    <v-chart
        ref="userCountMapChartRef"
        style="height: 485px"
        :option="userCountMapOption"/>
</template>

<script lang="ts" setup>
    import { ref, reactive, provide } from 'vue';
    import { use, registerMap } from 'echarts/core';
    import type { EChartsCoreOption } from 'echarts/core';
    import { CanvasRenderer } from 'echarts/renderers';
    import { MapChart } from 'echarts/charts';
    import {
        VisualMapComponent,
        GeoComponent,
        TooltipComponent
    } from 'echarts/components';
    import VChart from 'vue-echarts';
    import { getChinaMapData, getUserCountList } from '@/api/dashboard/monitor';
    import type { UserCount } from '@/api/dashboard/monitor/model';

    use([
        CanvasRenderer,
        MapChart,
        VisualMapComponent,
        GeoComponent,
        TooltipComponent
    ]);

    // 地图图表 ref
    const userCountMapChartRef = ref<InstanceType<typeof VChart> | null>(null);

    // 地图图表配置
    const userCountMapOption = reactive<EChartsCoreOption>({});

    /* 获取中国地图数据并注册地图 */
    const registerChinaMap = () => {
        getChinaMapData().then((data) => {
            registerMap('china', data);
            getUserCountData();
        }).catch((e) => {
            console.error(e);
        });
    };

    /* 获取数据 */
    const getUserCountData = () => {
        getUserCountList().then((data) => {
            const temp = data.sort((a, b) => b.value - a.value);
            const min = temp[temp.length - 1].value || 0;
            const max = temp[0].value || 1;
            //
            Object.assign(userCountMapOption, {
                tooltip: {
                    trigger: 'item'
                },
                visualMap: {
                    min: min,
                    max: max,
                    text: ['高', '低'],
                    calculable: true,
                    color: ['#1890FF', '#EBF3FF']
                },
                series: [
                    {
                        name: '用户数',
                        label: {
                            show: true
                        },
                        type: 'map',
                        map: 'china',
                        data: data
                    }
                ]
            });
        }).catch((e) => {
            console.error(e);
        });
    };

    registerChinaMap();
</script>
```

@tab JavaScript

```vue
<template>
    <v-chart
        ref="userCountMapChartRef"
        style="height: 485px"
        :option="userCountMapOption"/>
</template>

<script setup>
    import { ref, reactive, provide } from 'vue';
    import { use, registerMap } from 'echarts/core';
    import { CanvasRenderer } from 'echarts/renderers';
    import { MapChart } from 'echarts/charts';
    import {
        VisualMapComponent,
        GeoComponent,
        TooltipComponent
    } from 'echarts/components';
    import VChart from 'vue-echarts';
    import { getChinaMapData, getUserCountList } from '@/api/dashboard/monitor';

    use([
        CanvasRenderer,
        MapChart,
        VisualMapComponent,
        GeoComponent,
        TooltipComponent
    ]);

    // 地图图表 ref
    const userCountMapChartRef = ref(null);

    // 地图图表配置
    const userCountMapOption = reactive({});

    /* 获取中国地图数据并注册地图 */
    const registerChinaMap = () => {
        getChinaMapData().then((data) => {
            registerMap('china', data);
            getUserCountData();
        }).catch((e) => {
            console.error(e);
        });
    };

    /* 获取数据 */
    const getUserCountData = () => {
        getUserCountList().then((data) => {
            const temp = data.sort((a, b) => b.value - a.value);
            const min = temp[temp.length - 1].value || 0;
            const max = temp[0].value || 1;
            //
            Object.assign(userCountMapOption, {
                tooltip: {
                    trigger: 'item'
                },
                visualMap: {
                    min: min,
                    max: max,
                    text: ['高', '低'],
                    calculable: true,
                    color: ['#1890FF', '#EBF3FF']
                },
                series: [
                    {
                        name: '用户数',
                        label: {
                            show: true
                        },
                        type: 'map',
                        map: 'china',
                        data: data
                    }
                ]
            });
        }).catch((e) => {
            console.error(e);
        });
    };

    registerChinaMap();
</script>
```

:::

::: code-tabs

@tab TypeScript

```typescript
import request from '@/utils/request';
import type { ApiResult } from '@/api';
import type { UserCount } from './model';
const BASE_URL = import.meta.env.BASE_URL;

/** 获取中国地图 geo 数据 */
export async function getChinaMapData() {
    const res = await request.get<any>(
        BASE_URL + 'json/china-provinces.geo.json',
        { baseURL: '' }
    );
    if (res.data) {
        return res.data;
    }
    return Promise.reject(new Error('获取地图数据失败'));
}

/** 获取用户分布数据 */
export async function getUserCountList() {
    const res = await request.get<ApiResult<UserCount[]>>(
        'https://cdn.eleadmin.com/20200610/monitor-user-count.json'
    );
    if (res.data.code === 0 && res.data.data) {
        return res.data.data;
    }
    return Promise.reject(new Error(res.data.message));
}
```

@tab JavaScript

```javascript
import request from '@/utils/request';
const BASE_URL = import.meta.env.BASE_URL;

/** 获取中国地图 geo 数据 */
export async function getChinaMapData() {
    const res = await request.get(
        BASE_URL + 'json/china-provinces.geo.json',
        { baseURL: '' }
    );
    if (res.data) {
        return res.data;
    }
    return Promise.reject(new Error('获取地图数据失败'));
}

/** 获取用户分布数据 */
export async function getUserCountList() {
    const res = await request.get(
        'https://cdn.eleadmin.com/20200610/monitor-user-count.json'
    );
    if (res.data.code === 0 && res.data.data) {
        return res.data.data;
    }
    return Promise.reject(new Error(res.data.message));
}
```

:::

![](/images/6-13-3.png)

<br/>

## 6.13.5. 自适应尺寸  

在 `src/utils/on-size-change` 中封装了监听尺寸变化方法，使用示例：

::: code-tabs

@tab TypeScript

```vue
<template>
    <div>
        <v-chart
            ref="visitChartRef"
            style="height: 40px"
            :option="visitChartOption"
        />
        <v-chart
            ref="payNumChartRef"
            style="height: 40px"
            :option="payNumChartOption"
        />
    </div>
</template>

<script lang="ts" setup>
    import { ref } from 'vue';
    import VChart from 'vue-echarts';
    import { onSizeChange } from '@/utils/on-size-change';

    // 图表 ref
    const visitChartRef = ref<InstanceType<typeof VChart> | null>(null);
    const payNumChartRef = ref<InstanceType<typeof VChart> | null>(null);

    /* 重置图表尺寸 */
    const resizeCharts = () => {
        visitChartRef.value?.resize();
        payNumChartRef.value?.resize();
    };

    onSizeChange(() => {
        resizeCharts();
    });
</script>
```

@tab JavaScript

```vue
<template>
    <div>
        <v-chart
            ref="visitChartRef"
            style="height: 40px"
            :option="visitChartOption"
        />
        <v-chart
            ref="payNumChartRef"
            style="height: 40px"
            :option="payNumChartOption"
        />
    </div>
</template>

<script setup>
    import { ref } from 'vue';
    import VChart from 'vue-echarts';
    import { onSizeChange } from '@/utils/on-size-change';

    // 图表 ref
    const visitChartRef = ref(null);
    const payNumChartRef = ref(null);

    /* 重置图表尺寸 */
    const resizeCharts = () => {
        visitChartRef.value?.resize();
        payNumChartRef.value?.resize();
    };

    onSizeChange(() => {
        resizeCharts();
    });
</script>
```

:::

`onSizeChange` 主要是监听 theme 状态管理的 `contentWidth` 变化，详见文档 8.3，还监听了 onActivated 生命周期适配 KeepAlive 。

v1.10.0 新增了 `useEcharts` 封装，支持自动切换暗黑主题、重置尺寸，并且使用更加的简单，使用方式见文档 6.13.2 。