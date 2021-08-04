<template>
  <div class="search-field-container">
    <input
      class="search-field"
      type="text"
      :placeholder="placeholder"
      @input="sendValueToParent"
    />
    <button class="search-field-icon">
      <img src="../assets/search-field-magnifier.svg" />
    </button>
  </div>
</template>

<script>
export default {
  name: "SearchField",
  props: {
    placeholder: String,
  },
  methods: {
    sendValueToParent: function (e) {
      this.$emit("input", e.target.value);
    },
  },
};
</script>

<style>
.search-field-container {
  display: flex;
}
.search-field {
  color: #808080;
  font-weight: 500;
  font-size: 18px;
  line-height: 21px;
  font-family: inherit;
  background-color: transparent;
  border: 0px;
  border-bottom: 1px solid #808080;
  padding-bottom: 7.5px;
  width: 100%;
}
.search-field-icon {
  background: transparent;
  border: none;
  outline: none;
  margin-left: -29px;
  padding-right: 0;
}
</style>