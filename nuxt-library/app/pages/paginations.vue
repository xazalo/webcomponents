<template>
  <div :class="$style.showroomContainer">
    <header :class="$style.header">
      <h1>PAGINATION_PROTOCOLS</h1>
      <p>SEQUENCE_CONTROL // DATA_INDEXING</p>
    </header>

    <div :class="$style.grid">
      <section :class="$style.card">
        <span :class="$style.tag">Bento UI // Modern Tiles</span>
        <BentoPagination 
          v-model="currentPage" 
          :total-pages="12" 
        />
      </section>

      <section :class="$style.card">
        <span :class="$style.tag">Neo-Brutalism // Sharp Shadows</span>
        <BrutalismPagination 
          v-model="currentPage" 
          :total-pages="12" 
          color="#fcee0a"
        />
      </section>

      <section :class="[$style.card, $style.darkCard]">
        <span :class="$style.tag">Cyberpunk // Neural Index</span>
        <CyberPagination 
          v-model="currentPage" 
          :total-pages="12" 
        />
      </section>

      <section :class="[$style.card, $style.glassBg]">
        <span :class="$style.tag">Glassmorphism // Frosted Steps</span>
        <GlassPagination 
          v-model="currentPage" 
          :total-pages="12" 
        />
      </section>

      <section :class="[$style.card, $style.neuBg]">
        <span :class="$style.tag">Neumorphism // Tactile Press</span>
        <NeuPagination 
          v-model="currentPage" 
          :total-pages="12" 
        />
      </section>

      <section :class="[$style.card, $style.clayBg]">
        <span :class="$style.tag">Claymorphism // 3D Bubbles</span>
        <ClayPagination 
          v-model="currentPage" 
          :total-pages="12" 
        />
      </section>

      <section :class="[$style.card, $style.darkCard]">
        <span :class="$style.tag">Speed // Tachometer Style</span>
        <SpeedPagination 
          v-model="currentPage" 
          :total-pages="12" 
        />
      </section>

      <section :class="$style.card">
        <span :class="$style.tag">Bubble Style // Floating Points</span>
        <BubblePagination 
          v-model="currentPage" 
          :total-pages="12" 
        />
      </section>
    </div>
  </div>
</template>

<script setup lang="ts">
import { ref } from 'vue';
import { 
  BentoPagination, 
  BrutalismPagination, 
  CyberPagination, 
  GlassPagination, 
  NeuPagination, 
  ClayPagination, 
  SpeedPagination, 
  BubblePagination 
} from "../components/paginations"

const currentPage = ref(1);
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
  letter-spacing: -1px;
  color: #1e293b;
}

.grid {
  display: grid;
  grid-template-columns: 1fr; /* Una columna por fila para apreciar el ancho de la paginación */
  gap: 24px;
  max-width: 900px;
  margin: 0 auto;
}

.card {
  background: white;
  padding: 40px;
  border-radius: 24px;
  border: 1px solid #e2e8f0;
  display: flex;
  flex-direction: column;
  align-items: center; /* Centramos la paginación dentro de la tarjeta */
  gap: 24px;
}

.tag {
  align-self: flex-start;
  font-size: 0.7rem;
  font-weight: 800;
  text-transform: uppercase;
  color: #94a3b8;
  letter-spacing: 1px;
}

/* Fondos específicos */
.darkCard { background: #0f172a; border: none; }
.neuBg { background: #e0e5ec; border: none; }
.clayBg { background: #f0f7ff; border: none; }
.glassBg { 
  background: linear-gradient(135deg, #6366f1, #a855f7); 
  border: none;
}

@media (max-width: 600px) {
  .card { padding: 20px; }
}
</style>