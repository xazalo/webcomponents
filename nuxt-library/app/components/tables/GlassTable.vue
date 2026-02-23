<template>
  <div :class="$style.glassWrapper" :style="glassTheme">
    <div :class="$style.bgOrb1" :style="{ background: orb1Color }"></div>
    <div :class="$style.bgOrb2" :style="{ background: orb2Color }"></div>

    <div :class="$style.glassContainer">
      <header :class="$style.headerBlock">
        <div :class="$style.titleBox">
          <h1 :class="$style.mainTitle">{{ title }}</h1>
          <span :class="$style.subtitle">{{ subtitle }}</span>
        </div>
        <div :class="$style.searchBox">
          <input 
            type="text" 
            :placeholder="searchPlaceholder" 
            :class="$style.glassInput" 
            @input="$emit('search', $event.target.value)"
          />
          <span :class="$style.searchIcon">🔍</span>
        </div>
      </header>

      <div :class="$style.mainGrid">
        <aside :class="$style.filterBlock">
          <h3 :class="$style.subTitle">Categorías</h3>
          <div :class="$style.filterList">
            <label v-for="tag in tags" :key="tag" :class="$style.glassLabel">
              <input type="checkbox" :class="$style.glassCheck" @change="$emit('filter', tag)" />
              <span>{{ tag }}</span>
            </label>
          </div>
        </aside>

        <main :class="$style.tableBlock">
          <div :class="$style.scrollArea">
            <table :class="$style.glassTable">
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
                        <div :class="$style.userProfile">
                          <div :class="$style.glassAvatar" :style="{ background: item.avatarGradient || 'rgba(255,255,255,0.2)' }"></div>
                          <span>{{ item[col.key] }}</span>
                        </div>
                      </template>
                      <template v-else-if="col.key === 'level'">
                        <span :class="[$style.badge, item.isHigh ? $style.high : $style.med]">
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
          <button :class="$style.navLink" @click="$emit('prev')">PREV</button>
          <div :class="$style.pageIndicator">
            {{ currentPage.toString().padStart(2, '0') }} <span :style="{opacity: 0.4}">/</span> {{ totalPages }}
          </div>
          <button :class="$style.navLink" @click="$emit('next')">NEXT</button>
        </div>
      </footer>
    </div>
  </div>
</template>

<script setup lang="ts">
import { computed } from 'vue';

interface Props {
  title?: string;
  subtitle?: string;
  searchPlaceholder?: string;
  tags?: string[];
  columns?: { label: string; key: string }[];
  data?: any[];
  currentPage?: number | string;
  totalPages?: number | string;
  // Glass Customization
  blurAmount?: string;
  glassOpacity?: number;
  orb1Color?: string;
  orb2Color?: string;
  accentColor?: string;
}

const props = withDefaults(defineProps<Props>(), {
  title: 'Crystal_Records',
  subtitle: 'Data Visualization Layer',
  searchPlaceholder: 'Search...',
  tags: () => ['Internal', 'Public', 'Secure'],
  columns: () => [
    { label: 'REF_ID', key: 'id' },
    { label: 'IDENTITY', key: 'user' },
    { label: 'CLEARANCE', key: 'level' }
  ],
  data: () => [],
  currentPage: 1,
  totalPages: 15,
  blurAmount: '12px',
  glassOpacity: 0.1,
  orb1Color: 'linear-gradient(#4facfe, #00f2fe)',
  orb2Color: 'linear-gradient(#f093fb, #f5576c)',
  accentColor: '#4facfe'
});

const glassTheme = computed(() => ({
  '--g-blur': props.blurAmount,
  '--g-bg': `rgba(255, 255, 255, ${props.glassOpacity})`,
  '--g-accent': props.accentColor,
  '--g-border': `rgba(255, 255, 255, ${props.glassOpacity + 0.15})`
}));
</script>

<style module>
.glassWrapper {
  background: #0f172a; /* Fondo oscuro base */
  padding: clamp(10px, 4vw, 40px);
  position: relative; border-radius: 40px;
  overflow: hidden; width: 100%; min-height: 500px;
  box-sizing: border-box;
}

/* Orbes de fondo */
.bgOrb1, .bgOrb2 {
  position: absolute; border-radius: 50%; filter: blur(60px); z-index: 0; opacity: 0.3;
}
.bgOrb1 { top: -50px; right: -50px; width: clamp(150px, 30vw, 300px); height: clamp(150px, 30vw, 300px); }
.bgOrb2 { bottom: 20px; left: 10%; width: clamp(100px, 20vw, 250px); height: clamp(100px, 20vw, 250px); }

/* Mixin de Cristal */
.glassContainer, .headerBlock, .filterBlock, .tableBlock, .footerBlock, .glassInput {
  background: var(--g-bg);
  backdrop-filter: blur(var(--g-blur));
  -webkit-backdrop-filter: blur(var(--g-blur));
  border: 1px solid var(--g-border);
}

.glassContainer {
  position: relative; z-index: 1; padding: clamp(15px, 3vw, 30px);
  border-radius: 30px; color: #fff; font-family: 'Inter', system-ui, sans-serif;
  box-shadow: 0 8px 32px 0 rgba(0, 0, 0, 0.4);
}

/* Header */
.headerBlock {
  border-radius: 20px; padding: 20px; display: flex;
  justify-content: space-between; align-items: center; 
  flex-wrap: wrap; gap: 20px; margin-bottom: 25px;
}

.mainTitle { margin: 0; font-size: clamp(1.2rem, 4vw, 1.8rem); font-weight: 300; }
.subtitle { font-size: 0.65rem; opacity: 0.5; text-transform: uppercase; letter-spacing: 2px; }

.searchBox { position: relative; flex: 1 1 250px; max-width: 100%; }
.glassInput {
  width: 100%; border-radius: 12px; padding: 12px 15px;
  color: #fff; outline: none; box-sizing: border-box;
}

/* Layout Grid */
.mainGrid { display: grid; grid-template-columns: 220px 1fr; gap: 25px; }

.filterBlock { border-radius: 20px; padding: 20px; height: fit-content; }
.subTitle { font-size: 0.85rem; font-weight: 600; margin-bottom: 15px; opacity: 0.9; }
.filterList { display: flex; flex-direction: column; gap: 12px; }

.glassLabel { display: flex; align-items: center; gap: 10px; font-size: 0.85rem; cursor: pointer; }
.glassCheck {
  appearance: none; width: 18px; height: 18px;
  border: 1px solid var(--g-border); border-radius: 6px; cursor: pointer;
}
.glassCheck:checked { background: var(--g-accent); box-shadow: 0 0 15px var(--g-accent); }

/* Table Section */
.tableBlock { border-radius: 20px; overflow: hidden; min-width: 0; }
.scrollArea { overflow-x: auto; width: 100%; }

.glassTable { width: 100%; border-collapse: collapse; min-width: 500px; }
.glassTable th { background: rgba(255,255,255,0.05); text-align: left; padding: 15px; font-size: 0.7rem; text-transform: uppercase; opacity: 0.5; }
.glassTable td { padding: 15px; border-bottom: 1px solid var(--g-border); font-size: 0.85rem; }

.userProfile { display: flex; align-items: center; gap: 10px; }
.glassAvatar { width: 30px; height: 30px; border-radius: 50%; border: 1px solid var(--g-border); flex-shrink: 0; }

.badge { padding: 3px 10px; border-radius: 12px; font-size: 0.65rem; font-weight: 800; border: 1px solid transparent; }
.high { background: rgba(245, 87, 108, 0.2); color: #ff6b7d; border-color: rgba(245, 87, 108, 0.4); }
.med { background: rgba(79, 172, 254, 0.2); color: #70c7ff; border-color: rgba(79, 172, 254, 0.4); }

/* Footer */
.footerBlock { margin-top: 25px; border-radius: 20px; padding: 15px 25px; }
.pagination { display: flex; justify-content: space-between; align-items: center; }
.navLink { background: transparent; border: none; color: var(--g-accent); cursor: pointer; font-weight: 800; font-size: 0.75rem; letter-spacing: 1px; }
.pageIndicator { font-family: monospace; font-size: 0.9rem; }

/* BREAKPOINTS */
@media (max-width: 850px) {
  .mainGrid { grid-template-columns: 1fr; }
  .headerBlock { flex-direction: column; align-items: flex-start; }
}

@media (max-width: 480px) {
  .glassWrapper { border-radius: 0; padding: 10px; }
  .glassTable th:nth-child(1), .glassTable td:nth-child(1) { display: none; } /* Ocultar ID en móviles */
}
</style>