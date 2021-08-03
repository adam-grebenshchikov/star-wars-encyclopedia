<template>
  <div id="app">
    <Header />
  </div>
</template>

<script>
import "reset-css/reset.css";
import "roboto-fontface/css/roboto/roboto-fontface.css";
import Header from "./components/Header.vue";

export default {
  name: "App",
  components: { Header },
};
</script>

<style>
#app {
  font-family: Roboto;
  font-weight: 400;
  color: #fff;
  background-color: #333;
  min-height: 100vh;
}
</style>
