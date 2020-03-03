<template>
  <div id="app">
    <Navbar />
    <router-view />
    <Footer />
  </div>
</template>

<script>
  // @ is an alias to /src
  import Navbar from "@/components/Navbar.vue";
  import Footer from "@/components/Footer.vue";

  export default {
    name: "navbar",
    components: {
      Navbar,
      Footer
    }
  };
</script>

<style>
  body {
    background-color: rgba(170, 231, 255, 0.2);
  }
</style>
