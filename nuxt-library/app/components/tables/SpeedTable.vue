<template>
  <div :class="$style.speedContainer">
    <header :class="$style.headerBlock">
      <div :class="$style.brandBox">
        <h1 :class="$style.mainTitle">TURBO_DATA</h1>
        <div :class="$style.engineStatus">ENGINE_RPM: 8.500</div>
      </div>
      <div :class="$style.searchBox">
        <input type="text" placeholder="BUSCAR NITRO..." :class="$style.speedInput" />
        <div :class="$style.inputDeco"></div>
      </div>
    </header>

    <div :class="$style.mainGrid">
      <aside :class="$style.sidebar">
        <h3 :class="$style.subTitle">_DASHBOARD</h3>
        <div :class="$style.filterList">
          <label v-for="tag in ['V12', 'V10', 'V8', 'TURBO']" :key="tag" :class="$style.speedLabel">
            <input type="checkbox" :class="$style.speedCheck" />
            <span :class="$style.checkText">{{ tag }}</span>
          </label>
        </div>
        <div :class="$style.fuelGauge">
          <div :class="$style.fuelLevel"></div>
        </div>
      </aside>

      <main :class="$style.tableBlock">
        <table :class="$style.speedTable">
          <thead>
            <tr>
              <th>RANK</th>
              <th>PILOT_ID</th>
              <th>VELOCITY</th>
              <th>STATUS</th>
            </tr>
          </thead>
          <tbody>
            <tr v-for="i in 5" :key="i">
              <td><span :class="$style.rankBox">0{{ i }}</span></td>
              <td :class="$style.userName">
                <span :class="$style.stripes"></span>
                RACER_CORE_{{ i }}
              </td>
              <td>
                <div :class="$style.velocityBar">
                  <div :class="$style.fill" :style="{ width: (100 - i * 15) + '%' }"></div>
                </div>
              </td>
              <td>
                <button :class="$style.raceBtn">GO_FAST</button>
              </td>
            </tr>
          </tbody>
        </table>
      </main>
    </div>

    <footer :class="$style.footerBlock">
      <div :class="$style.pagination">
        <button :class="$style.navBtn">LAP_PREV</button>
        <div :class="$style.lapCounter">LAP 01 // 99</div>
        <button :class="$style.navBtn">LAP_NEXT</button>
      </div>
    </footer>
  </div>
</template>

<style module>
:root {
  --speed-black: #0a0a0b;
  --speed-red: #ff0000;
  --speed-white: #f0f0f0;
  --speed-grey: #2a2a2e;
  --speed-accent: #fcee0a;
}

/* --- CONTENEDOR PRINCIPAL --- */
.speedContainer {
  background: var(--speed-black);
  padding: clamp(15px, 3vw, 30px);
  font-family: 'Krona One', 'Arial Black', sans-serif;
  color: var(--speed-white);
  max-width: 1000px;
  margin: 0 auto;
  border-right: 8px solid var(--speed-red);
  box-sizing: border-box;
  overflow: hidden; /* Evita que los skews generen scroll lateral */
}

/* --- HEADER DINÁMICO (CORREGIDO) --- */
.headerBlock {
  display: flex;
  justify-content: space-between;
  align-items: center;
  flex-wrap: wrap; /* Salto de línea en móviles */
  gap: 20px;
  background: var(--speed-grey);
  padding: 20px clamp(20px, 5vw, 40px);
  transform: skewX(-10deg);
  margin: 0 10px 30px 10px; /* Margen para compensar el skew */
  border-left: 10px solid var(--speed-red);
}

.brandBox { 
  transform: skewX(10deg); /* Re-ajuste para texto derecho */
  flex: 1 1 auto;
}

.mainTitle { 
  margin: 0; 
  font-size: clamp(1.2rem, 4vw, 1.8rem); 
  font-style: italic;
  background: linear-gradient(to bottom, #fff 50%, #888 50%);
  -webkit-text-fill-color: transparent;
  white-space: nowrap;
}

.engineStatus { font-size: 0.6rem; color: var(--speed-accent); letter-spacing: 2px; }

/* --- BUSCADOR NITRO --- */
.searchBox {
  position: relative;
  flex: 1 1 250px;
  max-width: 100%;
  transform: skewX(10deg);
}

.speedInput {
  width: 100%;
  background: #000;
  border: 1px solid #444;
  padding: 10px 15px;
  color: var(--speed-white);
  outline: none;
  font-family: monospace;
  box-sizing: border-box;
  transition: border-color 0.3s;
}

.speedInput:focus {
  border-color: var(--speed-accent);
}

/* --- GRID Y SIDEBAR --- */
.mainGrid {
  display: grid;
  grid-template-columns: 200px 1fr;
  gap: 25px;
}

.sidebar {
  background: var(--speed-grey);
  padding: 20px;
  border-top: 4px solid var(--speed-red);
  height: fit-content;
}

.subTitle { 
  font-size: 0.8rem; 
  letter-spacing: 2px; 
  border-bottom: 1px solid #444; 
  padding-bottom: 10px;
  text-transform: uppercase;
}

.filterList { display: flex; flex-direction: column; gap: 15px; }

.speedLabel { 
  display: flex; 
  align-items: center; 
  gap: 10px; 
  font-size: 0.7rem; 
  cursor: pointer;
  transition: color 0.2s;
}

.speedLabel:hover { color: var(--speed-accent); }

.speedCheck {
  appearance: none;
  width: 14px; height: 14px;
  border: 2px solid var(--speed-red);
  transform: skewX(-15deg);
  cursor: pointer;
}

.speedCheck:checked { 
  background: var(--speed-red); 
  box-shadow: 0 0 8px var(--speed-red);
}

/* --- TABLA AERODINÁMICA --- */
.tableBlock { 
  overflow-x: auto; /* Scroll si la tabla es más ancha que el móvil */
  width: 100%;
}

.speedTable { 
  width: 100%; 
  border-collapse: separate; 
  border-spacing: 0 10px;
  min-width: 450px; /* Asegura legibilidad */
}

.speedTable th { 
  text-align: left; 
  font-size: 0.6rem; 
  color: #888; 
  padding: 0 15px;
  text-transform: uppercase;
}

.speedTable tr td {
  background: var(--speed-grey);
  padding: 15px;
  font-weight: 900;
  font-style: italic;
  font-size: 0.85rem;
}

.speedTable tr td:first-child { 
  border-left: 6px solid var(--speed-red); 
}

.rankBox { 
  background: var(--speed-red); 
  padding: 4px 8px; 
  font-size: 0.7rem;
  color: #fff;
}

.userName { 
  position: relative; 
  padding-left: 30px !important; 
  white-space: nowrap;
}

.stripes {
  position: absolute; left: 0; top: 0; bottom: 0;
  width: 12px;
  background: repeating-linear-gradient(45deg, #444, #444 2px, transparent 2px, transparent 6px);
}

.velocityBar { 
  width: 100%; 
  max-width: 120px; 
  height: 6px; 
  background: #000; 
  overflow: hidden; 
}

.fill {
  height: 100%;
  background: linear-gradient(to right, var(--speed-accent), var(--speed-red));
  transition: width 1s cubic-bezier(0.23, 1, 0.32, 1);
}

.raceBtn {
  background: var(--speed-white);
  border: none;
  padding: 8px 18px;
  font-weight: 900;
  font-style: italic;
  font-size: 0.7rem;
  cursor: pointer;
  clip-path: polygon(15% 0, 100% 0, 85% 100%, 0 100%);
  transition: all 0.2s;
}

.raceBtn:hover {
  background: var(--speed-accent);
  transform: scale(1.05);
}

/* --- FOOTER --- */
.footerBlock {
  margin-top: 30px;
  border-top: 2px dashed #444;
  padding: 20px 0;
}

.pagination { 
  display: flex; 
  justify-content: space-between; 
  align-items: center; 
  flex-wrap: wrap;
  gap: 15px;
}

.navBtn {
  background: transparent;
  border: 1px solid var(--speed-white);
  color: var(--speed-white);
  padding: 8px 20px;
  font-size: 0.65rem;
  font-weight: 800;
  cursor: pointer;
  transition: 0.3s;
}

.navBtn:hover { 
  background: var(--speed-red); 
  border-color: var(--speed-red); 
  box-shadow: 0 0 10px var(--speed-red);
}

.lapCounter { 
  font-size: 0.8rem; 
  color: var(--speed-accent); 
  font-family: monospace;
}

/* --- RESPONSIVE --- */
@media (max-width: 850px) {
  .mainGrid { grid-template-columns: 1fr; }
  .headerBlock { 
    transform: none; 
    border-left: none; 
    border-top: 6px solid var(--speed-red);
    margin: 0 0 20px 0;
  }
  .brandBox, .searchBox { transform: none; }
  .mainTitle { white-space: normal; text-align: center; }
}

@media (max-width: 500px) {
  .speedContainer { padding: 15px; border-right: 4px solid var(--speed-red); }
  .speedTable tr td { padding: 10px 5px; font-size: 0.75rem; }
  .velocityBar { display: none; } /* Ocultar barra en móviles muy pequeños */
}
</style>