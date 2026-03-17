<script setup lang="ts">
import { useJuce } from '@/composables/useJuce'

const { initData } = useJuce()
</script>

<template>
  <div class="topbar">
    <div class="topbar-content">
      <h1 class="app-title">{{ initData?.pluginName || 'Delay Vue' }}</h1>
      <div class="topbar-controls">
        <!-- Add any topbar controls here -->
      </div>
    </div>
  </div>
</template>

<style scoped>
.topbar {
  width: 100%;
  height: 35px;
  background: linear-gradient(135deg, #1a1d2e 0%, #16192b 100%);
  border-bottom: 1px solid #4a9eff;
  box-shadow: 0 2px 6px rgba(0, 0, 0, 0.4);
  display: flex;
  align-items: center;
  padding: 0 8px;
  flex-shrink: 0;
}

.topbar-content {
  width: 100%;
  display: flex;
  justify-content: space-between;
  align-items: center;
}

.app-title {
  margin: 0;
  font-size: 13px;
  font-weight: 700;
  color: #e0e0e0;
  letter-spacing: 0.3px;
  white-space: nowrap;
  overflow: hidden;
  text-overflow: ellipsis;
}

.topbar-controls {
  display: flex;
  gap: 6px;
  align-items: center;
}
</style>
