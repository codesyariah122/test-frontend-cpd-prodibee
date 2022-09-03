<template>
	<div>
		<mdb-row class="justify-content-start pembayaran__card">
			<mdb-col col="12"  lg="8" xs="10" sm="12" class="mb-3">
				
				<b-card no-body class="overflow-hidden shadow-none">
					<b-row v-if="$device.isDesktop" no-gutters class="mt-2 row justify-content-start ml-2 rincian__event-table">
						<h5 style="margin-left:.5rem;">Ringkasan Belanja Paket Membership</h5>
						<div class="table-responsive">
							<table class="table table-borderless">
								<thead>
									<tr>
										<th scope="row">
											<b>Nama Paket</b>
										</th>
										<th scope="row">
											<b>Subtotal</b>
										</th>
									</tr>
								</thead>
								<tbody>
									<tr>
										<td>
											{{paket.paket_nama}}
										</td>
										
										<td>
											{{$format(paket.harga)}}
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
									<h2> {{$format(paket.harga)}} </h2>
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
										<b>Nama Paket</b>
									</th>
									<th scope="row">
										<b>Subtotal</b>
									</th>
								</tr>
							</thead>
							<tbody>
								<tr>
									<td>
										{{paket.paket_nama}}
									</td>

									<td>
										{{$format(paket.harga)}}
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
									<h2>{{$format(paket.harga)}}</h2>
								</b-card-text>
							</b-card>
						</b-col>
					</b-row>

				</b-card>
			</mdb-col>
		</mdb-row>

		<mdb-row  :class="`${$device.isDesktop ?  'row justify-content-end check__point' : 'row justify-content-center check__point'}`">
			<mdb-col col="12" lg="4" sm="12">
				<b-card class="shadow-none list__bank">
					<b-card-text>
						<div class="input__bank">
							<mdb-row v-if="error" class="row justify-content-center">
								<mdb-col md="12" class="mb-2">
									<mdb-alert v-if="validation" color="danger">
										{{validation.message}}
									</mdb-alert>
								</mdb-col>
							</mdb-row>
							<b-container>
								<div v-if="parseInt(bank.id) == 1">
									
									<b-card no-body class="shadow-none overflow-hidden card__bank-list">
										<b-row no-gutters>
											<b-col md="4" class="mt-5">
												<b-card-img :src="require('~/assets/images/bank/logo-bank-bca.svg')" alt="Image" class="rounded-0"></b-card-img>
											</b-col>
											<b-col md="8">
												<b-card-body title="Transfer Bank">
													<b-card-text>
														<h6 class="mt-2">Bank {{bank.nama}} </h6>
														<p> a.n : <span class="text-capitalize">{{bank.nama_rek}}</span> </p>

														<h5>
															{{bank.no_rek}}
														</h5>
													</b-card-text>
												</b-card-body>
											</b-col>
										</b-row>
									</b-card>
								</div>
								<div v-else>
									<b-card no-body class="shadow-none overflow-hidden card__bank-list">
										<b-row no-gutters>
											<b-col md="4" class="mt-5">
												<b-card-img :src="require('~/assets/images/bank/logo-bank-mandiri.svg')" alt="Image" class="rounded-0"></b-card-img>
											</b-col>
											<b-col md="8">
												<b-card-body title="Transfer Bank">
													<b-card-text>
														<h6 class="mt-2">Bank {{bank.nama}} </h6>
														<p> a.n : <span class="text-capitalize">{{bank.nama_rek}}</span> </p>

														<h5>
															{{bank.no_rek}}
														</h5>
													</b-card-text>
												</b-card-body>
											</b-col>
										</b-row>
									</b-card>
								</div>
								<div v-else>
									<small>No Bank History</small>
								</div>
								<div class="dropdown-divider mt-2"></div>
							</b-container>
						</div>
						<mdb-col lg="12" xs="12" sm="12" class="col__card-upload-file mt-2 mb-2">
							
							<form>
								<div v-if="photo" class="form-group">
									<mdb-row class="row justify-content-center">
										<mdb-col md="12" class="mt-2 mb-2">
											<b-badge pill variant="primary">Bukti Bayar Anda</b-badge>
										</mdb-col>
										<mdb-col md="12" class="mb-2">
											<img :src="photo" width="150" class="img-fluid">
										</mdb-col>
										<mdb-col md="12" class="mb-4">
											<div class="file-input-2">
												<input type="file" id="file" class="rounded-pill file" @change="FileImage($event)">
												<label for="file">
													Ubah Bukti Pembayaran
													<p class="file-name"></p>
												</label>
											</div>
										</mdb-col>
										<mdb-col md="12">
											<mdb-btn @click="LanjutPembayaran" class="btn my__btn-primary rounded-pill btn-block shadow-none">Lanjutkan Pendaftaran</mdb-btn>
										</mdb-col>

									</mdb-row>
								</div>

								<div v-else class="form-group mt-3">
									<div v-if="status_pembayaran" class="form-group">
										<mdb-alert color="info">
											{{new_message}}
										</mdb-alert>
									</div>
									<div class="file-input">
										<input type="file" id="file" class="rounded-pill file" @change="FileImage($event)">
										<label for="file">
											Unggah Bukti Pembayaran
											<p class="file-name"></p>
										</label>
									</div>
								</div>

							</form>
						</mdb-col>
					</b-card-text>
					
				</b-card>
			</mdb-col>
		</mdb-row>
	</div>
</template>


<script>
	export default{
		data(){
			return {
				loading: null,
				loading_btn:null,
				photo:null,
				paket: {},
				banks:[],
				bank: {},
				bank_id: null,
				field: {},
				error: null,
				validation: {},
				form_data:{},
				pembayaran: {
					bank: {},
					kegiatan: {}
				},
				status_pembayaran: '',
				data_paket: {},
				new_preview: null
			}
		},

		beforeMount(){
			this.CheckToken(),
			this.ConfigApiUrl()
		},

		mounted(){
			this.paketMembershipUser(),
			this.bankList()
		},

		methods: {
			CheckToken(){
				this.$store.dispatch('config/checkAuthLogin', 'token')
			},

			ConfigApiUrl(){
				const url = process.env.NUXT_ENV_API_URL
				this.$store.dispatch('config/storeConfigApiUrl', url)
			},

			paketMembershipUser(){
				if(this.token){
					const data_paket = JSON.parse(localStorage.getItem('paket-membership-user'))
					console.log(data_paket)
					this.bank_id = data_paket.bank_id
					this.data_paket = data_paket
					const url = `${this.api_url}/web/paket-membership-user/${JSON.stringify(data_paket.id)}`
					this.$axios.defaults.headers.common.Authorization = `Bearer ${this.token.accessToken}`
					this.$axios.get(url)
					.then(({data}) => {
						this.paket = data.paket
					})
					.catch(err => console.error(err.response))
				}
			},

			bankList(){
				const url = `${this.api_url}/web/pembayaran/list`
				this.$axios.get(url)
				.then(({data}) => {
					this.banks = data.list_data
					const banks = data.list_data.map(d => d)
					this.bank = banks.filter(d => d.id === this.bank_id)[0]
				})
				.catch(err => console.error(err.response))
			},

			FileImage(e){
				this.field.photo = e.target.files[0]
				this.field.preview = URL.createObjectURL(e.target.files[0])
				let formData = new FormData()
				formData.append("photo", this.field.photo)
				this.form_data = formData
				this.photo = URL.createObjectURL(e.target.files[0])
				console.log(this.photo)
			},

			LanjutPembayaran(){
				const config = {
					headers: {'content-type' : 'multipart/form-data'}
				}
				this.$axios.post(`${this.api_url}/web/paket-membership/buktibayar/${this.data_paket.id}`, this.form_data, config)
				.then(({data}) => {
					console.log(data)
					this.new_preview = data.paket_membership_user.bukti_bayar

					this.total_bayar = data.paket_membership_user.total_bayar

					this.$swal({
						position: 'top-end',
						icon: 'success',
						title: "Kami akan segera memverifikasi pembayaran anda",
						showConfirmButton: false,
						timer: 1500
					})

					this.status_bayar = true

					const data_storage = {
						data: this.paket,
						message: "Proses pembayaran Anda sedang di check oleh admin kami. Anda dapat mengakses kelas yang Anda beli, Setelah pembayaran Anda di verifiksi oleh sistem kami.",
						bank: this.bank,
					}

					localStorage.setItem("success", JSON.stringify(data_storage))

					this.$router.push({
						name: 'events-membership-id-success',
						params: {
							id: data.paket_membership_user.paket_membership_user_id,
							check: false,
							data_storage: data_storage
						}
					})
				})
				.catch(err => console.log(err))
				.finally(() => {
					console.log("Ok")
				})
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
			},
			api_url(){
				return this.$store.getters['config/ConfigApiUrl']
			}
		}
	}
</script>
