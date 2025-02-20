<script>
import GlobalField from "./components/GlobalField.vue";

export default {
  name: "App",
  components: {
    GlobalField,
  },
};
</script>

<template>
  <GlobalField :fieldSize="4"></GlobalField>
</template>

<style>
Globalfield {
  height: 100%;
  width: 100%;
}
</style>
