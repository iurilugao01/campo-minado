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
  <MineField :fieldSize="4"></MineField>
</template>

<style>
Globalfield {
  height: 100%;
  width: 100%;
}
</style>
