# pin-input
#### PIN-Input is a Vue component o capture PIN/MPIN like input

![screen.png](src%2Fpin-input%2Fimg%2Fscreen.png)

## Demo
### Live example [here](https://codesandbox.io/p/github/lilSvensken/pin-input/)

## Installation
```
npm install v3-pin-input --save
```

## Usage
```js
// index.js
import { createApp } from "vue";
import App from "./App.vue";
import PinInput from "/node_modules/pin-input/pin-input.es";
import "/node_modules/pin-input/style.css";

const app = createApp(App);
app.use(PinInput);
app.mount("#app");
```
```html
<!-- vue component -->
<template>
  <form @submit="onSubmit">
    <pin-input
      :count="6"
      :defaultValue="inputValue"
      :hidden="false"
      @on-change="onChange"
      @on-error="onError"
    />
  </form>
</template>

<script>
  export default {
    name: "component",
    data() {
      return {
        inputValue: 6,
      };
    },
    methods: {
      onChange(newInputValue) {
        this.inputValue = Number(newInputValue);
      },
      onClear() {
        this.inputValue = "";
      },
      onError(message) {
        alert(message);
      },
      onSubmit(e) {
        e.preventDefault();
        alert(`newInputValue: ${this.inputValue}`);
      },
    },
  };
</script>
```

## Props:

| Property                | Type    | Description                                                         |
|:------------------------|:--------|:--------------------------------------------------------------------|
| count                   | number  | the number of symbols in the pin code                               |
| defaultValue            | string  | Optional parameter. The default value to be entered in the inputs   |
| hidden                  | boolean | Do I need to hide characters during user input (`type="password"`)  |

## Methods:

| Property   | Event   | Description                                                               |
|:-----------|:--------|:--------------------------------------------------------------------------|
| @on-change | string  | triggered when the user changes the field. Returns the new pin-code value |
| @on-error  | string  | Triggered when data is entered incorrectly. Returns an error message      |
