<template>
  <div class="app">
    <Header />
    <SearchField
      class="search-field-custom-styles"
      placeholder="Search by Name"
      @input="toSaveSearchValue"
    />
    <CardList class="card-list-custom-styles" :data="people" />
  </div>
</template>

<script>
import "reset-css/reset.css";
import "roboto-fontface/css/roboto/roboto-fontface.css";
import Header from "./components/Header.vue";
import SearchField from "./components/SearchField.vue";
import CardList from "./components/CardList.vue";

export default {
  name: "App",
  components: { Header, SearchField, CardList },
  data() {
    return {
      searchValue: "",
      people: [],
      isLoading: true,
    };
  },
  async mounted() {
    const response = await fetch("https://swapi.dev/api/people?page=1");
    const people = (await response.json()).results;

    for await (const person of people) {
      for (let [i, speciesItem] of person.species.entries()) {
        const response = await fetch(speciesItem);
        const species = await response.json();
        person.species[i] = species.name;
      }
    }

    this.people = people;
    this.isLoading = false;
  },
  methods: {
    toSaveSearchValue: function (value) {
      this.searchValue = value;
    },
  },
};
</script>

<style>
* {
  box-sizing: border-box;
}

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

.card-list-custom-styles {
  max-width: 1280px;
}
</style>