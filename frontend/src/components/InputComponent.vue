<script setup lang="ts">
// Input component for the left side
</script>

<template>
  <div class="input-container">
    <div class="input-header">
      <h3>Input</h3>
    </div>
    <div class="input-content">
      <!-- Add input controls here -->
      <p>Input Controls</p>
    </div>
  </div>
</template>

<style scoped>
.input-container {
  width: 100%;
  height: 100%;
  display: flex;
  flex-direction: column;
  padding: 12px;
  background: linear-gradient(135deg, #2a2d3a 0%, #1f2129 100%);
  border-radius: 8px;
  border: 1px solid #444;
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.3);
  color: #e0e0e0;
}

.input-header {
  text-align: center;
  padding-bottom: 12px;
  border-bottom: 1px solid #444;
}

.input-header h3 {
  margin: 0;
  font-size: 16px;
  font-weight: 600;
  color: #4a9eff;
}

.input-content {
  flex: 1;
  display: flex;
  align-items: center;
  justify-content: center;
  padding: 1rem 0;
}

.input-content p {
  margin: 0;
  font-size: 12px;
  color: #999;
}
</style>
