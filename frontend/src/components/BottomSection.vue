<script setup lang="ts">
// Bottom section component
</script>

<template>
  <div class="bottom-section">
    <div class="bottom-content">
      <p>© 2026 Delay Vue</p>
      <div class="bottom-controls">
        <!-- Add any footer controls here -->
      </div>
    </div>
  </div>
</template>

<style scoped>
.bottom-section {
  width: 100%;
  height: 35px;
  background: linear-gradient(135deg, #1a1d2e 0%, #16192b 100%);
  border-top: 1px solid #4a9eff;
  box-shadow: 0 -2px 6px rgba(0, 0, 0, 0.4);
  display: flex;
  align-items: center;
  padding: 0 8px;
  flex-shrink: 0;
}

.bottom-content {
  width: 100%;
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.bottom-content p {
  margin: 0;
  font-size: 10px;
  color: #999;
  white-space: nowrap;
  overflow: hidden;
  text-overflow: ellipsis;
}

.bottom-controls {
  display: flex;
  gap: 6px;
  align-items: center;
}
</style>
