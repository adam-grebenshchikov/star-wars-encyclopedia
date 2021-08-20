<template>
  <div class="modal-backdrop" @click="closeModal">
    <div class="modal" @click.stop="">
      <section class="modal__control-panel">
        <button class="modal__close-button" @click="closeModal">
          <img
            class="modal__cross"
            src="../assets/modal-сross.svg"
            alt="Close"
          />
        </button>
      </section>

      <PulseLoader
        size="40px"
        color="#FFFF00"
        :loading="isLoading"
        style="text-align: center"
      />

      <section v-if="!isLoading" class="modal__person-info person-info">
        <div class="person-info__user">
          <Avatar
            :background="this.person.eye_color"
            :letter="getPersonFirstLetterOfName"
          />
          <h1 class="person-info__name">{{ this.person.name }}</h1>
        </div>

        <div class="person-info__description">
          <div class="person-info__block">
            <ul class="person-info__keys">
              <li class="person-info__item person-info__key">
                <img
                  class="person-info__icon"
                  src="../assets/modal-birth-year.svg"
                />
                Birth Day
              </li>
              <li class="person-info__item person-info__key">
                <img
                  class="person-info__icon"
                  src="../assets/modal-species.svg"
                />Species
              </li>
              <li class="person-info__item person-info__key">
                <img
                  class="person-info__icon"
                  src="../assets/modal-gender.svg"
                />Gender
              </li>
            </ul>
            <ul class="person-info__values">
              <li class="person-info__item person-info__value">
                {{ this.person.birth_year }}
              </li>
              <li class="person-info__item person-info__value">
                {{
                  this.person.species.name ? this.person.species.name : "n/a"
                }}
              </li>
              <li class="person-info__item person-info__value">
                {{ this.person.gender }}
              </li>
            </ul>
          </div>

          <div class="person-info__block">
            <ul class="person-info__keys">
              <li class="person-info__item person-info__key">
                <img
                  class="person-info__icon"
                  src="../assets/modal-homeworld.svg"
                />Homeworld
              </li>
              <li class="person-info__item person-info__key">
                <img
                  class="person-info__icon"
                  src="../assets/modal-films.svg"
                />Films
              </li>
            </ul>

            <ul class="person-info__values">
              <li class="person-info__item person-info__value">
                {{ this.person.homeworld.name }}
              </li>
              <li
                v-for="film in this.person.films"
                :key="film.url"
                class="person-info__item person-info__value"
              >
                {{ film.title }}
              </li>
            </ul>
          </div>
        </div>
      </section>
    </div>
  </div>
</template>

<script>
import Avatar from "./Avatar.vue";
import PulseLoader from "vue-spinner/src/PulseLoader.vue";

export default {
  components: { Avatar, PulseLoader },
  props: {
    closeModal: Function,
    person: Object,
    isLoading: Boolean,
  },
  computed: {
    getPersonFirstLetterOfName: function () {
      return this.person.name.charAt(0);
    },
  },
};
</script>

<style>
.modal-backdrop {
  position: fixed;
  top: 0;
  bottom: 0;
  left: 0;
  right: 0;
  background: rgba(128, 128, 128, 0.01);
  backdrop-filter: blur(30px);
  display: flex;
  justify-content: center;
  align-items: center;
  cursor: pointer;
}

.modal {
  max-height: 97vh;
  min-height: 60vh;
  flex-basis: 800px;
  background: #1a1a1a;
  border-radius: 8px;
  margin: 0 15px;
  cursor: auto;
  overflow-y: auto;
}

.modal__control-panel {
  height: 80px;
  padding: 22px;
  display: flex;
  justify-content: flex-end;
}

.modal__close-button {
  cursor: pointer;
  background: none;
  border: none;
}

.modal__cross {
  height: 12px;
  width: 12px;
}

.modal__person-info {
  padding: 0px 80px 80px 80px;
}

@media (max-width: 600px) {
  .modal__person-info {
    padding: 15px;
  }
}

.person-info__user {
  display: flex;
  align-items: center;
  padding-bottom: 80px;
  border-bottom: #808080 2px solid;
}

.person-info__name {
  margin-left: 16px;
  font-weight: 700;
  font-size: 24px;
  line-height: 28px;
  text-align: center;
}

.person-info__description {
  margin-top: 71px;
  display: flex;
  flex-wrap: wrap;
  justify-content: center;
}

.person-info__item {
  padding: 10px;
  font-size: 18px;
  line-height: 21px;
}

.person-info__block {
  display: flex;
  flex: 1;
}

.person-info__key {
  color: #808080;
  font-weight: 400;
  display: flex;
  align-items: center;
}

.person-info__value {
  color: #ffffff;
  font-weight: 700;
}

.person-info__icon {
  margin-right: 11px;
}
</style>  