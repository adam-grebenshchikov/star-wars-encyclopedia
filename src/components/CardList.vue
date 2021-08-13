<template>
  <div class="card-list">
    <CardListItem
      v-for="person in data"
      :key="person.url"
      :person="person"
      @click.native="showModal(person.url)"
    />
  </div>
</template>

<script>
import CardListItem from "./CardListItem.vue";
export default {
  name: "CardList",
  components: {
    CardListItem,
  },
  props: {
    data: Array,
    showModal: Function,
  },
};
</script>

<style>
.card-list {
  display: flex;
  margin: auto;
  flex-wrap: wrap;
}
</style>