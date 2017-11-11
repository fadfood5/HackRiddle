<template lang="pug">
    .row.container
        .col-xs-12.col-sm-12.col-md-12.col-lg-8.offset-lg-2
            v-form(v-model='valid')
                v-text-field(label='E-mail', v-model='email', :rules='emailRules', required='')
                v-text-field(label='Password', v-model='password', :rules='passwordRules', :append-icon="passBool ? 'visibility' : 'visibility_off'", :append-icon-cb='() => (passBool = !passBool)', :type="passBool ? 'password' : 'text'", required='')
                v-spacer
                v-btn(success, @click.native='Login', :loading='loading', :disabled='loading') Login


</template>

<script>
import Vue from 'vue';
import Axios from 'axios'
export default {
    components: {
    },
    data () {
        return {
            valid: false,
            email: 'test@test.com',
            emailRules: [
                (v) => /^\w+([\.-]?\w+)*@\w+([\.-]?\w+)*(\.\w{2,3})+$/.test(v) || 'E-mail must be valid.'
            ],
            loading: false,
            password: '123',
            passBool: true,
            passwordRules: [
              (v) => !!v || 'Password is required.'
            ],
        }
    },
    methods: {
        Login: function(){
            console.log(this.mail, this.pass)
            this.loading = true;
            var that = this;
            Axios.post('/login', {email: this.email, password: this.password}).then(function (response) {
            if(response.data){
                that.$store.commit('UpdateUser', response.data);
            }
              }).catch(function (error) {
                console.log(error);
              });
        }
    },
    sockets:{
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