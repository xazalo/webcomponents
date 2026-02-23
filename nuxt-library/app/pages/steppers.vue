<template>
  <div :class="$style.container">
    <header :class="$style.header">
      <span :class="$style.superTitle">INTERACTIVE_FLOWS</span>
      <h1 :class="$style.title">BENTO_STEPPERS_PRO</h1>
      <p :class="$style.subtitle">MULTI_STYLE_CONTROLS // BUBBLE_VARIANT // DYNAMIC_STATE</p>
    </header>

    <div :class="$style.showcaseGrid">
      
      <section :class="[$style.section, $style.bubbleSection]">
        <h2 :class="$style.sectionLabel">Bubble Style // Soft Onboarding</h2>
        <div :class="$style.bubbleCard">
          <DefaultStepper 
            variant="bubble"
            :current-step="states.bubble"
            :steps="onboardingSteps"
            accent-color="#ff85a1"
          />
          <div :class="$style.bubbleControls">
            <button @click="states.bubble--" :disabled="states.bubble === 0">Back</button>
            <button @click="states.bubble++" :disabled="states.bubble === onboardingSteps.length - 1">Next Step</button>
          </div>
        </div>
      </section>

      <CyberCard title="Neural_Upload" neon-color="#00f2fe">
        <div :class="$style.innerSpace">
          <DefaultStepper 
            variant="cyber"
            :current-step="states.cyber"
            :steps="cyberSteps"
            accent-color="#00f2fe"
          />
          <div :class="$style.cyberControls">
            <button @click="states.cyber--" :disabled="states.cyber === 0">PREV_BYTE</button>
            <button @click="states.cyber++" :disabled="states.cyber === cyberSteps.length - 1">NEXT_BYTE</button>
          </div>
        </div>
      </CyberCard>

      <BrutalismCard title="Raw Setup" primary-color="#fcee0a">
        <div :class="$style.innerSpace">
          <DefaultStepper 
            variant="brutal"
            :current-step="states.brutal"
            :steps="setupSteps"
            accent-color="#000"
          />
          <div :class="$style.brutalControls">
            <button @click="states.brutal--" :disabled="states.brutal === 0">BACK</button>
            <button @click="states.brutal++" :disabled="states.brutal === setupSteps.length - 1">GO_UP</button>
          </div>
        </div>
      </BrutalismCard>

      <div :class="$style.glassWrapper">
        <GlassCard title="Cloud Sync">
          <DefaultStepper 
            variant="glass"
            :current-step="states.glass"
            :steps="checkoutSteps"
            accent-color="#fff"
            base-color="rgba(255,255,255,0.2)"
          />
          <div :class="$style.glassControls">
            <button @click="states.glass--" :disabled="states.glass === 0">Previous</button>
            <button @click="states.glass++" :disabled="states.glass === checkoutSteps.length - 1">Continue</button>
          </div>
        </GlassCard>
      </div>

    </div>
  </div>
</template>

<script setup lang="ts">
import { reactive } from 'vue';
import { DefaultStepper } from "../components/steppers";
import { CyberCard, BrutalismCard, GlassCard } from "../components/cards";

// Estados independientes para cada stepper
const states = reactive({
  bubble: 0,
  cyber: 1,
  brutal: 2,
  glass: 0
});

const onboardingSteps = [
  { title: 'Welcome', description: 'Start your journey' },
  { title: 'Profile', description: 'Personal details' },
  { title: 'Done', description: 'All set!' }
];

const cyberSteps = [
  { title: 'SCAN_DNA', description: 'Identity lock' },
  { title: 'LINK_CORE', description: 'Mainframe access' },
  { title: 'SYNC_COMPLETE' }
];

const setupSteps = [
  { title: 'INIT', description: 'Booting...' },
  { title: 'CONFIG', description: 'Variables' },
  { title: 'LIVE', description: 'Production' }
];

const checkoutSteps = [
  { title: 'Cart', description: 'Review' },
  { title: 'Payment', description: 'Security' },
  { title: 'Finish', description: 'Confirm' }
];
</script>

<style module>
.container {
  min-height: 100vh;
  padding: 80px 20px;
  background-color: #f1f5f9;
  font-family: 'Inter', sans-serif;
}

.header { text-align: center; margin-bottom: 60px; }
.superTitle { font-weight: 900; color: #94a3b8; letter-spacing: 3px; font-size: 0.7rem; display: block; }
.title { font-size: 3rem; font-weight: 900; color: #0f172a; margin: 0; }
.subtitle { color: #64748b; font-weight: 600; font-size: 0.8rem; }

.showcaseGrid {
  max-width: 1200px;
  margin: 0 auto;
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(380px, 1fr));
  gap: 32px;
}

.section { display: flex; flex-direction: column; gap: 16px; }
.sectionLabel { font-size: 0.75rem; font-weight: 900; text-transform: uppercase; color: #64748b; }

/* --- BUBBLE STYLE --- */
.bubbleCard {
  background: #fff;
  padding: 35px;
  border-radius: 40px;
  box-shadow: 0 20px 40px rgba(255, 133, 161, 0.1);
  border: 4px solid #fff5f7;
}
.bubbleControls {
  display: flex;
  gap: 10px;
  margin-top: 20px;
}
.bubbleControls button {
  flex: 1; padding: 12px; border-radius: 20px; border: none;
  background: #ff85a1; color: white; font-weight: 800; cursor: pointer;
}
.bubbleControls button:disabled { background: #fee2e2; color: #fca5a5; }

/* --- CYBER CONTROLS --- */
.cyberControls { display: flex; gap: 8px; margin-top: 20px; }
.cyberControls button {
  flex: 1; padding: 10px; background: transparent; border: 1px solid #00f2fe;
  color: #00f2fe; font-family: 'JetBrains Mono', monospace; font-size: 0.7rem; cursor: pointer;
}
.cyberControls button:disabled { opacity: 0.2; }

/* --- BRUTAL CONTROLS --- */
.brutalControls { display: flex; gap: 0; margin-top: 20px; border: 3px solid #000; }
.brutalControls button {
  flex: 1; padding: 12px; background: #fff; border: none;
  font-weight: 900; cursor: pointer; border-right: 3px solid #000;
}
.brutalControls button:last-child { border-right: none; background: #000; color: #fff; }

/* --- GLASS CONTROLS --- */
.glassControls { display: flex; gap: 10px; margin-top: 20px; }
.glassControls button {
  flex: 1; padding: 10px; background: rgba(255,255,255,0.2); border: 1px solid rgba(255,255,255,0.3);
  border-radius: 12px; color: white; backdrop-filter: blur(5px); cursor: pointer;
}

.glassWrapper { background: linear-gradient(135deg, #6366f1, #a855f7); padding: 12px; border-radius: 32px; }
.innerSpace { padding: 10px 0; }

@media (max-width: 768px) {
  .showcaseGrid { grid-template-columns: 1fr; }
}
</style>