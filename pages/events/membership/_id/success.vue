<template>
	<div :class="`${$device.isDesktop ? 'event__pembayaran mb-5' : 'event__pembayaran mb-5'}`">
		<mdb-container>
			<EventpageMembershipSuccessHeader :api_url="api_url" :token="token"/>
			<EventpageMembershipSuccess :api_url="api_url" :token="token"/>
		</mdb-container>
	</div>
</template>

<script>
	export default {
		name: 'events-membership-id-success',
		layout: 'default',

		methods: {
			IsLoggedIn(){
				if(!this.token.accessToken){
					this.Alert('error', `Anda tidak dapat izin untuk mengakses halaman ini`)
					setTimeout(() => {
						this.$router.push({
							path: `/detail/event/${this.$route.params.id}`
						})
					}, 900)
				}
			},
			
			CheckToken(){
				this.$store.dispatch('config/checkAuthLogin', 'token')
			},

			ConfigApiUrl(){
				const url = process.env.NUXT_ENV_API_URL
				this.$store.dispatch('config/storeConfigApiUrl', url)
			},

		},

		computed: {
			token(){
				return this.$store.getters['config/ConfigCheckLogin']
			},

			api_url(){
				return this.$store.getters['config/ConfigApiUrl']
			},
		}
	}
</script>