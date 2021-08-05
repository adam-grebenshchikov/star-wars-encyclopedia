<template>
  <div class="card-list__item card">
    <div class="card__content">
      <Avatar
        :letter="getFirstLetterOfName()"
        :background="getPersonEyeColor()"
      />
      <h2 class="card__title">{{ person.name }}</h2>
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
  methods: {
    getFirstLetterOfName: function () {
      return this.person.name.charAt(0);
    },
    getPersonEyeColor: function () {
      return this.person.eye_color;
    },
    getPersonSpecies: function () {
      return this.person.species.name;
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
</style>