<template>
  <div :class="$style.bentoGrid">
    
    <div :class="[$style.cell, $style.searchCell]">
      <div :class="$style.headerInfo">
        <span :class="$style.label">DATABASE_ACCESS</span>
        <h2 :class="$style.title">USUARIOS</h2>
      </div>
      <div :class="$style.inputWrapper">
        <input type="text" placeholder="BUSCAR REGISTRO..." :class="$style.searchInput" />
        <span :class="$style.searchIcon">🔍</span>
      </div>
    </div>

    <div :class="[$style.cell, $style.statCell, $style.statActive]">
      <span :class="$style.statLabel">ACTIVOS</span>
      <span :class="$style.statValue">1,204</span>
    </div>
    
    <div :class="[$style.cell, $style.statCell, $style.statPending]">
      <span :class="$style.statLabel">PENDIENTES</span>
      <span :class="$style.statValue">48</span>
    </div>

    <div :class="[$style.cell, $style.tableCell]">
      <table :class="$style.table">
        <thead>
          <tr>
            <th>ID</th>
            <th>USUARIO</th>
            <th>ESTADO</th>
            <th>ACCIONES</th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="i in 4" :key="i">
            <td>#00{{ i }}</td>
            <td>
              <div :class="$style.userPilot">
                <div :class="$style.avatar"></div>
                <span>User_Alpha_{{ i }}</span>
              </div>
            </td>
            <td><span :class="$style.statusBadge">ONLINE</span></td>
            <td><button :class="$style.editBtn">EDIT</button></td>
          </tr>
        </tbody>
      </table>
    </div>

    <div :class="[$style.cell, $style.actionCell]">
      <div :class="$style.pagination">
        <button :class="$style.pageBtn">PREV</button>
        <span :class="$style.pageInfo">01 / 12</span>
        <button :class="$style.pageBtn">NEXT</button>
      </div>
      <button :class="$style.exportBtn">EXPORT_CSV</button>
    </div>

  </div>
</template>

<style module>
:root {
  --bento-bg: #f5f5f7;
  --bento-black: #000000;
  --bento-accent: #55efc4;
  --bento-border: 3px solid #000;
  --bento-radius: 24px;
}

.bentoGrid {
  display: grid;
  grid-template-columns: repeat(4, 1fr);
  grid-auto-rows: auto;
  gap: 16px;
  max-width: 900px;
  padding: 20px;
  background: var(--bento-bg);
  font-family: 'Inter', sans-serif;
}

.cell {
  background: #fff;
  border: var(--bento-border);
  border-radius: var(--bento-radius);
  padding: 20px;
  box-shadow: 6px 6px 0px #000;
  display: flex;
  flex-direction: column;
  transition: transform 0.2s ease;
}

.cell:hover {
  transform: translate(-2px, -2px);
  box-shadow: 8px 8px 0px #000;
}

/* Bloque Búsqueda: Ocupa 2 columnas */
.searchCell {
  grid-column: span 2;
  justify-content: space-between;
  gap: 15px;
}

.label {
  font-size: 0.65rem;
  font-weight: 900;
  letter-spacing: 1px;
  opacity: 0.5;
}

.title { margin: 0; font-size: 1.8rem; font-weight: 900; }

.inputWrapper {
  position: relative;
  display: flex;
  align-items: center;
}

.searchInput {
  width: 100%;
  padding: 12px;
  border: 2px solid #000;
  border-radius: 12px;
  font-weight: 600;
}

/* Bloques de Stats */
.statCell {
  grid-column: span 1;
  align-items: center;
  justify-content: center;
  text-align: center;
}

.statValue { font-size: 2rem; font-weight: 900; }
.statLabel { font-size: 0.7rem; font-weight: 800; opacity: 0.6; }

.statActive { background: var(--bento-accent); }
.statPending { background: #ffeaa7; }

/* Bloque Tabla: Ocupa todo el ancho */
.tableCell {
  grid-column: span 4;
  padding: 10px;
  min-height: 300px;
}

.table {
  width: 100%;
  border-collapse: separate;
  border-spacing: 0 8px;
}

.table th {
  text-align: left;
  padding: 12px;
  font-size: 0.75rem;
  text-transform: uppercase;
  opacity: 0.5;
}

.table td {
  padding: 12px;
  background: #f9f9f9;
  border-top: 2px solid #000;
  border-bottom: 2px solid #000;
}

.table td:first-child { border-left: 2px solid #000; border-radius: 12px 0 0 12px; }
.table td:last-child { border-right: 2px solid #000; border-radius: 0 12px 12px 0; }

.userPilot { display: flex; align-items: center; gap: 10px; font-weight: 700; }
.avatar { width: 30px; height: 30px; background: #ddd; border-radius: 50%; border: 2px solid #000; }

.statusBadge {
  background: #000;
  color: #fff;
  padding: 4px 8px;
  font-size: 0.6rem;
  border-radius: 6px;
  font-weight: 900;
}

/* Bloque Acciones */
.actionCell {
  grid-column: span 4;
  flex-direction: row;
  justify-content: space-between;
  align-items: center;
  background: #000;
  color: #fff;
}

.pageBtn, .exportBtn {
  background: #fff;
  border: 2px solid #000;
  padding: 8px 16px;
  border-radius: 10px;
  font-weight: 900;
  cursor: pointer;
}

.exportBtn { background: var(--bento-accent); }
.pageInfo { font-family: monospace; font-weight: bold; margin: 0 15px; }

@media (max-width: 768px) {
  .bentoGrid { grid-template-columns: repeat(2, 1fr); }
  .searchCell, .tableCell, .actionCell { grid-column: span 2; }
}
</style>