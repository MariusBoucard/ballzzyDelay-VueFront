<script setup lang="ts">
import { useJuce } from '@/composables/useJuce'
import { computed } from 'vue'
import RotaryKnob from './RotaryKnob.vue'

const { getSlider, getToggle } = useJuce()

// Ducking controls
const ducking = computed(() => getToggle('DUCKING'))
const duckingAttack = computed(() => getSlider('DUCKING_ATTACK'))
const duckingRelease = computed(() => getSlider('DUCKING_RELEASE'))
const duckingThreshold = computed(() => getSlider('DUCKING_THRESHOLD'))
const duckingRatio = computed(() => getSlider('DUCKING_RATIO'))
</script>

<template>
  <!-- Ducking Card: Toggle + 4 Rotary Knobs -->
  <div class="ducking-card">
    <div class="ducking-knobs">

      <RotaryKnob
        :model-value="duckingAttack.state.normalised"
        @update:model-value="duckingAttack.setNormalisedValue"
        label="Attack"
        size="small"
        :min="0"
        :max="500"
        unit="ms"
        value-position="left"
      />
                  <RotaryKnob
        :model-value="duckingRatio.state.normalised"
        @update:model-value="duckingRatio.setNormalisedValue"
        label="Ratio"
        size="small"
        :min="1"
        :max="20"
        unit="1:"
        value-position="right"
      />
      <RotaryKnob
        :model-value="duckingRelease.state.normalised"
        @update:model-value="duckingRelease.setNormalisedValue"
        label="Release"
        size="small"
        value-position="left"
        :min="5"
        :max="2000"
        unit="ms"
      />

      <RotaryKnob
        :model-value="duckingThreshold.state.normalised"
        @update:model-value="duckingThreshold.setNormalisedValue"
        label="Threshold"
        size="small"
        value-position="right"
        :min="-60"
        :max="0"
        unit="dB"
      />
    </div>
    
    <label class="ducking-toggle-label">
      <input 
        type="checkbox" 
        class="ducking-toggle"
        :checked="ducking.isActive()" 
        @change="e => ducking.toggle((e.target as HTMLInputElement).checked)"
      />
      <span>DUCKING</span>
    </label>
  </div>
</template>

<style scoped>
/* CSS Variables - Color Palette */
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

/* Ducking 3D Card */
.ducking-card {
  display: flex;
  width: 100%;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  margin-right: 10px;
  margin-left: 10px;
  gap: 2px;
  flex: 2;
  height: 100%;
  padding: 4px 8px;
  background: linear-gradient(135deg, rgba(74, 158, 255, 0.15) 0%, rgba(0, 212, 255, 0.08) 100%);
  border: 1px solid rgba(74, 158, 255, 0.3);
  border-radius: 8px;
  box-shadow: 
    0 8px 16px rgba(0, 0, 0, 0.4),
    inset 0 1px 0 rgba(255, 255, 255, 0.1),
    0 0 20px rgba(74, 158, 255, 0.2);
  position: relative;
  overflow: visible;
}

.ducking-card:has(.ducking-toggle:not(:checked))::after {
  content: '';
  position: absolute;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background: rgba(128, 128, 128, 0.4);
  pointer-events: none;
  z-index: 10;
}

.ducking-card::before {
  content: '';
  position: absolute;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background: linear-gradient(135deg, transparent 0%, rgba(0, 212, 255, 0.05) 100%);
  pointer-events: none;
}

.ducking-toggle-label {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 2px;
  cursor: pointer;
  font-size: 7px;
  font-weight: 700;
  color: var(--text-secondary);
  user-select: none;
  text-transform: uppercase;
  letter-spacing: 0.5px;
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  z-index: 20;
 /* background: linear-gradient(135deg, rgba(37, 40, 48, 0.95) 0%, rgba(31, 34, 41, 0.95) 100%);*/
  padding: 4px 6px;
  border-radius: 6px;
 /* border: 1px solid rgba(74, 158, 255, 0.4);*/
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.5);
}

.ducking-toggle-label:hover {
  color: var(--primary-cyan);
}

.ducking-toggle {
  appearance: none;
  width: 32px;
  height: 16px;
  background: var(--metal-dark);
  border-radius: 8px;
  border: 1px solid rgba(255, 255, 255, 0.1);
  cursor: pointer;
  position: relative;
  transition: all 0.3s cubic-bezier(0.4, 0, 0.2, 1);
  box-shadow: 
    inset 0 2px 4px rgba(0, 0, 0, 0.5),
    0 0 8px rgba(0, 0, 0, 0.3);
}

.ducking-toggle::before {
  content: '';
  position: absolute;
  width: 10px;
  height: 10px;
  border-radius: 50%; 
  top: 2px;
  left: 2px;
  background: linear-gradient(145deg, var(--metal-lighter), var(--metal-light));
  box-shadow: 
    0 2px 4px rgba(0, 0, 0, 0.3),
    0 0 0 1px rgba(255, 255, 255, 0.1);
  transition: all 0.3s cubic-bezier(0.4, 0, 0.2, 1);
}

.ducking-toggle:checked {
  background: linear-gradient(90deg, var(--primary-blue), var(--primary-cyan));
  border-color: var(--primary-cyan);
  box-shadow: 
    inset 0 2px 4px rgba(0, 0, 0, 0.3),
    0 0 12px var(--blue-glow);
}

.ducking-toggle:checked::before {
  left: 18px;
  background: linear-gradient(145deg, #ffffff, #e8eaed);
  box-shadow: 
    0 2px 6px rgba(0, 0, 0, 0.4),
    0 0 8px rgba(0, 212, 255, 0.6);
}

.ducking-toggle:hover:not(:checked)::before {
  background: linear-gradient(145deg, var(--metal-lighter), var(--metal-medium));
}

.ducking-knobs {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 2px;
  align-items: center;
  justify-items: center;
  width: 100%;
  flex: 1;
  position: relative;
}
</style>
