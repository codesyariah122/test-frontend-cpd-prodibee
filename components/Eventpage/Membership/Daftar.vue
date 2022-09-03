<template>
	<div>
		<mdb-row class="justify-content-start pembayaran__card">
			<mdb-col col="12"  lg="8" xs="10" sm="12" class="mb-3">
				<b-card no-body class="overflow-hidden shadow-none">
					<b-row v-if="$device.isDesktop" no-gutters class="mt-2 row justify-content-start ml-2 rincian__event-table">
						<h5 style="margin-left:.5rem;">Ringkasan Belanja</h5>
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

		<mdb-row :class="`${$device.isDesktop ?  'row justify-content-end check__point' : 'row justify-content-end check__point mt-3'}`">
			<mdb-col col="12" lg="4" sm="12">
				<b-card title="Transfer Bank" class="shadow-none list__bank">
					
					<b-card-text>
						<div class="input__bank">
							<mdb-row v-if="error" class="row justify-content-center">
								<mdb-col md="12" class="mb-2">
									<mdb-alert v-if="validation" color="danger">
										{{validation.message}}
									</mdb-alert>
								</mdb-col>
							</mdb-row>
							<form @submit.prevent="RegistrasiMembership">
								<b-container>
									<b-form-radio class="mb-2" v-for="bank in banks" :key="bank.id" name="bank" :id="`bank-${bank.id}`" :value="bank.id" v-model="field.bank_id" >
										<img :src="`${bank.nama === 'BCA' ? require('~/assets/images/bank/logo-bank-bca.svg') : require('~/assets/images/bank/logo-bank-mandiri.svg')}`" width="150" height="50"> <span>{{bank.nama}}</span>
									</b-form-radio>
									<div class="dropdown-divider mt-2"></div>
								</b-container>
								<b-btn type="submit" class="mt-2 btn btn-block btn-lg rounded-pill btn-white shadow-none">
									<div v-if="loading_btn">
										<span class="spinner-border spinner-border-sm" role="status" aria-hidden="true"></span>
										Loading...
									</div>
									<div v-else>
										<mdb-icon far icon="credit-card" size="lg" /> Bayar Pendaftaran
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
		data(){
			return {
				loading: null,
				loading_btn:null,
				paket: {},
				banks:[],
				field: {},
				error: null,
				validation: {},
			}
		},

		beforeMount(){
			this.CheckToken(),
			this.ConfigApiUrl()
		},

		mounted(){
			this.paketMembership(),
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

			paketMembership(){
				if(this.token){
					const url = `${this.api_url}/web/paket-membership/${this.$route.params.id}`
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
				})
				.catch(err => console.error(err.response))
			},

			RegistrasiMembership(){
				this.error = false
				this.validation.message=""
				this.loading_btn = true

				if(this.field.bank_id){
					const url = `${this.api_url}/web/paket-membership/daftar/${this.$route.params.id}`
					this.$axios.defaults.headers.common.Authorization = `Bearer ${this.token.accessToken}`
					this.$axios.post(url, {
						bank_id: this.field.bank_id
					})
					.then(({data}) => {
						if(data){
							localStorage.setItem('paket-membership-user', JSON.stringify(data.paket_membership_user))
							this.loading_btn = false
							setTimeout(() => {
								this.$router.push({
									name: 'events-membership-id-konfirmasi',
									params: {
										id: data.paket_membership_user.paket_membership_id
									}
								})
							}, 1500)
						}
					})
					.catch(err => console.error(err.response))
				}else{					
					this.error = true
					this.validation.message = "Harap checklist pada bagian bank yang tersedia"
					setTimeout(() => {
						this.loading_btn=false
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
			},
			api_url(){
				return this.$store.getters['config/ConfigApiUrl']
			}
		}
	}
</script>
