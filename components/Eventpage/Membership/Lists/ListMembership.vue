<template>
	<div class="container">
		<b-row class="justify-content-start">
			<b-col md="6" sm="12">
				<h1>Paket Membership Kamu</h1>
				<blockquote class="blockquote-footer">
					Pilih metode pembelian kelas menggunakan paket membership kamu yang telah "Aktif" kemudian lanjutkan dengan <strong>Beli Kelas</strong>.
				</blockquote>
			</b-col>
		</b-row>

		<b-row>
			<b-col
			v-for="(item, index) in listsPaket"
			md="4"
			xs="12"
			sm="12"
			class="mb-3 card__pelatihan"
			:key="item.id"
			>

			<mdb-card class="mt-2" :style="`${$device.isMobile ? 'width: 335px; margin-left: -3.5rem;border-radius: 25px' : 'border-radius: 25px'}`">
				<mdb-container class="mt-2 p-2 rounded-3">
					<h1 class="text-center font-weight-bold small" style="color: #013a51;">
						{{item.paket_nama}}
					</h1>
					<mdb-card-body>
						<h5 class="text-center font-weight-bold" style="color: #013a51">
							{{ $format(item.harga) }}
						</h5>
						<ul class="list-unstyled text-left mt-3 mb-0 small">
							<li class="my-1 mb-2">
								<b-button pill variant="white" size="md" disabled block>
									Total Pelatihan <b-badge pill variant="primary">{{total}}</b-badge>
								</b-button>
							</li>
							<li class="my-1 item-check">
								<i
								class="fa fa-check-circle"
								aria-hidden="true"
								style="color: #05a863; margin-right: 5px;"
								></i
								>Status {{item.status_value}}
							</li>
							
							<li class="my-1 item-check">
								<i
								class="fa fa-check-circle"
								aria-hidden="true"
								style="color: #05a863; margin-right: 5px;"
								></i
								>{{item.show_benefit_label}} : {{item.show_benefit_value}}
							</li>
							
							<li class="my-1 item-check">
								<i
								class="fa fa-check-circle"
								aria-hidden="true"
								style="color: #05a863; margin-right: 5px"
								></i
								>Tidak ada batasan waktu
							</li>
							<li class="my-1 item-check">
								<i
								class="fa fa-check-circle"
								aria-hidden="true"
								style="color: #05a863; margin-right: 5px"
								></i
								>Chat dengan pembimbing
							</li>
							<li
							class="my-1 item-check item-info"
							data-toggle="tooltip"
							data-placement="right"
							title=""
							tabindex="0"
							data-original-title="Gratis Domain ( .com / .net / .org / .info / .biz / .xyz / .web.id ), khusus 2 tahun kontrak"
							>
							<i
							class="fa fa-check-circle"
							aria-hidden="true"
							style="color: #05a863; margin-right: 5px"
							></i>
							Merchandise gratis dari pelatihan
						</li>
					</ul>
					
					<mdb-btn v-if="item.status_value === 'Aktif'" @click="BeliKelas(kegiatan_id, item.paket_membership_id)" block color="primary" :class="`btn rounded-pill mt-3 mb-2${
						$device.isMobile ? 'btn-sm' : 'btn-md'
					}`">
					<mdb-icon far icon="credit-card" size="lg"/> Beli Kelas
				</mdb-btn>

				<mdb-btn v-else block color="warning" disabled :class="`btn rounded-pill mt-3 mb-2${
					$device.isMobile ? 'btn-sm' : 'btn-md'
				}`">
				{{item.status_value}}
			</mdb-btn>

		</mdb-card-body>
	</mdb-container>
</mdb-card>
</b-col>
</b-row>
</div>
</template>

<script>
	export default {

		props: ['kegiatan_id', 'details'],

		data(){
			return {
				listsPaket: null,
				total: null
			}
		},

		mounted(){
			this.CheckToken(),
			this.paketMembershipUser(0),
			this.sisaTotalPelatihan()
		},
		
		methods: {
			CheckToken(){
				this.$store.dispatch('config/checkAuthLogin', 'token')
			},

			ConfigApiUrl(){
				const url = process.env.NUXT_ENV_API_URL
				this.$store.dispatch('config/storeConfigApiUrl', url)
			},
			paketMembershipUser(start){
				const url = `${this.api_url}/web/paket-membership-user/list?start=${start}`
				this.$axios.defaults.headers.common.Authorization = `Bearer ${this.token.accessToken}`
				this.$axios.get(url)
				.then(({data}) => {
					this.listsPaket = data.list_data
				})
				.catch(err => console.log(err.response))
			},

			sisaTotalPelatihan(){
				const url = `${this.api_url}/web/sisa-membership-user`
				this.$axios.defaults.headers.common.Authorization = `Bearer ${this.token.accessToken}`
				this.$axios.get(url)
				.then(({data}) => {
					console.log(data)
					this.total = data.paket.total_sisa
				})
				.catch(err => console.log(err.response))
			},

			BeliKelas(id, paket_id){
				localStorage.setItem('data-paket', JSON.stringify({paket_id: paket_id}))
				this.$router.push({
					name: 'events-id-paket',
					params: {
						id: id
					}
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