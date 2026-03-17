<script setup lang="ts">
import { useJuce } from '@/composables/useJuce'
import { computed } from 'vue'

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
</script>

<template>
  <div class="bottom-section">
    <!-- Left: SYNC Button -->
    <div class="control-group sync-group">
      <label class="toggle-label">
        <input 
          type="checkbox" 
          :checked="sync.isActive()" 
          @change="e => sync.toggle((e.target as HTMLInputElement).checked)"
        />
        <span>SYNC</span>
      </label>
    </div>

    <!-- PAN Slider -->
    <div class="control-group pan-group">
      <label>PAN</label>
      <input 
        type="range" 
        min="0" 
        max="1" 
        step="0.01"
        class="slider-horizontal"
        :value="globalPan.state.normalised"
        @input="e => globalPan.setNormalisedValue(Number((e.target as HTMLInputElement).value))"
      />
      <span class="value-display">{{ globalPan.state.normalised.toFixed(2) }}</span>
    </div>

    <!-- Feedback and Time Stacked -->
    <div class="control-group stacked-group">
      <div class="stacked-control">
        <label>FB</label>
        <input 
          type="range" 
          min="0" 
          max="1" 
          step="0.01"
          class="slider-small"
          :value="globalFeedback.state.normalised"
          @input="e => globalFeedback.setNormalisedValue(Number((e.target as HTMLInputElement).value))"
        />
        <span class="value-display">{{ globalFeedback.state.normalised.toFixed(2) }}</span>
      </div>
      <div class="stacked-control">
        <label>TM</label>
        <input 
          type="range" 
          min="0" 
          max="1" 
          step="0.01"
          class="slider-small"
          :value="globalTime.state.normalised"
          @input="e => globalTime.setNormalisedValue(Number((e.target as HTMLInputElement).value))"
        />
        <span class="value-display">{{ globalTime.state.normalised.toFixed(2) }}</span>
      </div>
    </div>

    <!-- Center: Global MIX Knob -->
    <div class="control-group mix-group">
      <label>MIX</label>
      <input 
        type="range" 
        min="0" 
        max="1" 
        step="0.01"
        class="slider-mix"
        :value="globalMix.state.normalised"
        @input="e => globalMix.setNormalisedValue(Number((e.target as HTMLInputElement).value))"
      />
      <span class="value-display">{{ globalMix.state.normalised.toFixed(2) }}</span>
    </div>

    <!-- Filter Buttons Stacked -->
    <div class="control-group filters-group">
      <button class="filter-btn">
        <span class="filter-label">LP</span>
        <input 
          type="range" 
          min="0" 
          max="1" 
          step="0.01"
          class="filter-slider"
          :value="globalLpFilter.state.normalised"
          @input="e => globalLpFilter.setNormalisedValue(Number((e.target as HTMLInputElement).value))"
        />
      </button>
      <button class="filter-btn">
        <span class="filter-label">HP</span>
        <input 
          type="range" 
          min="0" 
          max="1" 
          step="0.01"
          class="filter-slider"
          :value="globalHpFilter.state.normalised"
          @input="e => globalHpFilter.setNormalisedValue(Number((e.target as HTMLInputElement).value))"
        />
      </button>
    </div>

    <!-- Right: Ducking Knob -->
    <div class="control-group ducking-group">
      <label>DUCK</label>
      <input 
        type="range" 
        min="0" 
        max="1" 
        step="0.01"
        class="slider-horizontal"
        :value="ducking.state.normalised"
        @input="e => ducking.setNormalisedValue(Number((e.target as HTMLInputElement).value))"
      />
      <span class="value-display">{{ ducking.state.normalised.toFixed(2) }}</span>
    </div>
  </div>
</template>

<style scoped>
.bottom-section {
  width: 100%;
  height: 50px;
  background: linear-gradient(135deg, #1a1d2e 0%, #16192b 100%);
  border-top: 1px solid #4a9eff;
  box-shadow: 0 -2px 6px rgba(0, 0, 0, 0.4);
  display: flex;
  align-items: center;
  padding: 4px 8px;
  gap: 8px;
  flex-shrink: 0;
  justify-content: space-between;
}

.control-group {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  gap: 2px;
  flex: 1;
  height: 100%;
  min-width: 40px;
}

.sync-group {
  flex: 0.8;
}

.pan-group {
  flex: 1.2;
}

.stacked-group {
  display: flex;
  gap: 2px;
  flex: 1.2;
}

.stacked-control {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  gap: 1px;
  flex: 1;
  min-width: 35px;
}

.stacked-control label {
  font-size: 8px;
  font-weight: 600;
  color: #999;
}

.mix-group {
  flex: 1.5;
}

.mix-group label {
  font-size: 9px;
  font-weight: 600;
  color: #4a9eff;
}

.filters-group {
  display: flex;
  gap: 2px;
  flex: 1.2;
}

.filter-btn {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  gap: 1px;
  padding: 2px 4px;
  background: linear-gradient(135deg, #3a3d4a 0%, #2a2d3a 100%);
  border: 1px solid #555;
  border-radius: 2px;
  cursor: pointer;
  transition: all 0.2s ease;
  font-size: 8px;
  font-weight: 600;
  color: #ccc;
  flex: 1;
  min-width: 35px;
}

.filter-btn:hover {
  background: linear-gradient(135deg, #4a4d5a 0%, #3a3d4a 100%);
  border-color: #666;
  transform: translateY(-1px);
}

.filter-btn:active {
  transform: translateY(0);
}

.filter-label {
  font-size: 8px;
  font-weight: 600;
  color: #4a9eff;
}

.filter-slider {
  width: 30px;
  height: 6px;
  cursor: pointer;
  accent-color: #4a9eff;
}

.ducking-group {
  flex: 1.2;
}

label {
  font-size: 8px;
  font-weight: 600;
  color: #999;
}

.toggle-label {
  display: flex;
  align-items: center;
  gap: 4px;
  cursor: pointer;
  font-size: 9px;
  font-weight: 600;
  color: #4a9eff;
  user-select: none;
  white-space: nowrap;
}

.toggle-label input {
  width: 12px;
  height: 12px;
  cursor: pointer;
  accent-color: #4a9eff;
  flex-shrink: 0;
}

.slider-horizontal {
  width: 100%;
  height: 8px;
  cursor: pointer;
  accent-color: #4a9eff;
}

.slider-small {
  width: 100%;
  height: 6px;
  cursor: pointer;
  accent-color: #4a9eff;
}

.slider-mix {
  width: 100%;
  height: 10px;
  cursor: pointer;
  accent-color: #4a9eff;
}

.value-display {
  font-size: 7px;
  color: #888;
  font-weight: 500;
}
</style>
