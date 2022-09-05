<template>
	<div>
		<mdb-row class="row justify-content-start">
			<mdb-col col="12" lg="8" xs="12" sm="12" class="pembayaran__header">
				<h2> {{data_paket.status_daftar}} </h2>
				<div class="alert alert-info" role="alert">
					Pembayaran untuk paket membership {{data_paket.paket_nama}} anda telah berhasil, untuk selanjutnya menunggu konfirmasi oleh admin kami.
				</div>
			</mdb-col>
		</mdb-row>
	</div>
</template>

<script>
	export default {
		props: ['api_url', 'token'],
		data(){
			return {
				data_paket: {}
			}
		},

		mounted(){
			this.statusPembayaran()
		},

		methods: {
			statusPembayaran(){
				const url = `${this.api_url}/web/paket-membership-user/${this.$route.params.id}`
				this.$axios.defaults.headers.common.Authorization = `Bearer ${this.token.accessToken}`
				this.$axios.get(url)
				.then(({data}) => {
					console.log(data)
					this.data_paket = data.paket
				})
				.catch(err => console.error(err))
			}
		}

	}
</script>