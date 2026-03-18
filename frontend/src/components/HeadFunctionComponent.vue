<script setup lang="ts">
import { useJuce } from '@/composables/useJuce'
import { computed, ref, onMounted } from 'vue'

interface Props {
  headName: string
}

const props = defineProps<Props>()

const { getSlider, getToggle } = useJuce()

const getParam = (suffix: string) => `${props.headName}_${suffix}`

// Controls
const movementFunction = ref('sine')
const functionOptions = ['sine', 'square', 'triangle']
const movementOn = computed(() => getToggle(getParam('MOVEMENT_ON')))
const movementWidth = computed(() => getSlider(getParam('MOVEMENT_WIDTH')))

const canvasRef = ref<HTMLCanvasElement | null>(null)

const drawFunction = () => {
  if (!canvasRef.value) return
  
  const canvas = canvasRef.value
  const ctx = canvas.getContext('2d')
  if (!ctx) return

  const width = canvas.width
  const height = canvas.height
  const centerY = height / 2

  // Clear canvas
  ctx.fillStyle = '#1f2129'
  ctx.fillRect(0, 0, width, height)

  // Draw grid
  ctx.strokeStyle = 'rgba(74, 158, 255, 0.1)'
  ctx.lineWidth = 1
  for (let i = 0; i < width; i += 20) {
    ctx.beginPath()
    ctx.moveTo(i, 0)
    ctx.lineTo(i, height)
    ctx.stroke()
  }
  for (let i = 0; i < height; i += 10) {
    ctx.beginPath()
    ctx.moveTo(0, i)
    ctx.lineTo(width, i)
    ctx.stroke()
  }

  // Draw center line
  ctx.strokeStyle = 'rgba(74, 158, 255, 0.3)'
  ctx.lineWidth = 1
  ctx.beginPath()
  ctx.moveTo(0, centerY)
  ctx.lineTo(width, centerY)
  ctx.stroke()

  // Draw function
  ctx.strokeStyle = '#4a9eff'
  ctx.lineWidth = 2
  ctx.beginPath()

  const amplitude = (height - 4) / 2
  const xScale = width / (2 * Math.PI)
  const yScale = amplitude

  for (let x = 0; x < width; x++) {
    const t = (x / xScale) - Math.PI
    let y

    switch (movementFunction.value) {
      case 'sine':
        y = Math.sin(t)
        break
      case 'square':
        y = Math.sign(Math.sin(t))
        break
      case 'triangle':
        y = Math.asin(Math.sin(t)) * (2 / Math.PI)
        break
      default:
        y = Math.sin(t)
    }

    const canvasY = centerY - y * yScale

    if (x === 0) {
      ctx.moveTo(x, canvasY)
    } else {
      ctx.lineTo(x, canvasY)
    }
  }

  ctx.stroke()
}

onMounted(() => {
  drawFunction()
})

const handleFunctionChange = () => {
  drawFunction()
}
</script>

<template>
  <div class="head-function-container">
    <!-- Header with Dropdown and Bypass -->
    <div class="function-header">
      <select v-model="movementFunction" @change="handleFunctionChange" class="function-select">
        <option v-for="opt in functionOptions" :key="opt" :value="opt">
          {{ opt.toUpperCase() }}
        </option>
      </select>
      <label class="toggle-label">
        <input 
          type="checkbox" 
          :checked="movementOn.isActive()" 
          @change="e => movementOn.toggle((e.target as HTMLInputElement).checked)"
        />
        <span>ON</span>
      </label>
    </div>

    <!-- Canvas for drawing function -->
    <canvas 
      ref="canvasRef"
      class="function-canvas"
      width="160"
      height="80"
    ></canvas>

    <!-- Width Slider -->
    <div class="width-control">
      <label>Width</label>
      <input 
        type="range" 
        min="0" 
        max="1" 
        step="0.01"
        class="slider-width"
        :value="movementWidth.state.normalised"
        @input="e => movementWidth.setNormalisedValue(Number((e.target as HTMLInputElement).value))"
      />
      <span class="value-display">{{ movementWidth.state.normalised.toFixed(2) }}</span>
    </div>
  </div>
</template>

<style scoped>
:root {
  --primary-color: #4a9eff;
  --primary-dark: #357abd;
  --primary-light: #6db3ff;
  --text-primary: #e0e0e0;
  --text-secondary: #999;
  --border-color: #444;
}

.head-function-container {
  display: flex;
  flex-direction: column;
  gap: 3px;
  padding: 4px;
  background: linear-gradient(135deg, rgba(42, 45, 58, 0.5), rgba(31, 33, 41, 0.5));
  border: 1px solid rgba(74, 158, 255, 0.15);
  border-radius: 4px;
}

.function-header {
  display: flex;
  gap: 3px;
  align-items: center;
}

.function-select {
  flex: 1;
  padding: 4px 6px;
  background: linear-gradient(135deg, #3a3d4a 0%, #2a2d3a 100%);
  border: 1px solid rgba(74, 158, 255, 0.3);
  border-radius: 3px;
  color: var(--text-primary);
  font-size: 8px;
  font-weight: 600;
  cursor: pointer;
  transition: all 0.2s ease;
  box-shadow: inset 0 1px 2px rgba(0, 0, 0, 0.3);
}

.function-select:hover {
  border-color: var(--primary-color);
  background: linear-gradient(135deg, #4a4d5a 0%, #3a3d4a 100%);
}

.function-select:focus {
  outline: none;
  border-color: var(--primary-color);
  box-shadow: 
    inset 0 1px 2px rgba(0, 0, 0, 0.3),
    0 0 6px rgba(74, 158, 255, 0.3);
}

.function-select option {
  background: #2a2d3a;
  color: var(--text-primary);
}

.toggle-label {
  display: flex;
  align-items: center;
  gap: 2px;
  cursor: pointer;
  font-size: 8px;
  font-weight: 600;
  color: var(--primary-color);
  user-select: none;
  white-space: nowrap;
}

.toggle-label input {
  appearance: none;
  width: 14px;
  height: 14px;
  border-radius: 50%;
  border: 1px solid var(--border-color);
  background: linear-gradient(135deg, #2a2d3a 0%, #1f2129 100%);
  cursor: pointer;
  transition: all 0.2s ease;
  box-shadow: inset 0 1px 1px rgba(0, 0, 0, 0.5);
}

.toggle-label input:checked {
  background: radial-gradient(circle at 30% 30%, rgba(46, 204, 113, 0.3), rgba(46, 204, 113, 0.1));
  border-color: #2ecc71;
  box-shadow: 0 0 6px rgba(46, 204, 113, 0.4);
}

.function-canvas {
  width: 100%;
  height: 80px;
  background: linear-gradient(135deg, #1f2129 0%, #16192b 100%);
  border: 1px solid rgba(74, 158, 255, 0.2);
  border-radius: 3px;
  display: block;
  box-shadow: inset 0 1px 3px rgba(0, 0, 0, 0.5);
}

.width-control {
  display: flex;
  flex-direction: column;
  gap: 2px;
  align-items: center;
}

.width-control label {
  font-size: 8px;
  font-weight: 600;
  color: var(--text-secondary);
  text-transform: uppercase;
}

.slider-width {
  appearance: none;
  width: 100%;
  height: 12px;
  cursor: pointer;
  background: transparent;
  border: none;
  padding: 0;
  accent-color: var(--primary-color);
}

/* Glowy width slider - WebKit */
.slider-width::-webkit-slider-thumb {
  appearance: none;
  width: 18px;
  height: 18px;
  border-radius: 50%;
  background: radial-gradient(circle at 35% 35%, rgba(109, 179, 255, 0.8), var(--primary-color));
  border: 2px solid var(--primary-light);
  cursor: pointer;
  box-shadow: 
    0 0 12px rgba(74, 158, 255, 0.6),
    0 0 24px rgba(74, 158, 255, 0.3),
    inset 0 1px 2px rgba(255, 255, 255, 0.2),
    0 2px 4px rgba(0, 0, 0, 0.5);
  transition: all 0.2s ease;
}

.slider-width::-webkit-slider-thumb:active {
  transform: scale(1.15);
  box-shadow: 
    0 0 16px rgba(74, 158, 255, 0.8),
    0 0 32px rgba(74, 158, 255, 0.5),
    inset 0 1px 2px rgba(255, 255, 255, 0.2),
    0 2px 4px rgba(0, 0, 0, 0.5);
}

.slider-width::-webkit-slider-runnable-track {
  background: linear-gradient(90deg, 
    rgba(74, 158, 255, 0.15) 0%,
    rgba(74, 158, 255, 0.3) 50%,
    rgba(74, 158, 255, 0.15) 100%);
  height: 100%;
  border-radius: 4px;
  box-shadow: 
    inset 0 1px 2px rgba(0, 0, 0, 0.3),
    0 0 8px rgba(74, 158, 255, 0.2);
}

/* Glowy width slider - Firefox */
.slider-width::-moz-range-thumb {
  width: 18px;
  height: 18px;
  border-radius: 50%;
  background: radial-gradient(circle at 35% 35%, rgba(109, 179, 255, 0.8), var(--primary-color));
  border: 2px solid var(--primary-light);
  cursor: pointer;
  box-shadow: 
    0 0 12px rgba(74, 158, 255, 0.6),
    0 0 24px rgba(74, 158, 255, 0.3),
    inset 0 1px 2px rgba(255, 255, 255, 0.2),
    0 2px 4px rgba(0, 0, 0, 0.5);
}

.slider-width::-moz-range-track {
  background: linear-gradient(90deg, 
    rgba(74, 158, 255, 0.15) 0%,
    rgba(74, 158, 255, 0.3) 50%,
    rgba(74, 158, 255, 0.15) 100%);
  border-radius: 4px;
  border: none;
  box-shadow: 
    inset 0 1px 2px rgba(0, 0, 0, 0.3),
    0 0 8px rgba(74, 158, 255, 0.2);
}

.value-display {
  font-size: 7px;
  color: var(--text-secondary);
  font-weight: 500;
}
</style>
