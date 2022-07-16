<template>
	<div>
		<Nuxt/>
	</div>
</template>

<script>
	export default {
		data(){
			return{
				loading: null,
				slug: '',
				profiles: [],
				style: `${this.$device.isDesktop ? 'margin-bottom: 4.5rem;' : 'margin-bottom:7rem;'}`,
				data_event_path: {
					event_id: this.$route.params.id,
					event_path: this.$route.path
				},
				status_pendaftaran: '',
				scrolled: localStorage.getItem('scroll') ? JSON.parse(localStorage.getItem('scroll')) : ''
			}
		},

		head:{
			link: [
				{ rel: 'stylesheet', href: 'https://fonts.googleapis.com/css2?family=Overpass:ital,wght@0,100;0,200;0,300;0,400;0,600;0,700;0,800;0,900;1,100;1,200;1,300;1,400;1,600;1,700;1,800;1,900&display=swap'},
				{ rel: 'stylesheet', type: 'text/css', href: '/new-dashboard/css/simplebar.css'},
				{ rel: 'stylesheet', type: 'text/css', href: '/new-dashboard/css/feather.css' },
				{ rel: 'stylesheet', type: 'text/css', href: '/new-dashboard/css/select2.css' },
				{ rel: 'stylesheet', type: 'text/css', href: '/new-dashboard/css/dropzone.css' },
				{ rel: 'stylesheet', type: 'text/css', href: '/new-dashboard/css/uppy.min.css' },
				{ rel: 'stylesheet', type: 'text/css', href: '/new-dashboard/css/jquery.steps.css' },
				{ rel: 'stylesheet', type: 'text/css', href: '/new-dashboard/css/jquery.timepicker.css' },
				{ rel: 'stylesheet', type: 'text/css', href: '/new-dashboard/css/quill.snow.css' },
				{ rel: 'stylesheet', type: 'text/css', href: '/new-dashboard/css/daterangepicker.css' },
				{ rel: 'stylesheet', type: 'text/css', href: '/new-dashboard/css/app-light.css' },
				{ rel: 'stylesheet', type: 'text/css', href: '/new-dashboard/css/app-dark.css' }
			],
			
			script: [
				{
					src: '/new-dashboard/js/jquery.min.js',
					type: 'text/javascript'
				},
				{
					src: '/new-dashboard/js/popper.min.js',
					type: 'text/javascript'
				},
				{
					src: '/new-dashboard/js/moment.min.js',
					type: 'text/javascript'
				},
				{
					src: '/new-dashboard/js/bootstrap.min.js',
					type: 'text/javascript'
				},
				{
					src: '/new-dashboard/js/simplebar.min.js',
					type: 'text/javascript'
				},
				{
					src: '/new-dashboard/js/daterangepicker.js',
					type: 'text/javascript'
				},
				{
					src: '/new-dashboard/js/jquery.stickOnScroll.js',
					type: 'text/javascript'
				},
				{
					src: '/new-dashboard/js/tinycolor-min.js',
					type: 'text/javascript'
				},
				{
					src: '/new-dashboard/js/config.js',
					type: 'text/javascript'
				},
				{
					src: '/new-dashboard/js/d3.min.js',
					type: 'text/javascript'
				},
				{
					src: '/new-dashboard/js/topojson.min.js',
					type: 'text/javascript'
				},
				{
					src: '/new-dashboard/js/datamaps.all.min.js',
					type: 'text/javascript'
				},
				{
					src: '/new-dashboard/js/datamaps-zoomto.js',
					type: 'text/javascript'
				},
				{
					src: '/new-dashboard/js/datamaps.custom.js',
					type: 'text/javascript'
				},
				{
					src: '/new-dashboard/js/Chart.min.js',
					type: 'text/javascript'
				},
				{
					src: '/new-dashboard/js/gauge.min.js',
					type: 'text/javascript'
				},
				{
					src: '/new-dashboard/js/jquery.sparkline.min.js',
					type: 'text/javascript'
				},
				{
					src: '/new-dashboard/js/apexcharts.min.js',
					type: 'text/javascript'
				},
				{
					src: '/new-dashboard/js/apexcharts.custom.js',
					type: 'text/javascript'
				},
				{
					src: '/new-dashboard/js/jquery.mask.min.js',
					type: 'text/javascript'
				},
				{
					src: '/new-dashboard/js/select2.min.js',
					type: 'text/javascript'
				},
				{
					src: '/new-dashboard/js/jquery.steps.min.js',
					type: 'text/javascript'
				},
				{
					src: '/new-dashboard/js/jquery.validate.min.js',
					type: 'text/javascript'
				},
				{
					src: '/new-dashboard/js/jquery.timepicker.js',
					type: 'text/javascript'
				},
				{
					src: '/new-dashboard/js/dropzone.min.js',
					type: 'text/javascript'
				},
				{
					src: '/new-dashboard/js/uppy.min.js',
					type: 'text/javascript'
				},
				{
					src: '/new-dashboard/js/quill.min.js',
					type: 'text/javascript'
				},
				{
					src: '/new-dashboard/js/apps.js',
					type: 'text/javascript'
				}
				
			]
		},

		beforeMount(){
			this.ConfigApiUrl(),
			this.CheckToken()
		},

		mounted(){
			this.UserProfileData(),
			this.StatusPembayaran()
		},

		methods: {
			CheckToken(){
				this.$store.dispatch('config/checkAuthLogin', 'token')
			},
			ConfigApiUrl(){
				const api_url = process.env.NUXT_ENV_API_URL
				this.$store.dispatch('config/storeConfigApiUrl', api_url)
			},
			UserProfileData(){
				if(this.token){					
					const url = `${this.api_url}/web/user`
					this.$axios.defaults.headers.common.Authorization = `Bearer ${this.token.accessToken}`
					this.$axios.get(url)
					.then(({data}) => {
						this.profiles = data.user
						this.slug = data.user.nama
					})
					.catch(err => console.log(err.response ? err.response : ''))
				}
			},
			StatusPembayaran(){
				if(this.token.accessToken){
					this.loading=true
					const url = `${this.api_url}/web/event/${this.$route.params.id}`
					this.$axios.defaults.headers.common.Authorization = `Bearer ${this.token.accessToken}`
					this.$axios.get(url)
					.then(({data}) => {
						this.status_pendaftaran = data.kegiatan.status_pendaftaran_value
					})
					.catch(err => console.log(err))
					.finally(() => {
						setTimeout(() => {
							this.loading=false
						}, 1000)
					})
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