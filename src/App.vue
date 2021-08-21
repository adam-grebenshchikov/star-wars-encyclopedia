<template>
  <div class="app">
    <Header text="CHARACTER Encyclopedia" />
    <SearchField
      class="search-field-custom-styles"
      placeholder="Search by Name"
      @input="saveSearchValue"
      @click.native="scrollToElement"
    />
    <div
      class="loading-block"
      v-infinite-scroll="loadMoreData"
      infinite-scroll-disabled="isLoading"
    >
      <CardList
        class="card-list-custom-styles"
        :data="data.results"
        :showModal="showModal"
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
      data: {},
      isLoading: true,
      isModalLoading: false,
      isModalVisible: false,
      selectedPersonUrl: null,
    };
  },
  async mounted() {
    this.data = await this.fetchData();
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
        const searchUrl = `https://swapi.dev/api/people/?search=${this.searchValue}`;
        const response = await fetch(searchUrl, {
          mode: "cors",
          signal,
        });
        this.data = await response.json();
        await this.mapPeopleTo("species");
        this.isLoading = false;
      } catch {
        console.log();
      }
    },
  },
  computed: {
    getPerson: function () {
      let selectedPerson = this.data.results.find(
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
      for await (const person of this.data.results) {
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
      if (Array.isArray(person[`${key}`])) {
        for await (let objectUrl of person[`${key}`]) {
          const response = await fetch(objectUrl, {
            mode: "cors",
          });
          const object = await response.json();
          objectArray.push(object);
        }
        person[`${key}`] = objectArray;
      } else {
        const response = await fetch(person[`${key}`], { mode: "cors" });
        const object = await response.json();
        person[`${key}`] = object;
      }
    },
    fetchData: async function (page = 1) {
      const response = await fetch(
        `https://swapi.dev/api/people?page=${page}`,
        {
          mode: "cors",
          signal,
        }
      );
      return await response.json();
    },
    saveSearchValue: function (value) {
      this.searchValue = value;
      this.scrollToElement();
    },
    showModal: async function (personUrl) {
      this.isModalLoading = this.isModalVisible = true;
      this.selectedPersonUrl = personUrl;
      document.body.style = "overflow:hidden";

      const selectedPerson = this.getPerson;
      if (typeof selectedPerson.films[0] === "string") {
        await this.mapPersonTo(selectedPerson, "films");
        await this.mapPersonTo(selectedPerson, "homeworld");
      }
      this.isModalLoading = false;
    },
    closeModal: function () {
      this.isModalVisible = this.isModalLoading = false;
      this.selectedPersonUrl = null;
      document.body.style = "overflow: auto";
    },
    loadMoreData: async function () {
      if (this.data.next != null) {
        this.isLoading = true;
        const response = await fetch(this.data.next, { mode: "cors" });
        const nextData = await response.json();
        const results = this.data.results;
        const newResults = [...results, ...nextData.results];
        nextData.results = newResults;
        this.data = nextData;
        this.isLoading = false;
      }
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