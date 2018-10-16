<template>
  <label :class="{active: value==true}">
    <input type="checkbox" hidden
           v-on:input="$emit('update:value', $event.target.checked)">
    {{label}}
  </label>
</template>

<script>
export default {
  name: "CircleCheckbox",
  props: {
    label: String,
    value: Boolean
  }
};
</script>

<style scoped>
label {
  border: 2px solid gray;
  border-radius: 25%;
  padding-left: 2px;
  padding-right: 2px;
  margin: 2px;
}
label.active {
  background-color: lavender;
}
</style>
