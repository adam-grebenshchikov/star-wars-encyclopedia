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

      <section v-if="isLoading">
        <h1>Загрузка!</h1>
      </section>
    </div>
  </div>
</template>

<script>
export default {
  props: {
    closeModal: Function,
    person: Object,
    isLoading: Boolean,
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
  height: 492px;
  flex-basis: 800px;
  background: #1a1a1a;
  border-radius: 8px;
  margin: 0 15px;
  cursor: auto;
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
</style>