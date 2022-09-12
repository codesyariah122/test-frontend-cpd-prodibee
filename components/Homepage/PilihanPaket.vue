<template>
  <div  class="panel__target-content mt-5">
    <mdb-row class="row justify-content-center panel__header">
      <mdb-col col="12" lg="12" xs="12" sm="12">
        <h5 class="text-capitalize text-center">
          <strong class="font-weight-bold">Ayo bergabung bersama kami </strong>
        </h5>
      </mdb-col>
      <mdb-col col="12" lg="6" xs="12" sm="12">
        <h2 class="text-center">Segera pilih paket pelatihan pilihan kamu</h2>
      </mdb-col>
    </mdb-row>

    <mdb-row
    class="row row-cols-1 row-cols-md-5 justify-content-center"
    style="margin-top: 16px"
    >
    <mdb-col v-if="listpakets" v-for="item in listpakets" :key="item.id" col="12" xs="12" sm="12" md="4" lg="4" :class="`${$device.isMobile ? 'mb-3' : 'mt-2'}`">
      <mdb-card class="mt-2" :style="`${$device.isMobile ? 'width: 335px; margin-left: -3.5rem; border-radius: 25px' : 'border-radius: 25px'}`">
        <mdb-container class="mt-2 p-2 rounded-3">
          <h1 class="text-center font-weight-bold small" style="color: #013a51">
            {{item.paket_nama}}
          </h1>
          <mdb-card-body>
            <h2 class="text-center font-weight-bold" style="color: #013a51">
              {{ $format(item.harga) }}
            </h2>
            <ul class="list-unstyled text-left mt-5 mb-0 small">
              <li class="my-1 item-check">
                <i
                class="fa fa-check-circle"
                aria-hidden="true"
                style="color: #05a863; margin-right: 5px"
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
          <nuxt-link :to="{
            name: 'events-membership-id-registrasi',
            params: {
              id: item.id
            }
          }" :class="`btn my__btn-primary text-white rounded-pill mt-5 mb-2 btn-block ${
                $device.isMobile ? 'btn-sm' : 'btn-md'
              }`" style="color: #424242"
          >
            Pilih Sekarang <mdb-icon icon="box-open" size="lg"/>
          </nuxt-link>
        </mdb-card-body>
        </mdb-container>
      </mdb-card>
    </mdb-col>

  </mdb-row>

  <mdb-row class="row justify-content-center">
    <mdb-col col="12" lg="12" xs="12" sm="12" class="bg__content2">
      <img
      :src="require('~/assets/art/vector-3.png')"
      :width="`${$device.isMobile ? '370px' : ''}`"
      />
    </mdb-col>
  </mdb-row>
</div>
</template>

<script>
  export default {
    data() {
      return {
        listpakets: [],
        panels: [
        {
          id: 1,
          vector: require("~/assets/art/vector-6.svg"),
          title: "Akses Kelas Tak Terbatas Waktu",
          context:
          "Setiap kelas yang telah dibeli dapat diakses selamanya melalui berbagai perangkat desktop atau mobile.",
          harga: "10.900",
        },
        {
          id: 2,
          vector: require("~/assets/art/vector-7.svg"),
          title: "Diskusi Sesama Anggota dan Konsultasi Bersama",
          context:
          "Member yang terdaftar dalam sebuah kelas mendapatkan akses ke group diskusi anggota dan dapat berkonsultasi dengan pengajar.",
          harga: "20.000",
        },
        {
          id: 3,
          vector: require("~/assets/art/vector-8.svg"),
          title: "Kelas Bersertifikat Resmi",
          context:
          "Sertifikat resmi akan didapatkan member setelah menyelesaikan seluruh modul pembelajaran dalam suatu kelas.",
          harga: "30.000",
        },
        {
          id: 4,
          vector: require("~/assets/art/vector-8.svg"),
          title: "Kelas Bersertifikat Resmi",
          context:
          "Sertifikat resmi akan didapatkan member setelah menyelesaikan seluruh modul pembelajaran dalam suatu kelas.",
          harga: "50.000",
        },
        {
          id: 5,
          vector: require("~/assets/art/vector-8.svg"),
          title: "Kelas Bersertifikat Resmi",
          context:
          "Sertifikat resmi akan didapatkan member setelah menyelesaikan seluruh modul pembelajaran dalam suatu kelas.",
          harga: "60.000",
        },
        ],
      }
    },

    beforeMount(){
      this.ConfigApiUrl()
    },

    mounted(){
      this.CheckToken(),
      this.listsPaket()
    },

    methods: {
      listsPaket(){
        const url = `${this.api_url}/web/paket-membership/list/home`
        // this.$axios.defaults.headers.common.Authorization = `Bearer ${this.token.accessToken}`
        this.$axios.get(url)
        .then(({data}) => {
          this.listpakets = data.list_data
        })
        .catch(err => console.error(err.response))
      },

      CheckToken(){
        this.$store.dispatch('config/checkAuthLogin', 'token')
      },

      ConfigApiUrl(){
        const api_url = process.env.NUXT_ENV_API_URL
        this.$store.dispatch('config/storeConfigApiUrl', api_url)
      }
    },

    computed: {
      token(){
        return this.$store.getters['config/ConfigCheckLogin']
      },
      api_url(){
        return this.$store.getters['config/ConfigApiUrl']
      },
      logout_data(){
        return this.$store.getters['config/ConfigProfileDataLogout']
      }
    }
  };
</script>