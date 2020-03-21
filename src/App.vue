<template>
  <div id="app">
    <div class="wrapper">
      <Header />
      <Container />
      <Footer />
    </div>
  </div>
</template>

<script>
import Container from "./components/Container.vue";
import Header from "./components/Header.vue";
import Footer from "./components/Footer.vue";

export default {
  name: "App",
  components: {
    Container,
    Header,
    Footer
  }
};
</script>

<style>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}
#app {
  background: #eee;
  min-height: 100vh;
}
.wrapper {
  width: 100%;
  margin: 0 auto;
}
</style>
