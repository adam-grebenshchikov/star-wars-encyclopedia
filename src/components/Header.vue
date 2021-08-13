<template>
  <header class="header">
    <img class="header__logo" src="../assets/header-star.svg" alt="STAR" />
    <h1 class="header__title">{{ text }}</h1>
    <img class="header__logo" src="../assets/header-wars.svg" alt="WARS" />
  </header>
</template>

<script>
export default {
  name: "Header",
  props: {
    text: {
      type: String,
    },
  },
};
</script>

<style>
.header {
  height: 480px;
  background: url("../assets/header-background.png") center;
  background-size: cover;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  padding: 15px;
}

.header__title {
  margin: 10px 0;
  font-weight: 700;
  font-size: 24px;
  letter-spacing: 0.1em;
  text-transform: uppercase;
  text-align: center;
}

.header__logo {
  max-width: 100%;
  height: auto;
}
</style>