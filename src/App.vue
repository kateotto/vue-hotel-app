<template>
  <div class="app">
    <Container />
  </div>
</template>

<script>
import Container from "./components/Container.vue";

export default {
  name: "App",
  components: {
    Container
  }
};
</script>

<style>
.app {
  display: flex;
  flex-wrap: wrap;
  justify-content: center;
}
</style>
