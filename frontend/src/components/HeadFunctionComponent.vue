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
.head-function-container {
  display: flex;
  flex-direction: column;
  gap: 3px;
}

.function-header {
  display: flex;
  gap: 3px;
  align-items: center;
}

.function-select {
  flex: 1;
  padding: 3px 4px;
  background: linear-gradient(135deg, #3a3d4a 0%, #2a2d3a 100%);
  border: 1px solid #555;
  border-radius: 3px;
  color: #e0e0e0;
  font-size: 8px;
  font-weight: 600;
  cursor: pointer;
}

.function-select:focus {
  outline: none;
  border-color: #4a9eff;
  box-shadow: 0 0 4px rgba(74, 158, 255, 0.3);
}

.function-select option {
  background: #2a2d3a;
  color: #e0e0e0;
}

.toggle-label {
  display: flex;
  align-items: center;
  gap: 2px;
  cursor: pointer;
  font-size: 8px;
  font-weight: 600;
  color: #4a9eff;
  user-select: none;
  white-space: nowrap;
}

.toggle-label input {
  width: 10px;
  height: 10px;
  cursor: pointer;
  accent-color: #4a9eff;
  flex-shrink: 0;
}

.function-canvas {
  width: 100%;
  height: 80px;
  background: #1f2129;
  border: 1px solid #444;
  border-radius: 3px;
  display: block;
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
  color: #999;
}

.slider-width {
  width: 100%;
  height: 8px;
  cursor: pointer;
  accent-color: #4a9eff;
}

.value-display {
  font-size: 7px;
  color: #888;
  font-weight: 500;
}
</style>
