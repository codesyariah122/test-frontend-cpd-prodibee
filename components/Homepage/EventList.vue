<template>
  <div class="card__list mt-5">
    <div class="row justify-content-between card__header">
      <mdb-col md="12" xs="12" sm="12">
        <h5 class="text-capitalize" :style="`${$device.isDesktop ? 'font-size:21px!important;' : ''}`">
          <strong class="font-weight-bold">mulai sekarang</strong>
        </h5>
      </mdb-col>

      <mdb-col md="12" xs="12" sm="12" :class="`text-capitalize ${$device.isDesktop ? 'mb-5' : 'mb-2'}`">
        <h2>Temukan kelas sesuai kompetensi yang ingin dicapai</h2>
      </mdb-col>
    </div>

    <CardEventList :listToShow="listToShow" :lists="lists"/>

  </div>
</template>

<script>
import CardEventList from "@/components/Molecules/HomepageMolecules/CardEventList";

export default {
  props: ["lists", "listToShow"],
  components: {
    CardEventList
  }
}
</script>
