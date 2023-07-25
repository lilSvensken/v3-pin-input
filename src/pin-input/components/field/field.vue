<template>
  <label class="sv-pin__filed-wrapper">
    <input
      ref="input"
      :type="getType"
      :value="value"
      class="sv-pin__filed"
      @input="onChange"
      @focus="onFocus"
    />
  </label>
</template>

<script>
export default {
  name: "field",
  emits: ["onChange", "onFocus"],
  props: {
    isFocus: Boolean,
    value: String,
    hidden: Boolean,
  },
  watch: {
    isFocus(newValue, oldValue) {
      if (newValue && !oldValue) {
        this.setFocus();
      }
    },
  },
  mounted() {
    if (this.isFocus) {
      this.setFocus();
    }
  },
  methods: {
    setFocus() {
      this.$refs["input"].focus();
      this.$refs["input"].select();
    },
    onFocus() {
      this.$refs["input"].select();
      this.$emit("onFocus");
    },
    onChange(event) {
      const sliceValue = event.target.value.slice(-1);
      this.$refs["input"].value = sliceValue;
      this.$emit("onChange", sliceValue);
    },
  },
  computed: {
    getType() {
      return this.hidden ? "password" : "number";
    },
  },
};
</script>

<style lang="scss" src="./style.scss"></style>
