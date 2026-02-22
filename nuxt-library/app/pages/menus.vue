<template>
  <div :class="$style.showroomContainer">
    <header :class="$style.header">
      <h1>NAVIGATION_ARCHITECTURES</h1>
      <p>MENU_COLLECTION // FLOW_CONTROL</p>
    </header>

    <div :class="$style.grid">
      <section :class="[$style.card, $style.darkCard]">
        <span :class="$style.tag">Cyberpunk // Neural Menu</span>
        <CyberMenu 
          v-model="activePath" 
          :links="navLinks" 
        />
      </section>

      <section :class="$style.card">
        <span :class="$style.tag">Bento UI // Modern Tiles</span>
        <BentoMenu 
          v-model="activePath" 
          :links="navLinks" 
          buttonText="close"
        />
      </section>

      <section :class="[$style.card, $style.glassBg]">
        <span :class="$style.tag">Glassmorphism // Crystal Navigation</span>
        <GlassMenu 
          v-model="activePath" 
          :links="navLinks" 
          buttonText="close"
        />
      </section>

      <section :class="$style.card">
        <span :class="$style.tag">Neo-Brutalism // Pop Bold</span>
        <BrutalismMenu 
          v-model="activePath" 
          :links="navLinks" 
          color="#55efc4"
          buttonText="close"
        />
      </section>

      <section :class="[$style.card, $style.neuBg]">
        <span :class="$style.tag">Neumorphism // Soft Logic</span>
        <NeuMenu 
          v-model="activePath" 
          :links="navLinks" 
          buttonText="close"
        />
      </section>

      <section :class="[$style.card, $style.clayBg]">
        <span :class="$style.tag">Claymorphism // 3D Squishy</span>
        <ClayMenu 
          v-model="activePath" 
          :links="navLinks" 
          buttonText="close"
        />
      </section>

      <section :class="$style.card">
        <span :class="$style.tag">Bubble Style // Playful Flow</span>
        <BubbleMenu 
          v-model="activePath" 
          :links="navLinks" 
          buttonText="close"
        />
      </section>

      <section :class="[$style.card, $style.darkCard]">
        <span :class="$style.tag">Speed // Racing HUD</span>
        <SpeedMenu 
          v-model="activePath" 
          :links="navLinks" 
          buttonText="close"
        />
      </section>
    </div>
  </div>
</template>

<script setup lang="ts">
import { ref } from 'vue';
import { 
  CyberMenu, 
  BentoMenu, 
  GlassMenu,
  ClayMenu, 
  BrutalismMenu, 
  NeuMenu, 
  SpeedMenu,
  BubbleMenu 
} from "../components/menus"

const activePath = ref('/dashboard');

const navLinks = [
  { 
    label: 'DASHBOARD', 
    path: '/dashboard', 
    description: 'Central operations control',
    icon: 'lucide:layout-dashboard' 
  },
  { 
    label: 'DATABASE', 
    path: '/db', 
    description: 'Encrypted storage access',
    icon: 'lucide:database' 
  },
  { 
    label: 'SECURITY', 
    path: '/security', 
    description: 'Firewall & protocols',
    icon: 'lucide:shield-check' 
  },
  { 
    label: 'SETTINGS', 
    path: '/settings', 
    description: 'System reconfiguration',
    icon: 'lucide:settings' 
  }
];
</script>

<style module>
.showroomContainer {
  min-height: 100vh;
  padding: 80px 40px;
  background-color: #f1f5f9;
  font-family: 'Inter', sans-serif;
}

.header {
  text-align: center;
  margin-bottom: 80px;
}

.header h1 {
  font-size: 2.8rem;
  font-weight: 900;
  color: #0f172a;
  letter-spacing: -2px;
}

.grid {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(550px, 1fr));
  gap: 32px;
  max-width: 1400px;
  margin: 0 auto;
}

.card {
  background: #ffffff;
  padding: 40px;
  border-radius: 32px;
  border: 1px solid rgba(0,0,0,0.05);
  display: flex;
  flex-direction: column;
  gap: 24px;
}

.tag {
  font-size: 0.7rem;
  font-weight: 800;
  text-transform: uppercase;
  letter-spacing: 1.5px;
  color: #94a3b8;
}

/* --- Contextos de Diseño --- */
.darkCard { background: #050505; border: none; }
.neuBg { background: #e0e5ec; border: none; }
.clayBg { background: #fdf2f8; border: none; } /* Fondo rosa muy suave para Clay */
.glassBg { 
  background: linear-gradient(135deg, #0f172a 0%, #334155 100%); 
  border: none;
}

@media (max-width: 650px) {
  .grid { grid-template-columns: 1fr; }
  .card { padding: 20px; }
}
</style>