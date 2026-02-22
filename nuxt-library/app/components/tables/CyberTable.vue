<template>
  <div :class="$style.cyberContainer">
    <div :class="$style.scanlines"></div>

    <header :class="$style.headerBlock">
      <div :class="$style.glitchWrapper">
        <h1 :class="$style.mainTitle" data-text="NET_RUNNER_v4.2">NET_RUNNER_v4.2</h1>
        <div :class="$style.subHeader">STATUS: CONNECTION_ENCRYPTED</div>
      </div>
      <div :class="$style.searchWrapper">
        <div :class="$style.inputTag">QUERY</div>
        <input type="text" placeholder="BUSCAR EN LA RED..." :class="$style.cyberInput" />
        <div :class="$style.searchGlow"></div>
      </div>
    </header>

    <div :class="$style.mainGrid">
      <aside :class="$style.filterBlock">
        <div :class="$style.cornerDeco"></div>
        <h3 :class="$style.subTitle">_FILTROS</h3>
        <div :class="$style.filterList">
          <label v-for="tag in ['ADMIN', 'USER', 'DAEMON', 'ROOT']" :key="tag" :class="$style.cyberLabel">
            <input type="checkbox" :class="$style.cyberCheck" />
            <span :class="$style.checkText">{{ tag }}</span>
          </label>
        </div>
        <button :class="$style.rebootBtn">FORCE_REBOOT</button>
      </aside>

      <main :class="$style.tableBlock">
        <table :class="$style.cyberTable">
          <thead>
            <tr>
              <th>[ID]</th>
              <th>[IDENTIDAD]</th>
              <th>[NIVEL_ACCESO]</th>
              <th>[EJECUTAR]</th>
            </tr>
          </thead>
          <tbody>
            <tr v-for="i in 5" :key="i">
              <td><span :class="$style.idText">#0x{{ i }}FF</span></td>
              <td :class="$style.userName">
                <span :class="$style.userIcon">◮</span>
                CYBER_UNIT_{{ i }}
              </td>
              <td>
                <div :class="$style.levelContainer">
                  <div :class="[$style.levelBar, i > 3 ? $style.low : $style.high]"></div>
                  <span>{{ i > 3 ? 'LOW_PRIO' : 'CRITICAL' }}</span>
                </div>
              </td>
              <td>
                <button :class="$style.actionBtn">TERMINATE</button>
              </td>
            </tr>
          </tbody>
        </table>
      </main>
    </div>

    <footer :class="$style.footerBlock">
      <div :class="$style.pagination">
        <button :class="$style.pageBtn">PREV_SEC</button>
        <div :class="$style.terminalInfo">PAGE_01_OF_04 // [SYSTEM_STABLE]</div>
        <button :class="$style.pageBtn">NEXT_SEC</button>
      </div>
    </footer>
  </div>
</template>

<style module>
/* --- VARIABLES Y RESET --- */
:root {
  --neon-blue: #00f0ff;
  --neon-pink: #ff003c;
  --neon-yellow: #fcee0a;
  --cyber-bg: #050505;
  --cyber-surface: #0f0f0f;
  --cyber-border: rgba(0, 240, 255, 0.3);
  --glitch-speed: 0.2s;
}

.cyberContainer {
  background: var(--cyber-bg);
  padding: 30px;
  font-family: 'Electrolize', 'Orbitron', monospace;
  color: var(--neon-blue);
  position: relative;
  max-width: 1200px;
  margin: 0 auto;
  border: 1px solid var(--cyber-border);
  overflow: hidden;
  box-sizing: border-box;
}

/* --- EFECTOS DE AMBIENTE --- */
.scanlines {
  position: absolute;
  inset: 0;
  background: linear-gradient(
    to bottom,
    transparent 50%,
    rgba(0, 240, 255, 0.02) 50%
  );
  background-size: 100% 4px;
  pointer-events: none;
  z-index: 10;
}

/* --- HEADER HUD (CORREGIDO) --- */
.headerBlock {
  display: flex;
  justify-content: space-between;
  align-items: flex-end;
  flex-wrap: wrap; /* Fix para desbordamiento */
  gap: 25px;
  margin-bottom: 30px;
  border-bottom: 2px solid var(--neon-blue);
  padding-bottom: 15px;
}

.glitchWrapper {
  flex: 1 1 auto;
  min-width: 280px;
}

.mainTitle {
  margin: 0;
  font-size: 2.2rem;
  letter-spacing: 4px;
  color: var(--neon-blue);
  text-transform: uppercase;
  text-shadow: 2px 0 var(--neon-pink), -2px 0 rgba(255,255,255,0.5);
  position: relative;
}

.subHeader {
  font-size: 0.7rem;
  opacity: 0.7;
  letter-spacing: 2px;
  margin-top: 5px;
}

/* --- BUSCADOR FLEXIBLE --- */
.searchWrapper { 
  position: relative; 
  flex: 1 1 300px; /* Crece hasta 300px pero puede encogerse */
  max-width: 100%; 
}

.inputTag {
  position: absolute;
  top: -10px; 
  left: 10px;
  background: var(--neon-blue);
  color: black;
  font-size: 0.6rem;
  padding: 2px 8px;
  font-weight: bold;
  z-index: 2;
}

.cyberInput {
  width: 100%;
  background: rgba(0, 240, 255, 0.05);
  border: 1px solid var(--neon-blue);
  padding: 12px 15px;
  color: var(--neon-blue);
  font-family: monospace;
  outline: none;
  box-sizing: border-box; /* Fix: el padding no suma al ancho */
  transition: all 0.3s;
}

.cyberInput:focus {
  background: rgba(0, 240, 255, 0.1);
  box-shadow: 0 0 15px rgba(0, 240, 255, 0.2);
}

.searchGlow {
  position: absolute;
  bottom: 0; left: 0;
  width: 0%;
  height: 2px;
  background: var(--neon-pink);
  transition: width 0.4s ease;
}

.cyberInput:focus + .searchGlow { width: 100%; }

/* --- GRID PRINCIPAL --- */
.mainGrid {
  display: grid;
  grid-template-columns: 240px 1fr;
  gap: 30px;
}

/* --- SIDEBAR FILTROS --- */
.filterBlock {
  background: var(--cyber-surface);
  border-left: 4px solid var(--neon-pink);
  padding: 25px;
  position: relative;
  clip-path: polygon(0 0, 100% 0, 100% 90%, 90% 100%, 0 100%);
}

.subTitle { 
  font-size: 0.9rem; 
  margin-bottom: 25px; 
  color: var(--neon-pink); 
  text-transform: uppercase;
  letter-spacing: 1.5px;
}

.filterList { 
  display: flex; 
  flex-direction: column; 
  gap: 18px; 
}

.cyberLabel { 
  display: flex; 
  align-items: center; 
  gap: 12px; 
  cursor: pointer; 
  font-size: 0.8rem;
  transition: color 0.2s;
}

.cyberLabel:hover { color: white; }

.cyberCheck {
  appearance: none;
  width: 16px;
  height: 16px;
  border: 1px solid var(--neon-blue);
  cursor: pointer;
  position: relative;
  background: transparent;
}

.cyberCheck:checked { 
  background: var(--neon-blue); 
  box-shadow: 0 0 10px var(--neon-blue); 
}

.rebootBtn {
  margin-top: 35px;
  width: 100%;
  background: transparent;
  border: 1px solid var(--neon-pink);
  color: var(--neon-pink);
  padding: 12px;
  font-size: 0.7rem;
  font-weight: bold;
  cursor: pointer;
  text-transform: uppercase;
  transition: all 0.2s;
}

.rebootBtn:hover { 
  background: var(--neon-pink); 
  color: black; 
  box-shadow: 0 0 15px var(--neon-pink); 
}

/* --- TABLA CYBER --- */
.tableBlock {
  background: rgba(0, 0, 0, 0.3);
  overflow-x: auto; /* Scroll horizontal si la tabla es muy ancha */
}

.cyberTable { 
  width: 100%; 
  border-collapse: collapse; 
  min-width: 500px;
}

.cyberTable th {
  text-align: left;
  padding: 18px 15px;
  font-size: 0.7rem;
  color: var(--neon-yellow);
  border-bottom: 1px solid var(--cyber-border);
  text-transform: uppercase;
  letter-spacing: 1px;
}

.cyberTable tr {
  border-bottom: 1px solid rgba(0, 240, 255, 0.05);
}

.cyberTable tr:hover {
  background: rgba(0, 240, 255, 0.04);
}

.cyberTable td { 
  padding: 18px 15px; 
  font-size: 0.85rem; 
}

.idText { color: var(--neon-pink); font-family: monospace; font-weight: bold; }

.userName { color: white; display: flex; align-items: center; gap: 10px; }

.userIcon { color: var(--neon-blue); animation: blink 1.5s infinite; }

.levelContainer { display: flex; align-items: center; gap: 12px; font-size: 0.7rem; }

.levelBar { 
  width: 35px; 
  height: 4px; 
  background: var(--neon-blue); 
  box-shadow: 0 0 8px var(--neon-blue); 
}

.low { background: #333; box-shadow: none; }

.actionBtn {
  background: transparent;
  border: 1px solid var(--neon-blue);
  color: var(--neon-blue);
  padding: 6px 12px;
  font-size: 0.65rem;
  cursor: pointer;
  text-transform: uppercase;
  clip-path: polygon(10% 0, 100% 0, 90% 100%, 0 100%);
  transition: all 0.2s;
}

.actionBtn:hover {
  background: var(--neon-blue);
  color: black;
  box-shadow: 0 0 10px var(--neon-blue);
}

/* --- FOOTER TERMINAL --- */
.footerBlock {
  margin-top: 30px;
  border: 1px solid var(--cyber-border);
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
  background: transparent;
  border: none;
  color: var(--neon-blue);
  font-family: monospace;
  font-weight: bold;
  cursor: pointer;
  text-decoration: underline;
  font-size: 0.8rem;
  letter-spacing: 1px;
}

.pageBtn:hover { color: white; }

.terminalInfo { 
  font-size: 0.7rem; 
  font-family: monospace; 
  color: var(--neon-yellow); 
}

/* --- ANIMACIONES Y RESPONSIVE --- */
@keyframes blink { 
  0%, 100% { opacity: 1; } 
  50% { opacity: 0.2; } 
}

@media (max-width: 900px) {
  .mainGrid { grid-template-columns: 1fr; }
  
  .headerBlock {
    flex-direction: column;
    align-items: flex-start;
  }
  
  .searchWrapper { width: 100%; }
  
  .mainTitle { font-size: 1.8rem; }
}

@media (max-width: 600px) {
  .cyberContainer { padding: 15px; }
  
  .cyberTable th:nth-child(1), 
  .cyberTable td:nth-child(1) { display: none; } /* Ocultar ID en móvil */
  
  .actionBtn { padding: 8px; }
}
</style>