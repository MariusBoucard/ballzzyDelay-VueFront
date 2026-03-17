<script setup lang="ts">
import { useJuce } from '@/composables/useJuce'
import { computed } from 'vue'

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

    <!-- Pan Slider - Centered and Bigger -->
    <div class="section pan-section">
      <label>Pan</label>
      <input 
        type="range" 
        min="0" 
        max="1" 
        step="0.01"
        class="slider-large"
        :value="pan.state.normalised"
        @input="e => pan.setNormalisedValue(Number((e.target as HTMLInputElement).value))"
      />
      <span class="value-display">{{ pan.state.normalised.toFixed(2) }}</span>
    </div>

    <!-- Feedback and Time - Side by Side -->
    <div class="section split-section">
      <div class="split-control">
        <label>Feedback</label>
        <input 
          type="range" 
          min="0" 
          max="1" 
          step="0.01"
          class="slider-small"
          :value="feedback.state.normalised"
          @input="e => feedback.setNormalisedValue(Number((e.target as HTMLInputElement).value))"
        />
        <span class="value-display">{{ feedback.state.normalised.toFixed(2) }}</span>
      </div>

      <div class="split-control">
        <label>Time</label>
        <input 
          type="range" 
          min="0" 
          max="1" 
          step="0.01"
          class="slider-small"
          :value="time.state.normalised"
          @input="e => time.setNormalisedValue(Number((e.target as HTMLInputElement).value))"
        />
        <span class="value-display">{{ time.state.normalised.toFixed(2) }}</span>
      </div>
    </div>

    <!-- Gain - Centered -->
    <div class="section gain-section">
      <label>Gain</label>
      <input 
        type="range" 
        min="0" 
        max="1" 
        step="0.01"
        class="slider-medium"
        :value="gain.state.normalised"
        @input="e => gain.setNormalisedValue(Number((e.target as HTMLInputElement).value))"
      />
      <span class="value-display">{{ gain.state.normalised.toFixed(2) }}</span>
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
  </div>
</template>

<style scoped>
.delay-head-container {
  width: 200px;
  min-height: 800px;
  display: flex;
  flex-direction: column;
  gap: 12px;
  padding: 12px;
  background: linear-gradient(135deg, #2a2d3a 0%, #1f2129 100%);
  border-radius: 8px;
  border: 1px solid #444;
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.3);
  font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, sans-serif;
  color: #e0e0e0;
}

.head-header {
  text-align: center;
  padding-bottom: 8px;
  border-bottom: 1px solid #444;
}

.head-header h3 {
  margin: 0;
  font-size: 14px;
  font-weight: 600;
  color: #4a9eff;
}

.section {
  display: flex;
  flex-direction: column;
  gap: 6px;
}

.bypass-section {
  align-items: center;
}

.toggle-label {
  display: flex;
  align-items: center;
  gap: 8px;
  cursor: pointer;
  font-size: 13px;
  font-weight: 500;
  user-select: none;
}

.toggle-label input {
  width: 18px;
  height: 18px;
  cursor: pointer;
  accent-color: #4a9eff;
}

.pan-section {
  align-items: center;
  gap: 8px;
}

.pan-section label {
  font-size: 13px;
  font-weight: 600;
  color: #4a9eff;
  text-align: center;
}

.split-section {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 8px;
}

.split-control {
  display: flex;
  flex-direction: column;
  gap: 4px;
  min-width: 0;
}

.split-control label {
  font-size: 12px;
  font-weight: 600;
  color: #999;
  text-align: center;
}

.gain-section {
  align-items: center;
  gap: 8px;
}

.gain-section label {
  font-size: 13px;
  font-weight: 600;
  color: #4a9eff;
  text-align: center;
}

label {
  font-size: 12px;
  font-weight: 500;
  color: #999;
}

.slider-large {
  width: 100%;
  height: 24px;
  cursor: pointer;
  accent-color: #4a9eff;
}

.slider-medium {
  width: 100%;
  height: 20px;
  cursor: pointer;
  accent-color: #4a9eff;
}

.slider-small {
  width: 100%;
  height: 16px;
  cursor: pointer;
  accent-color: #4a9eff;
}

.value-display {
  font-size: 11px;
  color: #888;
  text-align: center;
  font-weight: 500;
}

.separator {
  height: 2px;
  background: linear-gradient(90deg, transparent, #444, transparent);
  margin: 8px 0;
}

.filter-section {
  gap: 10px;
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
  background: linear-gradient(135deg, #3a3d4a 0%, #2a2d3a 100%);
  border: 1px solid #555;
  border-radius: 6px;
  cursor: pointer;
  transition: all 0.2s ease;
  font-size: 11px;
  font-weight: 600;
  color: #ccc;
}

.filter-button:hover {
  background: linear-gradient(135deg, #4a4d5a 0%, #3a3d4a 100%);
  border-color: #666;
  transform: translateY(-1px);
}

.filter-button:active {
  transform: translateY(0);
}

.button-label {
  font-size: 11px;
  font-weight: 600;
  color: #4a9eff;
}

.filter-slider {
  width: 90%;
  height: 14px;
  cursor: pointer;
  accent-color: #4a9eff;
}

.button-value {
  font-size: 10px;
  color: #888;
}
</style>
