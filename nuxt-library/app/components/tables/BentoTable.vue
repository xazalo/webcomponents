<template>
  <div :class="$style.bentoGrid" :style="bentoStyles">
    
    <div :class="[$style.cell, $style.searchCell]">
      <div :class="$style.headerInfo">
        <span :class="$style.label">{{ superTitle }}</span>
        <h2 :class="$style.title">{{ title }}</h2>
      </div>
      <div :class="$style.inputWrapper">
        <input 
          type="text" 
          :placeholder="searchPlaceholder" 
          :class="$style.searchInput" 
          @input="$emit('search', $event.target.value)"
        />
        <span :class="$style.searchIcon">🔍</span>
      </div>
    </div>

    <div 
      v-for="(stat, index) in stats" 
      :key="index"
      :class="[$style.cell, $style.statCell]"
      :style="{ backgroundColor: stat.color || 'var(--bento-accent)' }"
    >
      <span :class="$style.statLabel">{{ stat.label }}</span>
      <span :class="$style.statValue">{{ stat.value }}</span>
    </div>

    <div :class="[$style.cell, $style.tableCell]">
      <div :class="$style.scrollArea">
        <table :class="$style.table">
          <thead>
            <tr>
              <th v-for="col in columns" :key="col.key">{{ col.label }}</th>
            </tr>
          </thead>
          <tbody>
            <tr v-for="(row, i) in data" :key="i">
              <td v-for="col in columns" :key="col.key">
                <slot :name="`cell-${col.key}`" :row="row">
                  <template v-if="col.key === 'user'">
                    <div :class="$style.userPilot">
                      <div :class="$style.avatar" :style="{ backgroundImage: `url(${row.avatar})` }"></div>
                      <span>{{ row[col.key] }}</span>
                    </div>
                  </template>
                  <template v-else-if="col.key === 'status'">
                    <span :class="$style.statusBadge">{{ row[col.key] }}</span>
                  </template>
                  <template v-else>
                    {{ row[col.key] }}
                  </template>
                </slot>
              </td>
            </tr>
          </tbody>
        </table>
      </div>
    </div>

    <div :class="[$style.cell, $style.actionCell]">
      <div :class="$style.pagination">
        <button :class="$style.pageBtn" @click="$emit('prev')">PREV</button>
        <span :class="$style.pageInfo">{{ currentPage }} / {{ totalPages }}</span>
        <button :class="$style.pageBtn" @click="$emit('next')">NEXT</button>
      </div>
      <button :class="$style.exportBtn" @click="$emit('export')">{{ exportText }}</button>
    </div>

  </div>
</template>

<script setup lang="ts">
import { computed } from 'vue';

interface Stat { label: string; value: string | number; color?: string; }
interface Column { label: string; key: string; }

interface Props {
  title?: string;
  superTitle?: string;
  searchPlaceholder?: string;
  stats?: Stat[];
  columns?: Column[];
  data?: any[];
  currentPage?: number | string;
  totalPages?: number | string;
  exportText?: string;
  accentColor?: string;
  borderRadius?: string;
  borderWidth?: string;
}

const props = withDefaults(defineProps<Props>(), {
  title: 'REGISTROS',
  superTitle: 'SYS_ACCESS',
  searchPlaceholder: 'BUSCAR...',
  stats: () => [
    { label: 'ONLINE', value: '842', color: '#55efc4' },
    { label: 'ERRORS', value: '12', color: '#ff7675' }
  ],
  columns: () => [
    { label: 'ID', key: 'id' },
    { label: 'PILOT', key: 'user' },
    { label: 'STATUS', key: 'status' }
  ],
  data: () => [],
  currentPage: 1,
  totalPages: 10,
  exportText: 'EXPORT_DATA',
  accentColor: '#55efc4',
  borderRadius: '24px',
  borderWidth: '3px'
});

defineEmits(['search', 'prev', 'next', 'export']);

const bentoStyles = computed(() => ({
  '--bento-accent': props.accentColor,
  '--bento-radius': props.borderRadius,
  '--bento-border-w': props.borderWidth,
}));
</script>

<style module>
/* --- CONTAINER --- */
.bentoGrid {
  display: grid;
  grid-template-columns: repeat(4, 1fr);
  gap: 16px;
  width: 100%;
  max-width: 1200px;
  margin: 0 auto;
  padding: clamp(10px, 3vw, 20px);
  background: #f5f5f7;
  font-family: 'Arial Black', sans-serif;
  box-sizing: border-box;
}

/* --- CELDA BASE --- */
.cell {
  background: #fff;
  border: var(--bento-border-w) solid #000;
  border-radius: var(--bento-radius);
  padding: clamp(15px, 4vw, 25px);
  box-shadow: 6px 6px 0px #000;
  display: flex;
  flex-direction: column;
  box-sizing: border-box;
  min-width: 0; /* Evita desbordamiento en flex child */
}

/* --- HEADER / SEARCH --- */
.searchCell { grid-column: span 2; justify-content: space-between; }
.label { font-size: 0.6rem; letter-spacing: 2px; opacity: 0.6; text-transform: uppercase; }
.title { margin: 5px 0 0; font-size: clamp(1.5rem, 4vw, 2.2rem); letter-spacing: -1px; }

.inputWrapper { position: relative; margin-top: 20px; }
.searchInput {
  width: 100%; padding: 12px 40px 12px 15px;
  border: var(--bento-border-w) solid #000;
  border-radius: calc(var(--bento-radius) / 2);
  font-weight: 800; font-family: monospace; outline: none; box-sizing: border-box;
}
.searchIcon { position: absolute; right: 15px; top: 50%; transform: translateY(-50%); }

/* --- STATS --- */
.statCell { grid-column: span 1; align-items: flex-start; justify-content: center; }
.statValue { font-size: clamp(1.8rem, 5vw, 2.5rem); line-height: 1; margin-top: 5px; }
.statLabel { font-size: 0.7rem; font-weight: 900; opacity: 0.8; }

/* --- TABLE --- */
.tableCell { grid-column: span 4; }
.scrollArea { overflow-x: auto; width: 100%; }

.table { width: 100%; border-collapse: separate; border-spacing: 0 8px; min-width: 500px; }
.table th { text-align: left; padding: 10px 15px; font-size: 0.65rem; opacity: 0.5; }
.table td {
  padding: 15px; background: #fff; border-top: 2px solid #000; border-bottom: 2px solid #000;
  font-weight: 700; font-size: 0.9rem;
}
.table td:first-child { border-left: 2px solid #000; border-radius: 12px 0 0 12px; }
.table td:last-child { border-right: 2px solid #000; border-radius: 0 12px 12px 0; }

.userPilot { display: flex; align-items: center; gap: 10px; }
.avatar { width: 30px; height: 30px; border-radius: 50%; border: 2px solid #000; background-size: cover; flex-shrink: 0; }
.statusBadge { background: #000; color: #fff; padding: 4px 8px; font-size: 0.6rem; border-radius: 4px; }

/* --- ACTIONS / FOOTER --- */
.actionCell {
  grid-column: span 4; flex-direction: row; justify-content: space-between;
  align-items: center; background: #000; color: #fff; flex-wrap: wrap; gap: 15px;
}

.pagination { display: flex; align-items: center; gap: 10px; flex-wrap: wrap; }
.pageBtn {
  background: #fff; border: 2px solid #000; padding: 8px 16px;
  border-radius: 8px; font-weight: 900; cursor: pointer; font-size: 0.75rem;
}
.pageInfo { font-family: monospace; font-weight: 900; font-size: 0.9rem; }
.exportBtn {
  background: var(--bento-accent); border: 2px solid #000; padding: 10px 20px;
  border-radius: 8px; font-weight: 900; cursor: pointer; color: #000;
}

/* --- BREAKPOINTS --- */
@media (max-width: 1024px) {
  .bentoGrid { grid-template-columns: repeat(2, 1fr); }
  .statCell { grid-column: span 1; }
}

@media (max-width: 600px) {
  .bentoGrid { grid-template-columns: 1fr; }
  .searchCell, .statCell, .tableCell, .actionCell { grid-column: span 1; }
  .actionCell { flex-direction: column; text-align: center; }
  .pagination { justify-content: center; }
}
</style>