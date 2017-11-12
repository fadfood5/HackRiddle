<template lang='pug'>
	v-layout(row='', justify-center='')
		v-dialog(v-model='contactmodal', fullscreen='', transition='dialog-bottom-transition', :overlay='false')
			v-card
				v-toolbar
					v-toolbar-title Message a Contact
					v-spacer
					v-toolbar-items
						v-btn(flat, v-on:click='CloseContactModal') X
				.container
					.row
						.col-sm-12(v-for='item in contacts') 
							v-btn(flat='', v-on:click='ChooseContact(item)') {{item.email}}
</template>

<script>
export default {
	components: {
	},
	data () {
		return {
			emailRules: [
				(v) => !!v || 'E-mail is required',
				(v) => /^\w+([\.-]?\w+)*@\w+([\.-]?\w+)*(\.\w{2,3})+$/.test(v) || 'E-mail must be valid',
			],
			tempContacts: {},
		}
	},
	methods: {
		ChooseContact: function(val){
			this.$emit('update:clickedContact', val);
			this.CloseContactModal();
		},
		CloseContactModal: function(){
			this.$emit('update:updatecontactmodal', false);
		},
	},
	sockets:{
	},
	props: ['contacts', 'contactmodal'],
	watch: {
		contactmodal: function(val){
			if(val){
				this.tempContacts = this.contact;
			}
		}
	}
}
</script>

<style lang="css">
</style>