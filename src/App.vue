<template>
<div>
    导航
    | <router-link to="/">Frank</router-link>
    | <router-link to="/xxx">Frank2</router-link>
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
