<template>
<div>
    导航
    | <router-link to="/">Home</router-link>
    | <router-link to="/doc">Doc</router-link>
</div>
<hr>
<router-view></router-view>
</template>

<script>
export default {
    name: 'App',
    components: {

    }
}
</script>
