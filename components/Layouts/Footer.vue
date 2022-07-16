<template>
  <!-- Footer -->

  <mdb-footer class="footer__layout font-small" >
    <mdb-container class="text-left footer__sitemap">
      <mdb-row
      :class="`${$device.isMobile ? 'd-flex justify-content-center' : 'row'}`"
      >
      <!-- footer branding content -->
      <mdb-col col="12" sm="3" class="mt-5 col-1">
        <img
        :src="brand"
        :style="`${$device.isMobile ? 'width:230px;' : 'width:800px;'}`"
        />

        <div class="mb-0">
          <h5>PT TELEMEDIKA INDONESIA MANAJEMEN
          (TEDIKA)</h5>
          <p>merupakan perusahaan teknologi <br> di Indonesia yang berfokus pada layanan berbasis pendidikan dan pelatihan di bidang kesehatan, memiliki member tenaga kesehatan yang teregistrasi maupun calon tenaga kesehatan serta Berpartner dengan ikatan himpunan kesehatan dan organisasi profesi.</p>
        </div>
        <address>Cipinang Indah Raya Blok M No 1,
        Jakarta Timur 13420</address>
      </mdb-col>

      <mdb-col
      col="12"
      :sm="`${$device.isDesktop ? '9' : '12'}`"
      :class="`${$device.isMobile ? 'mt-2 col-2' : 'col-2'}`"
      >
      <!-- sitemap footer content -->
      <MoleculesLayoutMoleculesSiteMap />
    </mdb-col>
  </mdb-row>

  <!-- Bonus Map -->
      <!-- <mdb-row>
        <mdb-col col="12" lg="12" class="col-3">
          <LayoutsMap />
        </mdb-col>
      </mdb-row> -->
      <!-- end map -->
    </mdb-container>
    <div class="footer-copyright text-center py-3">
      <mdb-container fluid>
        &copy;Copyright {{ $moment(year).year() }}
        <a target="_blank" href="http://tedika.id"
        >Tedika Indonesia</a
        >. All Rights Reserved
      </mdb-container>
    </div>
  </mdb-footer>
  <!-- Footer -->
</template>

<script>
  import LogoBrand from "~/assets/images/logo/new-logo-2.png";
  export default {
    data() {
      return {
        brand: LogoBrand,
        year: new Date(),
      };
    },
  };
</script>