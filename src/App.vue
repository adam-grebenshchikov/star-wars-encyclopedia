<template>
  <div id="app">
    <Header />
    <div class="search-field-wrapper">
      <SearchField
        placeholder="Search by Name"
        width="800px"
        @input="toSaveSearchValue"
      />
    </div>
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
#app {
  font-family: Roboto;
  font-weight: 400;
  color: #fff;
  background-color: #333;
  min-height: 100vh;
}

.search-field-wrapper {
  display: flex;
  justify-content: center;
  padding: 80px 0;
  margin: 0 32px;
}
</style>
