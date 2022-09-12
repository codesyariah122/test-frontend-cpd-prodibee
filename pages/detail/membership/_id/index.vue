<template>
	<div :class="`${$device.isDesktop ? 'event__detail mb-5' : 'event__detail mb-5'}`">
		
	</div>
</template>

<script>
	export default {
		name: 'detail-membership-id',
		layout: 'default',
		data(){
			return {
				paket_id: this.$route.params.id
			}
		},
		beforeMount(){
			this.ConfigApiUrl()
		},
		methods: {
			ConfigApiUrl(){
				const api_url = process.env.NUXT_ENV_API_URL
				this.$store.dispatch('config/storeConfigApiUrl', api_url)
			},

			DetailMembership(){
				const url = `${this.api_url}/`
				this.$axios.get()
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