<template>
	<div>
		<b-row class="row justify-content-center">
			<b-col lg="12" xs="12" sm="12">
				<b-jumbotron class="shadow-none bg-gray">
					<b-container>
						<b-row>
							<b-col md="8" xs="12" sm="12">
								<h1 id="header">{{ header.title }}</h1>
							</b-col>
						</b-row>
						<b-row class="mt-3">
							<b-col md="8" xs="12" sm="12">
								<p id="lead">{{ header.context }}</p>
							</b-col>
						</b-row>
					</b-container>

					<b-container v-if="lists.length > 0" class="mt-5">
						<b-row v-if="loading" class="row justify-content-center">
							<b-col
							v-for="(item, index) in lists"
							:key="item.id"
							md="4"
							xs="12"
							sm="12"
							class="mb-5"
							>
							<b-card>
								<b-row cols="12">
									<b-col md="12" xs="12" sm="12">
										<b-skeleton-img></b-skeleton-img>
									</b-col>
									<b-col md="12" xs="12" sm="12" class="mt-2">
										<b-skeleton animation="fade" width="85%"></b-skeleton>
										<b-skeleton animation="fade" width="55%"></b-skeleton>
										<b-skeleton animation="fade" width="70%"></b-skeleton>
									</b-col>
								</b-row>
							</b-card>
						</b-col>
					</b-row>

					<b-row v-else class="row justify-content-start">
						<b-col
						v-if="listIndex <= lists.length"
						v-for="listIndex in listToShow"
						md="4"
						xs="12"
						sm="12"
						class="mb-3 card__pelatihan"
						:key="lists[listIndex - 1].id"
						>

						<mdb-card class="mt-2" :style="`${$device.isMobile ? 'width: 335px; margin-left: -3.5rem;border-radius: 25px' : 'border-radius: 25px'}`">
							<mdb-container class="mt-2 p-2 rounded-3">
								<h1 class="text-center font-weight-bold small" style="color: #013a51;">
									{{lists[listIndex-1].paket_nama}}
								</h1>
								<mdb-card-body>
									<h5 class="text-center font-weight-bold" style="color: #013a51">
										{{ $format(lists[listIndex-1].harga) }}
									</h5>
									
									<ul class="list-unstyled text-left mb-0 small">
										<li class="my-1 mb-2">
											<b-button pill variant="white" size="md" block disabled>
												Total Pelatihan <b-badge pill variant="primary">{{total}}</b-badge>
											</b-button>
										</li>
										<li class="my-1 item-check">
											<i v-if="lists[listIndex-1].status_value === 'Aktif'"
											class="fa fa-check-circle"
											aria-hidden="true"
											style="color: #05a863; margin-right: 5px;"
											></i
											>
											<i v-else class="fa fa-exclamation-circle text-warning" aria-hidden="true" style="margin-right: 5px;"></i>
											Status {{lists[listIndex-1].status_value}}
										</li>
										<li class="my-1 item-check">
											<i
											class="fa fa-check-circle"
											aria-hidden="true"
											style="color: #05a863; margin-right: 5px;"
											></i
											>{{lists[listIndex-1].show_benefit_label}} : {{lists[listIndex-1].show_benefit_value}}
										</li>
										<li class="my-1 lists[listIndex-1]-check">
											<i
											class="fa fa-check-circle"
											aria-hidden="true"
											style="color: #05a863; margin-right: 5px"
											></i
											>Tidak ada batasan waktu
										</li>
										<li class="my-1 lists[listIndex-1]-check">
											<i
											class="fa fa-check-circle"
											aria-hidden="true"
											style="color: #05a863; margin-right: 5px"
											></i
											>Chat dengan pembimbing
										</li>
										<li
										class="my-1 lists[listIndex-1]-check lists[listIndex-1]-info"
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
							</mdb-card-body>
						</mdb-container>
					</mdb-card>
				</b-col>
			</b-row>

			<b-row
			v-if="lists.length > listToShow"
			class="row justify-content-center"
			>
			<b-col md="12" xs="12" sm="12">
				<b-button block pill variant="primary" @click="ShowMore"
				>Load More Event</b-button
				>
			</b-col>
		</b-row>
	</b-container>

	<b-container v-else>
		<b-alert show dismissible>
			Belum ada event yang Anda diikuti !
		</b-alert>
	</b-container>
</b-jumbotron>
</b-col>
</b-row>
</div>
</template>

<script>
	export default {
		data() {
			return {
				listToShow: 9,
				loading: null,
				lists: [],
				header: {
					title: "Paket Membership Saya",
					context:
					"Daftar paket membership anda yang telah aktif dan bisa langsung  di gunakan saat pembelian pelatihan yang anda inginkan.",
				},
				total: null
			}
		},

		beforeMount(){
			this.ConfigApiUrl()
		},

		mounted(){
			this.listPaketMembership(),
			this.sisaTotalPelatihan()
		},

		methods: {
			listPaketMembership(start=0){
				const url = `${this.api_url}/web/paket-membership-user/list?start=${start}`
				this.$axios.defaults.headers.common.Authorization = `Bearer ${this.token.accessToken}`
				this.$axios.get(url)
				.then(({data}) => {
					console.log(data)
					this.lists = data.list_data
				})
				.catch(err => console.error(err))
			},

			CheckToken(){
				this.$store.dispatch('config/checkAuthLogin', 'token')
			},

			ConfigApiUrl(){
				const api_url = process.env.NUXT_ENV_API_URL
				this.$store.dispatch('config/storeConfigApiUrl', api_url)
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
			}
		},

		computed: {
			api_url(){
				return this.$store.getters['config/ConfigApiUrl']
			},
			token(){
				return this.$store.getters['config/ConfigCheckLogin']
			}
		}
	}
</script>