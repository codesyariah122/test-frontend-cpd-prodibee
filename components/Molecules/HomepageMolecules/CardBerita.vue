<style lang="scss" scoped>
  .card{
    width: 18rem!important;
    margin-left: -3rem;
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
            background: linear-gradient(0deg, rgba(4,69,114,0.5) 0%, rgba(4,69,114,1) 100%);
          }
        }
      }
      .card-footer{
        margin-left: 2rem;
        margin-bottom: 2rem;
        background: none;
        border-top: none;
        .media{
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
      .card{
        width: 18rem!important;
        .card-body{
         margin-left: 1rem;
         margin-top: 2rem;
         p{
          width: 50%;
        }
      }
      .card-footer{
        margin-left: 2rem;
        margin-bottom: 2rem;
      }
    }
  }
</style>
<template>
  <div>
    <mdb-row class="row justify-content-center">
      <!-- bg content -->
      <mdb-col col="12" lg="12" xs="12" sm="12" class="bg__content">
        <img :src="require('~/assets/art/vector-5.png')" />
      </mdb-col>
    </mdb-row>

    <mdb-row class="row justify-content-center">
      <mdb-col v-for="(item, index) in lists" class="mb-4" md="4" xs="12" sm="12" :key="item.id" style="margin-left: 7rem;">
        <mdb-card v-if="index <= 3" class="text-white card-has-bg click-col" :style="`background-image:url('${item.foto_url}');`">
          <img class="card-img d-none" src="https://source.unsplash.com/600x900/?tech,street" alt="Goverment Lorem Ipsum Sit Amet Consectetur dipisi?">
          <nuxt-link class="text-white" :to="{
            name: `detail-berita-id-slug`,
            params: {
              id: item.id,
              slug: $slug(item.judul),
            },
          }">
          <div class="card-img-overlay d-flex flex-column">
            <div class="card-body">
              <h4 class="card-title mt-0 ">
                {{item.judul}}
              </h4>
              <!-- <p class="mt-3 mb-2 truncate2">
                {{ item.konten }}
              </p> -->
            </div>
            <div class="card-footer">
              <div class="media">
                <small><mdb-icon far icon="clock" size="lg"/>&nbsp; {{$moment(item.created_at).format("LL")}}</small>
              </div>
            </div>
          </div>
        </nuxt-link>
      </mdb-card>
    </mdb-col>
  </mdb-row>

  <mdb-row class="row justify-content-center text-center">
    <mdb-col
    lg="12"
    xs="12"
    sm="12"
    :class="`${$device.isDesktop ? 'mb-5 mt-5 shadow-none' : 'mb-2'}`"
    >
    <nuxt-link
    to="/tedika/berita"
    :class="`btn my__btn-primary rounded-pill ${
      $device.isMobile ? 'btn-block btn-sm btn__link' : 'btn-lg'
    }`"
    >Lihat Semua Berita <mdb-icon icon="arrow-right" size="lg"/></nuxt-link
    >
  </mdb-col>
</mdb-row>


</div>
</template>

<script>
  export default {
    props: ["lists"],
    data() {
      return {
        listToShow: 3,
      };
    },
  };
</script>