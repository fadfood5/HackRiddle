<template lang="pug">
    div
        card
</template>

<script>
import Vue from 'vue';
import VueSocketIO from 'vue-socket.io'
Vue.use(VueSocketIO, 'http://localhost');
import Card from './card.vue';

export default {
    components: {
        Card
    },
    data () {
        return {
        }
    },
    methods: {
    },
    sockets:{
        connect: function(){
          console.log('socket connected');
        }
    },
    mounted(){

    }
}
</script>

<style lang="css">
.marginTop{
    margin-top: 70px;
}
#filters{
    margin-top: 30px;
}
</style>
