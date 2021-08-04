<template>
  <div class="app">
    <Header />
    <SearchField placeholder="Search by Name" @input="toSaveSearchValue" />
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

.search-field-container {
  padding: 80px 32px;
  margin: auto;
  justify-content: center;
}

.search-field {
  flex-basis: 800px;
}
</style>
