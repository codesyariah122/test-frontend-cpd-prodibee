<template>
  <div>
    <mdb-row class="row justify-content-center">
      <!-- bg content -->
      <mdb-col col="12" lg="12" xs="12" sm="12" class="bg__content">
        <img :src="require('~/assets/art/vector-5.png')" />
      </mdb-col>
    </mdb-row>
    
    <mdb-row class="row justify-content-center card__berita-list">
      <mdb-col v-for="listIndex in listToShow" md="4" xs="12" sm="12" :key="lists[listIndex-1].id" class="mb-3">
        <mdb-card>
          <mdb-card-image :src="lists[listIndex-1].foto_url" alt="Card image cap"></mdb-card-image>
          <mdb-card-body>
            <mdb-card-title class="truncate2">{{lists[listIndex-1].judul}}</mdb-card-title>
            <mdb-card-text class="truncate2 mt-2">
              <span style="font-size: 12px;"
              ><i class="fa fa-calendar fa-fw fa-lg" aria-hidden="true"></i>
              {{ $moment(lists[listIndex-1].created_at).format("LL") }}</span
              >
              <p  class="mt-3 mb-2 truncate2">
                {{lists[listIndex-1].konten}}
              </p>
            </mdb-card-text>
            <hr />

            <nuxt-link :to="{name: `detail-berita-id-slug`, params: {id: lists[listIndex-1].id, slug: $slug(lists[listIndex-1].judul)}}" class="mt-5 mb-2">Baca Selengkapnya <mdb-icon icon="arrow-right" /></nuxt-link>
          </mdb-card-body>
        </mdb-card>
      </mdb-col>

      <mdb-col col="12"  xl="4" lg="12" xs="12" sm="12" :class="`${$device.isDesktop ? 'mb-5 mt-5 ml-5 shadow-none' : 'mb-2'}`">
        <nuxt-link to="/tedika/berita" :class="`btn my__btn-primary rounded-pill ${$device.isMobile ? 'btn-block btn-sm btn__link' : 'btn-lg'}`">Lihat Semua Berita</nuxt-link>
      </mdb-col>
    </mdb-row>

  </div>
</template>

<script>
  export default{
    props: ['lists'],
    data(){
      return {
        listToShow: 3
      }
    }
  }
</script>

