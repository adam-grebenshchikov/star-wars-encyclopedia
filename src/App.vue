<template>
  <div class="app">
    <Header text="CHARACTER Encyclopedia" />
    <SearchField
      class="search-field-custom-styles"
      placeholder="Search by Name"
      @input="saveSearchValue"
      @click.native="scrollToElement"
    />
    <div class="loading-block">
      <CardList
        class="card-list-custom-styles"
        :data="people"
        :showModal="showModal"
        v-show="!isLoading"
      />
      <PulseLoader
        size="40px"
        color="#FFFF00"
        :loading="isLoading"
        class="pulse-loader-custom-styles"
      />
    </div>
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

import PulseLoader from "vue-spinner/src/PulseLoader.vue";
import SearchField from "./components/SearchField.vue";
import CardList from "./components/CardList.vue";
import Header from "./components/Header.vue";
import Footer from "./components/Footer.vue";
import Modal from "./components/Modal.vue";

let controller = new AbortController();
let { signal } = controller;

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
  watch: {
    searchValue: async function () {
      controller.abort();
      controller = new AbortController();
      signal = controller.signal;
      this.isLoading = true;

      try {
        const response = await fetch(
          `https://swapi.dev/api/people/?search=${this.searchValue}`,
          {
            mode: "cors",
            signal,
          }
        );
        this.people = (await response.json()).results;
        this.isLoading = false;
      } catch {
        console.log();
      }
    },
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
          const response = await fetch(objectUrl, {
            mode: "cors",
            signal,
          });
          const object = await response.json();
          person[`${key}`] = object;
        }
      }
    },
    mapPersonTo: async function (person, key) {
      const objectArray = [];
      for await (let objectUrl of person[`${key}`]) {
        const response = await fetch(objectUrl, {
          mode: "cors",
        });
        const object = await response.json();
        objectArray.push(object);
      }
      person[`${key}`] = objectArray;
    },
    fetchPeople: async function (page = 1) {
      const response = await fetch(
        `https://swapi.dev/api/people?page=${page}`,
        {
          mode: "cors",
          signal,
        }
      );
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
  position: sticky;
  top: 0;
  padding: 80px 16px;
  background-color: #333;
  justify-content: center;
}

.card-list-custom-styles {
  max-width: 1280px;
}

.pulse-loader-custom-styles {
  text-align: center;
  position: sticky;
  position: -webkit-sticky;
  top: 200px;
}

.loading-block {
  min-height: calc(100vh - 190.3px - 160px);
}
</style>