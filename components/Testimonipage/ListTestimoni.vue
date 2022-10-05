<template>
  <div>
    <div ref="testimoni_square" class="card__list" :style="berita__list_style">
      <mdb-container>
        <mdb-row class="row justify-content-center header__ppkc-list-page mb-5">
           <mdb-col lg="12" xs="12" sm="12">
            <h1>Testimoni</h1>
          </mdb-col>
          <mdb-col class="mt-3" lg="12" xs="12" sm="12">
            <span style="color: #666666 !important;">
              Tedika Health Education telah melakukan berbagai macam pelatihan dan telah
              banyak membantu meningkatkan kemampuan tenaga kesehatan
            </span>
          </mdb-col>
        </mdb-row>

        <!-- show card berita -->
        <mdb-row class="row justify-content-center col__testimoni mt-2 mb-5">
          <mdb-col v-for="item in lists" md="6" xs="12" sm="12" class="col__testimoni-card" :key="item.id">
            <b-card no-body class="overflow-hidden card__testimoni-content">
              <b-container>
                <b-row no-gutters>
                  <b-col md="4" class="mt-2">
                    <b-avatar variant="primary" border-variant="none" v-if="item.foto_url !== null" :src="`https://api.ppkc-online.com/image-testimoni/${item.foto_url}`" size="10rem"></b-avatar>
                    <b-avatar variant="none" border-variant="none" v-else :src="require('~/assets/images/profile/profile.svg')" size="12rem"></b-avatar>
                  </b-col>
                  <b-col md="8">
                    <b-card-body :title="item.konsumen">
                      <h6>{{item.profesi}}</h6>
                      <b-card-text v-html="item.testimoni">
                      </b-card-text>
                    </b-card-body>
                  </b-col>
                </b-row>
              </b-container>
            </b-card>
          </mdb-col>
        </mdb-row>

        <mdb-row v-if="loading" class="row justify-content-center">
          <mdb-col lg="12" xs="12" sm="12">
            <div class="d-flex justify-content-center mt-5 mb-5">
              <div
                class="spinner-grow text-primary"
                role="status"
                style="width: 3rem; height: 3rem"
              >
                <span class="sr-only">Loading...</span>
              </div>
            </div>
          </mdb-col>
        </mdb-row>
        <mdb-row
          v-if="error"
          class="row justify-content-center header__ppkc-list-page"
        >
          <mdb-col lg="12" xs="12" sm="12">
            <button type="button" @click="LoadTestimoni" class="btn btn-info">
              Load More
            </button>
          </mdb-col>
        </mdb-row>
      </mdb-container>
    </div>
  </div>
</template>

<script>
import { TestimoniSamples } from "@/helpers";

export default {
  props: ["path", "lists", "loading", "error", "end"],
  data() {
    return {
      currentPage: 1,
      listToShow: 6,
      berita__list_style:
         this.$device.isDesktop
          ? "margin-top: 15rem;margin-bottom: 10rem;"
          : "margin-top: 7rem;",
    };
  },

  mounted() {
    this.getNextTestimoni(), this.ChekListsLength();
  },

  methods: {
    ChekListsLength() {
      console.log(this.lists.length);
      // if(this.lists.length <= 3){
      //   TestimoniSamples.map(d => {
      //     this.lists.push(d)
      //   })
      // }
    },

    LoadTestimoni() {
      this.$emit("load-more-testi");
    },

    getNextTestimoni() {
      window.onscroll = () => {
        if (
          !this.loading &&
          !this.end &&
          !this.error &&
          this.$route.path == "/ppkc/testimoni"
        ) {
          if (
            this.$refs.testimoni_square.getBoundingClientRect().bottom <= 450
          ) {
            this.$emit("load-more-testi");
          }
        } else {
          console.log("else");
        }
      };
    },
  },
};
</script>
<style lang="scss">
@include card_testimonial_mobile;

@media (min-width: 992px) {
  @include card_testimonial_desktop;
}
</style>
