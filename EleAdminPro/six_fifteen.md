---
title: 城市选择
createTime: 2025/01/10 09:30:40
permalink: /EleAdminPro/xajinzek/
---
## 6.15.1. 快速使用  

组件名 `RegionsSelect` ，位于 `src/components/RegionsSelect` ，v1.6.0 新增，使用方式：

::: code-tabs

@tab TypeScript

```vue
<template>
    <regions-select v-model:value="city" placeholder="请选择省市区"/>
</template>

<script lang="ts" setup>
    import { ref } from 'vue';
    import RegionsSelect from '@/components/RegionsSelect/index.vue';

    const city = ref<string[]>([]);
</script>
```

@tab JavaScript**

```vue
<template>
    <regions-select v-model:value="city" placeholder="请选择省市区"/>
</template>

<script setup>
    import { ref } from 'vue';
    import RegionsSelect from '@/components/RegionsSelect/index.vue';

    const city = ref([]);
</script>
```

:::

<br/>

## 6.15.2. 属性列表  

| 属性名      | 类型                 | 说明                    | 默认值                          |
| :---------- | :------------------- | :---------------------- | :------------------------------ |
| value       | String[]             | 绑定值(v-model)         |                                 |
| placeholder | String               | 级联选择器的提示信息    |                                 |
| options     | Array: RegionsData[] | 指定省市区数据          |                                 |
| valueField  | String               | value 的字段名称        | 默认 'value' 可选 'label'       |
| type        | String               | 类型                    | 可选 'provinceCity'、'province' |
| showSearch  | Boolean              | 是否可搜索 `v1.9.0新增` | true                            |

type 不指定是省市区选择，指定为 provinceCity 是省市选择，指定为 province 是省选择。

valueField 不指定默认选择后 value 存的是区号，指定为 label 后存的就是名称。

省市区数据 RegionsData 需要包含的字段：

- label   String   名称
- value   String   区号
- children   Array   子级, 最多包含三级, 第一级是省, 第二级是市, 第三级是区

<br/>

## 6.15.3. 事件列表  

| 事件名称       | 说明                         | 回调参数            |
| :------------- | :--------------------------- | :------------------ |
| update:value   | 更新 value 属性值            | value: String[]     |
| load-data-done | 省市区远程数据请求成功后回调 | data: RegionsData[] |

<br/>

## 6.15.4. 旧版使用  

v1.5.0 以及之前的版本并没有封装组件，只是提供了省市区数据，使用方法：

```javascript
import regions from 'ele-admin-pro/packages/regions';

console.log(regions.cityData);  // 省市区完整数据
console.log(regions.cityDataLabel);  // 省市区全中文，没有区号

console.log(regions.provinceData);  // 省数据
console.log(regions.provinceDataLabel);  // 省数据全中文

console.log(regions.provinceCityData);  // 省市数据
console.log(regions.provinceCityDataLabel);  // 省市数据全中文

console.log(regions.getLabel('110000'));  // 根据区号获取名称

console.log(regions.getLabel(['110000', '110100', '110101']));  // 根据区号获取名称
```

搭配级联选择器实现城市选择功能：

```vue
<template>
    <div>
        <a-cascader
            v-model:value="city"
            :options="regions.cityData"
            placeholder="请选择省市区"
            popup-class-name="ele-pop-wrap-higher"/>
        <a-cascader
            v-model:value="provinceCity"
            :options="regions.provinceCityData"
            placeholder="请选择省市"
            popup-class-name="ele-pop-wrap-higher"/>
        <a-cascader
            v-model:value="province"
            :options="regions.provinceData"
            placeholder="请选择省"
            popup-class-name="ele-pop-wrap-higher"/>
    </div>
</template>

<script>
    import regions from 'ele-admin-pro/packages/regions';

    export default {
        data() {
            return {
                // 省市区数据
                regions: regions,
                // 选中的省市区
                city: [],
                // 选中的省市
                provinceCity: [],
                // 选中的省
                province: []
            };
        }
    }
</script>
```