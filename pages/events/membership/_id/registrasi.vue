<template>
	<div :class="`${$device.isDesktop ? 'event__pembayaran mb-5' : 'event__pembayaran mb-5'}`">
		<mdb-container>
			<EventpageMembershipHeader/>

			<EventpageMembershipDaftar/>
		</mdb-container>
	</div>
</template>

<script>
	export default{
		name: 'events-membership-id-registrasi',
		layout: 'profile',

		data(){
			return {
				data_paket: {
					paket_id: this.$route.params.id,
					paket_nama: this.$route.params.slug
				}
			}
		},

		beforeMount(){
			this.CheckToken()
		},

		mounted(){
			this.savePaketToStorage(),
			this.checkLogin()
		},

		methods: {
			savePaketToStorage(){
				const data = JSON.stringify(this.data_paket)
				localStorage.setItem('data-paket', data)
			},

			CheckToken(){
				this.$store.dispatch('config/checkAuthLogin', 'token')
			},

			checkLogin(){
				if(!this.token.accessToken){
					this.Alert("error", "Login terlebih dahulu")
					setTimeout(() => {
						this.$router.push({
							name: 'auth-login'
						})
					}, 1500)
				}
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

		computed:{
			token(){
				return this.$store.getters['config/ConfigCheckLogin']
			}
		}

	}
</script>