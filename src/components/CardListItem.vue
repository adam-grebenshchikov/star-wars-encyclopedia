<template>
  <div class="card card-list__item">
    <div class="card__content">
      <Avatar
        :letter="getPersonFirstLetterOfName"
        :background="getPersonEyeColor"
      />
      <h2 class="card__title">{{ getPersonName }}</h2>
      <h2 class="card__description">{{ getPersonSpecies }}</h2>
    </div>
  </div>
</template>

<script>
import Avatar from "./Avatar.vue";

export default {
  name: "CardListItem",
  props: {
    person: {
      type: Object,
    },
  },
  computed: {
    getPersonFirstLetterOfName: function () {
      return this.person.name.charAt(0);
    },
    getPersonEyeColor: function () {
      return this.person.eye_color;
    },
    getPersonSpecies: function () {
      return Array.isArray(this.person.species)
        ? "n/a"
        : this.person.species.name;
    },
    getPersonName: function () {
      return this.person.name;
    },
  },
  components: {
    Avatar,
  },
};
</script>

<style>
.card-list__item {
  flex: 1 1 592px;
  height: 320px;
  margin: 16px;
  background: #1a1a1a;
  border-radius: 8px;
  box-shadow: 0px 10px 30px rgba(0, 0, 0, 0.4);
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
}
.card-list__item:hover {
  box-shadow: 0px 10px 40px rgba(37, 136, 167, 0.38);
  cursor: pointer;
}
.card__title {
  font-weight: 700;
  font-size: 18px;
  line-height: 21px;
  text-align: center;
}
.card__content {
  display: flex;
  align-items: center;
  flex-direction: column;
}
.card__description {
  color: #808080;
  font-size: 13px;
  line-height: 15px;
  margin-top: 9px;
}
</style>