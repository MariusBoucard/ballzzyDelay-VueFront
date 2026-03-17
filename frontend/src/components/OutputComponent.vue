<script setup lang="ts">
// Output component for the right side
</script>

<template>
  <div class="output-container">
    <div class="output-header">
      <h3>Output</h3>
    </div>
    <div class="output-content">
      <!-- Add output controls here -->
      <p>Output Controls</p>
    </div>
  </div>
</template>

<style scoped>
.output-container {
  width: 100%;
  height: 100%;
  display: flex;
  flex-direction: column;
  padding: 6px;
  background: linear-gradient(135deg, #2a2d3a 0%, #1f2129 100%);
  border-radius: 4px;
  border: 1px solid #444;
  box-shadow: 0 2px 6px rgba(0, 0, 0, 0.3);
  color: #e0e0e0;
}

.output-header {
  text-align: center;
  padding-bottom: 4px;
  border-bottom: 1px solid #444;
  flex-shrink: 0;
}

.output-header h3 {
  margin: 0;
  font-size: 11px;
  font-weight: 600;
  color: #4a9eff;
}

.output-content {
  flex: 1;
  display: flex;
  align-items: center;
  justify-content: center;
  padding: 4px 0;
}

.output-content p {
  margin: 0;
  font-size: 10px;
  color: #999;
}
</style>
