<template>
  <div :class="$style.container">
    <header :class="$style.header">
      <span :class="$style.superTitle">INTERACTIVE_ELEMENTS</span>
      <h1 :class="$style.title">UNIVERSAL_ACCORDIONS</h1>
      <p :class="$style.subtitle">SMOOTH_ANIMATIONS // MULTI_VARIANT // AUTO_SCALING</p>
    </header>

    <div :class="$style.showcaseGrid">
      
      <BentoCard title="Project FAQ" tag="Support">
        <div :class="$style.group">
          <DefaultAccordion title="How does billing work?" variant="modern">
            <p>Our billing is usage-based and calculated at the end of each month. You only pay for what you actually use.</p>
          </DefaultAccordion>
          <DefaultAccordion title="Can I cancel anytime?" variant="modern" initial-open>
            <p>Yes, there are no long-term contracts. You can downgrade or cancel your subscription at any time from your dashboard.</p>
          </DefaultAccordion>
        </div>
      </BentoCard>

      <section :class="[$style.section, $style.cyberBg]">
        <h2 :class="$style.sectionLabel">System_Overrides</h2>
        <DefaultAccordion title="NETWORK_ENCRYPTION" variant="cyber" accent-color="#00f2fe">
          <div :class="$style.cyberContent">
            <span>STATUS: ACTIVE</span>
            <span>LEVEL: QUANTUM_9</span>
          </div>
        </DefaultAccordion>
        <DefaultAccordion title="FIREWALL_PROTOCOLS" variant="cyber" accent-color="#ff0055">
          <p>Automatic threat detection is currently monitoring 4,209 incoming packets per second.</p>
        </DefaultAccordion>
      </section>

      <section :class="[$style.section, $style.brutalBg]">
        <h2 :class="$style.sectionLabel">Strict Rules</h2>
        <DefaultAccordion title="DESIGN GUIDELINES" variant="brutal">
          <p>Always use high contrast colors, thick borders (at least 3px), and avoid any kind of border-radius if possible.</p>
        </DefaultAccordion>
        <DefaultAccordion title="EXPORT SETTINGS" variant="brutal">
          <p>All assets must be exported in RAW format to maintain the brutalist aesthetic integrity.</p>
        </DefaultAccordion>
      </section>

      <div :class="$style.glassWrapper">
        <GlassCard title="Global Settings">
          <div :class="$style.group">
            <DefaultAccordion title="Privacy Policy" variant="glass">
              <p>Your data is encrypted using end-to-end protocols before being stored in our decentralized nodes.</p>
            </DefaultAccordion>
            <DefaultAccordion title="Terms of Service" variant="glass">
              <p>By using this interface, you agree to the terms of the automated smart contract.</p>
            </DefaultAccordion>
          </div>
        </GlassCard>
      </div>

      <section :class="[$style.section, $style.neuBg]">
        <h2 :class="$style.sectionLabel">Soft Settings</h2>
        <DefaultAccordion title="Audio Balance" variant="neu" accent-color="#6d5dfc">
          <p>Adjust the haptic feedback and spatial audio settings for your workspace environment.</p>
        </DefaultAccordion>
        <DefaultAccordion title="Display Brightness" variant="neu" accent-color="#6d5dfc">
          <p>Automatic blue light filtering is enabled based on your local timezone (CET).</p>
        </DefaultAccordion>
      </section>

    </div>
  </div>
</template>

<script setup lang="ts">
// Componente de Acordeón
import { DefaultAccordion } from "../components/accordions";
// Contenedores para contexto
import { BentoCard, GlassCard } from "../components/cards";
</script>

<style module>
.container {
  min-height: 100vh;
  padding: 80px 20px;
  background-color: #f1f5f9;
  font-family: 'Inter', sans-serif;
}

.header {
  text-align: center;
  margin-bottom: 60px;
}

.superTitle { font-weight: 900; color: #94a3b8; letter-spacing: 3px; font-size: 0.7rem; display: block; }
.title { font-size: 3rem; font-weight: 900; margin: 5px 0; color: #0f172a; }
.subtitle { color: #64748b; font-weight: 600; font-size: 0.8rem; }

.showcaseGrid {
  max-width: 1200px;
  margin: 0 auto;
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(400px, 1fr));
  gap: 32px;
}

.section {
  padding: 30px;
  border-radius: 32px;
  display: flex;
  flex-direction: column;
  gap: 12px;
}

.sectionLabel {
  font-size: 0.75rem;
  font-weight: 900;
  text-transform: uppercase;
  letter-spacing: 1px;
  margin-bottom: 10px;
  opacity: 0.5;
}

.group {
  display: flex;
  flex-direction: column;
}

/* --- CONTEXTOS DE COLOR --- */

.cyberBg { background-color: #050505; color: #fff; }
.cyberContent {
  display: flex;
  flex-direction: column;
  gap: 5px;
  font-family: 'JetBrains Mono', monospace;
  font-size: 0.8rem;
  color: #00f2fe;
}

.brutalBg { background-color: #ffdeeb; }

.glassWrapper {
  background: linear-gradient(135deg, #6366f1, #a855f7);
  padding: 10px;
  border-radius: 32px;
}

.neuBg { background-color: #e0e5ec; }

@media (max-width: 768px) {
  .showcaseGrid { grid-template-columns: 1fr; }
}
</style>