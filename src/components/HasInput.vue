<template>
  <div>
    {{label}}
    <label :class="{active: on==true}"
         v-on:click="clickedOn"> Yes </label>
    <label :class="{active: off==true}"
         v-on:click="clickedOff"> No </label>
  </div>
</template>

<script>
export default {
  name: "HasInput",
  props: {
    label: String
  },
  data() { return {
    on: false,
    off: false,
  }},
  methods: {
    clickedOn() {
      this.on = !this.on;
      if(this.on) {
        this.off = false;
      }
      this.$emit('input', this.status())
    },
    clickedOff() {
      this.off = !this.off;
      if(this.off) {
        this.on = false;
      }
      this.$emit('input', this.status())
    },
    status() {
      if(this.on && !this.off) {
        return true;
      }
      if(!this.on && this.off) {
        return false;
      }
      return null;
    }
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
