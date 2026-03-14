<script setup lang="ts">
import JuceControls from './components/JuceControls.vue'
</script>

<template>
  <div id="app">
    <div >
      LETS GOOO
    </div>
    <JuceControls />
  </div>
</template>

<style>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

#app {
  min-height: 100vh;
  display: flex;
  align-items: center;
  justify-content: center;
  background: linear-gradient(135deg, #1e3c72 0%, #2a5298 100%);
  padding: 1rem;
}
</style>
