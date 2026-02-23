<template>
  <div :class="$style.neoContainer" :style="neoTheme">
    <header :class="$style.headerBlock">
      <div :class="$style.titleBox">
        <h1 :class="$style.mainTitle">{{ title }}</h1>
        <div :class="$style.badge">{{ version }}</div>
      </div>
      <div :class="$style.searchBox">
        <input 
          type="text" 
          :placeholder="searchPlaceholder" 
          :class="$style.neoInput" 
          @input="$emit('search', $event.target.value)"
        />
        <button :class="$style.searchBtn">GO!</button>
      </div>
    </header>

    <div :class="$style.mainGrid">
      <aside :class="$style.filterBlock">
        <h3 :class="$style.subTitle">FILTROS_</h3>
        <div :class="$style.filterList">
          <label v-for="tag in tags" :key="tag" :class="$style.checkLabel">
            <input type="checkbox" :class="$style.neoCheck" @change="$emit('filter', tag)" />
            <span :class="$style.checkText">{{ tag }}</span>
          </label>
        </div>
        <button :class="$style.resetBtn" @click="$emit('reset')">WIPE_ALL</button>
      </aside>

      <main :class="$style.tableBlock">
        <div :class="$style.scrollArea">
          <table :class="$style.neoTable">
            <thead>
              <tr>
                <th v-for="col in columns" :key="col.key">{{ col.label }}</th>
              </tr>
            </thead>
            <tbody>
              <tr v-for="(item, i) in data" :key="i">
                <td v-for="col in columns" :key="col.key">
                  <slot :name="`col-${col.key}`" :item="item" :index="i">
                    <span :class="[col.key === 'id' ? $style.mono : '', col.key === 'user' ? $style.userName : '']">
                      {{ item[col.key] }}
                    </span>
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
        <button :class="$style.pageBtn" @click="$emit('prev')">⇽ BACK</button>
        <div :class="$style.pageNumber">PAGE_{{ currentPage }}</div>
        <button :class="$style.pageBtn" @click="$emit('next')">NEXT ⇾</button>
      </div>
    </footer>
  </div>
</template>

<script setup lang="ts">
import { computed } from 'vue';

interface Props {
  title?: string;
  version?: string;
  searchPlaceholder?: string;
  tags?: string[];
  columns?: { label: string; key: string }[];
  data?: any[];
  currentPage?: number | string;
  primaryColor?: string;
  secondaryColor?: string;
  accentColor?: string;
  successColor?: string;
  borderWidth?: string;
  shadowDepth?: string;
}

const props = withDefaults(defineProps<Props>(), {
  title: 'DATA_CORE',
  version: 'v.3.0_LIVE',
  searchPlaceholder: 'BUSCAR...',
  tags: () => ['ADMIN', 'USER', 'BOT'],
  columns: () => [
    { label: 'UUID', key: 'id' },
    { label: 'IDENTIDAD', key: 'user' },
    { label: 'ESTADO', key: 'status' }
  ],
  data: () => [],
  currentPage: 1,
  primaryColor: '#f4fd05',
  secondaryColor: '#ff00f5',
  accentColor: '#32feff',
  successColor: '#00ff41',
  borderWidth: '4px',
  shadowDepth: '8px'
});

const neoTheme = computed(() => ({
  '--neo-primary': props.primaryColor,
  '--neo-secondary': props.secondaryColor,
  '--neo-accent': props.accentColor,
  '--neo-success': props.successColor,
  '--neo-border-w': props.borderWidth,
  '--neo-shadow-d': props.shadowDepth,
}));
</script>

<style module>
/* --- BASE --- */
.neoContainer {
  --neo-black: #000;
  --neo-white: #fff;
  background: var(--neo-white);
  padding: clamp(10px, 4vw, 30px);
  font-family: 'Arial Black', 'Inter', sans-serif;
  color: var(--neo-black);
  display: flex;
  flex-direction: column;
  gap: clamp(15px, 4vw, 25px);
  width: 100%;
  box-sizing: border-box;
}

/* --- HEADER --- */
.headerBlock {
  background: var(--neo-primary);
  border: var(--neo-border-w) solid var(--neo-black);
  padding: clamp(15px, 4vw, 25px);
  box-shadow: var(--neo-shadow-d) var(--neo-shadow-d) 0px var(--neo-black);
  display: flex;
  justify-content: space-between;
  align-items: center;
  flex-wrap: wrap;
  gap: 20px;
}

.titleBox { flex: 1 1 200px; }
.mainTitle { 
  margin: 0; 
  font-size: clamp(1.8rem, 8vw, 3.5rem); 
  line-height: 0.9; 
  letter-spacing: -2px; 
  text-transform: uppercase;
}

.badge {
  background: var(--neo-black);
  color: var(--neo-white);
  padding: 4px 10px;
  display: inline-block;
  margin-top: 8px;
  font-size: 0.75rem;
  font-weight: normal;
}

.searchBox { 
  display: flex; 
  gap: 0; /* Unimos input y botón estilo bloque */
  flex: 1 1 300px; 
  min-width: 0;
}

.neoInput {
  flex: 1;
  border: var(--neo-border-w) solid var(--neo-black);
  padding: 12px;
  font-weight: 900;
  font-family: 'Courier New', monospace;
  min-width: 0;
  outline: none;
}

.searchBtn {
  background: var(--neo-secondary);
  border: var(--neo-border-w) solid var(--neo-black);
  border-left: none; /* Evita doble borde en la unión */
  padding: 0 20px;
  font-weight: 900;
  cursor: pointer;
}

.searchBtn:hover { background: var(--neo-accent); }

/* --- GRID --- */
.mainGrid {
  display: grid;
  grid-template-columns: 260px 1fr;
  gap: clamp(15px, 4vw, 25px);
}

.filterBlock {
  background: var(--neo-accent);
  border: var(--neo-border-w) solid var(--neo-black);
  padding: 20px;
  box-shadow: var(--neo-shadow-d) var(--neo-shadow-d) 0px var(--neo-black);
  height: fit-content;
}

.subTitle { font-size: 1rem; margin-bottom: 15px; text-decoration: underline; }
.filterList { display: flex; flex-direction: column; gap: 12px; margin-bottom: 20px; }

.checkLabel { display: flex; align-items: center; gap: 10px; cursor: pointer; font-size: 0.9rem; }
.neoCheck {
  width: 24px; height: 24px;
  border: 3px solid var(--neo-black);
  appearance: none;
  background: white;
  cursor: pointer;
  flex-shrink: 0;
}
.neoCheck:checked { background: var(--neo-black); position: relative; }
.neoCheck:checked::after {
  content: '✕'; color: var(--neo-white); position: absolute;
  left: 50%; top: 50%; transform: translate(-50%, -50%); font-size: 14px;
}

.resetBtn {
  width: 100%; background: var(--neo-black); color: var(--neo-white);
  border: none; padding: 10px; font-weight: 900; cursor: pointer;
  text-transform: uppercase; font-size: 0.8rem;
}

/* --- TABLE --- */
.tableBlock {
  background: var(--neo-white);
  border: var(--neo-border-w) solid var(--neo-black);
  box-shadow: var(--neo-shadow-d) var(--neo-shadow-d) 0px var(--neo-black);
  min-width: 0;
}

.scrollArea { overflow-x: auto; width: 100%; }

.neoTable { width: 100%; border-collapse: collapse; min-width: 500px; }
.neoTable th {
  background: var(--neo-black); color: var(--neo-white);
  padding: 15px; text-align: left; font-size: 0.8rem;
}

.neoTable td {
  padding: 15px;
  border-bottom: var(--neo-border-w) solid var(--neo-black);
  font-weight: 800; font-size: 0.9rem;
}

.userName { color: var(--neo-secondary); text-transform: uppercase; }
.mono { font-family: monospace; opacity: 0.7; }

/* --- FOOTER --- */
.footerBlock {
  background: var(--neo-black);
  padding: 15px;
  border: var(--neo-border-w) solid var(--neo-black);
  box-shadow: var(--neo-shadow-d) var(--neo-shadow-d) 0px var(--neo-black);
}

.pagination { 
  display: flex; 
  justify-content: space-between; 
  align-items: center; 
  flex-wrap: wrap;
  gap: 15px;
}

.pageBtn {
  background: var(--neo-primary);
  border: 3px solid var(--neo-black);
  padding: 8px 20px;
  font-weight: 900;
  cursor: pointer;
  box-shadow: 4px 4px 0px var(--neo-white);
}

.pageBtn:active { transform: translate(2px, 2px); box-shadow: none; }
.pageNumber { color: var(--neo-white); font-size: 1rem; letter-spacing: 1px; }

/* --- BREAKPOINTS --- */
@media (max-width: 900px) {
  .mainGrid { grid-template-columns: 1fr; }
  .headerBlock { flex-direction: column; align-items: stretch; }
  .searchBox { max-width: 100%; }
}

@media (max-width: 480px) {
  .neoContainer { padding: 10px; }
  .mainTitle { line-height: 1; }
  .neoTable td { padding: 10px; font-size: 0.8rem; }
}
</style>