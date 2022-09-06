<template>
	<div :class="`${$device.isDesktop ? 'event__detail mb-5' : 'event__detail mb-5'}`">
		<mdb-container>
			<mdb-row :class="`${$device.isMobile ? 'row justify-content-center' : ''}`">
				<mdb-col v-if="$device.isMobile" lg="4" xs="4" sm="12" col="12" class="event__flyer">
					<img :src="details.photo">
				</mdb-col>
				<mdb-col lg="6" xs="6" sm="12" col="12" class="event__info">
					<mdb-badge
					class="mb-2 badge__category shadow-none"
					>{{details.kategori_value}}</mdb-badge
					>     
					<h1>
						{{details.kegiatan_title}}
					</h1>
					<h4 class="mt-3 idr__color">
						{{details.harga ? $format(details.harga) : 'Rp. -'}}
					</h4>
					<p>
						{{details.kegiatan_desc}}
					</p>
					<mdb-row col="12">
						<mdb-col md="6" sm="4">
							<h5>Waktu</h5>
							<p>
								{{$moment(details.tanggal_awal).format("LL")}} - {{$moment(details.tanggal_akhir).format("LL")}}
							</p>
						</mdb-col>
						<mdb-col md="6" sm="4">
							<h5>Nilai SKP</h5>
							<p>
								{{details.nomor_skp ? details.nomor_skp : '-'}}
							</p>
						</mdb-col>
					</mdb-row>

					<mdb-row class="mt-3" col="12">
						<mdb-col v-if="status_pendaftaran == 'Menunggu Konfirmasi'" md="12" xs="12" sm="12" class="mt-5">
							<div v-if="loading">
								<span class="spinner-border spinner-border-sm text-primary" role="status" aria-hidden="true"></span>
								Loading...
							</div>
							<div v-else>
								<mdb-btn @click="MenungguKonfirmasi(details.kegiatan_id)" class="btn my__btn-secondary rounded-pill btn-block shadow-none" :size="`${$device.isDesktop ? 'md' : 'sm'}`"> 
									<mdb-icon icon="check" :size="`${$device.isDesktop ? 'lg' : 'sm'}`"/> {{status_pendaftaran == 'Menunggu Konfirmasi' ? 'Check Status' : status_pendaftaran}}
								</mdb-btn>
							</div>
						</mdb-col>
						<mdb-col v-else md="12" xs="12" sm="12" lg="12" class="mt-2">
							<mdb-btn @click="LanjutPelatihan(details.kegiatan_id, details.kegiatan_title, username)" class="btn btn-success rounded-pill btn-block shadow-none" :size="`${$device.isDesktop ? 'lg' : 'sm'}`"> 
								{{details.status_pendaftaran_value == 'Menunggu Konfirmasi' ? 'Check Status' : details.status_pendaftaran_value == 'Terdaftar' ? 'Lanjut Pelatihan' : details.status_pendaftaran_value}} &nbsp; <mdb-icon icon="chalkboard-teacher" :size="`${$device.isDesktop ? 'lg' : 'sm'}`"/>
							</mdb-btn>
						</mdb-col>
					</mdb-row>
				</mdb-col>

				<mdb-col v-if="$device.isDesktop" lg="4" xs="4" sm="12" class="event__flyer">
					<img :src="details.photo">
				</mdb-col>
			</mdb-row>
		</mdb-container>
	</div>
</template>
<script>
	export default {
		name: 'events-id-konfirmasi',
		layout: 'profile',
		data(){
			return {
				loading: null,
				id: this.$route.params.id,
				kegiatan: {},
				details: {},
				username: null
			}
		},
		beforeMount(){
			this.ConfigApiUrl(),
			this.CheckToken(),
			this.UserProfileData()
		},
		mounted(){
			this.CheckPembayaran(),
			this.IsLoggedIn(),
			this.DetailEventProfileLogin()
		},
		methods:{
			LanjutPelatihan(id, slug, username){
				this.$router.push({
					name: 'profile-name-events-id-slug',
					params: {
						name: username,
						id: id,
						slug: slug
					}
				})
			},

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
				const api_url = process.env.NUXT_ENV_API_URL
				this.$store.dispatch('config/storeConfigApiUrl', api_url)
			},
			CheckPembayaran(){
				this.loading = true
				const url = `${this.api_url}/web/event/${this.id}/konfirmasi`
				this.$axios.defaults.headers.common.Authorization = `Bearer ${this.token.accessToken}`
				this.$axios.get(url)
				.then(({data}) => {
					this.kegiatan = data.kegiatan
				})
				.catch(err => console.log(err))
				.finally(() => {
					this.loading = false
				})
			},
			DetailEventProfileLogin(){
				if(this.token.accessToken){
					const url = `${this.api_url}/web/event/${this.$route.params.id}`
					this.$axios.defaults.headers.common.Authorization = `Bearer ${this.token.accessToken}`
					this.$axios.get(url)
					.then(({data}) => {
						this.details = data.kegiatan
					})
					.catch(err => console.log(err))
				}
			},
			UserProfileData() {
				if (this.token) {
					this.loading = true;
					const url = `${this.api_url}/web/user`;
					this.$axios.defaults.headers.common.Authorization = `Bearer ${this.token.accessToken}`;
					this.$axios
					.get(url)
					.then(({ data }) => {
						this.username = this.$username(data.user.nama);
					})
					.catch((err) => console.log(err.response ? err.response : ""))
					.finally(() => {
						setTimeout(() => {
							this.loading = false;
						}, 1000);
					});
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