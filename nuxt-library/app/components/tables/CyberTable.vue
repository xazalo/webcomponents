<template>
  <div :class="$style.cyberContainer" :style="cyberTheme">
    <div v-if="showScanlines" :class="$style.scanlines"></div>

    <header :class="$style.headerBlock">
      <div :class="$style.glitchWrapper">
        <h1 :class="$style.mainTitle" :data-text="title">{{ title }}</h1>
        <div :class="$style.subHeader">STATUS: {{ connectionStatus }} // SYS_READY</div>
      </div>
      <div :class="$style.searchWrapper">
        <div :class="$style.inputTag">QUERY_SYS</div>
        <input 
          type="text" 
          :placeholder="searchPlaceholder" 
          :class="$style.cyberInput" 
          @input="$emit('search', $event.target.value)"
        />
        <div :class="$style.searchGlow"></div>
      </div>
    </header>

    <div :class="$style.mainGrid">
      <aside :class="$style.filterBlock">
        <div :class="$style.cornerDeco"></div>
        <h3 :class="$style.subTitle">_FILTROS_ACCESO</h3>
        <div :class="$style.filterList">
          <label v-for="tag in tags" :key="tag" :class="$style.cyberLabel">
            <input type="checkbox" :class="$style.cyberCheck" @change="$emit('filter', tag)" />
            <span :class="$style.checkText">{{ tag }}</span>
          </label>
        </div>
        <button :class="$style.rebootBtn" @click="$emit('reboot')">FORCE_REBOOT</button>
      </aside>

      <main :class="$style.tableBlock">
        <div :class="$style.scrollArea">
          <table :class="$style.cyberTable">
            <thead>
              <tr>
                <th v-for="col in columns" :key="col.key">[{{ col.label }}]</th>
              </tr>
            </thead>
            <tbody>
              <tr v-for="(item, i) in data" :key="i">
                <td v-for="col in columns" :key="col.key">
                  <slot :name="`col-${col.key}`" :item="item" :index="i">
                    <template v-if="col.key === 'id'">
                      <span :class="$style.idText">#0x{{ item[col.key] || i.toString(16) }}</span>
                    </template>
                    <template v-else-if="col.key === 'user'">
                      <div :class="$style.userName">
                        <span :class="$style.userIcon">◮</span>
                        {{ item[col.key] }}
                      </div>
                    </template>
                    <template v-else-if="col.key === 'level'">
                      <div :class="$style.levelContainer">
                        <div :class="[$style.levelBar, item.isCritical ? $style.high : $style.low]" 
                             :style="{ width: (item.progress || 30) + 'px' }"></div>
                        <span :class="$style.levelText">{{ item[col.key] }}</span>
                      </div>
                    </template>
                    <template v-else>
                      {{ item[col.key] }}
                    </template>
                  </slot>
                </td>
              </tr>
            </tbody>
          </table>
        </div>
      </main>
    </div>

    <footer :class="$style.footerBlock">
      <div :class="$style.pagination">
        <button :class="$style.pageBtn" @click="$emit('prev')">PREV_SEC</button>
        <div :class="$style.terminalInfo">
          PAGE_{{ currentPage }}_OF_{{ totalPages }} // [STABLE]
        </div>
        <button :class="$style.pageBtn" @click="$emit('next')">NEXT_SEC</button>
      </div>
    </footer>
  </div>
</template>

<script setup lang="ts">
import { computed } from 'vue';

interface Props {
  title?: string;
  connectionStatus?: string;
  searchPlaceholder?: string;
  tags?: string[];
  columns?: { label: string; key: string }[];
  data?: any[];
  currentPage?: number | string;
  totalPages?: number | string;
  // Cyber Customization
  primaryNeon?: string;   
  secondaryNeon?: string; 
  accentNeon?: string;    
  showScanlines?: boolean;
  glitchIntensity?: number; 
}

const props = withDefaults(defineProps<Props>(), {
  title: 'NET_RUNNER_v4.2',
  connectionStatus: 'ENCRYPTED',
  searchPlaceholder: 'BUSCAR...',
  tags: () => ['ADMIN', 'USER', 'DAEMON'],
  columns: () => [
    { label: 'ID', key: 'id' },
    { label: 'IDENTIDAD', key: 'user' },
    { label: 'ACCESO', key: 'level' }
  ],
  data: () => [],
  currentPage: 1,
  totalPages: 4,
  primaryNeon: '#00f0ff',
  secondaryNeon: '#ff003c',
  accentNeon: '#fcee0a',
  showScanlines: true,
  glitchIntensity: 2
});

const cyberTheme = computed(() => ({
  '--n-blue': props.primaryNeon,
  '--n-pink': props.secondaryNeon,
  '--n-yellow': props.accentNeon,
  '--glitch-offset': `${props.glitchIntensity}px`,
}));
</script>

<style module>
/* --- BASE CONTAINER --- */
.cyberContainer {
  background: #050505;
  padding: clamp(15px, 4vw, 30px);
  font-family: 'Orbitron', 'Segoe UI', monospace;
  color: var(--n-blue);
  position: relative;
  width: 100%;
  max-width: 1200px;
  margin: 0 auto;
  border: 1px solid rgba(0, 240, 255, 0.2);
  box-sizing: border-box;
  overflow: hidden;
}

.scanlines {
  position: absolute; inset: 0;
  background: linear-gradient(to bottom, transparent 50%, rgba(0, 240, 255, 0.03) 50%);
  background-size: 100% 4px;
  pointer-events: none; z-index: 5;
}

/* --- HEADER --- */
.headerBlock {
  display: flex;
  justify-content: space-between;
  align-items: flex-end;
  flex-wrap: wrap;
  gap: 20px;
  border-bottom: 2px solid var(--n-blue);
  padding-bottom: 20px;
  margin-bottom: 30px;
}

.mainTitle {
  margin: 0;
  font-size: clamp(1.4rem, 5vw, 2.2rem);
  letter-spacing: 4px;
  text-transform: uppercase;
  position: relative;
  text-shadow: var(--glitch-offset) 0 var(--n-pink), calc(var(--glitch-offset) * -1) 0 rgba(255,255,255,0.3);
}

.subHeader { font-size: 0.65rem; opacity: 0.8; letter-spacing: 2px; margin-top: 5px; }

.searchWrapper {
  position: relative;
  flex: 1 1 300px;
  max-width: 100%;
}

.inputTag {
  position: absolute; top: -10px; left: 10px;
  background: var(--n-blue); color: #000;
  font-size: 0.55rem; padding: 2px 8px; font-weight: 900;
  z-index: 2;
}

.cyberInput {
  width: 100%; background: rgba(0, 240, 255, 0.05);
  border: 1px solid var(--n-blue); padding: 12px 15px;
  color: var(--n-blue); outline: none; font-family: monospace;
  box-sizing: border-box;
}

.searchGlow {
  position: absolute; bottom: 0; left: 0; width: 0%; height: 2px;
  background: var(--n-pink); transition: width 0.3s ease;
}

.cyberInput:focus + .searchGlow { width: 100%; }

/* --- MAIN GRID --- */
.mainGrid {
  display: grid;
  grid-template-columns: 240px 1fr;
  gap: 30px;
}

.filterBlock {
  background: #0f0f0f;
  border-left: 4px solid var(--n-pink);
  padding: 25px;
  position: relative;
  clip-path: polygon(0 0, 100% 0, 100% 90%, 90% 100%, 0 100%);
  height: fit-content;
}

.subTitle { font-size: 0.75rem; color: var(--n-pink); margin-bottom: 20px; letter-spacing: 2px; text-transform: uppercase; }

.filterList { display: flex; flex-direction: column; gap: 15px; }

.cyberLabel { display: flex; align-items: center; gap: 10px; cursor: pointer; font-size: 0.75rem; }

.cyberCheck {
  appearance: none; width: 16px; height: 16px; border: 1px solid var(--n-blue);
  cursor: pointer; position: relative;
}

.cyberCheck:checked { background: var(--n-blue); box-shadow: 0 0 10px var(--n-blue); }
.cyberCheck:checked::after { content: '×'; position: absolute; color: #000; font-weight: 900; left: 3px; top: -2px; }

.rebootBtn {
  margin-top: 30px; width: 100%; background: transparent;
  border: 1px solid var(--n-pink); color: var(--n-pink);
  padding: 10px; font-size: 0.65rem; cursor: pointer;
  font-family: inherit; font-weight: bold;
}

.rebootBtn:hover { background: var(--n-pink); color: #000; box-shadow: 0 0 15px var(--n-pink); }

/* --- TABLE SECTION --- */
.tableBlock { background: rgba(0, 0, 0, 0.4); min-width: 0; }
.scrollArea { overflow-x: auto; width: 100%; }

.cyberTable { width: 100%; border-collapse: collapse; min-width: 550px; }

.cyberTable th {
  text-align: left; padding: 15px; font-size: 0.65rem;
  color: var(--n-yellow); border-bottom: 1px solid rgba(0, 240, 255, 0.2);
  text-transform: uppercase;
}

.cyberTable td { 
  padding: 15px; 
  border-bottom: 1px solid rgba(0, 240, 255, 0.05); 
  font-size: 0.8rem; 
  white-space: nowrap;
}

.idText { color: var(--n-pink); font-family: monospace; font-weight: 700; }

.userName { display: flex; align-items: center; gap: 10px; color: #fff; }

.userIcon { color: var(--n-blue); animation: blink 1.5s infinite; }

.levelContainer { display: flex; align-items: center; gap: 10px; }
.levelBar { height: 4px; background: var(--n-blue); box-shadow: 0 0 8px var(--n-blue); }
.high { background: var(--n-pink); box-shadow: 0 0 8px var(--n-pink); }
.low { background: #333; box-shadow: none; }
.levelText { font-size: 0.7rem; opacity: 0.8; }

/* --- FOOTER --- */
.footerBlock {
  margin-top: 30px; 
  border: 1px solid rgba(0, 240, 255, 0.2);
  padding: 15px 25px; 
  background: rgba(0, 240, 255, 0.02);
}

.pagination { 
  display: flex; 
  justify-content: space-between; 
  align-items: center; 
  flex-wrap: wrap; 
  gap: 15px; 
}

.pageBtn { 
  background: transparent; border: none; color: var(--n-blue); 
  cursor: pointer; text-decoration: underline; font-size: 0.75rem; 
  font-family: inherit; font-weight: 800;
}

.terminalInfo { font-size: 0.65rem; color: var(--n-yellow); font-family: monospace; }

@keyframes blink { 0%, 100% { opacity: 1; } 50% { opacity: 0.2; } }

/* --- RESPONSIVE BREAKPOINTS --- */
@media (max-width: 850px) {
  .mainGrid { grid-template-columns: 1fr; }
  .headerBlock { flex-direction: column; align-items: flex-start; }
  .filterBlock { clip-path: none; border-right: 4px solid var(--n-pink); }
}

@media (max-width: 480px) {
  .cyberContainer { padding: 10px; border: none; }
  .footerBlock { padding: 10px; }
  .terminalInfo { display: none; } /* Ocultar info extra en móviles pequeños */
}
</style>