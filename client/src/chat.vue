<template lang="pug">
div
    v-toolbar.blue(fixed='', dark='')
        v-toolbar-title
            v-btn(flat, fab, dark, v-on:click.stop='GoBack')
                v-icon fa-arrow-left
        v-spacer
        v-toolbar-title HackRiddle
    .row
        //- .head {{GetContactName(clickedSessionContact)}}
        section.chatlist
            ul
                .row(v-for='item in texts', v-on:click="item.showtranslation=!item.showtranslation")
                    li(v-if='CheckUser(item)', style='float:right;width: 90%;')
                        .chat-mine
                            .time {{item.email}}
                            .chat-text(v-if='item.showtranslation') {{item.translation}}
                            .chat-text(v-else) {{item.content}}
                    li(v-else, style='float:left;')
                        .time {{item.email}}
                            .chat-text(v-if='item.showtranslation') {{item.translation}}
                            .chat-text(v-else) {{item.content}}
        section.foot
            .container
                .col-xs-9.col-sm-9.col-md-9.col-lg-9(style='float: left;')
                    v-text-field(name='input1', label='Type message', v-model='message')
                .col-xs-3.col-sm-3.col-md-3.col-lg-3(style='float: right;')
                    v-btn(fab, dark, success, v-on:click='SendMessage()')
                        v-icon fa-comments
</template>

<script>
import Vue from 'vue';
import VueSocketIO from 'vue-socket.io';
import ContactModal from './contactModal.vue';
Vue.use(VueSocketIO, 'http://localhost');

export default {
    created(){
    },
    components: {
    },
    data () {
        return {
            message: '',
        }
    },
    methods: {
        GetContactName: function(val){
            if(val.user1 === this.$store.getters.getUser.email)
                return val.user2;
            return this.$store.getters.getUser.email;
        },
        GoBack: function(){
            this.texts = [];
            this.$emit('update:opentextspage', false);
        },
        CheckUser: function(val){
            return val.email === this.$store.getters.getUser.email;
        },
        GetOtherUser: function(val){
            if(val.user1 === this.$store.getters.getUser.lang)
                return this.$store.getters.getUser.email;
            return val.user2;
        },
        SendMessage: function(){
            this.$socket.emit('SEND_MESSAGE', {content: this.message, email: this.$store.getters.getUser.email, sessionid: this.clickedSessionContact.sessionid, language: this.GetOtherUser(this.clickedSessionContact)});
            this.message = '';
        }
    },
    sockets:{
        SEND_MESSAGE_SUCCESS: function(val){
            this.texts.push(val.content.ops[0]);
        }
    },
    mounted(){

    },
    props: ['clickedSessionContact', 'texts'],
    watch: {
    }
}
</script>

<style lang="css">
.chatlist {
        position: absolute;
        top: 60px;
        bottom: 48px;
        left: 0px;
        right: 0px;
        overflow-y: scroll;
        overflow-x: hidden;
        padding: 10px;
    }
    
    .chatlist-bottom {
        bottom: 48px;
    }
    
    .chatlist-bottom-collapse {
        bottom: 198px;
    }
    
    .chatlist ul {
        min-height: 300px;
        list-style: none;
    }
    
    .chatlist ul .chat-mine {
        text-align: right;
        padding-left: 0;
        /*padding-right: 60px;*/
    }
    
    .chatlist ul li {
        position: relative;
        /*font-size: 0;*/
        margin-bottom: 10px;
        /*padding-left: 60px;*/
        min-height: 68px;
    }
    
    .chat-mine .chat-user {
        left: auto;
        right: 3px;
    }
    
    .chat-user {
        position: absolute;
        left: 3px;
    }
    
    .chat-text,
    .chat-user {
        display: inline-block;
        vertical-align: top;
        /*font-size: 14px;*/
    }
    
    .chat-user img {
        width: 40px;
        height: 40px;
        border-radius: 100%;
    }
    
    .time {
        width: 100%;
    }
    
    cite {
        left: auto;
        right: 60px;
        text-align: right;
    }
    
    cite {
        font-style: normal;
        line-height: 24px;
        font-size: 12px;
        white-space: nowrap;
        color: #999;
        text-align: left;
    }
    
    cite i {
        font-style: normal;
        padding-left: 5px;
        padding-right: 5px;
        font-size: 12px;
    }
    
    .chat-mine .chat-text {
        margin-left: 0;
        text-align: left;
        background-color: #33DF83;
        color: #fff;
    }
    
    .chat-text {
        position: relative;
        line-height: 22px;
        /*margin-top: 25px;*/
        padding: 10px 15px;
        background-color: #eee;
        border-radius: 3px;
        color: #333;
        word-break: break-all;
        max-width: 462px\9;
    }
    
    .chat-text,
    .chat-user {
        display: inline-block;
        vertical-align: top;
        font-size: 14px;
    }
    
    .chat-text img {
        max-width: 100%;
        vertical-align: middle;
    }
    
    .chat-user {
        position: absolute;
        left: 3px;
    }
    
    .chat-text:after {
        content: '';
        position: absolute;
        left: -10px;
        top: 15px;
        width: 0;
        height: 0;
        border-style: solid dashed dashed;
        border-color: #eee transparent transparent;
        overflow: hidden;
        border-width: 10px;
    }
    
    .chat-mine .chat-text:after {
        left: auto;
        right: -10px;
        border-top-color: #33DF83;
    }
    
    .foot {
        width: 100%;
        min-height: 48px;
        position: fixed;
        bottom: 0px;
        left: 0px;
        background-color: #F8F8F8;
    }
    
    .foot .con {
        position: absolute;
        left: 66px;
        right: 40px;
    }
    
    .foot .btn-plus {
        width: 28px;
        padding: 9px 3px;
        position: absolute;
        left: 0px;
        border-left: 1px solid #d9d9d9;
    }
    
    .foot .btn-plus i {
        font-size: 2em;
        color: #ccc;
    }
    
    .foot .btn-face {
        width: 28px;
        padding: 9px 3px 9px 0px;
        position: absolute;
        left: 35px;
        /*border-right: 1px solid #d9d9d9;*/
    }
    
    .foot .btn-face i {
        font-size: 2em;
        color: #d9d9d9;
    }
    
    .foot .selbox {
        height: 150px;
        margin-top: 48px;
        border-top: 1px solid #d9d9d9;
    }
    
    .show {
        display: block;
    }
    
    .hide {
        display: none;
    }
    
    .face-box {
        /* position: absolute; */
        /* top: 48px; */
        /* left: 0px; */
        /* right: 0px; */
        /* bottom: 0px; */
        padding: 15px 15px 0px 15px;
        overflow: hidden;
        width: 290px;
        margin: 0px auto;
        height: 135px;
    }
    
    .face-box li {
        width: 30px;
        float: left;
        padding: 6px 10px 0px 8px;
    }
    
    .btn {
        display: inline-block;
        vertical-align: top;
        font-size: 14px;
        line-height: 32px;
        margin-left: 5px;
        padding: 0 6px;
        background-color: #33DF83;
        color: #fff;
        border-radius: 3px;
    }
    
    .btn-send {
        position: absolute;
        right: 0px;
        top: 8px;
    }
</style>
