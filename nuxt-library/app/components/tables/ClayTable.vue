<template>
  <div :class="$style.clayContainer">
    <header :class="$style.headerBlock">
      <div :class="$style.titleBox">
        <h1 :class="$style.mainTitle">Clay_Database</h1>
        <div :class="$style.statusBadge">Sincronizado</div>
      </div>
      <div :class="$style.searchBox">
        <input type="text" placeholder="Buscar registros..." :class="$style.clayInput" />
      </div>
    </header>

    <div :class="$style.mainGrid">
      <aside :class="$style.filterBlock">
        <h3 :class="$style.subTitle">Filtros</h3>
        <div :class="$style.filterList">
          <label v-for="tag in ['Admin', 'User', 'Bot', 'Guest']" :key="tag" :class="$style.checkLabel">
            <input type="checkbox" :class="$style.clayCheck" />
            <span :class="$style.checkText">{{ tag }}</span>
          </label>
        </div>
        <button :class="$style.resetBtn">Reiniciar</button>
      </aside>

      <main :class="$style.tableBlock">
        <table :class="$style.clayTable">
          <thead>
            <tr>
              <th>ID</th>
              <th>Usuario</th>
              <th>Estado</th>
              <th>Acción</th>
            </tr>
          </thead>
          <tbody>
            <tr v-for="i in 5" :key="i">
              <td><span :class="$style.idBox">{{ i }}</span></td>
              <td :class="$style.userName">
                <div :class="$style.clayAvatar"></div>
                Model_User_{{ i }}
              </td>
              <td>
                <span :class="[$style.status, i % 2 === 0 ? $style.clayGreen : $style.clayYellow]">
                  {{ i % 2 === 0 ? 'Activo' : 'Inactivo' }}
                </span>
              </td>
              <td>
                <button :class="$style.actionBtn">EDITAR</button>
              </td>
            </tr>
          </tbody>
        </table>
      </main>
    </div>

    <footer :class="$style.footerBlock">
      <div :class="$style.pagination">
        <button :class="$style.navBtn">Ant.</button>
        <span :class="$style.pageCounter">01 / 10</span>
        <button :class="$style.navBtn">Sig.</button>
      </div>
    </footer>
  </div>
</template>

<style module>
:root {
  --clay-bg: #e2eafc;
  --clay-card: #ffffff;
  --clay-text: #4a5568;
  --clay-accent: #8e94f2;
  --c-blue: #a2d2ff;
  --c-pink: #ffafcc;
  --c-green: #b9fbc0;
  --c-yellow: #fde4cf;
}

/* --- CONTENEDOR --- */
.clayContainer {
  background: var(--clay-bg);
  padding: clamp(15px, 4vw, 35px);
  font-family: 'Quicksand', sans-serif;
  color: var(--clay-text);
  display: flex;
  flex-direction: column;
  gap: 25px;
  max-width: 1000px;
  margin: 0 auto;
  border-radius: 40px;
  box-sizing: border-box;
}

/* Efecto Claymorphism Maestro */
.headerBlock, .filterBlock, .navBtn, .actionBtn, .idBox, .footerBlock {
  background: var(--clay-card);
  box-shadow: 
    10px 10px 20px rgba(0, 0, 0, 0.05),
    inset -8px -8px 15px rgba(0, 0, 0, 0.05),
    inset 8px 8px 15px rgba(255, 255, 255, 0.8);
  box-sizing: border-box;
}

/* --- HEADER FLEXIBLE --- */
.headerBlock {
  border-radius: 30px;
  padding: 20px clamp(20px, 5vw, 45px);
  display: flex;
  justify-content: space-between;
  align-items: center;
  flex-wrap: wrap; /* Fix para el buscador */
  gap: 20px;
}

.titleBox { flex: 1 1 auto; }
.mainTitle { margin: 0; font-size: clamp(1.4rem, 5vw, 2rem); font-weight: 800; color: #5a67d8; }

.statusBadge {
  display: inline-block;
  margin-top: 5px;
  background: var(--c-green);
  padding: 4px 12px;
  border-radius: 15px;
  font-size: 0.7rem;
  font-weight: 700;
  box-shadow: inset -3px -3px 6px rgba(0,0,0,0.05), inset 3px 3px 6px rgba(255,255,255,0.5);
}

.searchBox { flex: 1 1 250px; width: 100%; }

.clayInput {
  width: 100%;
  border: none;
  background: var(--clay-bg);
  padding: 12px 25px;
  border-radius: 20px;
  box-shadow: inset 4px 4px 8px rgba(0,0,0,0.05), inset -4px -4px 8px rgba(255,255,255,0.8);
  font-weight: 600;
  box-sizing: border-box;
  outline: none;
}

/* --- GRID LAYOUT --- */
.mainGrid {
  display: grid;
  grid-template-columns: 240px 1fr;
  gap: 25px;
}

.filterBlock {
  border-radius: 30px;
  padding: 25px;
  height: fit-content;
}

.subTitle { font-weight: 800; margin-bottom: 20px; opacity: 0.7; font-size: 0.9rem; }

.filterList { display: flex; flex-direction: column; gap: 15px; }

.checkLabel { display: flex; align-items: center; gap: 12px; cursor: pointer; font-weight: 700; font-size: 0.85rem; }

.clayCheck {
  width: 22px;
  height: 22px;
  appearance: none;
  background: var(--clay-bg);
  border-radius: 8px;
  box-shadow: inset 3px 3px 5px rgba(0,0,0,0.05), inset -3px -3px 5px rgba(255,255,255,0.8);
  position: relative;
  cursor: pointer;
  flex-shrink: 0;
}

.clayCheck:checked { background: var(--clay-accent); }

.resetBtn {
  margin-top: 20px;
  width: 100%;
  border: none;
  background: var(--c-pink);
  padding: 10px;
  border-radius: 15px;
  font-weight: 800;
  color: white;
  cursor: pointer;
  box-shadow: inset -4px -4px 8px rgba(0,0,0,0.1), 4px 4px 10px rgba(0,0,0,0.05);
}

/* --- TABLA CLAY (AERODINÁMICA) --- */
.tableBlock {
  overflow-x: auto; /* Scroll para tablas en móvil */
  width: 100%;
}

.clayTable { 
  width: 100%; 
  border-collapse: separate; 
  border-spacing: 0 12px; 
  min-width: 500px; /* Asegura que no se aplaste el contenido */
}

.clayTable th { padding: 0 20px; text-align: left; opacity: 0.5; font-size: 0.8rem; text-transform: uppercase; }

.clayTable tr td {
  background: var(--clay-card);
  padding: 15px 20px;
  font-weight: 700;
  box-shadow: 
    8px 8px 16px rgba(0, 0, 0, 0.03),
    inset -6px -6px 12px rgba(0, 0, 0, 0.03),
    inset 6px 6px 12px rgba(255, 255, 255, 0.7);
}

.clayTable tr td:first-child { border-radius: 20px 0 0 20px; }
.clayTable tr td:last-child { border-radius: 0 20px 20px 0; }

.idBox {
  padding: 4px 10px;
  border-radius: 10px;
  font-size: 0.75rem;
  font-family: monospace;
}

.userName { display: flex; align-items: center; gap: 12px; white-space: nowrap; }

.clayAvatar {
  width: 32px;
  height: 32px;
  background: var(--c-blue);
  border-radius: 50%;
  flex-shrink: 0;
  box-shadow: inset -4px -4px 8px rgba(0,0,0,0.1), inset 4px 4px 8px rgba(255,255,255,0.4);
}

.status {
  padding: 5px 12px;
  border-radius: 12px;
  font-size: 0.75rem;
  box-shadow: inset -2px -2px 4px rgba(0,0,0,0.05), inset 2px 2px 4px rgba(255,255,255,0.3);
}

.clayGreen { background: var(--c-green); color: #2d6a4f; }
.clayYellow { background: var(--c-yellow); color: #997b66; }

.actionBtn {
  border: none;
  padding: 8px 15px;
  border-radius: 12px;
  font-weight: 800;
  font-size: 0.7rem;
  color: var(--clay-accent);
  cursor: pointer;
  transition: transform 0.2s;
}

.actionBtn:hover { transform: scale(1.05); }

/* --- FOOTER --- */
.footerBlock {
  padding: 15px 30px;
  border-radius: 25px;
}

.pagination { display: flex; justify-content: space-between; align-items: center; gap: 15px; }

.navBtn {
  border: none;
  padding: 10px 20px;
  border-radius: 15px;
  font-weight: 800;
  font-size: 0.8rem;
  cursor: pointer;
}

.pageCounter { font-weight: 800; opacity: 0.6; font-size: 0.85rem; }

/* --- RESPONSIVE --- */
@media (max-width: 850px) {
  .mainGrid { grid-template-columns: 1fr; }
  .headerBlock { flex-direction: column; text-align: center; }
  .searchBox { width: 100%; }
}

@media (max-width: 480px) {
  .clayContainer { border-radius: 20px; padding: 15px; }
  .clayTable td:nth-child(1) { display: none; } /* Ocultar ID en móvil */
  .pagination { flex-direction: column; }
}
</style>