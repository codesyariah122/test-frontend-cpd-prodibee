<template>
  <div>
    <mdb-navbar
    class="fixed-top lighten-4 navbar-9  text-primary"
    light
    color="white"
    expand="large"
    togglerIcon=""
    animated
    animation="2"
    >
    <!-- Navbar brand -->
    <mdb-navbar-brand>
      <nuxt-link to="/">
        <!-- <MoleculesLayoutMoleculesBrand /> -->
        <img :src="require('~/assets/images/logo/new-logo-2.png')" class="img-fluid">
      </nuxt-link>
    </mdb-navbar-brand>
    <mdb-navbar-toggler class="mt-3 mb-2">
      <mdb-navbar-nav right>
        <mdb-nav-item active waves-fixed> 
          <nuxt-link to="/" tag="li" class="nav-item">
            Home     
          </nuxt-link>
        </mdb-nav-item>
        <mdb-nav-item waves-fixed> 
          <nuxt-link to="/events" tag="li" class="nav-item">
            Katalog Kelas     
          </nuxt-link>
        </mdb-nav-item>
        <mdb-nav-item waves-fixed> 
          <nuxt-link to="/agendapelatihan" tag="li" class="nav-item">
            Agenda Pelatihan    
          </nuxt-link>
        </mdb-nav-item>
        <mdb-nav-item
        v-if="token.accessToken"
        waves-fixed
        >
        <nuxt-link :to="`/profile/${$username(slug)}/events`" tag="li" class="nav-item">
          Akses Pelatihan
        </nuxt-link>
      </mdb-nav-item>
      <mdb-dropdown tag="li" class="nav-item">
        <mdb-dropdown-toggle tag="a" navLink slot="toggle" waves-fixed class="no-caret">
          Tentang Tedika <mdb-icon icon="external-link-alt" />
        </mdb-dropdown-toggle>
        <mdb-dropdown-menu :class="`${$device.isDesktop ? 'mt-4' : ''}`">
          <mdb-dropdown-item v-for="item in links" :key="item.id">
            <nuxt-link :to="item.link">
              {{ item.name }}
            </nuxt-link>
          </mdb-dropdown-item>
        </mdb-dropdown-menu>
      </mdb-dropdown>

    </mdb-navbar-nav>
    <!-- check user login -->

    <mdb-col lg="12">
     <div class="dropdown-divider"></div>
   </mdb-col>

   <mdb-row v-if="token.accessToken">
    <mdb-col col="12" sm="12">
      <mdb-dropdown tag="li" class="nav-item mt-4 text-black">
        <mdb-dropdown-toggle tag="a" navLink slot="toggle">
          <b-avatar
          v-if="
          profiles.photo !==
          'https://api.ppkc-online.com/image-profiles/null'
          "
          variant="primary"
          :src="profiles.photo"
          ></b-avatar>
          <b-avatar
          v-else
          variant="primary"
          :text="slug.charAt(0)"
          ></b-avatar>
          <!--   <b-img v-else :src="`${require('~/assets/images/profile/profile.svg')}`" rounded="circle" alt="Circle image" width="30" height="30"></b-img> -->
          {{ profiles.nama }}
        </mdb-dropdown-toggle>
        <mdb-dropdown-menu>
          <mdb-dropdown-item>
            <nuxt-link
            :to="{
              name: 'profile-name',
              params: { name: $username(slug) },
            }"
            class="text-center"
            >
            <b-avatar v-if="profiles.photo !== 'https://api.ppkc-online.com/image-profiles/null'" variant="primary" :src="profiles.photo" size="1.7rem"></b-avatar>
            <b-avatar v-else variant="primary" :text="slug.charAt(0)" size="1.7rem"></b-avatar> 
            &nbsp; <b>{{ profiles.nama }}</b>
          </nuxt-link>
          <!-- <mdb-icon far icon="user-circle" size="lg"/>  -->
                    <!-- <a :href="`/profile/${$username(slug)}`" class="text-center">
                    <mdb-icon icon="user-md" /> Profile
                  </a> -->
                </mdb-dropdown-item>
                <div class="dropdown-divider"></div>
                <mdb-dropdown-item @click="Logout" class="text-center">
                  <mdb-icon icon="sign-out-alt" /> Logout
                </mdb-dropdown-item>
              </mdb-dropdown-menu>
            </mdb-dropdown>
          </mdb-col>
        </mdb-row>

        <div v-else>
          <mdb-row class="row justify-content-center">
            <mdb-col col="12" lg="12" xs="12" sm="12" class="mt-2">
              <nuxt-link 
              to="/auth/login"
              class="my__btn-secondary rounded-pill btn btn-block btn-md shadow-none"
              size="md"
              >Masuk</nuxt-link>
            </mdb-col>
            <mdb-col lg="12" xs="12" sm="12" class="mt-2">
              <nuxt-link
              to="/auth/registrasi"
              class="my__btn-primary rounded-pill btn btn-block btn-md shadow-none"
              size="md"
              >Daftar</nuxt-link>
            </mdb-col>         
          </mdb-row>
        </div>

        <!-- end check -->
      </mdb-navbar-toggler>
    </mdb-navbar>
  </div>
</template>

<script>
  export default {
    props: ["token", "profiles", "slug"],
    data() {
      return {
        links: [
          { id: 1, name: "Tentang kami", link: "/tedika/tentang-kami"},
          { id: 2, name: "Produk/Layanan", link: "/tedika/product-layanan"},
          { id: 3, name: "Berita", link: "/tedika/berita"},
          { id: 4, name: "Testimoni", link: "/tedika/testimoni" },
          { id: 5, name: "Fasilitator", link: "/tedika/fasilitator" }
        ],
      }
    },

    methods: {
      Logout() {
        this.$emit("logout-profile");
      },
    },
  };
</script>
