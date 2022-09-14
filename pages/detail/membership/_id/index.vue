<template>
	<div :class="`${$device.isDesktop ? 'event__detail mb-5' : 'event__detail mb-5'}`">
		<mdb-container>
			<mdb-row v-if="loading" class="justify-content-center">
				<mdb-col lg="12" sm="12" xs="12">
					Loading ....
				</mdb-col>
			</mdb-row>		
			<pre v-else>
				{{paket}}
			</pre>
		</mdb-container>
	</div>
</template>

<script>
	export default {
		name: 'detail-membership-id',
		layout: 'default',
		data(){
			return {
				loading: null,
				paket_id: this.$route.params.id,
				paket: {}
			}
		},
		beforeMount(){
			this.ConfigApiUrl()
		},
		mounted(){
			this.DetailMembership()
		},
		methods: {
			ConfigApiUrl(){
				const api_url = process.env.NUXT_ENV_API_URL
				this.$store.dispatch('config/storeConfigApiUrl', api_url)
			},

			DetailMembership(){
				this.loading = true
				const url = `${this.api_url}/web/paket-membership-user/${this.paket_id}`
				this.$axios.get(url)
				.then(({data}) => {
					this.paket = data.paket
				})
				.catch(err => console.error(err))
				.finally(() => {
					setTimeout(() => {
						this.loading = false
					}, 1500)
				})
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