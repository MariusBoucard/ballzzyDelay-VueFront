<script setup lang="ts">
import { useJuce } from '@/composables/useJuce'
import { computed, ref } from 'vue'
import RotaryKnob from './RotaryKnob.vue'

const { getSlider, getToggle } = useJuce()

// Get global controls
const sync = computed(() => getToggle('SYNC'))
const globalPan = computed(() => getSlider('PAN'))
const globalFeedback = computed(() => getSlider('FEEDBACK'))
const globalTime = computed(() => getSlider('MOVEMENT_PERIOD_DURATION'))
const globalMix = computed(() => getSlider('MIX'))
const globalLpFilter = computed(() => getSlider('LP_FILTER_FREQ'))
const globalHpFilter = computed(() => getSlider('HP_FILTER_FREQ'))
const ducking = computed(() => getSlider('DUCKING'))

// Mode selection (local state - you can integrate with JUCE if needed)
const selectedMode = ref('normal')

const setMode = (mode: string) => {
  selectedMode.value = mode
  // You can add JUCE integration here if needed
}
</script>

<template>
  <div class="bottom-section">
    <!-- Left: SYNC Toggle -->
    <div class="control-group sync-group">
      <label class="toggle-label">
        <input 
          type="checkbox" 
          class="modern-toggle"
          :checked="sync.isActive()" 
          @change="e => sync.toggle((e.target as HTMLInputElement).checked)"
        />
        <span>SYNC</span>
      </label>
    </div>

    <!-- PAN Knob -->
    <div class="control-group">
      <RotaryKnob
        :model-value="globalPan.state.normalised"
        @update:model-value="globalPan.setNormalisedValue"
        label="Pan"
        size="small"
      />
    </div>

    <!-- Feedback & Time Stacked -->
    <div class="control-group stacked-group">
      <RotaryKnob
        :model-value="globalFeedback.state.normalised"
        @update:model-value="globalFeedback.setNormalisedValue"
        label="Feedback"
        size="small"
      />
      <RotaryKnob
        :model-value="globalTime.state.normalised"
        @update:model-value="globalTime.setNormalisedValue"
        label="Time"
        size="small"
      />
    </div>

    <!-- Center: Global MIX Knob -->
    <div class="control-group mix-group">
      <RotaryKnob
        :model-value="globalMix.state.normalised"
        @update:model-value="globalMix.setNormalisedValue"
        label="Mix"
        size="small"
      />
    </div>

    <!-- LP & HP Filter Stacked -->
    <div class="control-group stacked-group">
      <RotaryKnob
        :model-value="globalLpFilter.state.normalised"
        @update:model-value="globalLpFilter.setNormalisedValue"
        label="LP Filter"
        size="small"
      />
      <RotaryKnob
        :model-value="globalHpFilter.state.normalised"
        @update:model-value="globalHpFilter.setNormalisedValue"
        label="HP Filter"
        size="small"
      />
    </div>

    <!-- Ducking Knob -->
    <div class="control-group">
      <RotaryKnob
        :model-value="ducking.state.normalised"
        @update:model-value="ducking.setNormalisedValue"
        label="Ducking"
        size="small"
      />
    </div>

    <!-- Right: Mode Selection (3D Radio Buttons) -->
    <div class="control-group mode-group">
      <div class="mode-label">MODE</div>
      <div class="mode-buttons">
        <button 
          class="mode-button"
          :class="{ active: selectedMode === 'normal' }"
          @click="setMode('normal')"
        >
          NORMAL
        </button>
        <button 
          class="mode-button"
          :class="{ active: selectedMode === 'pingpong' }"
          @click="setMode('pingpong')"
        >
          PING-PONG
        </button>
        <button 
          class="mode-button"
          :class="{ active: selectedMode === 'swap' }"
          @click="setMode('swap')"
        >
          SWAP
        </button>
      </div>
    </div>
  </div>
</template>

<style scoped>
/* Use the same CSS variables from our design system */
:root {
  --primary-blue: #4a9eff;
  --primary-cyan: #00d4ff;
  --blue-glow: rgba(74, 158, 255, 0.6);
  --cyan-glow: rgba(0, 212, 255, 0.5);
  --metal-dark: #1a1d24;
  --metal-medium: #252830;
  --metal-light: #2f3239;
  --metal-lighter: #3a3d45;
  --text-primary: #e8eaed;
  --text-secondary: #9aa0a6;
  --text-dim: #6b7075;
  --shadow-deep: 0 4px 16px rgba(0, 0, 0, 0.6);
  --inset-shadow: inset 0 2px 4px rgba(0, 0, 0, 0.5);
}

.bottom-section {
  width: 100%;
  height: 120px;
  background: linear-gradient(145deg, var(--metal-medium) 0%, var(--metal-dark) 100%);
  border-top: 1px solid rgba(74, 158, 255, 0.3);
  box-shadow: 0 -2px 8px rgba(0, 0, 0, 0.4), var(--inset-shadow);
  display: flex;
  align-items: center;
  padding: 8px 16px;
  gap: 12px;
  flex-shrink: 0;
  justify-content: space-between;
}

.control-group {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  gap: 4px;
  flex: 1;
  height: 100%;
}

.sync-group {
  flex: 0.8;
  min-width: 60px;
}

.stacked-group {
  display: flex;
  flex-direction: column;
  gap: 4px;
  flex: 1;
  align-items: center;
  justify-content: center;
}

.mix-group {
  flex: 1;
  justify-content: center;
}

.mode-group {
  flex: 1.2;
  gap: 6px;
}

/* Modern Toggle Switch */
.toggle-label {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 8px;
  cursor: pointer;
  font-size: 9px;
  font-weight: 600;
  color: var(--text-secondary);
  user-select: none;
  text-transform: uppercase;
  letter-spacing: 0.5px;
}

.toggle-label:hover {
  color: var(--text-primary);
}

.modern-toggle {
  appearance: none;
  width: 48px;
  height: 24px;
  background: var(--metal-dark);
  border-radius: 12px;
  border: 1px solid rgba(255, 255, 255, 0.1);
  cursor: pointer;
  position: relative;
  transition: all 0.3s cubic-bezier(0.4, 0, 0.2, 1);
  box-shadow: var(--inset-shadow);
}

.modern-toggle::before {
  content: '';
  position: absolute;
  width: 18px;
  height: 18px;
  border-radius: 50%;
  top: 2px;
  left: 2px;
  background: linear-gradient(145deg, var(--metal-lighter), var(--metal-light));
  box-shadow: 
    0 2px 4px rgba(0, 0, 0, 0.3),
    0 0 0 1px rgba(255, 255, 255, 0.1);
  transition: all 0.3s cubic-bezier(0.4, 0, 0.2, 1);
}

.modern-toggle:checked {
  background: linear-gradient(90deg, var(--primary-blue), var(--primary-cyan));
  border-color: var(--primary-cyan);
  box-shadow: 
    var(--inset-shadow),
    0 0 12px var(--blue-glow);
}

.modern-toggle:checked::before {
  left: 26px;
  background: linear-gradient(145deg, #ffffff, #e8eaed);
  box-shadow: 
    0 2px 6px rgba(0, 0, 0, 0.4),
    0 0 8px rgba(0, 212, 255, 0.6);
}

.modern-toggle:hover:not(:checked)::before {
  background: linear-gradient(145deg, var(--metal-lighter), var(--metal-medium));
}

/* Mode Selection - 3D Buttons */
.mode-label {
  font-size: 8px;
  font-weight: 700;
  color: var(--text-dim);
  text-transform: uppercase;
  letter-spacing: 0.5px;
  margin-bottom: 2px;
}

.mode-buttons {
  display: flex;
  flex-direction: column;
  gap: 4px;
  width: 100%;
}

.mode-button {
  padding: 6px 12px;
  font-size: 8px;
  font-weight: 700;
  text-transform: uppercase;
  letter-spacing: 0.5px;
  border: none;
  border-radius: 4px;
  cursor: pointer;
  transition: all 0.15s ease;
  user-select: none;
  color: var(--text-secondary);
  
  /* 3D raised effect */
  background: linear-gradient(145deg, var(--metal-lighter), var(--metal-light));
  box-shadow: 
    0 3px 0 var(--metal-dark),
    0 4px 6px rgba(0, 0, 0, 0.4),
    inset 0 1px 0 rgba(255, 255, 255, 0.1);
  border-top: 1px solid rgba(255, 255, 255, 0.15);
  border-left: 1px solid rgba(255, 255, 255, 0.1);
  border-right: 1px solid rgba(0, 0, 0, 0.2);
  border-bottom: 1px solid rgba(0, 0, 0, 0.3);
}

.mode-button:hover:not(.active) {
  background: linear-gradient(145deg, var(--metal-lighter), var(--metal-medium));
  transform: translateY(-1px);
  box-shadow: 
    0 4px 0 var(--metal-dark),
    0 5px 8px rgba(0, 0, 0, 0.5),
    inset 0 1px 0 rgba(255, 255, 255, 0.15);
}

.mode-button.active {
  /* 3D pressed effect */
  background: linear-gradient(145deg, var(--primary-blue), var(--primary-cyan));
  color: #ffffff;
  transform: translateY(3px);
  box-shadow: 
    0 0 0 var(--metal-dark),
    inset 0 2px 4px rgba(0, 0, 0, 0.5),
    0 0 12px var(--blue-glow);
  border-top: 1px solid rgba(0, 0, 0, 0.3);
  border-left: 1px solid rgba(0, 0, 0, 0.2);
  border-right: 1px solid rgba(255, 255, 255, 0.1);
  border-bottom: 1px solid rgba(255, 255, 255, 0.05);
  text-shadow: 0 0 6px var(--cyan-glow);
}

.mode-button:active:not(.active) {
  transform: translateY(2px);
  box-shadow: 
    0 1px 0 var(--metal-dark),
    inset 0 1px 2px rgba(0, 0, 0, 0.3);
}
</style>