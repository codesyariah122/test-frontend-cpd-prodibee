<template>
  <div>
    <b-jumbotron
      class="jumbotron__home mt-5 shadow-none"
    >
      <b-container class="container__jumbotron">
        <b-row cols="12" class="row justify-content-start">
          <!-- Banner art image Mobile -->
          <MoleculesHomepageMoleculesBannerArtMobile v-if="$device.isMobile"/>
          <!-- Context -->
          <b-col md="7" xs="12" sm="12" :style="`${$device.isDesktop ? 'margin-top: 5rem;' : 'margin-top: 1rem;'}`">
            <h2 class="display-6 text-capitalize">
              kembangkan kompetensi tenaga <span class="block-text">kesehatan</span>
            </h2>
            <p>
             Tingkatkan kemampuan kompetensi sebagai tenaga kesehatan dengan ratusan kelas online bersertifikat resmi
            </p>
            <b-button :style="`${$device.isDesktop ? 'margin-left: -3rem;margin-top: -.3rem;' : ''}`"
              v-if="!token.accessToken" block
              :class="`my__btn-primary rounded-pill ${$device.isMobile ? 'btn-block btn-sm' : ''}`"
              @click="$router.push({ name: 'auth-registrasi' })"
              >Daftar Sekarang</b-button
            >

            <b-button :style="`${$device.isDesktop ? 'margin-left: -3rem;margin-top: -.3rem;' : ''}`"
              v-if="token.accessToken" block
              :class="`my__btn-secondary  btn rounded-pill ${$device.isMobile ? 'btn-sm mt-3' : ''}`"
              @click="$router.push({
                name: 'profile-name-events',
                params: {
                  name: username
                }
              })"
              >Lihat Kelas Saya</b-button
            >
          </b-col>

          <!-- Banner art image -->
          <MoleculesHomepageMoleculesBannerArtDesktop v-if="$device.isDesktop"/>

        </b-row>
      </b-container>
    </b-jumbotron>

  </div>
</template>

<script>
export default {
  props: ["items"],

  data(){
    return {
      profiles: {},
      username: null
    }
  },

  mounted(){
    this.CheckToken(),
    this.UserProfileData()
  },

  methods: {
    CheckToken() {
      this.$store.dispatch("config/checkAuthLogin", "token");
    },
    ConfigApiUrl() {
      const api_url = process.env.NUXT_ENV_API_URL;
      this.$store.dispatch("config/storeConfigApiUrl", api_url);
    },
    UserProfileData(){
        if(this.token){
          this.loading=true         
          const url = `${this.api_url}/web/user`
          this.$axios.defaults.headers.common.Authorization = `Bearer ${this.token.accessToken}`
          this.$axios.get(url)
          .then(({data}) => {
            this.profiles = data.user
            this.username = this.$username(data.user.nama)
          })
          .catch(err => console.log(err.response ? err.response : ''))
          .finally(() => {
            setTimeout(() => {
              this.loading=false
            },1500)
          })
        }
      },
  },

  computed: {
    token() {
      return this.$store.getters["config/ConfigCheckLogin"];
    },
    api_url() {
      return this.$store.getters["config/ConfigApiUrl"];
    }
  }
};
</script>
