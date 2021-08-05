<template>
  <div class="card-list">
    <CardListItem v-for="person in data" :key="person.url" :person="person" />
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
  },
};
</script>

<style>
.card-list {
  display: flex;
  margin: auto;
  flex-wrap: wrap;
  padding: 0 16px;
}
</style>