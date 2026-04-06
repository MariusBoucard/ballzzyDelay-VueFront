<script setup lang="ts">
import { computed, ref, onUnmounted } from 'vue'

interface Props {
  modelValue: number  // 0 to 1
  label?: string
  size?: 'small' | 'medium' | 'large'
  min?: number
  max?: number
  step?: number
  unit?: string
  valuePosition?: 'top' | 'bottom' | 'right' | 'hidden'
}

const props = withDefaults(defineProps<Props>(), {
  label: '',
  size: 'medium',
  min: 0,
  unit: '',
  max: 1,
  step: 0.01
})

const emit = defineEmits<{
  'update:modelValue': [value: number]
}>()

const isDragging = ref(false)
const startY = ref(0)
const startValue = ref(0)

// Size configurations
const sizeConfig = {
  /*small: { size: 50, ringWidth: 4 },
  medium: { size: 70, ringWidth: 5 },
  large: { size: 90, ringWidth: 6 }*/

   small: { size: 30, ringWidth: 4 },
  medium: { size: 50, ringWidth: 5 },
  large: { size: 70, ringWidth: 6 }
}

const config = computed(() => sizeConfig[props.size])

// Calculate rotation angle (270 degrees range, starting from -135deg to +135deg)
const rotationAngle = computed(() => {
  return -135 + (props.modelValue * 270)
})

// Calculate the conic gradient for progress ring
// Use a simple 0-270deg gradient and rotate the container to align it
const progressGradient = computed(() => {
  const fillDeg = props.modelValue * 270 // How much to fill (0 to 270 degrees)
  
  return `conic-gradient(
    var(--primary-cyan) 0deg,
    var(--primary-blue) ${fillDeg}deg,
    transparent ${fillDeg}deg
  )`
})

// Rotate the progress ring to align with the indicator starting position
const progressRotation = computed(() => {
  return 225 // Start at 225deg to align with -135deg indicator position
})

const handleMouseDown = (e: MouseEvent) => {
  isDragging.value = true
  startY.value = e.clientY
  startValue.value = props.modelValue
  console.log("Mouse down on knob, starting value:", startValue.value)
  document.addEventListener('mousemove', handleMouseMove)
  document.addEventListener('mouseup', handleMouseUp)
  e.preventDefault()
}

const handleMouseMove = (e: MouseEvent) => {
  if (!isDragging.value) return
  
  const deltaY = startY.value - e.clientY
  const sensitivity = 0.003 // Adjust sensitivity
  let newValue = startValue.value + (deltaY * sensitivity)
  
  // Clamp between 0 and 1
  newValue = Math.max(0, Math.min(1, newValue))
  console.log("Mouse move, deltaY:", deltaY, "new value:", newValue)
  emit('update:modelValue', newValue)
}

const handleMouseUp = () => {
  isDragging.value = false
  document.removeEventListener('mousemove', handleMouseMove)
  document.removeEventListener('mouseup', handleMouseUp)
}

const handleWheel = (e: WheelEvent) => {
  e.preventDefault()
  const delta = e.deltaY > 0 ? -0.01 : 0.01
  let newValue = props.modelValue + delta
  newValue = Math.max(0, Math.min(1, newValue))
  emit('update:modelValue', newValue)
}

// Cleanup on unmount
onUnmounted(() => {
  document.removeEventListener('mousemove', handleMouseMove)
  document.removeEventListener('mouseup', handleMouseUp)
})

// Format the display value
const displayValue = computed(() => {
  const value = props.min + (props.modelValue * (props.max - props.min))
  return value.toFixed(2)
})
</script>

<template>
  <div class="rotary-knob-container">
    <div 
      class="knob-wrapper"
      :class="{ 'is-dragging': isDragging, [`size-${size}`]: true }"
      :style="{ width: config.size + 'px', height: config.size + 'px' }"
      @mousedown="handleMouseDown"
      @wheel="handleWheel"
    >
      <!-- Background ring (inactive portion) -->
      <div 
        class="ring-background"
        :style="{ transform: `rotate(${progressRotation}deg)` }"
      ></div>
      
      <!-- Progress ring (active portion) -->
      <div 
        class="ring-progress"
        :style="{ 
          background: progressGradient,
          transform: `rotate(${progressRotation}deg)`
        }"
      ></div>
      
      <!-- Inner space to create ring effect -->
      <div class="ring-space"></div>
      
      <!-- Central knob with indicator -->
      <div class="knob-center">
        <div 
          class="knob-indicator-track"
          :style="{ transform: `rotate(${rotationAngle}deg)` }"
        >   

          <div class="knob-indicator"></div>
        </div>

      </div>
               <div v-if="valuePosition === 'right'" class="knob-value" style="margin-left: 56px;">{{ displayValue }}
               </div>

    </div>
    
    <!-- Label -->
    <div v-if="label" class="knob-label">{{ label }}</div>
    
    <!-- Value display -->
    <div v-if="valuePosition !== 'hidden' &&   valuePosition !== 'right'" class="knob-value">{{ displayValue }}
      <span v-if="unit" class="knob-unit">{{ unit }}</span>

    </div>
  </div>
</template>

<style scoped>
.rotary-knob-container {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 3px;
  user-select: none;
}

.knob-wrapper {
  position: relative;
  cursor: pointer;
  transition: transform 0.1s ease;
  display: flex;
  align-items: center;
  justify-content: center;
}

.knob-wrapper:hover {
  transform: scale(1.02);
}

.knob-wrapper.is-dragging {
  cursor: grabbing;
}

.knob-wrapper:active {
  transform: scale(0.98);
}

/* Background ring - the inactive track */
.ring-background {
  position: absolute;
  width: 100%;
  height: 100%;
  border-radius: 50%;
  background: conic-gradient(
    rgba(255, 255, 255, 0.05) 0deg,
    rgba(255, 255, 255, 0.05) 270deg,
    transparent 270deg
  );
}

/* Progress ring - the active portion */
.ring-progress {
  position: absolute;
  width: 100%;
  height: 100%;
  border-radius: 50%;
  transition: background 0.05s ease;
  filter: drop-shadow(0 0 6px var(--blue-glow));
}

/* Inner space - creates the ring effect */
.ring-space {
  position: absolute;
  width: 70%;
  height: 70%;
  border-radius: 50%;
  background: linear-gradient(145deg, var(--metal-medium), var(--metal-dark));
  box-shadow: var(--inset-shadow);
}

/* Central knob body */
.knob-center {
  position: absolute;
  width: 60%;
  height: 60%;
  border-radius: 50%;
  background: radial-gradient(circle at 30% 30%, var(--metal-lighter), var(--metal-light));
  box-shadow: 
    inset 0 2px 4px rgba(0, 0, 0, 0.6),
    inset 0 -1px 2px rgba(255, 255, 255, 0.05),
    0 2px 8px rgba(0, 0, 0, 0.5);
}

/* Indicator track (rotates around center) */
.knob-indicator-track {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  transition: transform 0.05s ease;
}

/* Indicator dot */
.knob-indicator {
  position: absolute;
  top: 8%;
  left: 50%;
  transform: translateX(-50%);
  width: 8px; 
  height: 8px;
  background: radial-gradient(circle, var(--primary-cyan), var(--primary-blue));
  border-radius: 50%;
  box-shadow: 
    0 0 8px var(--cyan-glow),
    0 0 4px var(--blue-glow);
}

.size-small .knob-indicator {
  width: 5px;
  height: 5px;
}

.size-medium .knob-indicator {
  width: 7px;
  height: 7px;
}

.size-large .knob-indicator {
  width: 9px;
  height: 9px;
}

/* Label */
.knob-label {
  font-size: 9px;
  font-weight: 600;
  color: var(--text-secondary);
  text-transform: uppercase;
  letter-spacing: 0.5px;
  text-align: center;
}

/* Value Display */
.knob-value {
  font-size: 8px;
  font-weight: 600;
  color: var(--text-dim);
  font-family: 'Courier New', monospace;
  text-align: center;
}
</style>