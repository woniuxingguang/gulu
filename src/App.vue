<template>
<router-view></router-view>
</template>

<script lang="ts">
import {
    provide,
    ref
} from 'vue'
import {
    router
} from './router'

export default {
    name: 'App',
    setup() {
        const width = document.documentElement.clientWidth
        const menuVisible = ref(width <= 500 ? false : true)
        provide('menuVisible', menuVisible)
        router.afterEach(() => {
            //只有在手机端时路由切换的时候 menu 消失
            if (width <= 500) {
                menuVisible.value = false
            }
        })
    },
    components: {

    }
}
</script>
