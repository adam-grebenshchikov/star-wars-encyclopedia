<template>
  <div class="app">
    <Header />
    <SearchField
      class="search-field-custom-styles"
      placeholder="Search by Name"
      @input="toSaveSearchValue"
    />
  </div>
</template>

<script>
import "reset-css/reset.css";
import "roboto-fontface/css/roboto/roboto-fontface.css";
import Header from "./components/Header.vue";
import SearchField from "./components/SearchField.vue";

export default {
  name: "App",
  components: { Header, SearchField },
  data() {
    return {
      searchValue: "",
    };
  },
  methods: {
    toSaveSearchValue: function (value) {
      this.searchValue = value;
    },
  },
};
</script>

<style>
.app {
  font-family: Roboto;
  font-weight: 400;
  color: #fff;
  background-color: #333;
  min-height: 100vh;
}
</style>

<style scoped>
.search-field-custom-styles {
  padding: 80px 32px;
  margin: auto;
  justify-content: center;
  max-width: 800px;
}
</style>