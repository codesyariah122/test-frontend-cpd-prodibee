<style lang="scss" scoped>
  .card{
    width: 21rem!important;
    margin-left: 1.7rem;
    margin-bottom: 1.5rem;
    border: none;
    transition: all 500ms cubic-bezier(0.19, 1, 0.22, 1);
    overflow:hidden;
    border-radius:20px;
    min-height:450px;
    box-shadow: 0 0 12px 0 rgba(0,0,0,0.2);

    @media (max-width: 768px) {
      min-height:350px;
    }

    @media (max-width: 420px) {
      min-height:450px;
    }

    &.card-has-bg{
      transition: all 500ms cubic-bezier(0.19, 1, 0.22, 1);
      background-size:120%;
      background-repeat:no-repeat;
      background-position: center center;
      &:before {
        content: '';
        position: absolute;
        top: 0;
        right: 0;
        bottom: 0;
        left: 0;
        background: inherit;
        -webkit-filter: grayscale(1);
        -moz-filter: grayscale(100%);
        -ms-filter: grayscale(100%);
        -o-filter: grayscale(100%);
        filter: grayscale(100%);}

        &:hover {
          transform: scale(0.98);
          box-shadow: 0 0 5px -2px rgba(0,0,0,0.3);
          background-size:130%;
          transition: all 500ms cubic-bezier(0.19, 1, 0.22, 1);

          .card-img-overlay {
            transition: all 800ms cubic-bezier(0.19, 1, 0.22, 1);
            background: rgb(35,79,109);
            background: linear-gradient(0deg, rgba(4, 130, 129,0.5) 0%, rgba(4, 130, 129,1) 100%);
          }
        }
      }
      .card-footer{
        margin-left: 2rem;
        margin-bottom: 2rem;
        background: none;
        border-top: none;
        .media{
          color: $default;
          img{
            border:solid 3px rgba(255,255,255,0.3);
          }
        }
      }
      .card-meta{color:#26BD75}
      .card-body{
        margin-left: 1rem;
        margin-top: 2rem;
        p{
          width: 80%;
        }
        transition: all 500ms cubic-bezier(0.19, 1, 0.22, 1);
      }
      
      &:hover {
        .card-body{
          margin-top:30px;
          transition: all 800ms cubic-bezier(0.19, 1, 0.22, 1);
        }
        cursor: pointer;
        transition: all 800ms cubic-bezier(0.19, 1, 0.22, 1);
      }
      .card-img-overlay {
        transition: all 800ms cubic-bezier(0.19, 1, 0.22, 1);
        background: rgb(35,79,109);
        background: linear-gradient(0deg, rgba(35,79,109,0.3785889355742297) 0%, rgba(69,95,113,1) 100%);
      }
    }

    @media (min-width: 992px) {
      
  }
</style>
<template>
  <div>
    <div ref="berita_square" class="card__list" :style="berita__list_style">
      <mdb-container>
        <mdb-row class="row justify-content-center header__ppkc-list-page mb-5">
          <mdb-col lg="12" xs="12" sm="12">
            <h1>Berita</h1>
          </mdb-col>
          <mdb-col class="mt-3" lg="12" xs="12" sm="12">
            <span style="color: #666666 !important;">
              Ikuti semua kegiatan dan info maupun berita terbaru seputar pelatihan tenaga kesehatan, dari Tedika Health Education.
            </span>
          </mdb-col>
        </mdb-row>
        <!-- delay looping data with loader spinner -->

        <!-- show card berita -->
        <mdb-row class="row justify-content-center mb-5 webinar__content">
          <!-- List berita inside global-components -->
          <mdb-col lg="12" xs="12" sm="12">
            <div class="card__list mt-3">
              <mdb-row>
                <mdb-col v-for="item in lists" class="col-sm-12 col-md-6 col-lg-4 mb-4" :key="item.id" :style="`${$device.isDesktop ? 'margin-left: 2rem;' : 'margin-left: -.3rem;'}`">
                  <mdb-card class="text-white card-has-bg click-col" :style="`background-image:url('${item.foto_url}');`">
                    <img class="card-img d-none" src="https://source.unsplash.com/600x900/?tech,street" alt="Goverment Lorem Ipsum Sit Amet Consectetur dipisi?">

                    <div class="card-img-overlay d-flex flex-column">
                      <div class="card-body">
                        <h4 class="card-title mt-0 mb-3">
                          {{item.judul}}
                        </h4>
                        <small><mdb-icon far icon="clock" size="lg"/>&nbsp; {{$moment(item.created_at).format("LL")}}</small>
                      </div>
                      <div class="card-footer">
                        <div class="media">
                          <nuxt-link
                          :to="{
                            name: `detail-berita-id-slug`,
                            params: {
                              id: item.id,
                              slug: $slug(item.judul),
                            },
                          }"
                          class="mt-5 mb-2 text-white"
                          >Baca Selengkapnya <mdb-icon icon="arrow-right"
                          /></nuxt-link>
                        </div>
                      </div>
                    </div>
                  </mdb-card>
                </mdb-col>
              </mdb-row>
            </div>
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
            <button type="button" @click="LoadBerita" class="btn btn-info">
              Load More
            </button>
          </mdb-col>
        </mdb-row>
      </mdb-container>
    </div>
  </div>
</template>

<script>
export default {
  props: ["lists", "loading", "error", "end"],
  data() {
    return {
      currentPage: 1,

      berita__list_style:
        this.$router.path == "berita" && this.$device.isDeskktop
          ? "margin-top: 15rem;"
          : "margin-top: 5rem;",
    };
  },

  mounted() {
    this.getNextBerita();
  },

  methods: {
    LoadBerita() {
      this.$emit("load-more-berita");
    },

    getNextBerita() {
      window.onscroll = () => {
        if (!this.loading && !this.end && !this.error) {
          if (this.$refs.berita_square.getBoundingClientRect().bottom <= 450) {
            this.$emit("load-more-berita");
          } else {
            console.log("out");
          }
        }
      };
    },
  },
};
</script>

<style lang="css">
.truncate {
  text-transform: capitalize;
  display: -webkit-box;
  -webkit-line-clamp: var(--line-clamp, 3);
  -webkit-box-orient: vertical;
  word-break: var(--word-break, "none");
  overflow: hidden;
  hyphens: auto;
  text-align: var(--align, left);

  --is-single-line: 1 - Clamp(0, Calc(var(--line-clamp) - 1), var(--line-clamp));
  --delay: Calc(-1s * (var(--is-single-line, 1) - 1));
  animation: states 1s var(--delay) paused;

  @keyframes states {
    0% {
      word-break: break-all;
    }
  }
}

.truncate2 {
  display: -webkit-box;
  -webkit-line-clamp: var(--line-clamp, 2);
  -webkit-box-orient: vertical;
  word-break: var(--word-break, "none");
  overflow: hidden;
  hyphens: auto;
  text-align: var(--align, left);

  --is-single-line: 1 - Clamp(0, Calc(var(--line-clamp) - 1), var(--line-clamp));
  --delay: Calc(-1s * (var(--is-single-line, 1) - 1));
  animation: states 1s var(--delay) paused;

  @keyframes states {
    0% {
      word-break: break-all;
    }
  }
}
</style>
