<style lang="scss">
  .inner-page{
    margin-bottom: 8rem;    
    article{
      font-family: 'Roboto Mono', monospace;
      line-height: 31px;
      word-spacing: 5px;
      text-align: justify;
      h2{
        font-weight: 800;
        font-family: 'Ubuntu', sans-serif;
      }
      h3{
        margin-top: 2rem;
        font-weight: 700;
        font-family: 'Ubuntu', sans-serif;
      }
      p{
        font-family: 'Roboto Mono', monospace;
        line-height: 31px;
        word-spacing: 5px;
        text-align: justify;
      }
      img{
        width: 90%;
        margin-bottom: 2rem;
        margin-top: 2rem;
      }
    }
  }
  @media (max-width: 768px) {

  }
  @media (max-width: 420px) {
    .inner-page{    
      article{
        h2{
          font-weight: 800;
          font-family: 'Ubuntu', sans-serif;
        }
        h3{
          margin-top: 2rem;
          font-weight: 700;
          font-family: 'Ubuntu', sans-serif;
        }
        p{
          font-family: 'Roboto Mono', monospace;
          line-height: 31px;
          word-spacing: 5px;
          text-align: justify;
        }
        img{
          max-width: 335px;
          margin-top:2rem;
          margin-bottom: 2rem;
        }
      }
    }
  }
</style>

<template>
  <div class="berita__detail" :style="berita__list_style">
    <section id="hero" :style="$device.isDesktop ? SetUpBackground($device.isDesktop, lists.berita.foto_url) : SetUpBackground($device.isMobile, lists.berita.foto_url)">
      <div class="hero-container">
        <h1>{{ lists.berita.judul }}</h1>
        <h2 class="mt-5">{{ $moment(lists.berita.created_at).format("LLL") }}</h2>
        <a href="#post" class="btn-scroll scrollto" title="Scroll Down">
          <i class="fa fa-arrow-down" aria-hidden="true"></i>
        </a>
      </div>
    </section>
    <section id="post" class="inner-page">
      <div class="container mb-5 mt-5">
        <div class="back float-start">
          <nuxt-link to="/tedika/berita"><i class="fa fa-arrow-left" aria-hidden="true"></i> Kembali Ke Blog Lists</nuxt-link>
        </div>
      </div>
      <mdb-container class="mb-5">
        <article v-html="lists.berita.konten"></article>

        <!-- <GlobalsOptionSlug :next="next" /> -->
      </mdb-container>
    </section>

  </div>
</template>

<script>
  import { SampleNews } from "@/helpers";

  export default {
    name: "detail-berita-id-slug",
    layout: "default",

    async asyncData({ $axios, params }) {
      const lists = await $axios.$get(`/web/berita/${params.id}`);
      console.log(lists);
      const next_id = parseInt(params.id) + 1;
      const next = await $axios.$get(`/web/berita/${next_id}`);
      console.log(next);
      return {
        lists,
        next,
      };
    },

    data() {
      return {
        berita__list_style: this.$device.isDesktop
        ? "margin-top: 8rem;"
        : "margin-top: 6rem;",
        id: this.$route.params.id,
        path: this.$route.name,
        berita: null,
        id_berita: this.$route.params.id,
        detail: {}
      };
    },



    beforeMount() {
      this.ConfigApiUrl();
    },

    methods: {
      SetUpBackground(device, img){
        if(device === this.$device.isDesktop){
          return `background: url(${img}) top center no-repeat; height: 70vh;width: 100%;background-size: cover;`
        }else{
          return `background: url(${img}) no-repeat center; height: 100vh;width: 100%;background-size: cover;`
        }
      },

      ConfigApiUrl() {
        const api_url = process.env.NUXT_ENV_API_URL;
        this.$store.dispatch("config/storeConfigApiUrl", api_url);
      },

      DetailBeritaSample() {
        this.detail = SampleNews.filter((d) => d.id == this.id_berita)[0];
      },
    },

    computed: {
      api_url() {
        return this.$store.getters["config/ConfigApiUrl"];
      },
    },
  };
</script>

