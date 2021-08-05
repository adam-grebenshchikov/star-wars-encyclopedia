<template>
  <div class="avatar" :style="cssProps()">
    <span>{{ letter }}</span>
  </div>
</template>

<script>
export default {
  name: "Avatar",
  props: {
    letter: {
      type: String,
    },
    background: {
      type: String,
    },
  },
  methods: {
    cssProps: function () {
      return {
        background: this.background.split("-")[0],
      };
    },
  },
};
</script>

<style>
.avatar {
  width: 80px;
  height: 80px;
  border-radius: 999px;
  text-align: center;
  display: flex;
  justify-content: center;
  align-items: center;
  margin-bottom: 10px;
  font-weight: 500;
  font-size: 48px;
  line-height: 56px;
}
</style>