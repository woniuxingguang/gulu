<template>
<div>
    <div class="gulu-tabs">
        <div class="gulu-tabs-nav" ref="container">
            <div class="gulu-tabs-nav-item" :class="{selected: t == selected}" v-for="(t,index) in titles" :key="index" :ref="el => { if (t==selected) selectedItem = el }" @click="select(t)">{{t}}</div>
            <div class="gulu-tabs-nav-indicator" ref="indicator"></div>
        </div>
        <div class="gulu-tabs-content">
            <component class="gulu-tabs-content-item" v-for="(c,index) in defaults" :is="c" :key="index" :class="{selected: c.props.title == selected}"></component>
            <component :is="current" :key="current.props.title"></component>
        </div>
    </div>
</div>
</template>

<script lang="ts">
import {
    computed,
    ref,
    onMounted,
    watchEffect
} from 'vue'
import Tab from './Tab.vue'

export default {
    props: {
        selected: {
            type: String
        }
    },
    setup(props, context) {
        const selectedItem = ref < HTMLDivElement > (null)
        const indicator = ref < HTMLDivElement > (null)
        const container = ref < HTMLDivElement > (null)
        //挂载之后打印出来,只在第一次渲染执行
        onMounted(() => {
            watchEffect(() => {
                // 获取 选中 宽度
                const {
                    width
                } = selectedItem.value.getBoundingClientRect()
                // 动态给导航 下边横线
                indicator.value.style.width = width + 'px'
                // container 的left坐标
                const {
                    left: left1
                } = container.value.getBoundingClientRect()
                //获取当前 选中 nav 的left坐标
                const {
                    left: left2
                } = selectedItem.value.getBoundingClientRect()
                const left = left2 - left1
                // 动态 indicator 的位置
                indicator.value.style.left = left + 'px'
            })
        })
        //更新的时候
        // onUpdated(x)

        const defaults = context.slots.default()
        defaults.forEach((tag) => {
            if (tag.type !== Tab) {
                throw new Error('Tabs 子标签必须是 Tab')
            }
        })
        const titles = defaults.map((tag) => {
            return tag.props.title
        })
        // 计算属性
        const current = computed(() => {
            return defaults.filter((tag) => {
                return tag.props.title == props.selected
            })[0]
            // return defaults.find(tag => tag.props.title == props.selected)
        })
        const select = (title: string) => {
            context.emit('update:selected', title)
        }
        return {
            defaults,
            titles,
            current,
            select,
            selectedItem,
            indicator,
            container
        }
    }
}
</script>

<style lang="scss">
$blue: #40a9ff;
$color: #333;
$border-color: #d9d9d9;

.gulu-tabs {
    &-nav {
        position: relative;
        display: flex;
        color: $color;
        border-bottom: 1px solid $border-color;

        &-item {
            padding: 8px 0;
            margin: 0 16px;
            cursor: pointer;

            &:first-child {
                margin-left: 0;
            }

            &.selected {
                color: $blue;
            }
        }

        &-indicator {
            position: absolute;
            height: 3px;
            background: $blue;
            left: 0;
            bottom: -1px;
            width: 100px;
            transition: all 250ms;
        }

    }

    &-content {
        padding: 8px 0;

        &-item {
            display: none;

            &.selected {
                display: block;
            }
        }
    }
}
</style>
