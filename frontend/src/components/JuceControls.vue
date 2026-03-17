<script setup lang="ts">
import { useJuce } from '@/composables/useJuce'
import { onMounted, ref } from 'vue'

const { getSlider, getToggle, initData, callNativeFunction, onBackendEvent } = useJuce()

// Initialize individual controls
const gain = getSlider('GAIN')
const bypass = getToggle('BYPASS')
const serverLog = ref('')
const mix = getSlider("MIX");

onMounted(() => {
  // Listen for that example event from your C++ code
  onBackendEvent('exampleEvent', (data) => {
    console.log('Received from C++:', data)
  })
})

const handleNativeClick = async () => {
  const result = await callNativeFunction('nativeFunction', 'Hello', 123)
  serverLog.value = result
}
</script>

<template>
  <div v-if="initData" class="plugin-container">
    <h1>{{ initData.pluginName }}</h1>
    
    <div class="control">
      <label>Gain</label>
      <input 
        type="range" 
        min="0" 
        max="1" 
        step="0.01"
        :value="gain.state.normalised"
        @input="e => gain.setNormalisedValue(Number((e.target as HTMLInputElement).value))"
      />
      <span>{{ gain.state.normalised.toFixed(2) }}</span>
    </div>

    <div class="control">
      <label>Bypass</label>
      <input 
        type="checkbox" 
        :checked="bypass.isActive()" 
        @change="e => bypass.toggle((e.target as HTMLInputElement).checked)"
      />
    </div>

    <div class="control">
      <label>Mix</label>
      <input 
      type="range"
        min="0" 
        max="100" 
        step="0.01"
        :value="mix.state.value "
        @input="e => mix.setNormalisedValue(Number((e.target as HTMLInputElement).value)/100    )"
      />
      <span>{{ mix.state.normalised.toFixed(2) }}</span>
    </div>
    <button @click="handleNativeClick">Call C++ Function</button>
    <p>Last result: {{ serverLog }}</p>
  </div>
</template>