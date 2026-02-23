<template>
  <div :class="$style.bubbleContainer" :style="bubbleTheme">
    <header :class="$style.headerBlock">
      <div :class="$style.titleBox">
        <h1 :class="$style.mainTitle">{{ title }}</h1>
        <div :class="$style.statusDot">{{ statusLabel }}</div>
      </div>
      <div :class="$style.searchBox">
        <input 
          type="text" 
          :placeholder="searchPlaceholder" 
          :class="$style.bubbleInput" 
          @input="$emit('search', $event.target.value)"
        />
        <button :class="$style.searchBtn">✨</button>
      </div>
    </header>

    <div :class="$style.mainGrid">
      <aside :class="$style.filterBlock">
        <h3 :class="$style.subTitle">Filtros</h3>
        <div :class="$style.filterList">
          <label v-for="tag in tags" :key="tag" :class="$style.checkLabel">
            <input type="checkbox" :class="$style.bubbleCheck" @change="$emit('filter', tag)" />
            <span :class="$style.checkText">{{ tag }}</span>
          </label>
        </div>
        <button :class="$style.resetBtn" @click="$emit('reset')">Limpiar todo</button>
      </aside>

      <main :class="$style.tableBlock">
        <div :class="$style.scrollArea">
          <table :class="$style.bubbleTable">
            <thead>
              <tr>
                <th v-for="col in columns" :key="col.key">{{ col.label }}</th>
              </tr>
            </thead>
            <tbody>
              <tr v-for="(item, i) in data" :key="i">
                <td v-for="col in columns" :key="col.key">
                  <slot :name="`col-${col.key}`" :item="item">
                    <template v-if="col.key === 'user'">
                      <div :class="$style.userName">
                        <div :class="$style.avatar" :style="{ background: item.avatarColor || 'var(--bb-gradient)' }"></div>
                        <span>{{ item[col.key] }}</span>
                      </div>
                    </template>
                    <template v-else-if="col.key === 'status'">
                      <span :class="[$style.status, item.isActive ? $style.active : $style.pending]">
                        {{ item[col.key] }}
                      </span>
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
        <button :class="$style.pageBtn" @click="$emit('prev')">Anterior</button>
        <div :class="$style.pageNumber">Página {{ currentPage }}</div>
        <button :class="$style.pageBtn" @click="$emit('next')">Siguiente</button>
      </div>
    </footer>
  </div>
</template>

<script setup lang="ts">
import { computed } from 'vue';

interface Props {
  title?: string;
  statusLabel?: string;
  searchPlaceholder?: string;
  tags?: string[];
  columns?: { label: string; key: string }[];
  data?: any[];
  currentPage?: number | string;
  primaryColor?: string;
  secondaryColor?: string;
  softBg?: string;
  roundness?: string;
  shadowOpacity?: number;
}

const props = withDefaults(defineProps<Props>(), {
  title: 'Data_Cloud',
  statusLabel: 'LIVE_STREAM',
  searchPlaceholder: 'Buscar...',
  tags: () => ['Admin', 'User', 'Bot'],
  columns: () => [
    { label: 'ID', key: 'id' },
    { label: 'Identidad', key: 'user' },
    { label: 'Estado', key: 'status' }
  ],
  data: () => [],
  currentPage: 1,
  primaryColor: '#ff85a1',
  secondaryColor: '#70d6ff',
  softBg: '#f0f4f8',
  roundness: '30px',
  shadowOpacity: 0.05
});

defineEmits(['search', 'filter', 'reset', 'prev', 'next']);

const bubbleTheme = computed(() => ({
  '--bb-primary': props.primaryColor,
  '--bb-secondary': props.secondaryColor,
  '--bb-bg': props.softBg,
  '--bb-radius': props.roundness,
  '--bb-shadow': `0 10px 30px rgba(0,0,0, ${props.shadowOpacity})`,
  '--bb-gradient': `linear-gradient(45deg, ${props.primaryColor}, ${props.secondaryColor})`
}));
</script>

<style module>
/* --- CORE --- */
.bubbleContainer {
  background: var(--bb-bg);
  padding: clamp(10px, 3vw, 30px);
  font-family: 'Quicksand', sans-serif;
  color: #2d3436;
  display: flex;
  flex-direction: column;
  gap: 20px;
  width: 100%;
  max-width: 1100px;
  margin: 0 auto;
  border-radius: calc(var(--bb-radius) * 1.2);
  box-sizing: border-box;
}

/* Base de burbuja blanca */
.headerBlock, .filterBlock, .footerBlock, .bubbleTable td {
  background: #fff;
  border-radius: var(--bb-radius);
  box-shadow: var(--bb-shadow);
}

/* --- HEADER --- */
.headerBlock {
  padding: 20px clamp(15px, 5vw, 40px);
  display: flex;
  justify-content: space-between;
  align-items: center;
  flex-wrap: wrap;
  gap: 20px;
}

.titleBox { flex: 1 1 auto; }
.mainTitle { 
  margin: 0; font-size: clamp(1.5rem, 5vw, 2.2rem); font-weight: 800; 
  background: var(--bb-gradient);
  -webkit-background-clip: text;
  background-clip: text;
  color: transparent;
}

.statusDot {
  font-size: 0.65rem; font-weight: 800; color: #fff;
  background: var(--bb-gradient);
  padding: 3px 12px; border-radius: 50px;
  display: inline-block; margin-top: 5px;
}

.searchBox { display: flex; gap: 10px; flex: 1 1 300px; min-width: 0; }
.bubbleInput {
  flex: 1; border: 3px solid #f1f2f6; background: #f8f9fa;
  padding: 12px 20px; border-radius: 50px; font-weight: 700;
  outline: none; min-width: 0; transition: border-color 0.3s;
}
.bubbleInput:focus { border-color: var(--bb-secondary); }

.searchBtn {
  width: 48px; height: 48px; border: none; border-radius: 50%;
  background: var(--bb-secondary); color: white; cursor: pointer;
  flex-shrink: 0; box-shadow: 0 5px 15px rgba(0,0,0,0.1);
  display: flex; align-items: center; justify-content: center;
}

/* --- LAYOUT GRID --- */
.mainGrid { display: grid; grid-template-columns: 260px 1fr; gap: 20px; }

.filterBlock { padding: 25px; height: fit-content; }
.subTitle { font-weight: 800; margin-bottom: 15px; color: #b2bec3; text-transform: uppercase; font-size: 0.75rem; }

.filterList { display: flex; flex-direction: column; gap: 12px; }
.checkLabel { display: flex; align-items: center; gap: 10px; cursor: pointer; font-weight: 700; font-size: 0.85rem; }

.bubbleCheck {
  width: 24px; height: 24px; border: 3px solid #f1f2f6;
  appearance: none; border-radius: 8px; cursor: pointer;
  position: relative; flex-shrink: 0;
}
.bubbleCheck:checked { background: var(--bb-primary); border-color: var(--bb-primary); }
.bubbleCheck:checked::after {
  content: '✓'; color: white; position: absolute; left: 50%; top: 50%; transform: translate(-50%, -50%);
}

.resetBtn {
  width: 100%; margin-top: 20px; background: #f1f2f6; border: none;
  padding: 10px; border-radius: 15px; font-weight: 800; cursor: pointer;
  transition: background 0.2s;
}
.resetBtn:hover { background: #e2e3e9; }

/* --- TABLE --- */
.tableBlock { min-width: 0; }
.scrollArea { overflow-x: auto; width: 100%; padding-bottom: 10px; }

.bubbleTable { width: 100%; border-collapse: separate; border-spacing: 0 12px; min-width: 500px; }
.bubbleTable th { padding: 0 20px 5px; text-align: left; color: #b2bec3; font-size: 0.75rem; font-weight: 800; }

.bubbleTable td { padding: 15px 20px; font-weight: 700; font-size: 0.9rem; border: none; }
.bubbleTable td:first-child { border-radius: var(--bb-radius) 0 0 var(--bb-radius); }
.bubbleTable td:last-child { border-radius: 0 var(--bb-radius) var(--bb-radius) 0; }

.userName { display: flex; align-items: center; gap: 12px; white-space: nowrap; }
.avatar { width: 34px; height: 34px; border-radius: 50%; flex-shrink: 0; border: 2px solid #fff; box-shadow: 0 4px 8px rgba(0,0,0,0.1); }

.status { padding: 5px 12px; border-radius: 50px; font-size: 0.7rem; font-weight: 800; }
.active { background: #e3fcef; color: #00b894; }
.pending { background: #fff4e5; color: #e67e22; }

/* --- FOOTER --- */
.footerBlock { padding: 15px 30px; }
.pagination { display: flex; justify-content: space-between; align-items: center; flex-wrap: wrap; gap: 15px; }

.pageBtn {
  background: #f1f2f6; border: none; padding: 10px 20px;
  border-radius: 50px; font-weight: 800; cursor: pointer; font-size: 0.8rem;
  transition: transform 0.2s;
}
.pageBtn:active { transform: scale(0.95); }
.pageNumber { font-weight: 800; color: #b2bec3; font-size: 0.85rem; }

/* --- RESPONSIVE --- */
@media (max-width: 900px) {
  .mainGrid { grid-template-columns: 1fr; }
  .headerBlock { justify-content: center; text-align: center; }
  .searchBox { width: 100%; }
}

@media (max-width: 480px) {
  .bubbleContainer { padding: 10px; }
  .bubbleTable td { padding: 12px 10px; font-size: 0.8rem; }
  .avatar { width: 28px; height: 28px; }
  .pagination { justify-content: center; }
}
</style>