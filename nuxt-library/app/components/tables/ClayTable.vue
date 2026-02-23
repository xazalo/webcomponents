<template>
  <div :class="$style.clayContainer" :style="clayTheme">
    <header :class="$style.headerBlock">
      <div :class="$style.titleBox">
        <h1 :class="$style.mainTitle">{{ title }}</h1>
        <div :class="$style.statusBadge">{{ statusLabel }}</div>
      </div>
      <div :class="$style.searchBox">
        <input 
          type="text" 
          :placeholder="searchPlaceholder" 
          :class="$style.clayInput" 
          @input="$emit('search', $event.target.value)"
        />
      </div>
    </header>

    <div :class="$style.mainGrid">
      <aside :class="$style.filterBlock">
        <h3 :class="$style.subTitle">Filtros</h3>
        <div :class="$style.filterList">
          <label v-for="tag in tags" :key="tag" :class="$style.checkLabel">
            <input type="checkbox" :class="$style.clayCheck" @change="$emit('filter', tag)" />
            <span :class="$style.checkText">{{ tag }}</span>
          </label>
        </div>
        <button :class="$style.resetBtn" @click="$emit('reset')">Reiniciar</button>
      </aside>

      <main :class="$style.tableBlock">
        <div :class="$style.scrollArea">
          <table :class="$style.clayTable">
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
                        <div :class="$style.clayAvatar" :style="{ background: item.avatarColor || 'var(--clay-accent)' }"></div>
                        <span>{{ item[col.key] }}</span>
                      </div>
                    </template>
                    <template v-else-if="col.key === 'status'">
                      <span :class="[$style.status, item.isActive ? $style.clayGreen : $style.clayYellow]">
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
        <button :class="$style.navBtn" @click="$emit('prev')">Ant.</button>
        <span :class="$style.pageCounter">{{ currentPage }} / {{ totalPages }}</span>
        <button :class="$style.navBtn" @click="$emit('next')">Sig.</button>
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
  totalPages?: number | string;
  baseColor?: string;
  cardColor?: string;
  accentColor?: string;
  depth?: number;
  borderRadius?: string;
}

const props = withDefaults(defineProps<Props>(), {
  title: 'Clay_Database',
  statusLabel: 'Sincronizado',
  searchPlaceholder: 'Buscar...',
  tags: () => ['Admin', 'User', 'Bot'],
  columns: () => [
    { label: 'ID', key: 'id' },
    { label: 'Usuario', key: 'user' },
    { label: 'Estado', key: 'status' }
  ],
  data: () => [],
  currentPage: 1,
  totalPages: 10,
  baseColor: '#e2eafc',
  cardColor: '#ffffff',
  accentColor: '#8e94f2',
  depth: 8,
  borderRadius: '30px'
});

const clayTheme = computed(() => {
  const d = props.depth;
  return {
    '--clay-bg': props.baseColor,
    '--clay-card': props.cardColor,
    '--clay-accent': props.accentColor,
    '--clay-radius': props.borderRadius,
    '--clay-shadow': `${d * 1.2}px ${d * 1.2}px ${d * 2.5}px rgba(0, 0, 0, 0.05), inset -${d}px -${d}px ${d * 1.8}px rgba(0, 0, 0, 0.05), inset ${d}px ${d}px ${d * 1.8}px rgba(255, 255, 255, 0.8)`,
    '--clay-inset': `inset ${d / 2}px ${d / 2}px ${d}px rgba(0, 0, 0, 0.05), inset -${d / 2}px -${d / 2}px ${d}px rgba(255, 255, 255, 0.8)`
  };
});
</script>

<style module>
/* --- CORE --- */
.clayContainer {
  background: var(--clay-bg);
  padding: clamp(15px, 4vw, 35px);
  font-family: 'Quicksand', sans-serif;
  color: #4a5568;
  display: flex;
  flex-direction: column;
  gap: 25px;
  width: 100%;
  max-width: 1100px;
  margin: 0 auto;
  border-radius: calc(var(--clay-radius) * 1.2);
  box-sizing: border-box;
}

/* Mixin de piezas infladas */
.headerBlock, .filterBlock, .navBtn, .footerBlock, .clayTable tr td {
  background: var(--clay-card);
  box-shadow: var(--clay-shadow);
  border: none;
}

/* --- HEADER --- */
.headerBlock {
  border-radius: var(--clay-radius);
  padding: 20px clamp(20px, 5vw, 45px);
  display: flex;
  justify-content: space-between;
  align-items: center;
  flex-wrap: wrap;
  gap: 20px;
}

.titleBox { display: flex; align-items: center; gap: 15px; flex-wrap: wrap; }
.mainTitle { margin: 0; font-size: clamp(1.4rem, 4vw, 2rem); font-weight: 800; color: #5a67d8; }

.statusBadge {
  background: #b9fbc0;
  padding: 4px 12px;
  border-radius: 15px;
  font-size: 0.7rem; font-weight: 700;
  box-shadow: var(--clay-inset);
  white-space: nowrap;
}

.searchBox { flex: 1 1 250px; max-width: 100%; }

.clayInput {
  width: 100%; border: none; background: var(--clay-bg);
  padding: 12px 25px; border-radius: calc(var(--clay-radius) / 1.5);
  box-shadow: var(--clay-inset);
  font-weight: 600; outline: none; box-sizing: border-box;
}

/* --- GRID --- */
.mainGrid {
  display: grid;
  grid-template-columns: 240px 1fr;
  gap: 25px;
}

.filterBlock { border-radius: var(--clay-radius); padding: 25px; height: fit-content; }
.subTitle { font-weight: 800; margin-bottom: 20px; opacity: 0.7; font-size: 0.9rem; text-transform: uppercase; }

.filterList { display: flex; flex-direction: column; gap: 15px; }
.checkLabel { display: flex; align-items: center; gap: 12px; cursor: pointer; font-weight: 700; font-size: 0.9rem; }

.clayCheck {
  width: 24px; height: 24px; appearance: none;
  background: var(--clay-bg); border-radius: 8px;
  box-shadow: var(--clay-inset); position: relative; cursor: pointer; flex-shrink: 0;
}
.clayCheck:checked { background: var(--clay-accent); }
.clayCheck:checked::after {
  content: ''; position: absolute; width: 6px; height: 10px;
  border: solid white; border-width: 0 3px 3px 0;
  transform: rotate(45deg); left: 8px; top: 4px;
}

.resetBtn {
  margin-top: 25px; width: 100%; border: none;
  background: #ffafcc; padding: 12px; border-radius: 15px;
  font-weight: 800; color: white; cursor: pointer;
  box-shadow: inset -4px -4px 8px rgba(0,0,0,0.1), 4px 4px 10px rgba(0,0,0,0.05);
  transition: transform 0.2s;
}
.resetBtn:active { transform: scale(0.95); }

/* --- TABLE --- */
.tableBlock { min-width: 0; }
.scrollArea { overflow-x: auto; width: 100%; padding: 5px; }

.clayTable { width: 100%; border-collapse: separate; border-spacing: 0 15px; min-width: 500px; }
.clayTable th { padding: 0 20px; text-align: left; opacity: 0.5; font-size: 0.75rem; font-weight: 800; }

.clayTable tr td { padding: 18px 25px; font-weight: 700; font-size: 0.9rem; }
.clayTable tr td:first-child { border-radius: var(--clay-radius) 0 0 var(--clay-radius); }
.clayTable tr td:last-child { border-radius: 0 var(--clay-radius) var(--clay-radius) 0; }

.userName { display: flex; align-items: center; gap: 15px; white-space: nowrap; }
.clayAvatar {
  width: 38px; height: 38px; border-radius: 50%;
  box-shadow: inset -4px -4px 8px rgba(0,0,0,0.15), inset 4px 4px 8px rgba(255,255,255,0.4);
  flex-shrink: 0;
}

.status { padding: 6px 15px; border-radius: 12px; font-size: 0.75rem; box-shadow: var(--clay-inset); }
.clayGreen { background: #b9fbc0; color: #2d6a4f; }
.clayYellow { background: #fde4cf; color: #997b66; }

/* --- FOOTER --- */
.footerBlock { border-radius: var(--clay-radius); padding: 20px 40px; margin-top: 10px; }
.pagination { display: flex; justify-content: space-between; align-items: center; flex-wrap: wrap; gap: 15px; }
.navBtn { padding: 12px 25px; border-radius: 15px; font-weight: 800; font-size: 0.8rem; transition: 0.2s; }
.navBtn:active { transform: scale(0.95); box-shadow: var(--clay-inset); }

/* --- RESPONSIVE --- */
@media (max-width: 900px) {
  .mainGrid { grid-template-columns: 1fr; }
  .headerBlock { flex-direction: column; align-items: stretch; text-align: center; }
  .titleBox { justify-content: center; }
  .searchBox { margin-left: 0; }
}

@media (max-width: 480px) {
  .clayContainer { padding: 10px; border-radius: 20px; }
  .clayTable td { padding: 15px 10px; font-size: 0.8rem; }
  .clayAvatar { width: 30px; height: 30px; }
}
</style>