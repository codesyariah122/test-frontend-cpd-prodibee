<!-- Component base on directory /components/ppkc -->
<template>
  <div>
    <tedikaFasilitas :path="path" :lists="lists" />
  </div>
</template>

<script>
export default {
  name: "tedika-fasilitas",
  layout: "default",

  data() {
    return {
      path: this.$route.name,
    };
  },

  async asyncData({ $axios }) {
    const lists = await $axios.$get("/web/fasilitas");

    console.log(lists);

    return {
      lists,
    };
  },
};
</script>
