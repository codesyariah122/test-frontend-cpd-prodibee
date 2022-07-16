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
        <b-col md="7" xs="12" sm="12">
          <h2 class="display-6 text-capitalize">
            <span class="copywrite">Tedika Health Education</span> <br>
            <div :style="`${$device.isDesktop ? 'margin-top:-1.9rem!important;' : 'margin-top:-.5rem!important;'}`">kembangkan kompetensi tenaga <span class="block-text">kesehatan</span> </div>
          </h2>
          <p>
           Tingkatkan kemampuan kompetensi sebagai tenaga kesehatan dengan ratusan kelas online bersertifikat resmi
         </p>
         <b-button
         v-if="!token.accessToken"
         :class="`my__btn-primary shadow-none ${$device.isMobile ? 'btn-block btn-sm' : ''}`"
         @click="$router.push({ name: 'auth-registrasi' })"
         >Daftar Sekarang</b-button
         >
         
        <b-button
        v-if="token.accessToken"
        :class="`my__btn-secondary ${$device.isMobile ? 'btn-block btn-sm mt-3' : ''}`" :style="`${$device.isDesktop ? 'width: 450px;' : ''}`"
        @click="$router.push({ path: `/profile/${$username(slug)}/events` })"
        >Lihat kelas saya</b-button>
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
    props: ["items", "token"],

    data() {
      return {
        profiles: {},
        slug: "",
        event_id: localStorage.getItem("event_id"),
        event_path: this.$route.path,
      };
    },

    beforeMount() {
      this.ConfigApiUrl()
    },

    mounted() {
      this.UserProfileData()
    },

    methods: {
      ConfigApiUrl() {
        const api_url = process.env.NUXT_ENV_API_URL;
        this.$store.dispatch("config/storeConfigApiUrl", api_url);
      },
      UserProfileData() {
        if (this.token.accessToken) {
          const url = `${this.api_url}/web/user`;
          this.$axios.defaults.headers.common.Authorization = `Bearer ${this.token.accessToken}`;
          this.$axios
          .get(url)
          .then(({ data }) => {
            console.log(data)
            this.profiles = data.user;
            this.slug = data.user.nama;
          })
          .catch((err) => console.log(err.response ? err.response : ""));
        } else {
          console.log("No-Login");
        }
      },
    },
    computed: {
      api_url() {
        return this.$store.getters["config/ConfigApiUrl"];
      },
    }
  };
</script>
