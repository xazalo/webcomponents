<template>
  <div :class="$style.showroomContainer">
    <header :class="$style.header">
      <h1>SYSTEM_TELEMETRY</h1>
      <p>STATUS_MONITORS // REAL-TIME_METRICS</p>
    </header>

    <div :class="$style.stack">
      <section :class="$style.section">
        <span :class="$style.tag">Bento UI // Modular Glass</span>
        <BentoStatusbar color="#6366f1" />
      </section>

      <section :class="$style.section">
        <span :class="$style.tag">Neo-Brutalism // Solid Impact</span>
        <BrutalismStatusbar color="#fcee0a" />
      </section>

      <section :class="[$style.section, $style.darkBg]">
        <span :class="$style.tag">Cyberpunk // Glitch HUD</span>
        <CyberStatusbar />
      </section>

      <section :class="[$style.section, $style.glassGradient]">
        <span :class="$style.tag">Glassmorphism // Crystal Line</span>
        <GlassStatusbar />
      </section>

      <section :class="[$style.section, $style.neuBg]">
        <span :class="$style.tag">Neumorphism // Soft Groove</span>
        <NeuStatusbar />
      </section>

      <section :class="[$style.section, $style.clayBg]">
        <span :class="$style.tag">Claymorphism // 3D Pill</span>
        <ClayStatusbar color="#f472b6" />
      </section>

      <section :class="[$style.section, $style.darkBg]">
        <span :class="$style.tag">Speed // Tachometer HUD</span>
        <SpeedStatusbar />
      </section>

      <section :class="$style.section">
        <span :class="$style.tag">Bubble Style // Organic Float</span>
        <BubbleStatusbar />
      </section>
    </div>
  </div>
</template>

<script setup lang="ts">
import { 
  BentoStatusbar, 
  BrutalismStatusbar, 
  BubbleStatusbar, 
  ClayStatusbar, 
  CyberStatusbar, 
  GlassStatusbar, 
  NeuStatusbar, 
  SpeedStatusbar 
} from "../components/statusbars"
</script>

<style module>
.showroomContainer {
  min-height: 100vh;
  padding: 60px 20px;
  background-color: #f3f4f6;
  font-family: 'Inter', sans-serif;
}

.header {
  text-align: center;
  margin-bottom: 60px;
}

.header h1 {
  font-size: 2.2rem;
  font-weight: 900;
  letter-spacing: -1px;
  color: #111827;
}

.stack {
  display: flex;
  flex-direction: column;
  gap: 40px;
  max-width: 1000px;
  margin: 0 auto;
}

.section {
  display: flex;
  flex-direction: column;
  gap: 16px;
  padding: 30px;
  background: white;
  border-radius: 24px;
  border: 1px solid rgba(0,0,0,0.05);
}

.tag {
  font-size: 0.7rem;
  font-weight: 800;
  text-transform: uppercase;
  color: #9ca3af;
  letter-spacing: 1px;
}

/* Contextos visuales */
.darkBg { background: #0a0a0b; border: none; }
.neuBg { background: #e0e5ec; border: none; }
.clayBg { background: #eff6ff; border: none; }
.glassGradient { 
  background: linear-gradient(135deg, #4338ca 0%, #7e22ce 100%); 
  border: none;
}

@media (max-width: 768px) {
  .section { padding: 15px; overflow-x: auto; }
}
</style>