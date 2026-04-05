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
const bypass = computed(() => getToggle(getParam('ON')))
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
const gainSlave = computed(() => getToggle(getParam('GAIN_  SLAVE')))
const hpBp = computed(() => getToggle(getParam('HP_FILTER_BYPASS')))
const lpBp = computed(() => getToggle(getParam('LP_FILTER_BYPASS')))
</script>

<template>
  <div v-if="bypass" class="delay-head-container">
    <!-- Header with head name -->
    <div class="head-header">
      <h3>{{ headName }}</h3>
      <div class="section bypass-section">
        <label class="toggle-label">
          <span>Bypass</span>
          <input 
            type="checkbox" 
            :checked="bypass.isActive()" 
            @change="e => bypass.toggle((e.target as HTMLInputElement).checked)"
          />
        </label>
      </div>
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

    <!-- Filter Section -->
<div class="section filter-section">
  
  <!-- LP Filter Group -->
  <div class="filter-group">
    <div class="filter-row">
      <label class="filter-label">LP</label>
      <div class="slider-container">
        <input 
          type="range" min="0" max="1" step="0.01" class="filter-slider"
          :value="lpFilterFreq.state.normalised"
          @input="e => lpFilterFreq.setNormalisedValue(Number((e.target as HTMLInputElement).value))"
          :style="{ '--fill-percent': (lpFilterFreq.state.normalised * 100) + '%' }"
        />
      </div>
      <span class="slider-value">{{ (lpFilterFreq.state.normalised * 100).toFixed(0) }}</span>
    </div>
    
    <div class="bypass-container">
      <span class="bypass-text">Bypass</span>
      <label class="custom-checkbox">
        <input 
          type="checkbox" 
          :checked="lpBp.isActive()" 
          @change="e => lpBp.toggle((e.target as HTMLInputElement).checked)"
        />
        <span class="checkmark"></span>
      </label>
    </div>
  </div>

  <!-- HP Filter Group -->
  <div class="filter-group">
    <div class="filter-row">
      <label class="filter-label">HP</label>
      <div class="slider-container">
        <input 
          type="range" min="0" max="1" step="0.01" class="filter-slider"
          :value="hpFilterFreq.state.normalised"
          @input="e => hpFilterFreq.setNormalisedValue(Number((e.target as HTMLInputElement).value))"
          :style="{ '--fill-percent': (hpFilterFreq.state.normalised * 100) + '%' }"
        />
      </div>
      <span class="slider-value">{{ (hpFilterFreq.state.normalised * 100).toFixed(0) }}</span>
    </div>
    
    <div class="bypass-container">
      <span class="bypass-text">Bypass</span>
      <label class="custom-checkbox">
        <input 
          type="checkbox" 
          :checked="hpBp.isActive()" 
          @change="e => hpBp.toggle((e.target as HTMLInputElement).checked)"
        />
        <span class="checkmark"></span>
      </label>
    </div>
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
  overflow-y: hidden;
  position: relative;
}

/* ============================================
   Header
   ============================================ */
.head-header {
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding-bottom: 8px;
  border-bottom: 1px solid rgba(74, 158, 255, 0.15);
  flex-shrink: 0;
  gap: 12px;
}

.head-header h3 {
  margin: 0;
  font-size: 13px;
  font-weight: 600;
  color: var(--primary-cyan);
  text-shadow: 0 0 10px var(--cyan-glow);
  letter-spacing: 1px;
  text-transform: uppercase;
  flex: 1;
  text-align: center;
}

.bypass-section {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  gap: 2px;
  flex-shrink: 0;
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
  transform: translateY(-10px);
  gap: 8px;
}

.split-section {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 40px;
  transform: translateX(-10px) translateY(-15px);
  align-items: center;
  justify-items: center;
}

/* ============================================
   Modern Bypass Toggle
   ============================================ */
.toggle-label {
  display: flex;
  flex-direction: column;
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
  width: 24px;
  height: 12px;
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
  width: 9px;
  height: 9px;
  border-radius: 50%;
  top: 0.4px;
  left: 1px;
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
    0 0 6px var(--blue-glow);
}

.toggle-label input:checked::before {
  left: 13px;
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
   Modern Filter Sliders - Full Width Stacked
   ============================================ */
.filter-section {
  display: flex;
  flex-direction: column;
  gap: 12px; /* Space between LP and HP blocks */
  width: 100%;
}

.filter-group {
  display: flex;
  flex-direction: column;
  gap: 4px; /* Space between slider and its bypass row */
  width: 100%;
}

.filter-row {
  display: flex;
  align-items: center;
  gap: 8px;
  width: 100%;
}

/* Bypass Row Styling */
.bypass-container {
  display: flex;
  
  align-items: center;
  padding-left: 24px; /* Aligns "Bypass" text under the start of the slider */
  padding-right: 4px;
}
.bypass-text {
  font-size: 8px;
  font-weight: 600;
  color: var(--text-secondary);
  text-transform: uppercase;
  letter-spacing: 1px;
}

/* Custom Themed Checkbox */
.custom-checkbox {
  position: relative;
  display: inline-block;
  width: 14px;
  height: 14px;
  margin-left: 15px;
}

.custom-checkbox input {
  opacity: 0;
  width: 0;
  height: 0;
}

.checkmark {
  position: absolute;
  top: 0;
  left: 0;
  height: 14px;
  width: 14px;
  padding: 5px;
  background-color: var(--metal-dark);
  border: 1px solid rgba(74, 158, 255, 0.3);
  border-radius: 2px;
  transition: all 0.2s ease;
  cursor: pointer;
}

/* Checked state */
.custom-checkbox input:checked + .checkmark {
  background-color: rgba(0, 212, 255, 0.1);
  border-color: var(--primary-cyan);
  box-shadow: 0 0 8px var(--blue-glow);
}

/* The checkmark indicator */
.checkmark:after {
  content: "";
  position: absolute;
  display: none;
  left: 4px;
  top: 1px;
  width: 3px;
  height: 7px;
  border: solid var(--primary-cyan);
  border-width: 0 2px 2px 0;
  transform: rotate(45deg);
}

.custom-checkbox input:checked + .checkmark:after {
  display: block;
}

/* Existing slider styles remain same, just ensure .filter-label min-width is consistent */
.filter-label {
  font-size: 9px;
  font-weight: 700;
  color: var(--text-secondary);
  min-width: 16px;
}

.slider-value {
  font-size: 8px;
  font-weight: 600;
  color: var(--primary-cyan);
  min-width: 24px;
  text-align: right;
  font-family: 'Courier New', monospace;
  text-shadow: 0 0 4px var(--blue-glow);
}

.slider-container {
  flex: 1;
  position: relative;
  display: flex;
  align-items: center;
}

.filter-slider {
  --fill-percent: 0%;
  appearance: none;
  width: 100%;
  height: 6px;
  background: linear-gradient(90deg,
    var(--primary-blue) 0%,
    var(--primary-blue) var(--fill-percent),
    var(--metal-dark) var(--fill-percent),
    var(--metal-dark) 100%
  );
  border-radius: 3px;
  border: 1px solid rgba(74, 158, 255, 0.2);
  cursor: pointer;
  outline: none;
  box-shadow: inset 0 1px 2px rgba(0, 0, 0, 0.4);
  transition: border-color 0.2s ease, box-shadow 0.2s ease;
}

.filter-slider:hover {
  border-color: rgba(74, 158, 255, 0.4);
  box-shadow: inset 0 1px 2px rgba(0, 0, 0, 0.4), 0 0 6px rgba(74, 158, 255, 0.15);
}

.filter-slider::-webkit-slider-thumb {
  -webkit-appearance: none;
  appearance: none;
  width: 10px;
  height: 16px;
  background: linear-gradient(90deg, var(--primary-cyan) 0%, var(--primary-blue) 100%);
  border: 1px solid rgba(0, 212, 255, 0.6);
  cursor: pointer;
  box-shadow: 0 0 8px var(--blue-glow);
  position: relative;
}

.filter-slider::-webkit-slider-thumb:hover {
  box-shadow: 0 0 12px var(--blue-glow), 0 0 20px rgba(74, 158, 255, 0.4);
  background: linear-gradient(90deg, #00ffff 0%, var(--primary-cyan) 100%);
}

.filter-slider::-moz-range-thumb {
  width: 10px;
  height: 16px;
  background: linear-gradient(90deg, var(--primary-cyan) 0%, var(--primary-blue) 100%);
  border: 1px solid rgba(0, 212, 255, 0.6);
  cursor: pointer;
  box-shadow: 0 0 8px var(--blue-glow);
  position: relative;
}

.filter-slider::-moz-range-thumb:hover {
  box-shadow: 0 0 12px var(--blue-glow), 0 0 20px rgba(74, 158, 255, 0.4);
  background: linear-gradient(90deg, #00ffff 0%, var(--primary-cyan) 100%);
}

.filter-slider::-moz-range-track {
  background: transparent;
  border: none;
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