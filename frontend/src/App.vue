<script setup lang="ts">
import TopBarComponent from './components/TopBarComponent.vue'
import InputComponent from './components/InputComponent.vue'
import DelayHeadComponent from './components/DelayHeadComponent.vue'
import OutputComponent from './components/OutputComponent.vue'
import BottomSection from './components/BottomSection.vue'
</script>

<template>
  <div id="app">
    <!-- Top Bar -->
    <TopBarComponent />

    <!-- Main Content Area -->
    <div class="main-container">
      <!-- Left Input -->
      <div class="section-wrapper input-section">
        <InputComponent />
      </div>

      <!-- Delay Heads -->
      <div class="section-wrapper head-1">
        <DelayHeadComponent headName="HEAD_1" />
      </div>
      <div class="section-wrapper head-2">
        <DelayHeadComponent headName="HEAD_2" />
      </div>
      <div class="section-wrapper head-3">
        <DelayHeadComponent headName="HEAD_3" />
      </div>
      <div class="section-wrapper head-4">
        <DelayHeadComponent headName="HEAD_4" />
      </div>

      <!-- Right Output -->
      <div class="section-wrapper output-section">
        <OutputComponent />
      </div>
    </div>

    <!-- Bottom Section -->
    <BottomSection />
  </div>
</template>

<style>
html,
body {
  width: 100%;
  height: 100%;
  margin: 0;
  padding: 0;
}

* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

#app {
  width: 100%;
  height: 100%;
  display: flex;
  flex-direction: column;
  background: linear-gradient(135deg, #1e3c72 0%, #2a5298 100%);
}

.main-container {
  flex: 1;
  display: flex;
  gap: 4px;
  padding: 4px;
  overflow-x: auto;
  align-items: stretch;
}

.section-wrapper {
  display: flex;
  flex-direction: column;
  min-width: 0;
  min-height: 0;
  flex: 1;
}

.input-section {
  flex: 0 0 10%;
}

.head-1,
.head-2,
.head-3,
.head-4 {
  flex: 0 0 19.3%;
}

.output-section {
  flex: 0 0 10%;
}
</style>
