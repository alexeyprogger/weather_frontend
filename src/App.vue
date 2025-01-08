<template>
  <div id="app">
    <PredictionForm />
  </div>
</template>

<script>
import PredictionForm from "./components/PredictionForm.vue";

export default {
  name: "App",
  components: {
    PredictionForm,
  },
};
</script>

<style>
body {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  margin: 0;
  padding: 0;
  background-color: #f3f4f6;
}

#app {
  display: flex;
  justify-content: center;
  align-items: center;
  height: 100vh;
}
</style>
