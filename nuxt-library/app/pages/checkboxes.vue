<template>
  <div :class="$style.showroomContainer">
    <header :class="$style.header">
      <h1>INPUT_SELECTIONS</h1>
      <p>CHECKBOX_COLLECTION // STATE_MANAGEMENT</p>
    </header>

    <div :class="$style.grid">
      <section :class="$style.card">
        <span :class="$style.tag">Bento UI // Modern Tiles</span>
        <div :class="$style.stack">
          <BentoCheckbox v-model="states.bento1" label="Enable Notifications" />
          <BentoCheckbox v-model="states.bento2" label="Dark Mode" color="#10b981" />
          <BentoCheckbox v-model="states.bento3" label="Cloud Sync" color="#f59e0b" />
        </div>
      </section>

      <section :class="$style.card">
        <span :class="$style.tag">Neo-Brutalism // Sharp Edge</span>
        <div :class="$style.stack">
          <BrutalismCheckbox v-model="states.brutal1" label="ACCEPT_TERMS" />
          <BrutalismCheckbox v-model="states.brutal2" label="KILL_PROCESS" color="#ff0000" />
        </div>
      </section>

      <section :class="$style.card">
        <span :class="$style.tag">Bubble Style // Round & Playful</span>
        <div :class="$style.stack">
          <BubbleCheckbox v-model="states.bubble1" label="Playful Option" color="#ff6b6b" />
          <BubbleCheckbox v-model="states.bubble2" label="Soft Toggle" color="#4ecdc4" />
        </div>
      </section>

      <section :class="[$style.card, $style.darkCard]">
        <span :class="$style.tag">Cyberpunk // Neural Sync</span>
        <div :class="$style.stack">
          <CyberCheckbox v-model="states.cyber1" label="ICE_BREAKER_PROTOCOL"/>
          <CyberCheckbox v-model="states.cyber2" label="UPLINK_ESTABLISHED" color="#00f2fe" />
        </div>
      </section>

      <section :class="[$style.card, $style.glassBg]">
        <span :class="$style.tag">Glassmorphism // Translucent</span>
        <div :class="$style.stack">
          <GlassCheckbox v-model="states.glass1" label="Crystal View" />
          <GlassCheckbox v-model="states.glass2" label="Frost Filter" />
        </div>
      </section>

      <section :class="[$style.card, $style.neuBg]">
        <span :class="$style.tag">Neumorphism // Soft Inset</span>
        <div :class="$style.stack">
          <neuCheckbox v-model="states.neu1" label="Soft Toggle" />
          <neuCheckbox v-model="states.neu2" label="Inner Shadow" />
        </div>
      </section>

      <section :class="[$style.card, $style.clayBg]">
        <span :class="$style.tag">Claymorphism // 3D Inflatable</span>
        <div :class="$style.stack">
          <ClayCheckbox v-model="states.clay1" label="Squishy Check" color="#a855f7" />
        </div>
      </section>

      <section :class="[$style.card, $style.darkCard]">
        <span :class="$style.tag">Speed // Racing HUD</span>
        <div :class="$style.stack">
          <SpeedCheckbox v-model="states.speed1" label="NITRO_ENABLED" color="#fcee0a" />
          <SpeedCheckbox v-model="states.speed2" label="DRS_ACTIVE" color="#ffffff" />
        </div>
      </section>
    </div>
  </div>
</template>

<script setup lang="ts">
import { ref } from 'vue';
import { 
  BentoCheckbox, 
  BrutalismCheckbox, 
  BubbleCheckbox, 
  ClayCheckbox, 
  GlassCheckbox, 
  CyberCheckbox, 
  neuCheckbox, 
  SpeedCheckbox 
} from "../components/checkboxes"

// Estado reactivo centralizado
const states = ref({
  bento1: true, bento2: false, bento3: true,
  brutal1: false, brutal2: true,
  bubble1: true, bubble2: false,
  cyber1: true, cyber2: false,
  glass1: true, glass2: false,
  neu1: false, neu2: true,
  clay1: true,
  speed1: true, speed2: false
});
</script>

<style module>
/* Se mantienen tus estilos de contenedor que ya estaban geniales */
.showroomContainer {
  min-height: 100vh;
  padding: 80px 40px;
  background-color: #f8fafc;
  font-family: 'Inter', sans-serif;
}

.header {
  text-align: center;
  margin-bottom: 60px;
}

.header h1 {
  font-size: 2.5rem;
  font-weight: 900;
  letter-spacing: -1px;
  color: #1e293b;
}

.grid {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(350px, 1fr));
  gap: 24px;
  max-width: 1400px;
  margin: 0 auto;
}

.card {
  background: white;
  padding: 32px;
  border-radius: 20px;
  border: 1px solid #e2e8f0;
  display: flex;
  flex-direction: column;
  gap: 20px;
}

.tag {
  font-size: 0.7rem;
  font-weight: 800;
  text-transform: uppercase;
  color: #94a3b8;
  letter-spacing: 1px;
}

.stack {
  display: flex;
  flex-direction: column;
  gap: 12px;
}

/* Fondos específicos para que el estilo respire */
.darkCard { background: #000000; border: none; }
.neuBg { background: #e0e5ec; border: none; }
.clayBg { background: #f0fdf4; border: none; }
.glassBg { 
  background: linear-gradient(135deg, #3b82f6, #8b5cf6); 
  border: none;
}

@media (max-width: 600px) {
  .grid { grid-template-columns: 1fr; }
}
</style>