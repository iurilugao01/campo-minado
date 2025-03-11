<script>
import MineField from "./components/MineField.vue";

export default {
  name: "App",
  components: {
    MineField,
  },
};
</script>

<template>
  <MineField :fieldSize="5"></MineField>
</template>

<style>
@import url("https://cdn.jsdelivr.net/npm/bootstrap-icons@1.11.3/font/bootstrap-icons.min.css");

MIneField {
  height: 100%;
  width: 100%;
}
</style>
