<template>
  <div :class="$style.showroomContainer">
    <header :class="$style.header">
      <h1>MODAL_INTERFACES</h1>
      <p>OVERLAY_COLLECTION // VIEW_PORTALS</p>
    </header>

    <div :class="$style.grid">
      <section :class="$style.card">
        <span :class="$style.tag">Bento UI // Rounded Layers</span>
        <BentoButton @click="modals.bento = true" label="OPEN_BENTO_MODAL" />
        <BentoPop
          v-model="modals.bento"
          title="System Access"
        >
          <p>Esta es una interfaz bento con bordes suaves y jerarquía clara.</p>
        </BentoPop>
      </section>

      <section :class="$style.card">
        <span :class="$style.tag">Neo-Brutalism // Sharp Pop</span>
        <BrutalismButton
          @click="modals.brutal = true"
          label="LAUNCH_MODAL"
          color="#fcee0a"
        />
        <BrutalismPop
          v-model="modals.brutal"
          title="WARNING_ALERT"
        >
          <p>DIAGNÓSTICO: El sistema requiere atención inmediata en el sector 7.</p>
        </BrutalismPop>
      </section>

      <section :class="[$style.card, $style.darkCard]">
        <span :class="$style.tag">Cyberpunk // Glitch Portal</span>
        <CyberButton @click="modals.cyber = true" label="INIT_UPLINK" />
        <CyberPop
          v-model="modals.cyber"
          title="NEURAL_SYNC_V4"
        >
          <p>Estableciendo conexión segura con la red principal... 99%</p>
        </CyberPop>
      </section>

      <section :class="[$style.card, $style.glassBg]">
        <span :class="$style.tag">Glassmorphism // Frosted Focus</span>
        <GlassButton @click="modals.glass = true" label="VIEW_CRYSTAL" />
        <GlassPop
          v-model="modals.glass"
          title="Glass Insight"
        >
          <p>Efecto de desenfoque profundo sobre capas traslúcidas.</p>
        </GlassPop>
      </section>

      <section :class="[$style.card, $style.neuBg]">
        <span :class="$style.tag">Neumorphism // Soft Extrusion</span>
        <NeuButton @click="modals.neu = true" label="PRESS_TO_POP" />
        <NeuPop
          v-model="modals.neu"
          title="Soft Panel"
        >
          <p>Un modal que parece emerger directamente del fondo del sitio.</p>
        </NeuPop>
      </section>

      <section :class="[$style.card, $style.clayBg]">
        <span :class="$style.tag">Claymorphism // 3D Depth</span>
        <ClayButton @click="modals.clay = true" label="SQUISH_MODAL" />
        <ClayPop
          v-model="modals.clay"
          title="Clay World"
        >
          <p>Bordes inflados y sombras internas para un efecto 3D amigable.</p>
        </ClayPop>
      </section>

      <section :class="[$style.card, $style.darkCard]">
        <span :class="$style.tag">Speed // HUD Dashboard</span>
        <SpeedButton @click="modals.speed = true" label="ENGAGE_HUD" />
        <SpeedPop
          v-model="modals.speed"
          title="PERFORMANCE_METRICS"
        >
          <p>Análisis de telemetría completado. Velocidad óptima alcanzada.</p>
        </SpeedPop>
      </section>

      <section :class="$style.card">
        <span :class="$style.tag">Bubble Style // Organic Float</span>
        <BubbleButton @click="modals.bubble = true" label="FLOAT_MODAL" />
        <BubblePop
          v-model="modals.bubble"
          title="Floating Bubble"
        >
          <p>Formas orgánicas y animaciones de rebote (bounce).</p>
        </BubblePop>
      </section>
    </div>
  </div>
</template>

<script setup lang="ts">
import { reactive } from "vue";

// Importaciones unificadas de Pops
import {
  BentoPop,
  BrutalismPop,
  CyberPop,
  GlassPop,
  NeuPop,
  ClayPop,
  SpeedPop,
  BubblePop,
} from "../components/pops";

// Importaciones unificadas de Buttons (Corregidos nombres para que coincidan con el template)
import {
  BentoButton,
  BrutalismButton,
  CyberButton, // Antes CyberButton
  GlassButton,
  NeuButton,
  ClayButton, // Antes ClayButton
  SpeedButton,
  BubbleButton,
} from "../components/buttons";

const modals = reactive({
  bento: false,
  brutal: false,
  cyber: false,
  glass: false,
  neu: false,
  clay: false,
  speed: false,
  bubble: false,
});
</script>

<style module>
.showroomContainer {
  min-height: 100vh;
  padding: 80px 20px;
  background-color: #f8fafc;
  font-family: "Inter", sans-serif;
}

.header {
  text-align: center;
  margin-bottom: 60px;
}

.header h1 {
  font-size: 2.5rem;
  font-weight: 900;
  color: #1e293b;
  letter-spacing: -1.5px;
}

.grid {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(320px, 1fr));
  gap: 24px;
  max-width: 1200px;
  margin: 0 auto;
}

.card {
  background: white;
  padding: 40px;
  border-radius: 24px;
  border: 1px solid #e2e8f0;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  gap: 20px;
  min-height: 240px;
  transition: all 0.3s ease;
}

.card:hover {
  border-color: #cbd5e1;
  transform: translateY(-4px);
}

.tag {
  align-self: flex-start;
  font-size: 0.65rem;
  font-weight: 800;
  text-transform: uppercase;
  color: #94a3b8;
  letter-spacing: 1px;
}

/* Fondos específicos */
.darkCard { background: #0f172a; border-color: #1e293b; }
.neuBg { background: #e0e5ec; border: none; }
.clayBg { background: #f0fdf4; border: none; }
.glassBg { 
  background: linear-gradient(135deg, #6366f1, #a855f7); 
  border: none;
}

@media (max-width: 600px) {
  .grid { grid-template-columns: 1fr; }
}
</style>