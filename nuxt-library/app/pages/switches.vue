<template>
  <div :class="$style.showroomContainer">
    <header :class="$style.header">
      <h1>TOGGLE_SYSTEMS</h1>
      <p>SWITCH_COLLECTION // BINARY_STATES</p>
    </header>

    <div :class="$style.grid">
      <section :class="$style.card">
        <span :class="$style.tag">Bento UI // Modular Toggle</span>
        <div :class="$style.stack">
          <BentoSwitch v-model="states.bento1" label="WIFI_ANTENNA" />
          <BentoSwitch v-model="states.bento2" label="BLUETOOTH_HD" />
        </div>
      </section>

      <section :class="$style.card">
        <span :class="$style.tag">Neo-Brutalism // Hard Click</span>
        <div :class="$style.stack">
          <BrutalSwitch v-model="states.brutal1" label="POWER_GRID" />
          <BrutalSwitch v-model="states.brutal2" label="OVERRIDE" />
        </div>
      </section>

      <section :class="[$style.card, $style.darkCard]">
        <span :class="$style.tag">Cyberpunk // Neural Link</span>
        <div :class="$style.stack">
          <CyberSwitch v-model="states.cyber1" label="NET_RUNNER_MODE" />
          <CyberSwitch v-model="states.cyber2" label="ICE_BREAKER" />
        </div>
      </section>

      <section :class="[$style.card, $style.glassBg]">
        <span :class="$style.tag">Glassmorphism // Frosted Air</span>
        <div :class="$style.stack">
          <GlassSwitch v-model="states.glass1" label="SMOOTH_RENDER" />
        </div>
      </section>

      <section :class="[$style.card, $style.neuBg]">
        <span :class="$style.tag">Neumorphism // Concave State</span>
        <div :class="$style.stack">
          <NeuSwitch v-model="states.neu1" label="HAPTIC_FEEDBACK" />
        </div>
      </section>

      <section :class="[$style.card, $style.clayBg]">
        <span :class="$style.tag">Claymorphism // 3D Squish</span>
        <div :class="$style.stack">
          <ClaySwitch v-model="states.clay1" label="PLASTIC_ON" />
        </div>
      </section>

      <section :class="[$style.card, $style.darkCard]">
        <span :class="$style.tag">Speed // Nitro Engage</span>
        <div :class="$style.stack">
          <SpeedSwitch v-model="states.speed1" label="NOS_INJECTION" />
        </div>
      </section>

      <section :class="$style.card">
        <span :class="$style.tag">Bubble Style // Jelly State</span>
        <div :class="$style.stack">
          <BubbleSwitch v-model="states.bubble1" label="FLOAT_EFFECT" />
        </div>
      </section>
    </div>
  </div>
</template>

<script setup lang="ts">
import { ref } from 'vue';
import { 
  BentoSwitch, 
  BrutalSwitch, 
  BubbleSwitch, 
  ClaySwitch, 
  CyberSwitch, 
  GlassSwitch, 
  NeuSwitch, 
  SpeedSwitch 
} from "../components/switches";

const states = ref({
  bento1: true, bento2: false,
  brutal1: false, brutal2: true,
  cyber1: true, cyber2: false,
  glass1: true,
  neu1: false,
  clay1: true,
  speed1: true,
  bubble1: false
});
</script>

<style module>
.showroomContainer {
  min-height: 100vh;
  padding: 80px 40px;
  background-color: #f1f5f9;
}

.header {
  text-align: center;
  margin-bottom: 60px;
}

.header h1 {
  font-size: 2.5rem;
  font-weight: 900;
  color: #0f172a;
  letter-spacing: -1.5px;
}

.grid {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(320px, 1fr));
  gap: 24px;
  max-width: 1300px;
  margin: 0 auto;
}

.card {
  background: white;
  padding: 32px;
  border-radius: 24px;
  border: 1px solid rgba(0,0,0,0.05);
  display: flex;
  flex-direction: column;
  gap: 20px;
  align-items: flex-start;
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
  width: 100%;
}

/* Fondos específicos */
.darkCard { background: #0a0a0b; border: none; }
.neuBg { background: #e0e5ec; border: none; }
.clayBg { background: #fdf2f8; border: none; }
.glassBg { 
  background: linear-gradient(135deg, #4f46e5 0%, #7c3aed 100%); 
  border: none;
}

@media (max-width: 600px) {
  .grid { grid-template-columns: 1fr; }
}
</style>