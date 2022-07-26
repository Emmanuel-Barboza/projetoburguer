<template>
  <Banner />
  <div class="main-container">
    <h1>Monter seu Hamburguer</h1>
    <FormHam />
  </div>
</template>

<script>
import Banner from "@/components/Banner.vue";
import FormHam from "@/components/FormHam.vue";

  export default{
    name: "Home",
    components: { Banner, FormHam }
  }
</script>
