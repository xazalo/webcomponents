<template>
  <div :class="$style.glassWrapper">
    <div :class="$style.bgOrb1"></div>
    <div :class="$style.bgOrb2"></div>

    <div :class="$style.glassContainer">
      <header :class="$style.headerBlock">
        <div :class="$style.titleBox">
          <h1 :class="$style.mainTitle">Crystal_Records</h1>
          <span :class="$style.subtitle">Data Visualization Layer</span>
        </div>
        <div :class="$style.searchBox">
          <input type="text" placeholder="Search records..." :class="$style.glassInput" />
          <span :class="$style.searchIcon">🔍</span>
        </div>
      </header>

      <div :class="$style.mainGrid">
        <aside :class="$style.filterBlock">
          <h3 :class="$style.subTitle">Categories</h3>
          <div :class="$style.filterList">
            <label v-for="tag in ['Internal', 'Public', 'Secure', 'Archived']" :key="tag" :class="$style.glassLabel">
              <input type="checkbox" :class="$style.glassCheck" />
              <span>{{ tag }}</span>
            </label>
          </div>
        </aside>

        <main :class="$style.tableBlock">
          <table :class="$style.glassTable">
            <thead>
              <tr>
                <th>REF_ID</th>
                <th>IDENTITY</th>
                <th>CLEARANCE</th>
                <th>ACTION</th>
              </tr>
            </thead>
            <tbody>
              <tr v-for="i in 5" :key="i">
                <td :class="$style.idCell">#G-0{{ i }}</td>
                <td>
                  <div :class="$style.userProfile">
                    <div :class="$style.glassAvatar"></div>
                    <span>Glass_Entity_{{ i }}</span>
                  </div>
                </td>
                <td>
                  <span :class="[$style.badge, i < 3 ? $style.high : $style.med]">
                    Level {{ 6 - i }}
                  </span>
                </td>
                <td>
                  <button :class="$style.viewBtn">Access</button>
                </td>
              </tr>
            </tbody>
          </table>
        </main>
      </div>

      <footer :class="$style.footerBlock">
        <div :class="$style.pagination">
          <button :class="$style.navLink">Previous</button>
          <div :class="$style.pageIndicator">Step 01 / 15</div>
          <button :class="$style.navLink">Next</button>
        </div>
      </footer>
    </div>
  </div>
</template>

<style module>
:root {
  --glass-bg: rgba(255, 255, 255, 0.1);
  --glass-border: rgba(255, 255, 255, 0.2);
  --glass-text: #ffffff;
}

/* --- ESCENA PRINCIPAL --- */
.glassWrapper {
  background: radial-gradient(circle at top left, #1a1a2e, #16213e);
  padding: clamp(20px, 5vw, 40px); /* Padding responsivo */
  position: relative;
  border-radius: 40px;
  overflow: hidden;
  max-width: 1000px;
  margin: 0 auto;
  min-height: 700px;
  box-sizing: border-box;
}

/* Orbes de color (Decorativos) */
.bgOrb1, .bgOrb2 {
  position: absolute;
  border-radius: 50%;
  filter: blur(80px);
  z-index: 0;
}

.bgOrb1 {
  top: -50px; right: -50px;
  width: 300px; height: 300px;
  background: linear-gradient(#4facfe, #00f2fe);
  opacity: 0.4;
}

.bgOrb2 {
  bottom: 50px; left: 100px;
  width: 250px; height: 250px;
  background: linear-gradient(#f093fb, #f5576c);
  opacity: 0.3;
}

/* --- CLASE MAESTRA GLASS --- */
.glassContainer, .headerBlock, .filterBlock, .tableBlock, .footerBlock, .glassInput, .viewBtn {
  background: var(--glass-bg);
  backdrop-filter: blur(12px);
  -webkit-backdrop-filter: blur(12px);
  border: 1px solid var(--glass-border);
  box-shadow: 0 8px 32px 0 rgba(0, 0, 0, 0.37);
  box-sizing: border-box;
}

.glassContainer {
  position: relative;
  z-index: 1;
  padding: clamp(15px, 4vw, 30px);
  border-radius: 30px;
  color: var(--glass-text);
  font-family: 'Inter', sans-serif;
}

/* --- HEADER FLUIDO --- */
.headerBlock {
  border-radius: 20px;
  padding: 20px;
  display: flex;
  justify-content: space-between;
  align-items: center;
  flex-wrap: wrap; /* Permite salto de línea en móviles */
  gap: 20px;
  margin-bottom: 25px;
}

.titleBox { flex: 1; min-width: 200px; }
.mainTitle { margin: 0; font-size: 1.8rem; font-weight: 300; letter-spacing: 1px; }
.subtitle { font-size: 0.7rem; opacity: 0.6; text-transform: uppercase; }

/* Buscador que ya no se sale */
.searchBox {
  position: relative;
  flex: 1 1 250px; /* Base de 250px, pero elástico */
  max-width: 100%;
}

.glassInput {
  width: 100%; /* Ocupa el 100% de su contenedor elástico */
  border-radius: 12px;
  padding: 12px 15px;
  color: white;
  outline: none;
  background: rgba(255, 255, 255, 0.05);
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
  display: flex; /* Cambiamos a flex para mejor control de wrap */
  flex-wrap: wrap;
  gap: 25px;
}

.filterBlock {
  flex: 0 0 220px; /* Ancho fijo en desktop */
  border-radius: 20px;
  padding: 20px;
}

.tableBlock {
  flex: 1 1 400px; /* Ocupa el resto, mínimo 400px antes de saltar */
  border-radius: 20px;
  overflow-x: auto; /* Scroll interno para la tabla si es pequeña */
}

.subTitle { 
  font-size: 0.9rem; 
  font-weight: 600; 
  margin-bottom: 15px; 
  border-bottom: 1px solid var(--glass-border); 
  padding-bottom: 8px; 
}

.filterList { display: flex; flex-direction: column; gap: 12px; }

.glassLabel { 
  display: flex; 
  align-items: center; 
  gap: 10px; 
  font-size: 0.85rem; 
  cursor: pointer; 
  transition: 0.2s;
}

.glassLabel:hover { opacity: 1; transform: translateX(5px); }

.glassCheck {
  appearance: none;
  width: 18px; height: 18px;
  border: 1px solid var(--glass-border);
  border-radius: 6px;
  cursor: pointer;
  background: rgba(255, 255, 255, 0.05);
}

.glassCheck:checked { 
  background: #4facfe; 
  border-color: #4facfe;
  box-shadow: 0 0 15px rgba(79, 172, 254, 0.4); 
}

/* --- TABLA GLASS --- */
.glassTable { 
  width: 100%; 
  border-collapse: collapse; 
  min-width: 500px; /* Evita que la tabla se colapse demasiado */
}

.glassTable th {
  background: rgba(255, 255, 255, 0.05);
  text-align: left;
  padding: 15px;
  font-size: 0.75rem;
  letter-spacing: 1px;
  opacity: 0.7;
  text-transform: uppercase;
}

.glassTable tr td {
  padding: 15px;
  border-bottom: 1px solid var(--glass-border);
  font-size: 0.9rem;
}

.idCell { font-family: monospace; color: #4facfe; font-weight: bold; }

.userProfile { display: flex; align-items: center; gap: 12px; }

.glassAvatar {
  width: 32px; height: 32px;
  background: linear-gradient(45deg, rgba(255,255,255,0.1), rgba(255,255,255,0.3));
  border-radius: 50%;
  border: 1px solid var(--glass-border);
}

.badge {
  padding: 4px 10px;
  border-radius: 20px;
  font-size: 0.7rem;
  font-weight: 700;
  white-space: nowrap;
}

.high { background: rgba(245, 87, 108, 0.2); color: #f5576c; border: 1px solid #f5576c; }
.med { background: rgba(79, 172, 254, 0.2); color: #4facfe; border: 1px solid #4facfe; }

.viewBtn {
  border: 1px solid rgba(255,255,255,0.1);
  padding: 8px 16px;
  border-radius: 10px;
  color: white;
  font-size: 0.75rem;
  cursor: pointer;
  transition: all 0.3s;
}

.viewBtn:hover { 
  background: rgba(255, 255, 255, 0.2); 
  transform: translateY(-2px);
}

/* --- FOOTER --- */
.footerBlock {
  margin-top: 25px;
  border-radius: 20px;
  padding: 15px 25px;
}

.pagination { 
  display: flex; 
  justify-content: space-between; 
  align-items: center; 
  flex-wrap: wrap;
  gap: 15px;
}

.navLink { 
  background: transparent; 
  border: none; 
  color: #4facfe; 
  cursor: pointer; 
  font-weight: 700; 
  font-size: 0.9rem;
}

.pageIndicator { font-size: 0.8rem; opacity: 0.6; font-family: monospace; }

/* --- RESPONSIVE --- */
@media (max-width: 800px) {
  .mainGrid { flex-direction: column; }
  .filterBlock { flex: 1 1 auto; }
  .headerBlock { flex-direction: column; align-items: flex-start; }
  .searchBox { width: 100%; }
}

@media (max-width: 480px) {
  .glassWrapper { border-radius: 0; padding: 15px; }
  .mainTitle { font-size: 1.4rem; }
  .glassTable td:nth-child(1) { display: none; } /* Ocultar Ref_ID en muy pequeños */
}
</style>