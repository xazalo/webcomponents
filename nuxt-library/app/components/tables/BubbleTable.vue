<template>
  <div :class="$style.bubbleContainer">
    <header :class="$style.headerBlock">
      <div :class="$style.titleBox">
        <h1 :class="$style.mainTitle">Data_Cloud</h1>
        <div :class="$style.statusDot">LIVE_STREAM</div>
      </div>
      <div :class="$style.searchBox">
        <input type="text" placeholder="Buscar en la nube..." :class="$style.bubbleInput" />
        <button :class="$style.searchBtn">✨</button>
      </div>
    </header>

    <div :class="$style.mainGrid">
      <aside :class="$style.filterBlock">
        <h3 :class="$style.subTitle">Filtros</h3>
        <div :class="$style.filterList">
          <label v-for="tag in ['Admin', 'User', 'Bot', 'Guest']" :key="tag" :class="$style.checkLabel">
            <input type="checkbox" :class="$style.bubbleCheck" />
            <span :class="$style.checkText">{{ tag }}</span>
          </label>
        </div>
        <button :class="$style.resetBtn">Limpiar todo</button>
      </aside>

      <main :class="$style.tableBlock">
        <table :class="$style.bubbleTable">
          <thead>
            <tr>
              <th>ID</th>
              <th>Identidad</th>
              <th>Estado</th>
              <th>Acción</th>
            </tr>
          </thead>
          <tbody>
            <tr v-for="i in 5" :key="i">
              <td><span :class="$style.idTag">#{{ i }}</span></td>
              <td :class="$style.userName">
                <div :class="$style.avatar"></div>
                Bubble_User_{{ i }}
              </td>
              <td>
                <span :class="[$style.status, i % 2 === 0 ? $style.active : $style.pending]">
                  {{ i % 2 === 0 ? 'Activo' : 'En espera' }}
                </span>
              </td>
              <td>
                <button :class="$style.actionBtn">Ver</button>
              </td>
            </tr>
          </tbody>
        </table>
      </main>
    </div>

    <footer :class="$style.footerBlock">
      <div :class="$style.pagination">
        <button :class="$style.pageBtn">Anterior</button>
        <div :class="$style.pageNumber">Página 01</div>
        <button :class="$style.pageBtn">Siguiente</button>
      </div>
    </footer>
  </div>
</template>

<style module>
:root {
  --bubble-pink: #ff85a1;
  --bubble-blue: #70d6ff;
  --bubble-purple: #b5179e;
  --bubble-green: #9df9ef;
  --bubble-white: #ffffff;
  --bubble-soft-bg: #f0f4f8;
}

/* --- CONTENEDOR PRINCIPAL --- */
.bubbleContainer {
  background: var(--bubble-soft-bg);
  padding: clamp(15px, 4vw, 30px);
  font-family: 'Quicksand', sans-serif;
  color: #2d3436;
  display: flex;
  flex-direction: column;
  gap: 20px;
  max-width: 1000px;
  margin: 0 auto;
  border-radius: 50px;
  box-sizing: border-box;
}

/* --- HEADER DINÁMICO --- */
.headerBlock {
  background: var(--bubble-white);
  border-radius: 35px;
  padding: 20px clamp(20px, 5vw, 40px);
  display: flex;
  justify-content: space-between;
  align-items: center;
  flex-wrap: wrap; /* Fix para pantallas pequeñas */
  gap: 20px;
  box-shadow: 0 15px 30px rgba(0,0,0,0.05);
  border: 2px solid #fff;
}

.titleBox { flex: 1 1 auto; }

.mainTitle { 
  margin: 0; 
  font-size: clamp(1.5rem, 5vw, 2.2rem); 
  font-weight: 800; 
  background: linear-gradient(45deg, var(--bubble-pink), var(--bubble-blue));
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
}

.statusDot {
  font-size: 0.7rem;
  font-weight: 700;
  color: var(--bubble-purple);
  background: #f1e4ff;
  padding: 4px 12px;
  border-radius: 20px;
  margin-top: 5px;
  display: inline-block;
}

/* Buscador Elástico */
.searchBox { 
  display: flex; 
  gap: 10px; 
  flex: 1 1 300px; /* Base de 300px pero elástico */
  max-width: 100%; 
}

.bubbleInput {
  flex-grow: 1;
  width: 100%;
  border: 2px solid #eef2f7;
  background: #f8f9fa;
  padding: 12px 20px;
  border-radius: 25px;
  font-weight: 600;
  box-sizing: border-box;
  outline: none;
  transition: all 0.3s ease;
}

.bubbleInput:focus {
  border-color: var(--bubble-blue);
  background: #fff;
  box-shadow: 0 0 15px rgba(112, 214, 255, 0.1);
}

.searchBtn {
  flex-shrink: 0; /* Evita que el botón se deforme */
  background: var(--bubble-blue);
  border: none;
  width: 48px;
  height: 48px;
  border-radius: 50%;
  color: white;
  font-size: 1.2rem;
  cursor: pointer;
  box-shadow: 0 8px 15px rgba(112, 214, 255, 0.3);
  transition: transform 0.2s;
}

.searchBtn:hover { transform: scale(1.1); }

/* --- GRID LAYOUT --- */
.mainGrid {
  display: grid;
  grid-template-columns: 240px 1fr;
  gap: 25px;
}

.filterBlock {
  background: var(--bubble-white);
  border-radius: 35px;
  padding: 25px;
  height: fit-content;
  box-shadow: 0 10px 25px rgba(0,0,0,0.03);
}

.subTitle { font-weight: 800; margin-bottom: 20px; color: #636e72; font-size: 1rem; }

.filterList { display: flex; flex-direction: column; gap: 15px; }

.checkLabel { display: flex; align-items: center; gap: 12px; cursor: pointer; font-weight: 600; font-size: 0.9rem; }

.bubbleCheck {
  width: 24px;
  height: 24px;
  flex-shrink: 0;
  border: 2px solid #dfe6e9;
  appearance: none;
  background: white;
  border-radius: 8px;
  cursor: pointer;
  position: relative;
  transition: all 0.2s;
}

.bubbleCheck:checked {
  background: var(--bubble-pink);
  border-color: var(--bubble-pink);
  box-shadow: 0 4px 10px rgba(255, 133, 161, 0.3);
}

.bubbleCheck:checked::after {
  content: '✔';
  color: white;
  position: absolute;
  font-size: 0.9rem;
  left: 50%; top: 50%;
  transform: translate(-50%, -50%);
}

/* --- TABLA DE BURBUJAS --- */
.tableBlock {
  overflow-x: auto; /* Scroll para dispositivos móviles */
  width: 100%;
}

.bubbleTable { 
  width: 100%; 
  border-collapse: separate; 
  border-spacing: 0 12px; 
  min-width: 500px;
}

.bubbleTable th {
  padding: 0 20px 10px;
  text-align: left;
  font-weight: 700;
  color: #b2bec3;
  font-size: 0.8rem;
  text-transform: uppercase;
}

.bubbleTable tr td {
  background: var(--bubble-white);
  padding: 15px 20px;
  font-weight: 600;
  box-shadow: 0 4px 12px rgba(0,0,0,0.02);
  font-size: 0.9rem;
}

.bubbleTable tr td:first-child { border-radius: 25px 0 0 25px; }
.bubbleTable tr td:last-child { border-radius: 0 25px 25px 0; }

.idTag {
  background: #f0f2f5;
  padding: 5px 12px;
  border-radius: 12px;
  font-size: 0.75rem;
  font-family: monospace;
}

.userName { display: flex; align-items: center; gap: 12px; white-space: nowrap; }

.avatar {
  width: 36px;
  height: 36px;
  background: linear-gradient(135deg, var(--bubble-blue), var(--bubble-green));
  border-radius: 50%;
  flex-shrink: 0;
}

.status {
  padding: 6px 15px;
  border-radius: 20px;
  font-size: 0.75rem;
  font-weight: 700;
  white-space: nowrap;
}
.active { background: #e3fcef; color: #00b894; }
.pending { background: #fff4e5; color: #e67e22; }

.actionBtn {
  background: #f1f2f6;
  border: none;
  padding: 10px 20px;
  border-radius: 18px;
  font-weight: 700;
  cursor: pointer;
  color: #57606f;
  transition: all 0.2s;
}

.actionBtn:hover { 
  background: var(--bubble-blue); 
  color: white;
  transform: translateY(-2px);
}

/* --- FOOTER --- */
.footerBlock {
  background: var(--bubble-white);
  padding: 15px clamp(15px, 4vw, 30px);
  border-radius: 30px;
  box-shadow: 0 10px 25px rgba(0,0,0,0.03);
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
  border: 2px solid #f1f2f6;
  padding: 10px 20px;
  border-radius: 20px;
  font-weight: 700;
  font-size: 0.85rem;
  cursor: pointer;
  transition: all 0.2s;
}

.pageBtn:hover { 
  background: var(--bubble-soft-bg); 
  border-color: var(--bubble-blue);
  color: var(--bubble-blue);
}

.pageNumber { font-weight: 800; color: #2d3436; font-size: 0.9rem; }

/* --- RESPONSIVE --- */
@media (max-width: 850px) {
  .mainGrid { grid-template-columns: 1fr; }
  .headerBlock { flex-direction: column; align-items: flex-start; }
  .searchBox { width: 100%; }
  .bubbleContainer { border-radius: 30px; }
}

@media (max-width: 480px) {
  .bubbleTable td:nth-child(1) { display: none; } /* Ocultar ID */
  .mainTitle { font-size: 1.4rem; }
  .pagination { flex-direction: column; }
}
</style>