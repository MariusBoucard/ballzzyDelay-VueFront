<script setup lang="ts">
import { useJuce } from '@/composables/useJuce'
import { computed } from 'vue'
import RotaryKnob from './RotaryKnob.vue'
import HeadFunctionComponent from './HeadFunctionComponent.vue'

interface Props {
  headName: string
}

const props = defineProps<Props>()

const { getSlider, getToggle } = useJuce()

// Helper function to construct parameter IDs
const getParam = (suffix: string) => `${props.headName}_${suffix}`

// Get all the controls for this head
const bypass = computed(() => getToggle(getParam('BYPASS')))
const pan = computed(() => getSlider(getParam('PAN')))
const feedback = computed(() => getSlider(getParam('FEEDBACK')))
const time = computed(() => getSlider(getParam('TIME')))

const movementPeriodDuration = computed(() => getSlider(getParam('MOVEMENT_PERIOD_DURATION')))
const gain = computed(() => getSlider(getParam('GAIN')))
const lpFilterFreq = computed(() => getSlider(getParam('LP_FILTER_FREQ')))
const hpFilterFreq = computed(() => getSlider(getParam('HP_FILTER_FREQ')))

// Additional controls (toggles)
const movementOn = computed(() => getToggle(getParam('MOVEMENT_ON')))
const movementWidthSlave = computed(() => getToggle(getParam('MOVEMENT_WIDTH_SLAVE')))
const feedbackSlave = computed(() => getToggle(getParam('FEEDBACK_SLAVE')))
const gainSlave = computed(() => getToggle(getParam('GAIN_SLAVE')))
const hpSlave = computed(() => getToggle(getParam('HP_SLAVE')))
const lpSlave = computed(() => getToggle(getParam('LP_SLAVE')))
</script>

<template>
  <div v-if="bypass" class="delay-head-container">
    <!-- Header with head name -->
    <div class="head-header">
      <h3>{{ headName }}</h3>
    </div>

    <!-- Bypass Toggle - Top -->
    <div class="section bypass-section">
      <label class="toggle-label">
        <input 
          type="checkbox" 
          :checked="bypass.isActive()" 
          @change="e => bypass.toggle((e.target as HTMLInputElement).checked)"
        />
        <span>Bypass</span>
      </label>
    </div>

    <!-- Pan Knob -->
    <div class="section pan-section">
      <RotaryKnob
        :model-value="pan.state.normalised"
        @update:model-value="pan.setNormalisedValue"
        label="Pan"
        size="medium"
      />
    </div>

    <!-- Feedback and Time - Knobs Side by Side -->
    <div class="section split-section">
      <RotaryKnob
        :model-value="feedback.state.normalised"
        @update:model-value="feedback.setNormalisedValue"
        label="Feedback"
        size="small"
      />
      <RotaryKnob
        :model-value="time.state.normalised"
        @update:model-value="time.setNormalisedValue"
        label="Time"
        size="small"
      />
    </div>

    <!-- Gain - Knob -->
    <div class="section gain-section">
      <RotaryKnob
        :model-value="gain.state.normalised"
        @update:model-value="gain.setNormalisedValue"
        label="Gain"
        size="medium"
      />  
    </div>

    <!-- Separator -->
    <div class="separator"></div>

    <!-- Filter Buttons Section -->
    <div class="section filter-section">
      <div class="filter-button-group">
        <button class="filter-button">
          <div class="button-label">LP Filter</div>
          <input 
            type="range" 
            min="0" 
            max="1" 
            step="0.01"
            class="filter-slider"
            :value="lpFilterFreq.state.normalised"
            @input="e => lpFilterFreq.setNormalisedValue(Number((e.target as HTMLInputElement).value))"
          />
          <div class="button-value">{{ lpFilterFreq.state.normalised.toFixed(2) }}</div>
        </button>

        <button class="filter-button">
          <div class="button-label">HP Filter</div>
          <input 
            type="range" 
            min="0" 
            max="1" 
            step="0.01"
            class="filter-slider"
            :value="hpFilterFreq.state.normalised"
            @input="e => hpFilterFreq.setNormalisedValue(Number((e.target as HTMLInputElement).value))"
          />
          <div class="button-value">{{ hpFilterFreq.state.normalised.toFixed(2) }}</div>
        </button>
      </div>
    </div>

    <!-- Head Function Component -->
    <div class="section function-section">
      <HeadFunctionComponent :headName="headName" />
    </div>
  </div>
</template>

<style scoped>
/* ============================================
   CSS VARIABLES - Main Color Palette
   ============================================ */
:root {
  /* Primary Blue Colors */
  --primary-blue: #4a9eff;
  --primary-cyan: #00d4ff;
  --blue-glow: rgba(74, 158, 255, 0.6);
  --cyan-glow: rgba(0, 212, 255, 0.5);
  
  /* Metal Grey Background */
  --metal-dark: #1a1d24;
  --metal-medium: #252830;
  --metal-light: #2f3239;
  --metal-lighter: #3a3d45;
  
  /* Accent Colors */
  --accent-active: #00d4ff;
  --accent-inactive: #4a4d55;
  
  /* Text Colors */
  --text-primary: #e8eaed;
  --text-secondary: #9aa0a6;
  --text-dim: #6b7075;
  
  /* Shadows & Effects */
  --shadow-deep: 0 4px 16px rgba(0, 0, 0, 0.6);
  --shadow-medium: 0 2px 8px rgba(0, 0, 0, 0.4);
  --shadow-soft: 0 1px 4px rgba(0, 0, 0, 0.3);
  --inset-shadow: inset 0 2px 4px rgba(0, 0, 0, 0.5);
}

/* ============================================
   Main Container
   ============================================ */
.delay-head-container {
  width: 100%;
  height: 100%;
  display: flex;
  flex-direction: column;
  gap: 12px;
  padding: 16px;
  background: 
    linear-gradient(145deg, var(--metal-medium) 0%, var(--metal-dark) 100%);
  border-radius: 8px;
  border: 1px solid rgba(255, 255, 255, 0.05);
  box-shadow: 
    var(--inset-shadow),
    var(--shadow-deep);
  font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, 'Helvetica Neue', sans-serif;
  color: var(--text-primary);
  overflow-y: auto;
  position: relative;
}

/* ============================================
   Header
   ============================================ */
.head-header {
  text-align: center;
  padding-bottom: 8px;
  border-bottom: 1px solid rgba(74, 158, 255, 0.15);
  flex-shrink: 0;
}

.head-header h3 {
  margin: 0;
  font-size: 13px;
  font-weight: 600;
  color: var(--primary-cyan);
  text-shadow: 0 0 10px var(--cyan-glow);
  letter-spacing: 1px;
  text-transform: uppercase;
}

/* ============================================
   Sections
   ============================================ */
.section {
  display: flex;
  flex-direction: column;
  gap: 8px;
}

.bypass-section {
  align-items: center;
  justify-content: center;
}

.pan-section,
.gain-section {
  align-items: center;
  gap: 8px;
}

.split-section {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 12px;
  align-items: center;
  justify-items: center;
}

/* ============================================
   Modern Bypass Toggle
   ============================================ */
.toggle-label {
  display: flex;
  align-items: center;
  gap: 10px;
  cursor: pointer;
  font-size: 11px;
  font-weight: 500;
  color: var(--text-secondary);
  user-select: none;
  transition: color 0.3s ease;
}

.toggle-label:hover {
  color: var(--text-primary);
}

.toggle-label input {
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

.toggle-label input::before {
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

.toggle-label input:checked {
  background: linear-gradient(90deg, var(--primary-blue), var(--primary-cyan));
  border-color: var(--primary-cyan);
  box-shadow: 
    var(--inset-shadow),
    0 0 12px var(--blue-glow);
}

.toggle-label input:checked::before {
  left: 26px;
  background: linear-gradient(145deg, #ffffff, #e8eaed);
  box-shadow: 
    0 2px 6px rgba(0, 0, 0, 0.4),
    0 0 8px rgba(0, 212, 255, 0.6);
}

.toggle-label input:hover:not(:checked)::before {
  background: linear-gradient(145deg, var(--metal-lighter), var(--metal-medium));
}

/* ============================================
   Separator
   ============================================ */
.separator {
  height: 1px;
  background: linear-gradient(90deg, 
    transparent, 
    rgba(74, 158, 255, 0.3), 
    transparent);
  margin: 4px 0;
  flex-shrink: 0;
  box-shadow: 0 0 4px rgba(74, 158, 255, 0.2);
}

/* ============================================
   Modern Filter Buttons with Sliders
   ============================================ */
.filter-section {
  gap: 8px;
}

.filter-button-group {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 8px;
}

.filter-button {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 6px;
  padding: 10px 8px;
  background: linear-gradient(145deg, var(--metal-light), var(--metal-dark));
  border: 1px solid rgba(255, 255, 255, 0.08);
  border-radius: 6px;
  cursor: pointer;
  transition: all 0.2s ease;
  box-shadow: 
    var(--inset-shadow),
    var(--shadow-soft);
}

.filter-button:hover {
  background: linear-gradient(145deg, var(--metal-lighter), var(--metal-medium));
  border-color: rgba(74, 158, 255, 0.3);
  box-shadow: 
    var(--inset-shadow),
    0 0 8px rgba(74, 158, 255, 0.2);
}

.filter-button:active {
  transform: scale(0.98);
}

.button-label {
  font-size: 9px;
  font-weight: 600;
  color: var(--primary-cyan);
  text-transform: uppercase;
  letter-spacing: 0.5px;
  text-shadow: 0 0 6px var(--cyan-glow);
}

.filter-slider {
  width: 90%;
  height: 4px;
  -webkit-appearance: none;
  appearance: none;
  background: var(--metal-dark);
  border-radius: 2px;
  outline: none;
  cursor: pointer;
  box-shadow: var(--inset-shadow);
}

.filter-slider::-webkit-slider-thumb {
  -webkit-appearance: none;
  appearance: none;
  width: 14px;
  height: 14px;
  border-radius: 50%;
  background: linear-gradient(145deg, var(--primary-cyan), var(--primary-blue));
  cursor: pointer;
  box-shadow: 
    0 2px 4px rgba(0, 0, 0, 0.4),
    0 0 8px var(--blue-glow);
  transition: all 0.2s ease;
}

.filter-slider::-webkit-slider-thumb:hover {
  transform: scale(1.1);
  box-shadow: 
    0 2px 6px rgba(0, 0, 0, 0.5),
    0 0 12px var(--cyan-glow);
}

.filter-slider::-moz-range-thumb {
  width: 14px;
  height: 14px;
  border-radius: 50%;
  background: linear-gradient(145deg, var(--primary-cyan), var(--primary-blue));
  cursor: pointer;
  border: none;
  box-shadow: 
    0 2px 4px rgba(0, 0, 0, 0.4),
    0 0 8px var(--blue-glow);
  transition: all 0.2s ease;
}

.filter-slider::-moz-range-thumb:hover {
  transform: scale(1.1);
  box-shadow: 
    0 2px 6px rgba(0, 0, 0, 0.5),
    0 0 12px var(--cyan-glow);
}

.button-value {
  font-size: 8px;
  font-weight: 600;
  color: var(--text-secondary);
  font-family: 'Courier New', monospace;
}

/* ============================================
   Function Section
   ============================================ */
.function-section {
  gap: 6px;
  flex: 1;
  min-height: 0;
}

/* ============================================
   Labels & Text
   ============================================ */
label {
  font-size: 9px;
  font-weight: 600;
  color: var(--text-dim);
  text-transform: uppercase;
  letter-spacing: 0.5px;
}

.value-display {
  font-size: 8px;
  color: var(--text-secondary);
  text-align: center;
  font-weight: 600;
  font-family: 'Courier New', monospace;
}
</style>