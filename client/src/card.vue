<template lang="pug">
div
    navbar
    Chat(v-if='opentexts', @update:opentextspage="val => opentexts=val", v-bind:clickedSessionContact='clickedSessionContact', v-bind:texts='texts')
    ContactModal(v-bind:contactmodal="opencontactmodal", @update:updatecontactmodal="val => opencontactmodal = val", v-bind:contacts="contacts", 
    v-bind:clickedContact='clickedContact', @update:clickedContact="val => clickedContact = val")
    .card(v-if='!opentexts')
        header
            img(width='40', height='40', src='https://homepages.cae.wisc.edu/~ece533/images/airplane.png', style='float: left;')
            p.name(style='float: left;') Fadi
            v-spacer    
            v-btn(fab, dark, success, v-on:click.stop='OpenContactModal()', style='float: right;')
                v-icon fa-comments
        footer
            input.search(type='text', placeholder='Search...')
        v-list(two-line='')
          template(v-for='(item, index) in sessions')
            v-divider(v-if='index > 0', v-bind:inset='true')
            v-list-tile(avatar='', v-on:click.stop='OpenSession(item)')
              v-list-tile-avatar
                img(v-bind:src='item.avatar')
              v-list-tile-content
                v-list-tile-title {{GetContactName(item)}}
                v-list-tile-sub-title {{item.subtitle}}
        //- .list
        //-     div(v-for='item in sessions', v-on:click='OpenSession(item)')
        //-         img.avatar(width='30', height='30')
        //-         p.name {{GetContactName(item)}}
</template>

<script>
import Vue from 'vue';
import VueSocketIO from 'vue-socket.io';
import Chat from './chat.vue';
import ContactModal from './contactModal.vue';
import Navbar from './navbar.vue';
Vue.use(VueSocketIO, 'http://localhost');

export default {
    created(){
        console.log(this.$store.getters.getUser._id);
        console.log(this.$store.getters.getUser.email);
        this.$socket.emit('GET_CONTACTS', {user: this.$store.getters.getUser.email})
        this.$socket.emit('GET_SESSIONS', {user: this.$store.getters.getUser.email})
    },
    components: {
        Chat,
        ContactModal,
        Navbar
    },
    data () {
        return {
            contacts: [],
            opentexts: false,
            opencontactmodal: false,
            clickedContact: {email: ''},
            clickedSessionContact: {email: ''},
            showcontacts: false,
            sessions: [],
            texts: []
        }
    },
    methods: {
        CreateSession: function(){
            this.$socket.emit('CREATE_SESSION', {user1: this.$store.getters.getUser.email, user2: this.clickedContact.email});
        },
        GetContactName: function(val){
            if(val.user1 === this.$store.getters.getUser.email)
                return val.user2;
            return this.$store.getters.getUser.email;
        },
        OpenContactModal: function(){
            this.opencontactmodal = true;
        },
        OpenSession: function(val){
            this.clickedSessionContact = val;
            this.$socket.emit('GET_SESSION_DATA', {sessionid: val.sessionid});
        }
    },
    sockets:{
        connect: function(){
          console.log('socket connected');
        },
        GET_CONTACTS_SUCCESS: function(val){
            this.contacts = val.contacts;
        },
        GET_SESSIONS_SUCCESS: function(val){
            this.sessions = val.sessions;
        },
        CREATE_SESSION_SUCCESS: function(val){
            this.$socket.emit('GET_SESSIONS', {user: this.$store.getters.getUser.email})
        },
        GET_SESSION_DATA_SUCCESS: function(val){
            this.texts = val.texts;
            this.opentexts = true;
        }
    },
    mounted(){

    },
    watch: {
        clickedContact: function(val){
            if(val){
                this.CreateSession();
            }
        },
        opentexts: function(val){
            // if(val){
                console.log("OPEN")
                console.log(val)
            // }
        }
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
