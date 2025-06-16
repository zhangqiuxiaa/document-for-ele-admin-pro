---
title: 头像列表
createTime: 2025/01/10 09:30:40
permalink: /EleAdminPro/w94u02c0/
---
## 6.8.1. 快速使用  

```vue
<template>
    <ele-avatar-list :data="users" :size="22" />
</template>

<script setup>
    import { ref } from 'vue';

    const  users = ref([
        {
            name: 'SunSmile',
            avatar: 'http://cdn.eleadmin.com/20200609/c184eef391ae48dba87e3057e70238fb.jpg'
        },
        {
            name: '你的名字很好听',
            avatar: 'http://cdn.eleadmin.com/20200609/b6a811873e704db49db994053a5019b2.jpg'
        },
        {
            name: '全村人的希望',
            avatar: 'http://cdn.eleadmin.com/20200609/948344a2a77c47a7a7b332fe12ff749a.jpg'
        }
    ]);
</script>
```

![](/images/6-8-1.png)

```vue
<script>
    import EleAvatarList from '@/components/EleAvatarList';

    export default {
        components: { EleAvatarList }
    }
</script>
```

<br/>

## 6.8.2. 属性列表  

| 属性名           | 类型          | 说明                      | 默认值                          |
| :--------------- | :------------ | :------------------------ | :------------------------------ |
| data             | Array         | 数据                      |                                 |
| name             | String        | 数据名称的字段名          | 'name'                          |
| avatar           | String        | 数据头像的字段名          | 'avatar'                        |
| itemKey          | String        | key 字段名 `v1.6.0新增`   |                                 |
| max              | Number        | 最大显示个数，超出显示 +1 | 无限制                          |
| size             | String/Number | 头像的尺寸                | 60, 'large', 'default', 'small' |
| avatarStyle      | Object        | 头像自定义样式            |                                 |
| itemStyle        | Object        | 自定义样式                |                                 |
| moreStyle        | Object        | 数量超出显示 +1 的样式    |                                 |
| tooltip          | Boolean       | 是否显示鼠标移入显示名称  | true                            |
| placement        | String        | tooltip 的出现位置        | 'top'                           |
| overlayClassName | String        | tooltip 自定义 class      |                                 |
| overlayStyle     | String        | tooltip 自定义样式        |                                 |

tooltip 相关的几个参数同 ant design vue 的 tooltip 组件的参数。

<br/>

## 6.8.3. 事件列表  

| 事件名称   | 说明          | 回调参数            |
| :--------- | :------------ | :------------------ |
| item-click | item 点击事件 | item 当前点击的数据 |
| more-click | more 点击事件 | 无                  |