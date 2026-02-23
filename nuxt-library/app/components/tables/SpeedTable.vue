<template>
  <div :class="$style.speedContainer" :style="speedTheme">
    <header :class="$style.headerBlock">
      <div :class="$style.brandBox">
        <h1 :class="$style.mainTitle">{{ title }}</h1>
        <div :class="$style.engineStatus">ENGINE_RPM: {{ rpmValue }}</div>
      </div>
      <div :class="$style.searchWrapper">
        <div :class="$style.inputTag">NITRO_SEARCH</div>
        <input 
          type="text" 
          :placeholder="searchPlaceholder" 
          :class="$style.speedInput" 
          @input="$emit('search', $event.target.value)"
        />
        <div :class="$style.inputDeco"></div>
      </div>
    </header>

    <div :class="$style.mainGrid">
      <aside :class="$style.sidebar">
        <h3 :class="$style.subTitle">_DASHBOARD</h3>
        <div :class="$style.filterList">
          <label v-for="tag in tags" :key="tag" :class="$style.speedLabel">
            <input type="checkbox" :class="$style.speedCheck" @change="$emit('filter', tag)" />
            <span :class="$style.checkText">{{ tag }}</span>
          </label>
        </div>
        <div :class="$style.fuelGauge">
          <div :class="$style.fuelLevel" :style="{ width: fuelLevel + '%' }"></div>
        </div>
      </aside>

      <main :class="$style.tableBlock">
        <table :class="$style.speedTable">
          <thead>
            <tr>
              <th v-for="col in columns" :key="col.key">[{{ col.label }}]</th>
            </tr>
          </thead>
          <tbody>
            <tr v-for="(item, i) in data" :key="i">
              <td v-for="col in columns" :key="col.key">
                <slot :name="`col-${col.key}`" :item="item" :index="i">
                  <template v-if="col.key === 'rank'">
                    <span :class="$style.rankBox">{{ item[col.key] || i + 1 }}</span>
                  </template>
                  
                  <template v-else-if="col.key === 'pilot'">
                    <div :class="$style.userName">
                      <span :class="$style.stripes"></span>
                      {{ item[col.key] }}
                    </div>
                  </template>
                  
                  <template v-else-if="col.key === 'velocity'">
                    <div :class="$style.velocityBar">
                      <div :class="$style.fill" :style="{ width: (item[col.key] || 0) + '%' }"></div>
                    </div>
                  </template>

                  <template v-else-if="col.key === 'action'">
                    <button :class="$style.raceBtn" @click="$emit('action', item)">GO_FAST</button>
                  </template>

                  <template v-else>
                    {{ item[col.key] }}
                  </template>
                </slot>
              </td>
            </tr>
          </tbody>
        </table>
      </main>
    </div>

    <footer :class="$style.footerBlock">
      <div :class="$style.pagination">
        <button :class="$style.navBtn" @click="$emit('prev')">LAP_PREV</button>
        <div :class="$style.lapCounter">LAP {{ currentLap }} // {{ totalLaps }}</div>
        <button :class="$style.navBtn" @click="$emit('next')">LAP_NEXT</button>
      </div>
    </footer>
  </div>
</template>

<script setup lang="ts">
import { computed } from 'vue';

interface Props {
  title?: string;
  rpmValue?: string;
  searchPlaceholder?: string;
  tags?: string[];
  columns?: { label: string; key: string }[];
  data?: any[];
  currentLap?: string;
  totalLaps?: string;
  fuelLevel?: number | string;
  // Speed Tuning
  primaryColor?: string;   /* Rojo Nitro */
  accentColor?: string;    /* Amarillo RPM */
  bgTable?: string;        /* Color de fondo de las filas */
  skewAngle?: number;      /* Inclinación de la UI */
}

const props = withDefaults(defineProps<Props>(), {
  title: 'TURBO_DATA',
  rpmValue: '8.500',
  searchPlaceholder: 'BUSCAR NITRO...',
  tags: () => ['V12', 'V10', 'V8', 'TURBO'],
  columns: () => [
    { label: 'RANK', key: 'rank' },
    { label: 'PILOT_ID', key: 'pilot' },
    { label: 'VELOCITY', key: 'velocity' },
    { label: 'STATUS', key: 'action' }
  ],
  data: () => [],
  currentLap: '01',
  totalLaps: '99',
  fuelLevel: 75,
  primaryColor: '#ff0000',
  accentColor: '#fcee0a',
  bgTable: '#2a2a2e',
  skewAngle: -10
});

const speedTheme = computed(() => ({
  '--s-red': props.primaryColor,
  '--s-accent': props.accentColor,
  '--s-grey': props.bgTable,
  '--s-skew': `${props.skewAngle}deg`,
  '--s-skew-inv': `${props.skewAngle * -1}deg`,
}));
</script>

<style module>
/* --- CORE & LAYOUT --- */
.speedContainer {
  background: #0a0a0b;
  padding: clamp(15px, 3vw, 30px);
  font-family: 'Arial Black', sans-serif;
  color: #f0f0f0;
  max-width: 1000px;
  margin: 0 auto;
  border-right: 8px solid var(--s-red);
  box-sizing: border-box;
  overflow: hidden;
}

/* --- HEADER DINÁMICO --- */
.headerBlock {
  display: flex;
  justify-content: space-between;
  align-items: center;
  flex-wrap: wrap;
  gap: 20px;
  background: var(--s-grey);
  padding: 20px clamp(20px, 4vw, 40px);
  transform: skewX(var(--s-skew));
  margin: 0 15px 35px 15px;
  border-left: 10px solid var(--s-red);
}

.brandBox { transform: skewX(var(--s-skew-inv)); flex: 1; }

.mainTitle { 
  margin: 0; 
  font-size: clamp(1.2rem, 4vw, 1.8rem); 
  font-style: italic;
  background: linear-gradient(to bottom, #fff 50%, #888 50%);
  -webkit-text-fill-color: transparent;
  white-space: nowrap;
}

.engineStatus { font-size: 0.6rem; color: var(--s-accent); letter-spacing: 2px; }

.searchWrapper {
  position: relative;
  flex: 1 1 250px;
  max-width: 100%;
  transform: skewX(var(--s-skew-inv));
}

.inputTag { font-size: 0.5rem; color: var(--s-red); font-weight: 900; margin-bottom: 4px; }

.speedInput {
  width: 100%;
  background: #000;
  border: 1px solid #444;
  padding: 10px 15px;
  color: #fff;
  outline: none;
  font-family: monospace;
  box-sizing: border-box;
}

.speedInput:focus { border-color: var(--s-accent); }

/* --- GRID --- */
.mainGrid {
  display: grid;
  grid-template-columns: 200px 1fr;
  gap: 25px;
}

.sidebar {
  background: var(--s-grey);
  padding: 20px;
  border-top: 4px solid var(--s-red);
  height: fit-content;
}

.subTitle { font-size: 0.8rem; letter-spacing: 2px; border-bottom: 1px solid #444; padding-bottom: 10px; margin-bottom: 15px; }

.filterList { display: flex; flex-direction: column; gap: 12px; }

.speedLabel { display: flex; align-items: center; gap: 10px; font-size: 0.7rem; cursor: pointer; transition: 0.2s; }
.speedLabel:hover { color: var(--s-accent); }

.speedCheck {
  appearance: none; width: 14px; height: 14px;
  border: 2px solid var(--s-red);
  transform: skewX(var(--s-skew));
  cursor: pointer;
}

.speedCheck:checked { background: var(--s-red); box-shadow: 0 0 8px var(--s-red); }

.fuelGauge { height: 4px; background: #000; margin-top: 25px; position: relative; }
.fuelLevel { height: 100%; background: var(--s-accent); box-shadow: 0 0 10px var(--s-accent); transition: width 0.5s; }

/* --- TABLA --- */
.tableBlock { overflow-x: auto; width: 100%; }

.speedTable { width: 100%; border-collapse: separate; border-spacing: 0 10px; min-width: 450px; }

.speedTable th { text-align: left; font-size: 0.6rem; color: #888; padding: 0 15px; text-transform: uppercase; }

.speedTable tr td {
  background: var(--s-grey);
  padding: 15px;
  font-weight: 900;
  font-style: italic;
  font-size: 0.85rem;
}

.speedTable tr td:first-child { border-left: 6px solid var(--s-red); }

.rankBox { background: var(--s-red); padding: 4px 8px; font-size: 0.7rem; color: #fff; }

.userName { position: relative; padding-left: 30px; white-space: nowrap; }

.stripes {
  position: absolute; left: 0; top: 0; bottom: 0;
  width: 12px;
  background: repeating-linear-gradient(45deg, #444, #444 2px, transparent 2px, transparent 6px);
}

.velocityBar { width: 100%; max-width: 120px; height: 6px; background: #000; overflow: hidden; }

.fill {
  height: 100%;
  background: linear-gradient(to right, var(--s-accent), var(--s-red));
  transition: width 1s ease-out;
}

.raceBtn {
  background: #f0f0f0; border: none; padding: 8px 18px;
  font-weight: 900; font-style: italic; font-size: 0.7rem;
  cursor: pointer; clip-path: polygon(15% 0, 100% 0, 85% 100%, 0 100%);
  transition: 0.2s;
}

.raceBtn:hover { background: var(--s-accent); transform: scale(1.05); }

/* --- FOOTER --- */
.footerBlock { margin-top: 30px; border-top: 2px dashed #444; padding: 20px 0; }
.pagination { display: flex; justify-content: space-between; align-items: center; flex-wrap: wrap; gap: 15px; }

.navBtn {
  background: transparent; border: 1px solid #fff; color: #fff;
  padding: 8px 15px; font-size: 0.6rem; font-weight: 800; cursor: pointer; transition: 0.3s;
}

.navBtn:hover { background: var(--s-red); border-color: var(--s-red); box-shadow: 0 0 10px var(--s-red); }

.lapCounter { font-size: 0.8rem; color: var(--s-accent); font-family: monospace; }

/* --- RESPONSIVE FIXES --- */
@media (max-width: 850px) {
  .mainGrid { grid-template-columns: 1fr; }
  .headerBlock { 
    transform: none; border-left: none; border-top: 6px solid var(--s-red); 
    margin: 0 0 25px 0; padding: 20px;
  }
  .brandBox, .searchWrapper { transform: none; }
}

@media (max-width: 500px) {
  .speedContainer { padding: 15px; }
  .speedTable tr td { padding: 10px; font-size: 0.75rem; }
  .velocityBar { display: none; }
}
</style>