<script setup lang="ts">
import { ref } from 'vue'

const isOpen = ref(false)
</script>

<template>
  <div class="preset-manager">
    <button class="preset-button" @click="isOpen = !isOpen">
      <span class="preset-icon">📋</span>
      <span class="preset-label">Presets</span>
    </button>

    <div v-if="isOpen" class="preset-dropdown">
      <p class="empty-message">No presets available</p>
    </div>
  </div>
</template>

<style scoped>
.preset-manager {
  position: relative;
}

.preset-button {
  display: flex;
  align-items: center;
  gap: 6px;
  padding: 6px 12px;
  background: rgba(74, 158, 255, 0.1);
  border: 1px solid rgba(74, 158, 255, 0.3);
  border-radius: 6px;
  color: #e0e0e0;
  font-size: 11px;
  font-weight: 600;
  cursor: pointer;
  transition: all 0.2s ease;
  backdrop-filter: blur(10px);
}

.preset-button:hover {
  background: rgba(74, 158, 255, 0.15);
  border-color: rgba(74, 158, 255, 0.5);
  transform: translateY(-1px);
  box-shadow: 0 4px 12px rgba(74, 158, 255, 0.2);
}

.preset-button:active {
  transform: translateY(0);
}

.preset-icon {
  font-size: 12px;
}

.preset-label {
  white-space: nowrap;
}

.preset-dropdown {
  position: absolute;
  top: 100%;
  left: 0;
  margin-top: 4px;
  background: linear-gradient(135deg, #2a2d3a 0%, #1f2129 100%);
  border: 1px solid rgba(74, 158, 255, 0.3);
  border-radius: 6px;
  min-width: 150px;
  padding: 8px;
  box-shadow: 0 8px 24px rgba(0, 0, 0, 0.3);
  backdrop-filter: blur(10px);
  z-index: 100;
}

.empty-message {
  margin: 0;
  padding: 8px;
  font-size: 11px;
  color: #999;
  text-align: center;
}
</style>
