<template>
	<div :class="`${$device.isDesktop ? 'event__pembayaran mb-5' : 'event__pembayaran mb-5'}`">
		<mdb-container>
			<EventpageMembershipKonfirmasiHeader/>
			
			<EventpageMembershipKonfirmasi/>
		</mdb-container>
	</div>
</template>
<script>
	export default {
		name: 'events-membership-id-konfirmasi',
		layout: 'profile',
		
		beforeMount(){
			this.ConfigApiUrl(),
			this.CheckToken()
		},

		mounted(){

		},

		methods:{
			CheckToken(){
				this.$store.dispatch('config/checkAuthLogin', 'token')
			},
			ConfigApiUrl(){
				const api_url = process.env.NUXT_ENV_API_URL
				this.$store.dispatch('config/storeConfigApiUrl', api_url)
			},
			Alert(status, data){
				switch(status){
					case 'error':
					return this.$swal({
						icon: status,
						title: 'Oops...',
						text: data,
					})
					break;
					case 'success':
					return this.$swal({
						position: 'top-end',
						icon: status,
						title: data,
						showConfirmButton: false,
						timer: 1500
					})
					break;
				}
			}
		},
		computed: {
			token(){
				return this.$store.getters['config/ConfigCheckLogin']
			},
			api_url(){
				return this.$store.getters['config/ConfigApiUrl']
			}
		}
	}
</script>