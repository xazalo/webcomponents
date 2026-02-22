<template>
  <div :class="$style.showroomContainer">
    <header :class="$style.header">
      <h1>INPUT_DROPDOWNS</h1>
      <p>SELECT_COLLECTION // SELECTION_FLOW</p>
    </header>

    <div :class="$style.grid">
      <section :class="$style.card">
        <span :class="$style.tag">Bento UI // Modern Logic</span>
        <BentoSelect 
          v-model="states.bento" 
          :options="options" 
          placeholder="Seleccionar categoría..."
        />
      </section>

      <section :class="$style.card">
        <span :class="$style.tag">Neo-Brutalism // Thick Strokes</span>
        <BrutallismSelect 
          v-model="states.brutal" 
          :options="options" 
          color="#fcee0a"
        />
      </section>

      <section :class="[$style.card, $style.darkCard]">
        <span :class="$style.tag">Cyberpunk // Neural Interface</span>
        <CyberSelect 
          v-model="states.cyber" 
          :options="options" 
        />
      </section>

      <section :class="[$style.card, $style.glassBg]">
        <span :class="$style.tag">Glassmorphism // Translucent Drop</span>
        <GlassSelect 
          v-model="states.glass" 
          :options="options" 
        />
      </section>

      <section :class="[$style.card, $style.neuBg]">
        <span :class="$style.tag">Neumorphism // Soft Inset</span>
        <NeuSelect 
          v-model="states.neu" 
          :options="options" 
        />
      </section>

      <section :class="[$style.card, $style.clayBg]">
        <span :class="$style.tag">Claymorphism // 3D Press</span>
        <ClaySelect 
          v-model="states.clay" 
          :options="options" 
          color="#a855f7"
        />
      </section>

      <section :class="[$style.card, $style.darkCard]">
        <span :class="$style.tag">Speed // Racing HUD</span>
        <SpeedSelect 
          v-model="states.speed" 
          :options="options" 
        />
      </section>

      <section :class="$style.card">
        <span :class="$style.tag">Bubble Style // Organic Drop</span>
        <BubbleSelect 
          v-model="states.bubble" 
          :options="options" 
        />
      </section>
    </div>
  </div>
</template>

<script setup lang="ts">
import { ref } from 'vue';
import { 
  BentoSelect,
  BrutallismSelect,
  CyberSelect,
  GlassSelect,
  NeuSelect,
  ClaySelect,
  SpeedSelect,
  BubbleSelect
} from "../components/selects";

// Opciones compartidas para el testeo
const options = [
  { label: 'Administrador', value: 'admin' },
  { label: 'Editor de Contenido', value: 'editor' },
  { label: 'Analista de Datos', value: 'analyst' },
  { label: 'Invitado', value: 'guest' },
];

const states = ref({
  bento: null,
  brutal: 'admin',
  cyber: null,
  glass: null,
  neu: 'editor',
  clay: null,
  speed: null,
  bubble: null
});
</script>

<style module>
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
  letter-spacing: -1.5px;
  color: #1e293b;
}

.grid {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(350px, 1fr));
  gap: 32px;
  max-width: 1400px;
  margin: 0 auto;
}

.card {
  background: white;
  padding: 40px;
  border-radius: 28px;
  border: 1px solid #e2e8f0;
  display: flex;
  flex-direction: column;
  gap: 20px;
  /* Importante: para que el menú desplegable se vea si no usas Teleport en el Select */
  min-height: 300px; 
}

.tag {
  font-size: 0.7rem;
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
  background: linear-gradient(135deg, #4f46e5 0%, #06b6d4 100%); 
  border: none;
}

@media (max-width: 600px) {
  .grid { grid-template-columns: 1fr; }
}
</style>