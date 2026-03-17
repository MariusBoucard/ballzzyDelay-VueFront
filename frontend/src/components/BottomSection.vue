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
  height: 70px;
  background: linear-gradient(135deg, #1a1d2e 0%, #16192b 100%);
  border-top: 2px solid #4a9eff;
  box-shadow: 0 -4px 12px rgba(0, 0, 0, 0.4);
  display: flex;
  align-items: center;
  padding: 0 2rem;
}

.bottom-content {
  width: 100%;
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.bottom-content p {
  margin: 0;
  font-size: 12px;
  color: #999;
}

.bottom-controls {
  display: flex;
  gap: 1rem;
  align-items: center;
}
</style>
