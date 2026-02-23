<template>
  <div :class="$style.showroomContainer">
    <header :class="$style.header">
      <h1>NAVIGATION_TRAILS</h1>
      <p>BREADCRUMBS_COLLECTION // LOCATION_TRACKING</p>
    </header>

    <div :class="$style.grid">
      <section :class="$style.card">
        <span :class="$style.tag">Bento Style</span>
        <BentoBreadcrumb :items="navItems" borderRadius="1rem" color="#F54927"/>
      </section>

      <section :class="[$style.card, $style.darkCard]">
        <span :class="$style.tag">Cyber / Terminal</span>
        <CyberBreadcrumb :items="navItems" />
      </section>

      <section :class="[$style.card, $style.glassBg]">
        <span :class="$style.tag">Glassmorphism</span>
        <GlassBreadcrumb :items="navItems" />
      </section>

      <section :class="[$style.card, $style.neuBg]">
        <span :class="$style.tag">Neumorphism</span>
        <NeuBreadcrumb :items="navItems" />
      </section>

      <section :class="$style.card">
        <span :class="$style.tag">Neo-Brutalism</span>
        <BrutalismBreadcrumb :items="navItems" color="#fcee0a" />
      </section>

      <section :class="[$style.card, $style.darkCard]">
        <span :class="$style.tag">Speed / Racing</span>
        <SpeedBreadcrumb :items="navItems" />
      </section>

      <section :class="$style.card">
        <span :class="$style.tag">Bubble Style</span>
        <BubbleBreadcrumb :items="navItems" />
      </section>

      <section :class="[$style.card, $style.clayBg]">
        <span :class="$style.tag">Claymorphism</span>
        <ClayBreadcrumb :items="navItems" />
      </section>
    </div>
  </div>
</template>

<script setup lang="ts">
import { 
  BentoBreadcrumb, 
  BrutalismBreadcrumb, 
  BubbleBreadcrumb, 
  ClayBreadcrumb, 
  CyberBreadcrumb, 
  GlassBreadcrumb, 
  NeuBreadcrumb, 
  SpeedBreadcrumb 
} from "../components/breadcrumbs"

const navItems = [
  { label: 'ROOT', to: '/' },
  { label: 'STORAGE', to: '/storage' },
  { label: 'NODES', to: '/storage/nodes' },
  { label: 'CURRENT_FILE', to: '/storage/nodes/file' }
];
</script>

<style module>
.showroomContainer {
  min-height: 100vh;
  padding: 60px 20px;
  background-color: #f8fafc;
  font-family: sans-serif;
}

.header {
  text-align: center;
  margin-bottom: 60px;
}

.header h1 {
  font-family: 'Orbitron', sans-serif;
  letter-spacing: 4px;
  font-size: 2rem;
  color: #1e293b;
}

.grid {
  display: grid;
  grid-template-columns: 1fr;
  gap: 40px;
  max-width: 900px;
  margin: 0 auto;
}

.card {
  padding: 40px;
  border-radius: 16px;
  background: white;
  border: 1px solid #e2e8f0;
  display: flex;
  flex-direction: column;
  gap: 20px;
}

.tag {
  font-size: 0.65rem;
  font-weight: 800;
  color: #94a3b8;
  text-transform: uppercase;
  letter-spacing: 1.5px;
}

.darkCard { background: #0f172a; border-color: #1e293b; }
.neuBg { background: #e0e5ec; border: none; }
.clayBg { background: #f0f9ff; border: none; }
.glassBg { 
  background: linear-gradient(45deg, #6366f1, #a855f7); 
  border: none;
}

@media (max-width: 600px) {
  .card { padding: 20px; }
}
</style>