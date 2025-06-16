---
title: 视频播放
createTime: 2025/01/10 09:30:40
permalink: /EleAdminPro/t1m965wl/
---
## 6.17.1. 快速使用  

基于 [西瓜视频播放器](http://h5player.bytedance.com/) 封装的 vue3 组件 `ele-xg-player` ，v1.6.0 新增，使用方式：

::: code-tabs

@tab TypeScript

```vue
<template>
    <div>
        <a-button @click="play" :disabled="!ready">播放</a-button>
        <a-button @click="pause" :disabled="!ready">暂停</a-button>
        <a-button @click="replay" :disabled="!ready">重新播放</a-button>
        <a-button @click="changeSrc" :disabled="!ready">切换视频源</a-button>
        <!-- 播放器 -->
        <ele-xg-player :config="config" @player="onPlayer" />
    </div>
</template>

<script lang="ts" setup>
    import { ref, reactive } from 'vue';
    import type Player from 'xgplayer';

    // 视频播放器配置
    const config = reactive({
        id: 'demoPlayer1',
        lang: 'zh-cn',
        fluid: true,
        // 视频地址
        url: 'https://s1.pstatp.com/cdn/expire-1-M/byted-player-videos/1.0.0/xgplayer-demo.mp4',
        // 封面
        poster: 'https://imgcache.qq.com/open_proj/proj_qcloud_v2/gateway/solution/general-video/css/img/scene/1.png',
        // 开启倍速播放
        playbackRate: [0.5, 1, 1.5, 2],
        // 开启画中画
        pip: true
    });

    // 视频播放器是否实例化完成
    const ready = ref(false);

    // 视频播放器实例
    let playerIns: Player;

    /* 播放器渲染完成 */
    const onPlayer = (player) => {
        playerIns = player;
        playerIns.on('play', () => {
            ready.value = true;
        });
    };

    /* 播放 */
    const play = () => {
        if (playerIns && playerIns.paused) {
            playerIns.play();
        }
    };

    /* 暂停 */
    const pause = () => {
        if (playerIns) {
            playerIns.pause();
        }
    };

    /* 重新播放 */
    const replay = () => {
        if (playerIns) {
            playerIns.replay();
        }
    };

    /* 切换视频源 */
    const changeSrc = () => {
        if (playerIns) {
            playerIns.src = 'https://blz-videos.nosdn.127.net/1/OverWatch/AnimatedShots/Overwatch_TheatricalTeaser_WeAreOverwatch_zhCN.mp4';
            if (playerIns.paused) {
                playerIns.play();
            }
        }
    };
</script>
```

@tab JavaScript

```vue
<template>
    <div>
        <a-button @click="play" :disabled="!ready">播放</a-button>
        <a-button @click="pause" :disabled="!ready">暂停</a-button>
        <a-button @click="replay" :disabled="!ready">重新播放</a-button>
        <a-button @click="changeSrc" :disabled="!ready">切换视频源</a-button>
        <!-- 播放器 -->
        <ele-xg-player :config="config" @player="onPlayer" />
    </div>
</template>

<script setup>
    import { ref, reactive } from 'vue';

    // 视频播放器配置
    const config = reactive({
        id: 'demoPlayer1',
        lang: 'zh-cn',
        fluid: true,
        // 视频地址
        url: 'https://s1.pstatp.com/cdn/expire-1-M/byted-player-videos/1.0.0/xgplayer-demo.mp4',
        // 封面
        poster: 'https://imgcache.qq.com/open_proj/proj_qcloud_v2/gateway/solution/general-video/css/img/scene/1.png',
        // 开启倍速播放
        playbackRate: [0.5, 1, 1.5, 2],
        // 开启画中画
        pip: true
    });

    // 视频播放器是否实例化完成
    const ready = ref(false);

    // 视频播放器实例
    let playerIns;

    /* 播放器渲染完成 */
    const onPlayer = (player) => {
        playerIns = player;
        playerIns.on('play', () => {
            ready.value = true;
        });
    };

    /* 播放 */
    const play = () => {
        if (playerIns && playerIns.paused) {
            playerIns.play();
        }
    };

    /* 暂停 */
    const pause = () => {
        if (playerIns) {
            playerIns.pause();
        }
    };

    /* 重新播放 */
    const replay = () => {
        if (playerIns) {
            playerIns.replay();
        }
    };

    /* 切换视频源 */
    const changeSrc = () => {
        if (playerIns) {
            playerIns.src = 'https://blz-videos.nosdn.127.net/1/OverWatch/AnimatedShots/Overwatch_TheatricalTeaser_WeAreOverwatch_zhCN.mp4';
            if (playerIns.paused) {
                playerIns.play();
            }
        }
    };
</script>
```

:::

属性就一个 `config` 配置 XgPlayer 的参数，必须指定 id 且建议唯一，事件就一个 `player` 用于获取渲染完成后的实例。

注意，v1.5.0 以及之前版本并没有封装 **ele-xg-player** ，使用方式如下：

```vue
<template>
    <xgplayer :config="config" @player="onPlayer"/>
</template>

<script>
    import Xgplayer from 'xgplayer-vue';

    export default {
        components: { Xgplayer },
        data() {
            return {
                // 视频播放器配置
                config: {
                    id: 'demoPlayer1',
                    lang: 'zh-cn',
                    fluid: true,
                    // 视频地址
                    url: 'https://s1.pstatp.com/cdn/expire-1-M/byted-player-videos/1.0.0/xgplayer-demo.mp4',
                    // 封面
                    poster: 'https://imgcache.qq.com/open_proj/proj_qcloud_v2/gateway/solution/general-video/css/img/scene/1.png',
                    // 开启倍速播放
                    playbackRate: [0.5, 1, 1.5, 2],
                    // 开启画中画
                    pip: true
                },
                // 视频播放器实例
                playerIns: null,
                // 视频播放器是否实例化完成
                ready: false
            };
        },
        methods: {
            /* 播放器渲染完成 */
            onPlayer(player) {
                this.playerIns = player;
                this.playerIns.on('play', () => {
                    this.ready = true;
                });
            },
            /* 播放 */
            play() {
                if (this.playerIns && this.playerIns.paused) {
                    this.playerIns.play();
                }
            },
            /* 暂停 */
            pause() {
                if (this.playerIns) {
                    this.playerIns.pause();
                }
            }
        }
    }
</script>
```

因为 `xgplayer-vue` 这个组件是 vue2 的，所以 v1.6.0 之后重新封装了一个以便更好的用于 Vue3 ，但使用方式是完全一样的。

<br/>

## 6.17.2. 弹幕功能  

::: code-tabs

@tab TypeScript

```vue
<template>
    <div>
        <a-input v-model:value="text" placeholder="请输入弹幕内容" />
        <a-button @click="shoot">发射</a-button>
        <!-- 播放器 -->
        <ele-xg-player :config="config" @player="onPlayer" />
    </div>
</template>

<script lang="ts" setup>
    import { ref, reactive } from 'vue';
    import type Player from 'xgplayer';

    // 视频播放器配置
    const config = reactive({
        id: 'demoPlayer',
        lang: 'zh-cn',
        fluid: true,
        url: 'https://blz-videos.nosdn.127.net/1/OverWatch/AnimatedShots/Overwatch_TheatricalTeaser_WeAreOverwatch_zhCN.mp4',
        poster: 'https://imgcache.qq.com/open_proj/proj_qcloud_v2/gateway/solution/general-video/css/img/scene/1.png',
        danmu: {
            comments: [
                {
                    id: '1',
                    start: 0,
                    txt: '空降',
                    duration: 15000,
                    color: true,
                    style: {
                        color: '#ffcd08',
                        fontSize: '20px'
                    }
                },
                {
                    id: '2',
                    start: 1500,
                    txt: '前方高能',
                    duration: 15000,
                    color: true,
                    style: {
                        color: '#ffcd08',
                        fontSize: '20px'
                    }
                },
                {
                    id: '3',
                    start: 3500,
                    txt: '弹幕护体',
                    duration: 15000,
                    color: true,
                    style: {
                    color: '#ffcd08',
                        fontSize: '20px'
                    }
                }
            ]
        }
    });

    // 弹幕内容
    const text = ref('');

    // 视频播放器是否实例化完成
    const ready = ref(false);

    // 视频播放器实例
    let playerIns: Player;

    /* 播放器渲染完成 */
    const onPlayer = (player: Player) => {
        playerIns = player;
        playerIns.on('play', () => {
            ready.value = true;
        });
    };

    /* 发射弹幕 */
    const shoot = () => {
        if (!playerIns) {
            return;
        }
        if (!text.value) {
            return;
        }
        playerIns.danmu.sendComment({
            id: new Date().getTime(),
            duration: 15000,
            color: true,
            start: playerIns.currentTime * 1000,
            txt: text.value,
            style: {
                color: '#fa1f41',
                fontSize: '20px',
                border: 'solid 1px #fa1f41'
            }
        });
        text.value = '';
    };
</script>
```

@tab JavaScript

```vue
<template>
    <div>
        <a-input v-model:value="text" placeholder="请输入弹幕内容" />
        <a-button @click="shoot">发射</a-button>
        <!-- 播放器 -->
        <ele-xg-player :config="config" @player="onPlayer" />
    </div>
</template>

<script setup>
    import { ref, reactive } from 'vue';

    // 视频播放器配置
    const config = reactive({
        id: 'demoPlayer',
        lang: 'zh-cn',
        fluid: true,
        url: 'https://blz-videos.nosdn.127.net/1/OverWatch/AnimatedShots/Overwatch_TheatricalTeaser_WeAreOverwatch_zhCN.mp4',
        poster: 'https://imgcache.qq.com/open_proj/proj_qcloud_v2/gateway/solution/general-video/css/img/scene/1.png',
        danmu: {
            comments: [
                {
                    id: '1',
                    start: 0,
                    txt: '空降',
                    duration: 15000,
                    color: true,
                    style: {
                        color: '#ffcd08',
                        fontSize: '20px'
                    }
                },
                {
                    id: '2',
                    start: 1500,
                    txt: '前方高能',
                    duration: 15000,
                    color: true,
                    style: {
                        color: '#ffcd08',
                        fontSize: '20px'
                    }
                },
                {
                    id: '3',
                    start: 3500,
                    txt: '弹幕护体',
                    duration: 15000,
                    color: true,
                    style: {
                    color: '#ffcd08',
                        fontSize: '20px'
                    }
                }
            ]
        }
    });

    // 弹幕内容
    const text = ref('');

    // 视频播放器是否实例化完成
    const ready = ref(false);

    // 视频播放器实例
    let playerIns;

    /* 播放器渲染完成 */
    const onPlayer = (player) => {
        playerIns = player;
        playerIns.on('play', () => {
            ready.value = true;
        });
    };

    /* 发射弹幕 */
    const shoot = () => {
        if (!playerIns) {
            return;
        }
        if (!text.value) {
            return;
        }
        playerIns.danmu.sendComment({
            id: new Date().getTime(),
            duration: 15000,
            color: true,
            start: playerIns.currentTime * 1000,
            txt: text.value,
            style: {
                color: '#fa1f41',
                fontSize: '20px',
                border: 'solid 1px #fa1f41'
            }
        });
        text.value = '';
    };
</script>
```

:::

<br/>

## 6.17.3. 播放 flv 格式  

因为 xgplayer 播放 flv 格式是另一个插件，所以封装的 ele-xg-player 不支持 flv ，可以再封装一个使用 flv 插件的。

先安装依赖 `pnpm install xgplayer-flv.js -S` ，然后在 src/components 下创建 `FlvXgPlayer/index.vue` ，代码如下：

::: code-tabs

@tab TypeScript

```vue
<template>
    <div :id="playerId"></div>
</template>

<script lang="ts">
    import type { PropType } from 'vue';
    import {
        defineComponent,
        computed,
        onMounted,
        onBeforeUpdate,
        onBeforeUnmount
    } from 'vue';
    import FlvJsPlayer from 'xgplayer-flv.js';
    import type { IPlayerOptions } from 'xgplayer';

    export default defineComponent({
        name: 'FlvXgPlayer',
        props: {
            config: {
                type: Object as PropType<IPlayerOptions>,
                required: true
            }
        },
        emits: ['player'],
        setup(props, { emit }) {
            let player: Player;
            
            const playerId = computed(() => props.config?.id);
            
            const init = () => {
                if (props.config?.url) {
                    player = new FlvJsPlayer(props.config);
                    emit('player', player);
                }
            };
            
            onMounted(() => {
                init();
            });
            
            onBeforeUpdate(() => {
                init();
            });
            
            onBeforeUnmount(() => {
                if (player && typeof player.destroy === 'function') {
                    player.destroy();
                }
            });
            
            return { playerId };
        }
    });
</script>
```

@tab JavaScript

```vue
<template>
    <div :id="playerId"></div>
</template>

<script>
    import {
        defineComponent,
        computed,
        onMounted,
        onBeforeUpdate,
        onBeforeUnmount
    } from 'vue';
    import FlvJsPlayer from 'xgplayer-flv.js';

    export default defineComponent({
        name: 'FlvXgPlayer',
        props: {
            config: {
                type: Object,
                required: true
            }
        },
        emits: ['player'],
        setup(props, { emit }) {
            let player;
            
            const playerId = computed(() => props.config?.id);
            
            const init = () => {
                if (props.config?.url) {
                    player = new FlvJsPlayer(props.config);
                    emit('player', player);
                }
            };
            
            onMounted(() => {
                init();
            });
            
            onBeforeUpdate(() => {
                init();
            });
            
            onBeforeUnmount(() => {
                if (player && typeof player.destroy === 'function') {
                    player.destroy();
                }
            });
            
            return { playerId };
        }
    });
</script>
```

:::

然后就可以使用 `<flv-xg-player>` 组件了，使用方式同上，还有 HLS、MPEG-DASH 格式都可以按照这种方式封装成组件。