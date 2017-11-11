<template lang="pug">
    div
        v-toolbar.blue(fixed='', dark='')
          v-toolbar-title HackRiddle
</template>

<script>
export default {
    components: {
    },
    data (){
        return{
            drawer: false
        }
    },
    methods: {
    },
    mounted(){
    }
}
</script>

<style lang="css">
nav{
    background-color: #1c84b5;
}
.nav-item{
    color: white;
}
#collapser{
    color: white;
}
</style>
