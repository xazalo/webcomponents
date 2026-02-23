<template>
  <div :class="$style.neumoContainer" :style="neumoTheme">
    <header :class="$style.headerBlock">
      <div :class="$style.titleBox">
        <h1 :class="$style.mainTitle">{{ title }}</h1>
        <span :class="$style.subHeader">{{ versionTag }}</span>
      </div>
      <div :class="$style.searchWrapper">
        <input 
          type="text" 
          :placeholder="searchPlaceholder" 
          :class="$style.neumoInput" 
          @input="$emit('search', $event.target.value)"
        />
        <span :class="$style.searchIcon">🔍</span>
      </div>
    </header>

    <div :class="$style.mainGrid">
      <aside :class="$style.sidebar">
        <h3 :class="$style.sectionTitle">Categorías</h3>
        <div :class="$style.filterList">
          <label v-for="tag in tags" :key="tag" :class="$style.neumoLabel">
            <input type="checkbox" :class="$style.neumoCheck" @change="$emit('filter', tag)" />
            <span :class="$style.labelText">{{ tag }}</span>
          </label>
        </div>
      </aside>

      <main :class="$style.tableBlock">
        <div :class="$style.scrollArea">
          <table :class="$style.neumoTable">
            <thead>
              <tr>
                <th v-for="col in columns" :key="col.key">{{ col.label }}</th>
              </tr>
            </thead>
            <tbody>
              <tr v-for="(item, i) in data" :key="i">
                <td v-for="col in columns" :key="col.key">
                  <slot :name="`col-${col.key}`" :item="item">
                    <template v-if="col.key === 'id'">
                      <div :class="$style.idInset">#{{ item[col.key] || i + 1 }}</div>
                    </template>
                    <template v-else-if="col.key === 'user'">
                      <div :class="$style.userBox">
                        <div :class="$style.miniAvatar" :style="{ background: item.color || '#ccc' }"></div>
                        <span>{{ item[col.key] }}</span>
                      </div>
                    </template>
                    <template v-else-if="col.key === 'progress'">
                      <div :class="$style.progressTrack">
                        <div :class="$style.progressFill" :style="{ width: (item[col.key] || 0) + '%' }"></div>
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
        <button :class="$style.pageBtn" @click="$emit('prev')">ANTERIOR</button>
        <span :class="$style.pageInfo">Página {{ currentPage }} de {{ totalPages }}</span>
        <button :class="$style.pageBtn" @click="$emit('next')">SIGUIENTE</button>
      </div>
    </footer>
  </div>
</template>

<script setup lang="ts">
import { computed } from 'vue';

interface Props {
  title?: string;
  versionTag?: string;
  searchPlaceholder?: string;
  tags?: string[];
  columns?: { label: string; key: string }[];
  data?: any[];
  currentPage?: number | string;
  totalPages?: number | string;
  // Neumo Tuning
  baseColor?: string;
  accentColor?: string;
  shadowDistance?: number;
  intensity?: number;
}

const props = withDefaults(defineProps<Props>(), {
  title: 'Soft_Metrics',
  versionTag: 'v2.0 // UI_LAYER_04',
  searchPlaceholder: 'Buscar...',
  tags: () => ['Interno', 'Global', 'Seguro'],
  columns: () => [
    { label: 'REF', key: 'id' },
    { label: 'USUARIO', key: 'user' },
    { label: 'PROGRESO', key: 'progress' }
  ],
  data: () => [],
  currentPage: 1,
  totalPages: 12,
  baseColor: '#e0e5ec',
  accentColor: '#6d5dfc',
  shadowDistance: 8,
  intensity: 0.15
});

const neumoTheme = computed(() => {
  const d = props.shadowDistance;
  const i = props.intensity;
  // Colores calculados para sombras basados en la intensidad
  return {
    '--n-bg': props.baseColor,
    '--n-accent': props.accentColor,
    '--n-shadow-out': `${d}px ${d}px ${d * 2}px rgba(163, 177, 198, ${i * 4}), -${d}px -${d}px ${d * 2}px rgba(255, 255, 255, 1)`,
    '--n-shadow-in': `inset ${d/2}px ${d/2}px ${d}px rgba(163, 177, 198, ${i * 4}), inset -${d/2}px -${d/2}px ${d}px rgba(255, 255, 255, 1)`,
    '--n-radius': '20px'
  };
});
</script>

<style module>
.neumoContainer {
  background: var(--n-bg);
  padding: clamp(15px, 4vw, 40px);
  font-family: 'Inter', system-ui, sans-serif;
  color: #44475a;
  max-width: 1000px;
  margin: 0 auto;
  border-radius: 40px;
  box-sizing: border-box;
}

/* Efectos base */
.headerBlock, .sidebar, .pageBtn, .miniAvatar, .neumoCheck {
  background: var(--n-bg);
  box-shadow: var(--n-shadow-out);
}

.neumoInput, .progressTrack, .idInset, .neumoCheck:checked {
  background: var(--n-bg);
  box-shadow: var(--n-shadow-in);
}

/* Header Adaptable */
.headerBlock {
  display: flex;
  justify-content: space-between;
  align-items: center;
  flex-wrap: wrap;
  gap: 20px;
  padding: 25px clamp(20px, 4vw, 35px);
  border-radius: var(--n-radius);
  margin-bottom: 35px;
}

.mainTitle { margin: 0; font-size: clamp(1.4rem, 4vw, 1.8rem); font-weight: 800; }
.subHeader { font-size: 0.7rem; opacity: 0.5; font-weight: bold; letter-spacing: 1px; }

.searchWrapper {
  position: relative;
  flex: 1 1 250px;
  max-width: 100%;
}

.neumoInput {
  width: 100%; border: none; padding: 12px 20px; 
  border-radius: 15px; outline: none; font-weight: 600;
  box-sizing: border-box;
}

/* Grid dinámico */
.mainGrid {
  display: grid;
  grid-template-columns: 240px 1fr;
  gap: 30px;
}

.sidebar { padding: 25px; border-radius: var(--n-radius); height: fit-content; }
.sectionTitle { font-size: 0.8rem; margin-bottom: 20px; opacity: 0.6; font-weight: 800; text-transform: uppercase; }

.filterList { display: flex; flex-direction: column; gap: 15px; }
.neumoLabel { display: flex; align-items: center; gap: 12px; cursor: pointer; font-weight: 700; font-size: 0.9rem; }

.neumoCheck {
  appearance: none; width: 22px; height: 22px; border-radius: 8px; 
  cursor: pointer; position: relative; transition: all 0.2s; border: none;
}
.neumoCheck:checked::after {
  content: '●'; position: absolute; top: 50%; left: 50%;
  transform: translate(-50%, -50%); color: var(--n-accent); font-size: 0.6rem;
}

/* Tabla con Scroll interno */
.tableBlock { width: 100%; min-width: 0; }
.scrollArea { overflow-x: auto; width: 100%; border-radius: var(--n-radius); }

.neumoTable { width: 100%; border-collapse: separate; border-spacing: 0 15px; min-width: 500px; }
.neumoTable th { text-align: left; padding: 0 15px; font-size: 0.7rem; opacity: 0.5; font-weight: 800; }

.neumoTable tr td { padding: 20px 15px; background: var(--n-bg); }
.neumoTable tr td:first-child { border-radius: var(--n-radius) 0 0 var(--n-radius); }
.neumoTable tr td:last-child { border-radius: 0 var(--n-radius) var(--n-radius) 0; }

.idInset { padding: 6px 15px; border-radius: 10px; font-size: 0.75rem; font-weight: 800; color: var(--n-accent); display: inline-block; }

.userBox { display: flex; align-items: center; gap: 12px; font-weight: 700; white-space: nowrap; }
.miniAvatar { width: 34px; height: 34px; border-radius: 50%; flex-shrink: 0; }

.progressTrack { width: 100%; max-width: 140px; height: 10px; border-radius: 10px; overflow: hidden; }
.progressFill { height: 100%; background: var(--n-accent); border-radius: 10px; }

/* Footer */
.footerBlock { margin-top: 25px; }
.pagination { display: flex; justify-content: space-between; align-items: center; flex-wrap: wrap; gap: 15px; }

.pageBtn {
  border: none; padding: 12px 25px; border-radius: 15px; 
  font-weight: 800; font-size: 0.75rem; cursor: pointer; transition: 0.2s;
}
.pageBtn:active { box-shadow: var(--n-shadow-in); transform: scale(0.96); }

/* RESPONSIVE */
@media (max-width: 850px) {
  .mainGrid { grid-template-columns: 1fr; }
  .headerBlock { flex-direction: column; align-items: flex-start; }
  .searchWrapper { width: 100%; }
}

@media (max-width: 480px) {
  .neumoContainer { border-radius: 0; padding: 15px; }
  .neumoTable td:nth-child(1), 
  .neumoTable th:nth-child(1) { display: none; } /* Ocultar ID en móviles pequeños */
}
</style>