<template>
  <div class="sv-pin">
    <field
      v-for="index in count"
      :key="index"
      :isFocus="isFocus(index)"
      :value="controller[index - 1]"
      :hidden="hidden"
      @on-focus="onFocusField(index - 1)"
      @on-change="onChangeInputField(index - 1, $event)"
      @keydown.delete="clearField($event)"
      @keydown.arrow-left="downArrowLeft"
      @keydown.arrow-right="downArrowRight"
      @paste="onPaste"
    />
  </div>
</template>

<script>
import Field from "@/pin-input/components/field/field.vue";

export default {
  name: "pin-input",
  components: { Field },
  props: {
    defaultValue: [String, Number],
    count: Number,
    hidden: Boolean,
  },
  emits: ["onChange", "onError"],
  data() {
    return {
      controller: [],
      numActiveField: 0,
    };
  },
  watch: {
    defaultValue(newValue) {
      if (!newValue) {
        this.setDefaultController();
      }
    },
  },
  mounted() {
    this.setDefaultController();
  },
  methods: {
    setDefaultController() {
      this.controller = new Array(this.count).fill("");

      if (this.defaultValue || this.defaultValue === 0) {
        const defaultValueReverse = String(this.defaultValue)
          .split("")
          .slice(0, this.count)
          .reverse();

        defaultValueReverse.forEach((item, index) => {
          const indexLastEmptyController = this.controller.length - (index + 1);
          this.controller[indexLastEmptyController] = item;
        });
        this.updateController();
      }
    },
    isFocus(index) {
      return index - 1 === this.numActiveField;
    },
    onChangeInputField(index, fieldValue) {
      this.controller[index] = fieldValue;
      this.updateController();
      this.nextField();
    },
    onFocusField(index) {
      this.numActiveField = index;
    },
    prevField() {
      if (this.numActiveField > 0) {
        this.numActiveField--;
      }
    },
    nextField() {
      if (this.numActiveField < this.count - 1) {
        this.numActiveField++;
      }
    },
    downArrowLeft() {
      setTimeout(() => {
        this.prevField();
      });
    },
    downArrowRight() {
      setTimeout(() => {
        this.nextField();
      });
    },
    clearField(event) {
      event.preventDefault();
      this.controller[this.numActiveField] = "";
      this.updateController();
      this.prevField();
    },
    onPaste(event) {
      event.preventDefault();
      let paste = (event.clipboardData || window.clipboardData).getData("text");
      if (Number(paste)) {
        this.controller.forEach((item, index) => {
          if (paste && index === this.numActiveField) {
            this.controller[this.numActiveField] = paste[0];
            paste = paste.slice(1);
            this.nextField();
          }
        });
        this.updateController();
      } else {
        this.$emit("onError", "Некорректный ввод");
      }
    },
    updateController() {
      let newInputValue = this.controller
        .map((item) => (item === "" ? "0" : item))
        .join("");
      this.$emit("onChange", newInputValue);
    },
  },
};
</script>

<style lang="scss" src="./style.scss"></style>
