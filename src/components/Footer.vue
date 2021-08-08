<template>
  <footer class="footer">
    <h2 class="footer__title">{{ text }}</h2>
  </footer>
</template>

<script>
export default {
  name: "Footer",
  props: {
    text: {
      type: String,
    },
  },
};
</script>

<style>
.footer {
  background: #1a1a1a;
  padding: 50px 0;
  margin-top: 160px;
}
.footer__title {
  text-transform: uppercase;
  text-align: center;
  letter-spacing: 0.11em;
  line-height: 21px;
  font-weight: 700;
}
</style>