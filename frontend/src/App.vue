<script setup lang="ts">
import TopBarComponent from './components/TopBarComponent.vue'
import InputComponent from './components/InputComponent.vue'
import DelayHeadComponent from './components/DelayHeadComponent.vue'
import OutputComponent from './components/OutputComponent.vue'
import BottomSection from './components/BottomSection.vue'
</script>

<template>
  <div id="app">
    <!-- Top Bar -->
    <TopBarComponent />

    <!-- Main Content Area -->
    <div class="main-container">
      <!-- Left Input -->
      <div class="section-wrapper input-section">
        <InputComponent />
      </div>

      <!-- Delay Heads -->
      <div class="section-wrapper head-1">
        <DelayHeadComponent headName="HEAD_1" />
      </div>
      <div class="section-wrapper head-2">
        <DelayHeadComponent headName="HEAD_2" />
      </div>
      <div class="section-wrapper head-3">
        <DelayHeadComponent headName="HEAD_3" />
      </div>
      <div class="section-wrapper head-4">
        <DelayHeadComponent headName="HEAD_4" />
      </div>

      <!-- Right Output -->
      <div class="section-wrapper output-section">
        <OutputComponent />
      </div>
    </div>

    <!-- Bottom Section -->
    <BottomSection />
  </div>
</template>

<style>
html,
body {
  width: 100%;
  height: 100%;
  margin: 0;
  padding: 0;
}

* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}

#app {
  width: 100%;
  height: 100%;
  display: flex;
  flex-direction: column;
  background: linear-gradient(135deg, #1e3c72 0%, #2a5298 100%);
}

.main-container {
  flex: 1;
  display: flex;
  gap: 4px;
  padding: 4px;
  overflow-x: auto;
  align-items: stretch;
}

.section-wrapper {
  display: flex;
  flex-direction: column;
  min-width: 0;
  min-height: 0;
  flex: 1;
}

.input-section {
  flex: 0 0 10%;
}

.head-1,
.head-2,
.head-3,
.head-4 {
  flex: 0 0 19.3%;
}

.output-section {
  flex: 0 0 10%;
}

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
      
      /* Text Colors */
      --text-primary: #e8eaed;
      --text-secondary: #9aa0a6;
      --text-dim: #6b7075;
      
      /* Shadows */
      --shadow-deep: 0 4px 16px rgba(0, 0, 0, 0.6);
      --shadow-medium: 0 2px 8px rgba(0, 0, 0, 0.4);
      --shadow-soft: 0 1px 4px rgba(0, 0, 0, 0.3);
      --inset-shadow: inset 0 2px 4px rgba(0, 0, 0, 0.5);
    }

    
    /* Rotary Knob Styles */
    .knob-container {
      display: flex;
      flex-direction: column;
      align-items: center;
      gap: 8px;
    }
 
    .knob-wrapper {
      position: relative;
      cursor: pointer;
      transition: transform 0.1s ease;
    }
 
    .knob-wrapper:hover {
      transform: scale(1.05);
    }
 
    .progress-ring {
      filter: drop-shadow(0 2px 4px rgba(0, 0, 0, 0.4));
    }
 
    .progress-ring-bg {
      fill: none;
      stroke: rgba(255, 255, 255, 0.05);
      stroke-linecap: round;
    }
 
    .progress-ring-fill {
      fill: none;
      stroke: url(#blueGradient);
      stroke-linecap: round;
      transition: stroke-dashoffset 0.3s ease;
    }
 
    .progress-ring-glow {
      fill: none;
      stroke: rgba(0, 212, 255, 0.4);
      stroke-linecap: round;
      filter: blur(3px);
      opacity: 0.8;
      transition: stroke-dashoffset 0.3s ease;
    }
 
    .knob-center {
      position: absolute;
      top: 50%;
      left: 50%;
      transform: translate(-50%, -50%);
      width: 60%;
      height: 60%;
      background: radial-gradient(circle at 30% 30%, #3a3d45, #1a1d24);
      border-radius: 50%;
      box-shadow: 
        inset 0 2px 4px rgba(0, 0, 0, 0.6),
        inset 0 -1px 2px rgba(255, 255, 255, 0.05),
        0 2px 8px rgba(0, 0, 0, 0.5);
    }
 
    .knob-indicator {
      position: absolute;
      width: 8px;
      height: 8px;
      background: radial-gradient(circle, #00d4ff, #4a9eff);
      border-radius: 50%;
      transform: translate(-50%, -50%);
      box-shadow: 
        0 0 8px rgba(0, 212, 255, 0.8),
        0 0 4px rgba(74, 158, 255, 0.6);
      transition: all 0.3s ease;
    }
 
    .knob-label {
      font-size: 10px;
      font-weight: 600;
      color: var(--text-secondary);
      text-transform: uppercase;
      letter-spacing: 0.5px;
    }
 
    .knob-value {
      font-size: 9px;
      font-weight: 600;
      color: var(--text-dim);
      font-family: 'Courier New', monospace;
    }
 
    /* Toggle Switch Styles */
    .toggle-container {
      display: flex;
      flex-direction: column;
      align-items: center;
      gap: 10px;
    }
 
    .toggle-wrapper {
      display: flex;
      align-items: center;
      gap: 12px;
    }
 
    .toggle-switch {
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
 
    .toggle-switch::before {
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
 
    .toggle-switch:checked {
      background: linear-gradient(90deg, var(--primary-blue), var(--primary-cyan));
      border-color: var(--primary-cyan);
      box-shadow: 
        var(--inset-shadow),
        0 0 12px var(--blue-glow);
    }
 
    .toggle-switch:checked::before {
      left: 26px;
      background: linear-gradient(145deg, #ffffff, #e8eaed);
      box-shadow: 
        0 2px 6px rgba(0, 0, 0, 0.4),
        0 0 8px rgba(0, 212, 255, 0.6);
    }
 
    .toggle-label-text {
      font-size: 11px;
      font-weight: 500;
      color: var(--text-secondary);
      transition: color 0.3s ease;
    }
 
    .toggle-switch:checked + .toggle-label-text {
      color: var(--text-primary);
    }
 
    /* Slider Styles */
    .slider-container {
      display: flex;
      flex-direction: column;
      align-items: center;
      gap: 8px;
      width: 200px;
    }
 
    .slider-wrapper {
      width: 100%;
      padding: 12px;
      background: linear-gradient(145deg, var(--metal-light), var(--metal-dark));
      border: 1px solid rgba(255, 255, 255, 0.08);
      border-radius: 8px;
      box-shadow: var(--inset-shadow), var(--shadow-soft);
    }
 
    .slider {
      width: 100%;
      height: 4px;
      -webkit-appearance: none;
      appearance: none;
      background: var(--metal-dark);
      border-radius: 2px;
      outline: none;
      cursor: pointer;
      box-shadow: var(--inset-shadow);
    }
 
    .slider::-webkit-slider-thumb {
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
 
    .slider::-webkit-slider-thumb:hover {
      transform: scale(1.1);
      box-shadow: 
        0 2px 6px rgba(0, 0, 0, 0.5),
        0 0 12px var(--cyan-glow);
    }
 
    .slider::-moz-range-thumb {
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
 
    .color-palette {
      display: grid;
      grid-template-columns: repeat(auto-fit, minmax(120px, 1fr));
      gap: 15px;
      margin-top: 20px;
    }
 
    .color-swatch {
      display: flex;
      flex-direction: column;
      align-items: center;
      gap: 8px;
    }
 
    .color-box {
      width: 80px;
      height: 80px;
      border-radius: 8px;
      box-shadow: var(--shadow-medium);
      border: 1px solid rgba(255, 255, 255, 0.1);
    }
 
    .color-name {
      font-size: 9px;
      color: var(--text-secondary);
      text-align: center;
    }
 
    .color-code {
      font-size: 8px;
      color: var(--text-dim);
      font-family: 'Courier New', monospace;
    }
</style>
