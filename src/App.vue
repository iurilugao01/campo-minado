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
  <MineField :fieldSize="10"></MineField>
</template>

<style>
Globalfield {
  height: 100%;
  width: 100%;
}
</style>
