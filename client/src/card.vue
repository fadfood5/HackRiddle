<template lang="pug">
div
    .card
        header
            img.avatar(width='40', height='40', src='https://homepages.cae.wisc.edu/~ece533/images/airplane.png')
            p.name Fadi Bitar
        v-btn(fab, dark, success, v-on:click='showcontacts=true')
            v-icon fa-comments
        //- ul
        //-     li(v-for='item in contacts', v-on:click='CreateSession(item._id)') {{contacts.name}}
        .list
          ul
            li(v-for='item in sessions', @click='selectSession(item.id)')
              img.avatar(width='30', height='30', :alt='item.name')
              p.name {{item.name}}
        footer
            input.search(type='text', placeholder='Search...')
</template>

<script>
import Vue from 'vue';
import VueSocketIO from 'vue-socket.io'
Vue.use(VueSocketIO, 'http://localhost');

export default {
    created(){
        console.log(this.$store.getters.getUser._id);
        this.$socket.emit('GET_SESSIONS', {user: this.$store.getters.getUser._id})
    },
    components: {
    },
    data () {
        return {
            contacts: [],
            showcontacts: false,
            sessions: [],
        }
    },
    methods: {
        CreateSession: function(user){
            this.$socket.emit('CREATE_SESSION', {user1: this.$store.getters.getUser._id, user2: user});
        }
    },
    sockets:{
        connect: function(){
          console.log('socket connected');
        },
        GET_SESSIONS_SUCCESS: function(val){
            console.log('Sessions received');
            console.log(val);
            this.sessions = val;
        },
        CREATE_SESSION_SUCCESS: function(val){
            console.log('Session created');
            console.log(val);
        }
    },
    mounted(){

    }
}
</script>

<style lang="css">
.card{
    padding: 12px;
    border-bottom: solid 1px #24272C;
}
footer {
    margin-top: 10px;
}
.avatar, .name {
    vertical-align: middle;
}
.avatar {
    border-radius: 2px;
}
.name {
    display: inline-block;
    margin: 0 0 0 15px;
    font-size: 16px;
}
.search {
    padding: 0 10px;
    width: 100%;
    font-size: 12px;
    color: #fff;
    height: 30px;
    line-height: 30px;
    border: solid 1px #3a3a3a;
    border-radius: 4px;
    outline: none;
    background-color: #26292E;
}
</style>
