<template>
  <div :class="$style.neumoContainer">
    <header :class="$style.headerBlock">
      <div :class="$style.titleBox">
        <h1 :class="$style.mainTitle">Soft_Metrics</h1>
        <span :class="$style.subHeader">v2.0 // UI_LAYER_04</span>
      </div>
      <div :class="$style.searchWrapper">
        <input type="text" placeholder="Buscar datos..." :class="$style.neumoInput" />
        <span :class="$style.searchIcon">🔍</span>
      </div>
    </header>

    <div :class="$style.mainGrid">
      <aside :class="$style.sidebar">
        <h3 :class="$style.sectionTitle">Categorías</h3>
        <div :class="$style.filterList">
          <label v-for="tag in ['Interno', 'Global', 'Seguro', 'Archivo']" :key="tag" :class="$style.neumoLabel">
            <input type="checkbox" :class="$style.neumoCheck" />
            <span :class="$style.labelText">{{ tag }}</span>
          </label>
        </div>
      </aside>

      <main :class="$style.tableBlock">
        <table :class="$style.neumoTable">
          <thead>
            <tr>
              <th>REF</th>
              <th>USUARIO</th>
              <th>PROGRESO</th>
              <th>ACCIÓN</th>
            </tr>
          </thead>
          <tbody>
            <tr v-for="i in 5" :key="i">
              <td><div :class="$style.idInset">#0{{ i }}</div></td>
              <td>
                <div :class="$style.userBox">
                  <div :class="$style.miniAvatar"></div>
                  <span>Soft_Entity_{{ i }}</span>
                </div>
              </td>
              <td>
                <div :class="$style.progressTrack">
                  <div :class="$style.progressFill" :style="{ width: (100 - i * 15) + '%' }"></div>
                </div>
              </td>
              <td>
                <button :class="$style.neumoBtn">SYNC</button>
              </td>
            </tr>
          </tbody>
        </table>
      </main>
    </div>

    <footer :class="$style.footerBlock">
      <div :class="$style.pagination">
        <button :class="$style.pageBtn">ANTERIOR</button>
        <span :class="$style.pageInfo">Página 1 de 12</span>
        <button :class="$style.pageBtn">SIGUIENTE</button>
      </div>
    </footer>
  </div>
</template>

<style module>
:root {
  --neumo-bg: #e0e5ec;
  --neumo-shadow-dark: #a3b1c6;
  --neumo-shadow-light: #ffffff;
  --neumo-text: #44475a;
  --neumo-accent: #6d5dfc;
}

.neumoContainer {
  background: var(--neumo-bg);
  padding: clamp(15px, 4vw, 40px);
  font-family: 'Segoe UI', Roboto, Helvetica, Arial, sans-serif;
  color: var(--neumo-text);
  max-width: 1000px;
  margin: 0 auto;
  border-radius: 40px;
  box-sizing: border-box;
}

/* --- MIXINS DE SOMBRAS NEUMÓRFICAS --- */
.headerBlock, .sidebar, .neumoBtn, .pageBtn, .miniAvatar, .neumoCheck {
  background: var(--neumo-bg);
  box-shadow: 9px 9px 16px var(--neumo-shadow-dark), 
             -9px -9px 16px var(--neumo-shadow-light);
}

.neumoInput, .progressTrack, .idInset {
  background: var(--neumo-bg);
  box-shadow: inset 6px 6px 12px var(--neumo-shadow-dark), 
              inset -6px -6px 12px var(--neumo-shadow-light);
}

/* --- HEADER --- */
.headerBlock {
  display: flex;
  justify-content: space-between;
  align-items: center;
  flex-wrap: wrap;
  gap: 20px;
  padding: 25px 35px;
  border-radius: 25px;
  margin-bottom: 35px;
  border: 1px solid rgba(255, 255, 255, 0.2);
}

.mainTitle { margin: 0; font-size: 1.8rem; font-weight: 700; color: var(--neumo-text); }
.subHeader { font-size: 0.7rem; opacity: 0.6; letter-spacing: 1px; font-weight: bold; }

.searchWrapper {
  position: relative;
  flex: 1 1 250px;
  max-width: 100%;
}

.neumoInput {
  width: 100%;
  border: none;
  padding: 12px 20px;
  border-radius: 15px;
  outline: none;
  color: var(--neumo-text);
  box-sizing: border-box;
  font-weight: 600;
}

.searchIcon {
  position: absolute;
  right: 15px;
  top: 50%;
  transform: translateY(-50%);
  opacity: 0.5;
}

/* --- GRID LAYOUT --- */
.mainGrid {
  display: grid;
  grid-template-columns: 240px 1fr;
  gap: 30px;
}

.sidebar {
  padding: 25px;
  border-radius: 25px;
  height: fit-content;
}

.sectionTitle { 
  font-size: 0.85rem; 
  margin-bottom: 20px; 
  text-transform: uppercase; 
  opacity: 0.7; 
  font-weight: 800;
  letter-spacing: 1px;
}

.filterList { display: flex; flex-direction: column; gap: 15px; }

.neumoLabel {
  display: flex;
  align-items: center;
  gap: 12px;
  cursor: pointer;
  font-weight: 600;
  font-size: 0.9rem;
  transition: color 0.2s;
}

.neumoLabel:hover { color: var(--neumo-accent); }

.neumoCheck {
  appearance: none;
  width: 22px;
  height: 22px;
  border-radius: 8px;
  cursor: pointer;
  position: relative;
  border: none;
}

.neumoCheck:checked {
  box-shadow: inset 3px 3px 6px var(--neumo-shadow-dark), 
              inset -3px -3px 6px var(--neumo-shadow-light);
}

.neumoCheck:checked::after {
  content: '●';
  position: absolute;
  top: 50%; left: 50%;
  transform: translate(-50%, -50%);
  color: var(--neumo-accent);
  font-size: 0.7rem;
}

/* --- TABLA --- */
.tableBlock {
  width: 100%;
  overflow-x: auto;
}

.neumoTable {
  width: 100%;
  border-collapse: separate;
  border-spacing: 0 15px;
  min-width: 550px;
}

.neumoTable th {
  text-align: left;
  padding: 0 15px;
  font-size: 0.75rem;
  opacity: 0.5;
  text-transform: uppercase;
  font-weight: 800;
}

.neumoTable tr td {
  padding: 20px 15px;
  background: var(--neumo-bg);
}

/* Redondear esquinas de las filas para el efecto de tarjetas individuales */
.neumoTable tr td:first-child { border-radius: 20px 0 0 20px; }
.neumoTable tr td:last-child { border-radius: 0 20px 20px 0; }

.idInset {
  padding: 6px 12px;
  border-radius: 10px;
  font-size: 0.75rem;
  font-weight: 800;
  color: var(--neumo-accent);
}

.userBox { display: flex; align-items: center; gap: 12px; font-weight: 600; }

.miniAvatar {
  width: 32px;
  height: 32px;
  border-radius: 50%;
  flex-shrink: 0;
}

.progressTrack {
  width: 100%;
  max-width: 150px;
  height: 12px;
  border-radius: 10px;
  position: relative;
}

.progressFill {
  height: 100%;
  background: var(--neumo-accent);
  border-radius: 10px;
  box-shadow: 2px 0 8px rgba(109, 93, 252, 0.3);
}

.neumoBtn {
  border: none;
  padding: 10px 20px;
  border-radius: 12px;
  font-weight: 800;
  color: var(--neumo-accent);
  cursor: pointer;
  font-size: 0.7rem;
  transition: all 0.2s;
}

.neumoBtn:active {
  box-shadow: inset 4px 4px 8px var(--neumo-shadow-dark), 
              inset -4px -4px 8px var(--neumo-shadow-light);
  transform: scale(0.96);
}

/* --- FOOTER --- */
.footerBlock {
  margin-top: 20px;
}

.pagination {
  display: flex;
  justify-content: space-between;
  align-items: center;
  flex-wrap: wrap;
  gap: 15px;
}

.pageBtn {
  border: none;
  padding: 10px 25px;
  border-radius: 15px;
  font-weight: 800;
  font-size: 0.75rem;
  cursor: pointer;
  transition: 0.2s;
}

.pageBtn:hover { color: var(--neumo-accent); }

.pageInfo { font-size: 0.85rem; font-weight: 700; opacity: 0.6; }

/* --- RESPONSIVE --- */
@media (max-width: 850px) {
  .mainGrid { grid-template-columns: 1fr; }
  .headerBlock { flex-direction: column; align-items: flex-start; }
}

@media (max-width: 500px) {
  .neumoContainer { border-radius: 0; padding: 20px 15px; }
  .neumoTable tr td:nth-child(1) { display: none; }
  .neumoTable th:nth-child(1) { display: none; }
}
</style>