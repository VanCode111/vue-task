<template>
  <button v-on:click="oClick" class="btn">{{ text }}</button>
</template>

<script>
export default {
  name: "Button",
  props: {
    text: String,
    oClick: Function,
  },
  methods: {
    click() {
      console.log("jhello");
    },
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">
$color-blue: #1d73bc;
.btn {
  background-color: $color-blue;
  padding: 10px 20px;
  color: white;
  border: none;
  cursor: pointer;
  border-radius: 4px;
  margin-bottom: 20px;
}
</style>
