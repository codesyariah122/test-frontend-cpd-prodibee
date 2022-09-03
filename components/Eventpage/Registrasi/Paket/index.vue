<template>
	<div>
		<mdb-row class="justify-content-start pembayaran__card">
			<mdb-col col="12"  lg="8" xs="10" sm="12" class="mb-3">
				<b-card no-body class="overflow-hidden shadow-none">
					<b-row v-if="$device.isDesktop" no-gutters class="mt-2 row justify-content-start ml-2 rincian__event-table">
						<h5>Ringkasan Belanja</h5>
						<div class="table-responsive">
							<table class="table table-borderless">
								<thead>
									<tr>
										<th scope="row">
											<b>Kelas</b>
										</th>
										<th scope="row">
											<b>Tanggal</b>
										</th>
										<th scope="row">
											<b>Subtotal</b>
										</th>
									</tr>
								</thead>
								<tbody>
									<tr>
										<td>
											{{event.kegiatan_title}}
										</td>
										<td>
											{{$moment(details.tanggal_awal).format("LLLL")}} - {{$moment(details.tanggal_akhir).format("LLLL")}}
										</td>
										<td>
											{{$format(event.harga)}}
										</td>
									</tr>
								</tbody>
							</table>
						</div>
						<b-col md="12" xs="12" sm="12">
							<div class="dropdown-divider"></div>
						</b-col>
						<b-col md="5" xs="12" sm="12">
							<b-card title="Total Harga" class="shadow-none rincian__bayar">
								<b-card-text>
									<h2> {{$format(event.harga)}} </h2>
								</b-card-text>
							</b-card>
						</b-col>
					</b-row>

					<b-row v-else no-gutters class="mt-2 row justify-content-start rincian__event-table">
						<h5>Ringkasan Belanja</h5>
						<table class="table table-borderless">
							<thead>
								<tr>
									<th scope="row">
										<b>Kelas</b>
									</th>
									<th scope="row">
										<b>Tanggal</b>
									</th>
									<th scope="row">
										<b>Subtotal</b>
									</th>
								</tr>
							</thead>
							<tbody>
								<tr>
									<td>
										{{event.kegiatan_title}}
									</td>
									<td>
										{{$moment(details.tanggal_awal).format("LLLL")}} - {{$moment(details.tanggal_akhir).format("LLLL")}}
									</td>
									<td>
										{{$format(event.harga)}}
									</td>
								</tr>
							</tbody>
						</table>
						<b-col md="12" xs="12" sm="12">
							<div class="dropdown-divider"></div>
						</b-col>
						<b-col md="5" xs="12" sm="12">
							<b-card title="Total Harga" class="shadow-none rincian__bayar">
								<b-card-text>
									<h2> {{$format(event.harga)}} </h2>
								</b-card-text>
							</b-card>
						</b-col>
					</b-row>

				</b-card>
			</mdb-col>
		</mdb-row>

		<mdb-row :class="`${$device.isDesktop ?  'row justify-content-end check__point' : 'row justify-content-end check__point mt-3'}`">
			<mdb-col col="12" lg="4" sm="12">
				<b-card title="Beli Pelatihan" class="shadow-none list__bank">
					
					<b-card-text>
						<div class="input__bank">
							<mdb-row v-if="error" class="row justify-content-center">
								<mdb-col md="12" class="mb-2">
									<mdb-alert v-if="validation" color="danger">
										{{validation.message}}
									</mdb-alert>
								</mdb-col>
							</mdb-row>
							<mdb-row class="row justify-content-center mb-5">
							
								<mdb-col md="12" class="mt-2 mb-2">
									<blockquote class="blockquote-footer">
										Pembelian melalui paket membership yang telah dipilih, yaitu paket membership <strong>{{data_paket.paket_nama}}</strong> 
									</blockquote>
								</mdb-col>
								<mdb-col md="12">
									<b-button pill variant="white" size="md" disabled block>
										Total Pelatihan <b-badge pill variant="primary">{{total_pelatihan}}</b-badge>
									</b-button>
								</mdb-col>
							</mdb-row>
							<form @submit.prevent="RegisterEvent">
								<b-container>
									<b-form-radio class="mb-2" name="kegiatan_id" :value="event.kegiatan_id" v-model="field.kegiatan_id" checked>
										Paket {{data_paket.paket_nama}}
									</b-form-radio>
									<div class="dropdown-divider mt-2"></div>
								</b-container>
								<b-btn type="submit" class="mt-2 btn btn-block btn-lg rounded-pill btn-white shadow-none">
									<div v-if="loading_btn">
										<span class="spinner-border spinner-border-sm" role="status" aria-hidden="true"></span>
										Loading...
									</div>
									<div v-else>
										<mdb-icon far icon="credit-card" size="lg" /> Lanjut Pembelian
									</div>
								</b-btn>
							</form>
						</div>
					</b-card-text>
					
				</b-card>
			</mdb-col>
		</mdb-row>
	</div>
</template>


<script>
	export default{
		props: ['id', 'paket_id', 'token', 'api_url', 'details'],

		data(){
			return {
				loading: null,
				loading_btn:null,
				event: {},
				banks:[],
				field: {},
				error: null,
				validation: {},
				data_paket: {},
				total_pelatihan: null
			}
		},

		mounted(){
			this.EventData(),
			this.paketMembershipUser(),
			this.sisaTotalPelatihan()
		},

		methods: {
			CheckEvent(){
				const url = `${this.api_url}/web/event/${this.$route.params.id}/daftar`
				this.$axios.defaults.headers.common.Authorization = `Bearer ${this.token.accessToken}`
				this.$axios.get(url)
				.then(({data}) => {
					console.log(data)
				})
				.catch(err => console.log(err.message))
			},

			EventData(){
				this.loading = true
				this.$axios.get(`/web/event/${this.id}/daftar`)
				.then(({data}) => {
					console.log(data)
					this.event = data.kegiatan
					this.banks = data.list_bank
				})
				.catch(err => console.log(err))
				.finally(() => {
					this.loading = false
				})
			},
			CheckPembayaran(id){
				this.$axios.defaults.headers.common.Authorization = `Bearer ${this.token.accessToken}`
				this.$axios.get(`/web/event/${id}/konfirmasi`)
				.then(({data}) => {
					console.log(data)
					if(data.kegiatan){
						this.$router.push({
							name: 'events-id-paket-konfirmasi',
							params: {
								id: id
							}
						})
					}
				})
				.catch(err => console.log(err))

			},

			RegisterEvent(){
				this.error = false
				this.validation.message=""
				this.loading_btn = true
				if(this.field.kegiatan_id){
					const url = `${this.api_url}/web/kegiatan/pelatihan/daftar-paket`

					this.$axios.defaults.headers.common.Authorization = `Bearer ${this.token.accessToken}`
					this.$axios.post(url, {
						kegiatan_id: this.field.kegiatan_id
					})
					.then(({data}) => {
						console.log(data)
						let new_message = ''
						if(data.kegiatan_peserta.kegiatan_id){
							if(data.message === "Anda telah terdaftar pada event ini" || data.message === ""){
								new_message = "Terima kasih telah mendaftar, segera lakukan pembayaran"	
							}else{
								new_message = data.message
							}
							this.Alert('success', new_message)
					
							this.CheckPembayaran(data.kegiatan_peserta.kegiatan_id)
						}
					})
					.catch(err => {
						console.log(err)
					})
					.finally(() => {
						setTimeout(() => {
							this.loading_btn = false
						}, 1000)
					})
				}else{					
					this.error = true
					this.validation.message = "Harap checklist pada bagian input yang tersedia"
					setTimeout(() => {
						this.loading_btn=false
					}, 1000)

				}
			},

			paketMembershipUser(){
				const url = `${this.api_url}/web/paket-membership/${this.paket_id.paket_id}`
				this.$axios.defaults.headers.common.Authorization = `Bearer ${this.token.accessToken}`
				this.$axios.get(url)
				.then(({data}) => {
					console.log(data)
					this.data_paket = data.paket
				})
				.catch(err => console.log(err.response))
			},

			sisaTotalPelatihan(){
				const url = `${this.api_url}/web/sisa-membership-user`
				this.$axios.defaults.headers.common.Authorization = `Bearer ${this.token.accessToken}`
				this.$axios.get(url)
				.then(({data}) => {
					console.log(data)
					this.total_pelatihan = data.paket.total_sisa
				})
				.catch(err => console.log(err.response))
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

		}
	}
</script>
