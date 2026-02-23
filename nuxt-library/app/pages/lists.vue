<template>
  <div :class="$style.container">
    <header :class="$style.header">
      <span :class="$style.superTitle">DATA_STRUCTURES</span>
      <h1 :class="$style.title">UNIVERSAL_LISTS</h1>
      <p :class="$style.subtitle">RESPONSIVE_ITEMS // STYLE_AGNOSTIC // SLOT_DRIVEN</p>
    </header>

    <div :class="$style.showcaseGrid">
      
      <BentoCard title="Project Activity" tag="Recent">
        <DefaultList :divider="true" gap-size="none">
          <DefaultListItem label="Design Review" description="Updated 2h ago" value="Done" />
          <DefaultListItem label="API Integration" description="In progress" value="80%" />
          <DefaultListItem label="Assets Export" description="Pending" value="Wait" />
        </DefaultList>
      </BentoCard>

      <CyberCard title="System Status" neon-color="#ff0055">
        <DefaultList :divider="false" gap-size="small" primary-color="#ff0055">
          <DefaultListItem label="CORE_TEMP" value="42°C" />
          <DefaultListItem label="UPLINK_STATUS" value="STABLE" />
          <DefaultListItem label="THREAT_LEVEL" value="LOW" />
        </DefaultList>
      </CyberCard>

      <BrutalismCard title="Checklist" primary-color="#fcee0a">
        <DefaultList :divider="true" gap-size="medium" padding="10px">
          <DefaultListItem label="Buy Domain" description="GoDaddy/Namecheap" clickable />
          <DefaultListItem label="Setup Nuxt" description="Install modules" clickable />
          <DefaultListItem label="Deploy App" description="Vercel/Netlify" clickable />
        </DefaultList>
      </BrutalismCard>

      <BentoCard title="Top Reviews" align="start">
        <DefaultList :divider="true">
          <DefaultListItem label="User_01" description="Excellent UI kit">
            <template #suffix>
              <StarRating :model-value="5" size="0.8rem" />
            </template>
          </DefaultListItem>
          <DefaultListItem label="Dev_Team" description="Very fast to implement">
            <template #suffix>
              <StarRating :model-value="4.5" size="0.8rem" />
            </template>
          </DefaultListItem>
        </DefaultList>
      </BentoCard>

      <div :class="$style.glassSection">
        <GlassCard title="Global Rankings">
          <DefaultList :divider="true" primary-color="#fff">
            <DefaultListItem label="🏆 Platinum" value="1,200 pts" />
            <DefaultListItem label="🥈 Gold" value="850 pts" />
            <DefaultListItem label="🥉 Silver" value="430 pts" />
          </DefaultList>
        </GlassCard>
      </div>

    </div>
  </div>
</template>

<script setup lang="ts">
// Importamos la lista y el item desde tu ruta
import { DefaultList, DefaultListItem } from "../components/lists";
// Importamos los estilos de tarjetas anteriores para el contexto
import { BentoCard, CyberCard, BrutalismCard, GlassCard } from "../components/cards";
// Opcional: El rating para ver cómo conviven
import { StarRating } from "../components/ratings";
</script>

<style module>
.container {
  min-height: 100vh;
  padding: 80px 20px;
  background-color: #f1f5f9;
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
  grid-template-columns: repeat(auto-fit, minmax(350px, 1fr));
  gap: 32px;
}

.glassSection {
  background: linear-gradient(135deg, #6366f1, #a855f7);
  padding: 10px;
  border-radius: 32px;
}

@media (max-width: 768px) {
  .showcaseGrid { grid-template-columns: 1fr; }
}
</style>