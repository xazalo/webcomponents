<template>
  <div :class="$style.neoContainer">
    <header :class="$style.headerBlock">
      <div :class="$style.titleBox">
        <h1 :class="$style.mainTitle">DATA_CORE</h1>
        <div :class="$style.badge">v.3.0_LIVE</div>
      </div>
      <div :class="$style.searchBox">
        <input type="text" placeholder="BUSCAR_REGISTROS..." :class="$style.neoInput" />
        <button :class="$style.searchBtn">GO!</button>
      </div>
    </header>

    <div :class="$style.mainGrid">
      <aside :class="$style.filterBlock">
        <h3 :class="$style.subTitle">FILTROS_</h3>
        <div :class="$style.filterList">
          <label v-for="tag in ['ADMIN', 'USER', 'BOT', 'GUEST']" :key="tag" :class="$style.checkLabel">
            <input type="checkbox" :class="$style.neoCheck" />
            <span :class="$style.checkText">{{ tag }}</span>
          </label>
        </div>
        <button :class="$style.resetBtn">WIPE_ALL</button>
      </aside>

      <main :class="$style.tableBlock">
        <table :class="$style.neoTable">
          <thead>
            <tr>
              <th>UUID</th>
              <th>IDENTIDAD</th>
              <th>ESTADO</th>
              <th>ACCION</th>
            </tr>
          </thead>
          <tbody>
            <tr v-for="i in 5" :key="i">
              <td :class="$style.mono">0x{{ i }}F4_99</td>
              <td :class="$style.userName">NEO_USER_{{ i }}</td>
              <td>
                <span :class="[$style.status, i % 2 === 0 ? $style.active : $style.error]">
                  {{ i % 2 === 0 ? 'READY' : 'FAIL' }}
                </span>
              </td>
              <td>
                <button :class="$style.miniBtn">DEL</button>
              </td>
            </tr>
          </tbody>
        </table>
      </main>
    </div>

    <footer :class="$style.footerBlock">
      <div :class="$style.pagination">
        <button :class="$style.pageBtn">⇽ BACK</button>
        <div :class="$style.pageNumber">PAGE_01</div>
        <button :class="$style.pageBtn">NEXT ⇾</button>
      </div>
    </footer>
  </div>
</template>

<style module>
:root {
  --neo-yellow: #f4fd05;
  --neo-pink: #ff00f5;
  --neo-blue: #32feff;
  --neo-green: #00ff41;
  --neo-black: #000000;
  --neo-white: #ffffff;
  --neo-shadow: 10px 10px 0px var(--neo-black);
}

.neoContainer {
  background: var(--neo-white);
  padding: 30px;
  font-family: 'Arial Black', sans-serif;
  color: var(--neo-black);
  display: flex;
  flex-direction: column;
  gap: 25px;
  max-width: 1000px;
}

/* Header Estilo Poster */
.headerBlock {
  background: var(--neo-yellow);
  border: 5px solid var(--neo-black);
  padding: 25px;
  box-shadow: var(--neo-shadow);
  display: flex;
  justify-content: space-between;
  align-items: center;
  flex-wrap: wrap;
  gap: 20px;
}

.mainTitle { margin: 0; font-size: 3.5rem; line-height: 0.8; letter-spacing: -2px; }

.badge {
  background: var(--neo-black);
  color: var(--neo-white);
  padding: 5px 10px;
  display: inline-block;
  margin-top: 10px;
  font-size: 0.8rem;
}

.searchBox { display: flex; gap: 10px; flex-grow: 1; max-width: 400px; }

.neoInput {
  flex-grow: 1;
  border: 4px solid var(--neo-black);
  padding: 12px;
  font-weight: 900;
  font-size: 1rem;
}

.searchBtn {
  background: var(--neo-pink);
  border: 4px solid var(--neo-black);
  padding: 0 20px;
  font-weight: 900;
  cursor: pointer;
  box-shadow: 4px 4px 0px var(--neo-black);
}

.searchBtn:active { transform: translate(2px, 2px); box-shadow: none; }

/* Grid Layout */
.mainGrid {
  display: grid;
  grid-template-columns: 250px 1fr;
  gap: 25px;
}

/* Sidebar Filtros */
.filterBlock {
  background: var(--neo-blue);
  border: 5px solid var(--neo-black);
  padding: 20px;
  box-shadow: var(--neo-shadow);
}

.filterList { display: flex; flex-direction: column; gap: 12px; margin: 20px 0; }

.checkLabel { display: flex; align-items: center; gap: 10px; cursor: pointer; }

.neoCheck {
  width: 25px;
  height: 25px;
  border: 3px solid var(--neo-black);
  appearance: none;
  background: white;
  cursor: pointer;
}

.neoCheck:checked { background: var(--neo-black); position: relative; }
.neoCheck:checked::after { content: '✕'; color: var(--neo-white); position: absolute; left: 4px; top: -2px; }

/* Tabla Masiva */
.tableBlock {
  background: var(--neo-white);
  border: 5px solid var(--neo-black);
  box-shadow: var(--neo-shadow);
  overflow-x: auto;
}

.neoTable { width: 100%; border-collapse: collapse; }

.neoTable th {
  background: var(--neo-black);
  color: var(--neo-white);
  padding: 15px;
  text-align: left;
}

.neoTable td {
  padding: 15px;
  border-bottom: 4px solid var(--neo-black);
  font-weight: 800;
}

.userName { color: var(--neo-pink); text-transform: uppercase; }
.mono { font-family: monospace; font-size: 0.9rem; }

.status {
  padding: 5px 10px;
  border: 3px solid var(--neo-black);
}
.active { background: var(--neo-green); }
.error { background: var(--neo-pink); color: white; }

.miniBtn {
  background: var(--neo-white);
  border: 3px solid var(--neo-black);
  padding: 5px 10px;
  font-weight: 900;
  cursor: pointer;
}

.miniBtn:hover { background: var(--neo-black); color: var(--neo-white); }

/* Footer */
.footerBlock {
  background: var(--neo-black);
  padding: 20px;
  border: 5px solid var(--neo-black);
  box-shadow: var(--neo-shadow);
}

.pagination { display: flex; justify-content: space-between; align-items: center; }

.pageBtn {
  background: var(--neo-yellow);
  border: 3px solid var(--neo-black);
  padding: 10px 20px;
  font-weight: 900;
  cursor: pointer;
  box-shadow: 4px 4px 0px var(--neo-white);
}

.pageNumber { color: var(--neo-white); font-size: 1.2rem; }

@media (max-width: 800px) {
  .mainGrid { grid-template-columns: 1fr; }
  .mainTitle { font-size: 2.5rem; }
}
</style>