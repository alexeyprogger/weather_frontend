<template>
  <div id="app">
    <div class="fixed fixed_top">Умная система прогнозирования</div>
    <div class="grid">
      <PredictionForm model-type="svm"/>
      <PredictionForm model-type="nn"/>
    </div>
    <div class="fixed fixed_bottom">© 2025</div>
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

<style lang="scss">
body {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  margin: 0;
  padding: 0;
  background-color: #121212;
}

#app {
  display: flex;
  gap: 15px;
  position: relative;
  align-items: center;
  height: 100vh;
  width: 100vw;
  word-break: break-all;
}

.grid {
  display: grid;
  grid-template-columns: repeat(2, 1fr);
  gap: 15px;
  margin-left: 10vw;
  margin-right: 10vw;
  position: relative;
  align-items: center;
  width: 100%;
  word-break: break-all;
}

.fixed {
  width: 100%; 
  height: 40px; 
  background-color: black; 
  position: fixed; 
  z-index: 1;  
  left: 0; 
  color: #E0E0E0; 
  text-align: center;
  display: flex; 
  justify-content: center; 
  align-items: center;
  &_top {
    top: 0;
  }
  &_bottom {
    bottom: 0;
  }
}
</style>
