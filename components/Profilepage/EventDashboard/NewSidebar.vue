<template>
	<div>
		<header id="main-header">
			<nav id="navbar">
				<img :src="details.photo" class="img-fluid img-thumbnail" width="250">
				<h5>
					{{ details.kegiatan_title }}
				</h5>
				<b-badge variant="success">
					Status : {{details.status_pendaftaran_value}} <mdb-icon icon="check" color="white" size="sm"/>
				</b-badge>
				<ul class="mt-5">
					<li class="nav-link">
						<a href="#introduction">Introduction</a>
					</li>
					<li class="nav-link">
						<a href="#what-you">What you should already know</a>
					</li>
					<li class="nav-link"><a href="#javaScript-and-java">JavaScript and Java</a></li>
					<li class="nav-link"><a href="#hello-world">Hello world</a></li>
					<li class="nav-link"><a href="#variables">Variables</a></li>
					<li class="nav-link"><a href="#declaring-variables">Declaring variables</a></li>
					<li class="nav-link"><a href="#variable-scope">Variable scope</a></li>
					<li class="nav-link"><a href="#global-variables">Global variables</a></li>
					<li class="nav-link"><a href="#constants">Constants</a></li>
					<li class="nav-link"><a href="#data-types">Data types</a></li>
					<li class="nav-link"><a href="#if-else">if...else statement</a></li>
					<li class="nav-link"><a href="#while-statement">while statement</a></li>
					<li class="nav-link"><a href="#function-declarations">Function declarations</a></li>
					<li class="nav-link"><a href="#reference">Reference</a></li>
				</ul>	
			</nav>
		</header>
	</div>
</template>


<script>
	export default {
		props: ['api_url', 'token', 'event_id'],
		data(){
			return {
				loading: null,
				details: [],
				pelatihans: [],
				categories: [],
				status_pendaftaran: null,
				pelatihan_id: null,
				kegiatan_id: null
			}
		},

		mounted(){
			this.DetailEventProfileLogin(),
			this.StatusPembayaran()
		},

		methods: {
			DetailEventProfileLogin() {
				if (this.token.accessToken) {
					const url = `${this.api_url}/web/event/${this.$route.params.id}`;
					this.$axios.defaults.headers.common.Authorization = `Bearer ${this.token.accessToken}`;
					this.$axios
					.get(url)
					.then(({ data }) => {
						if(data.kegiatan.status_pendaftaran_value === "Terdaftar"){
							this.details = data.kegiatan;    
						}else{
							this.$swal({
								icon: 'error',
								title: 'Oops...',
								text: 'Something went wrong!',
								footer: '<a href="">Anda tidak bisa mengakses kelas ini !</a>'
							})
							setTimeout(() => {
								this.$router.push({
									name: 'profile-name',
									params: {
										name: this.username
									}
								})
							}, 1500)
						}
					})
					.catch((err) => console.log(err));
				}
			},

			StatusPembayaran() {
				if (this.token.accessToken) {
					this.loading = true;
					const url = `${this.api_url}/web/event/${this.$route.params.id}`;
					this.$axios.defaults.headers.common.Authorization = `Bearer ${this.token.accessToken}`;
					this.$axios
					.get(url)
					.then(({ data }) => {
						this.status_pendaftaran = data.kegiatan.status_pendaftaran_value;
					})
					.catch((err) => console.log(err))
					.finally(() => {
						setTimeout(() => {
							this.loading = false;
						}, 1000);
					});
				}
			},
		}

	}
</script>