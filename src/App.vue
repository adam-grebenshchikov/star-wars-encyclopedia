<template>
  <div class="app">
    <Header text="CHARACTER Encyclopedia" />
    <SearchField
      class="search-field-custom-styles"
      placeholder="Search by Name"
      @input="saveSearchValue"
      @click.native="scrollToElement"
    />
    <CardList
      class="card-list-custom-styles"
      :data="people"
      :showModal="showModal"
    />
    <PulseLoader
      size="40px"
      color="#FFFF00"
      :loading="isLoading"
      class="pulse-loader-custom-styles"
    />
    <Footer text="STAR WARS CHARACTER Encyclopedia, 2019" />
    <Modal
      v-if="isModalVisible"
      :closeModal="closeModal"
      :person="getPerson"
      :isLoading="isModalLoading"
    />
  </div>
</template>

<script>
import "reset-css/reset.css";
import "roboto-fontface/css/roboto/roboto-fontface.css";

import Header from "./components/Header.vue";
import SearchField from "./components/SearchField.vue";
import CardList from "./components/CardList.vue";
import Footer from "./components/Footer.vue";
import Modal from "./components/Modal.vue";
import PulseLoader from "vue-spinner/src/PulseLoader.vue";

export default {
  name: "App",
  components: { Header, SearchField, CardList, Footer, Modal, PulseLoader },
  data() {
    return {
      searchValue: "",
      people: [],
      isLoading: true,
      isModalLoading: false,
      isModalVisible: false,
      selectedPersonUrl: null,
    };
  },
  async mounted() {
    this.people = await this.fetchPeople();
    await this.mapPeopleTo("species");
    this.isLoading = false;
  },
  computed: {
    getPerson: function () {
      let selectedPerson = this.people.find(
        (person) => person.url == this.selectedPersonUrl
      );
      return selectedPerson;
    },
  },
  methods: {
    scrollToElement: function () {
      const el = this.$el.getElementsByClassName(
        "search-field-custom-styles"
      )[0];

      if (el) {
        el.scrollIntoView({ behavior: "smooth" });
      }
    },
    mapPeopleTo: async function (key) {
      for await (const person of this.people) {
        for (let objectUrl of person[`${key}`]) {
          const response = await fetch(objectUrl);
          const object = await response.json();
          person[`${key}`] = object;
        }
      }
    },
    mapPersonTo: async function (person, key) {
      const objectArray = [];
      for await (let objectUrl of person[`${key}`]) {
        const response = await fetch(objectUrl);
        const object = await response.json();
        objectArray.push(object);
      }
      person[`${key}`] = objectArray;
    },
    fetchPeople: async function (page = 1) {
      const response = await fetch(`https://swapi.dev/api/people?page=${page}`);
      return (await response.json()).results;
    },
    saveSearchValue: function (value) {
      this.searchValue = value;
      this.scrollToElement();
    },
    showModal: async function (personUrl) {
      this.isModalLoading = this.isModalVisible = true;
      this.selectedPersonUrl = personUrl;

      const selectedPerson = this.getPerson;
      if (typeof selectedPerson.films[0] === "string")
        await this.mapPersonTo(selectedPerson, "films");
      this.isModalLoading = false;
    },
    closeModal: function () {
      this.isModalVisible = this.isModalLoading = false;
      this.selectedPersonUrl = null;
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
.pulse-loader-custom-styles {
  text-align: center;
}
</style>