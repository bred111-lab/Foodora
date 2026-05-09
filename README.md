[foodora-prehled.html](https://github.com/user-attachments/files/27559800/foodora-prehled.html)
<!DOCTYPE html>
<html lang="cs">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0">
<meta name="theme-color" content="#2d2d2d">
<meta name="apple-mobile-web-app-capable" content="yes">
<meta name="mobile-web-app-capable" content="yes">
<title>Foodora Přehled</title>
<link rel="preconnect" href="https://fonts.googleapis.com">
<link href="https://fonts.googleapis.com/css2?family=DM+Mono:wght@400;500&family=Syne:wght@700;800&display=swap" rel="stylesheet">
<script src="https://cdnjs.cloudflare.com/ajax/libs/Chart.js/4.4.1/chart.umd.js"></script>
<style>
  *, *::before, *::after { box-sizing: border-box; margin: 0; padding: 0; }

  :root {
    --pink: #2d2d2d;
    --pink-dark: #1a1a1a;
    --bg: #0f0f0f;
    --surface: #1a1a1a;
    --surface2: #242424;
    --text: #f5f5f5;
    --muted: #888;
    --green: #1D9E75;
    --blue: #378ADD;
    --orange: #D85A30;
    --gray: #666;
    --border: rgba(255,255,255,0.08);
  }

  html, body {
    background: var(--bg);
    color: var(--text);
    font-family: 'DM Mono', monospace;
    min-height: 100dvh;
    overscroll-behavior: none;
  }

  header {
    background: var(--pink);
    padding: 1.2rem 1.2rem 1rem;
    display: flex;
    align-items: center;
    gap: 10px;
    position: sticky;
    top: 0;
    z-index: 10;
  }

  header .logo {
    width: 32px;
    height: 32px;
    background: white;
    border-radius: 8px;
    display: flex;
    align-items: center;
    justify-content: center;
    flex-shrink: 0;
  }

  header .logo svg { width: 20px; height: 20px; }

  header h1 {
    font-family: 'Syne', sans-serif;
    font-size: 1.15rem;
    font-weight: 800;
    color: white;
    letter-spacing: -0.02em;
  }

  header span {
    font-size: 0.7rem;
    color: rgba(255,255,255,0.7);
    margin-top: 1px;
  }

  main { padding: 1rem; display: flex; flex-direction: column; gap: 1rem; }

  /* Stat cards */
  .stats-grid {
    display: grid;
    grid-template-columns: 1fr 1fr;
    gap: 0.6rem;
  }

  .stat-card {
    background: var(--surface);
    border: 1px solid var(--border);
    border-radius: 12px;
    padding: 0.9rem;
  }

  .stat-card.accent {
    background: var(--pink);
    border-color: var(--pink);
    grid-column: span 2;
    display: flex;
    align-items: center;
    justify-content: space-between;
  }

  .stat-card .label {
    font-size: 0.65rem;
    color: var(--muted);
    letter-spacing: 0.05em;
    text-transform: uppercase;
    margin-bottom: 4px;
  }

  .stat-card.accent .label { color: rgba(255,255,255,0.75); font-size: 0.7rem; }

  .stat-card .value {
    font-family: 'Syne', sans-serif;
    font-size: 1.35rem;
    font-weight: 700;
    color: var(--text);
  }

  .stat-card.accent .value { font-size: 1.8rem; color: white; }
  .stat-card.accent .sub { font-size: 0.7rem; color: rgba(255,255,255,0.7); margin-top: 2px; }

  /* Chart section */
  .chart-card {
    background: var(--surface);
    border: 1px solid var(--border);
    border-radius: 12px;
    padding: 1rem;
  }

  .chart-card h2 {
    font-family: 'Syne', sans-serif;
    font-size: 0.9rem;
    font-weight: 700;
    margin-bottom: 0.8rem;
    color: var(--text);
  }

  .legend {
    display: flex;
    flex-wrap: wrap;
    gap: 10px;
    margin-bottom: 0.8rem;
  }

  .legend-item {
    display: flex;
    align-items: center;
    gap: 5px;
    font-size: 0.65rem;
    color: var(--muted);
    text-transform: uppercase;
    letter-spacing: 0.04em;
  }

  .legend-dot {
    width: 8px;
    height: 8px;
    border-radius: 2px;
    flex-shrink: 0;
  }

  .chart-wrap { position: relative; width: 100%; height: 220px; }

  /* Add month form */
  .add-card {
    background: var(--surface);
    border: 1px solid var(--border);
    border-radius: 12px;
    padding: 1rem;
  }

  .add-card h2 {
    font-family: 'Syne', sans-serif;
    font-size: 0.9rem;
    font-weight: 700;
    margin-bottom: 0.9rem;
    color: var(--text);
  }

  .form-grid {
    display: grid;
    grid-template-columns: 1fr 1fr;
    gap: 0.6rem;
    margin-bottom: 0.8rem;
  }

  .form-field { display: flex; flex-direction: column; gap: 4px; }
  .form-field.full { grid-column: span 2; }

  .form-field label {
    font-size: 0.6rem;
    color: var(--muted);
    text-transform: uppercase;
    letter-spacing: 0.05em;
  }

  .form-field input, .form-field select {
    background: var(--surface2);
    border: 1px solid var(--border);
    border-radius: 8px;
    color: var(--text);
    font-family: 'DM Mono', monospace;
    font-size: 0.85rem;
    padding: 0.55rem 0.7rem;
    outline: none;
    width: 100%;
    -webkit-appearance: none;
  }

  .form-field input:focus, .form-field select:focus {
    border-color: var(--pink);
  }

  .form-field select option { background: #222; }

  .btn-add {
    width: 100%;
    background: var(--pink);
    color: white;
    border: none;
    border-radius: 10px;
    padding: 0.75rem;
    font-family: 'Syne', sans-serif;
    font-size: 0.95rem;
    font-weight: 700;
    cursor: pointer;
    letter-spacing: 0.02em;
    transition: background 0.15s;
  }

  .btn-add:active { background: var(--pink-dark); transform: scale(0.98); }

  /* History */
  .history-card {
    background: var(--surface);
    border: 1px solid var(--border);
    border-radius: 12px;
    padding: 1rem;
    margin-bottom: 1rem;
  }

  .history-card h2 {
    font-family: 'Syne', sans-serif;
    font-size: 0.9rem;
    font-weight: 700;
    margin-bottom: 0.8rem;
  }

  .history-row {
    display: flex;
    align-items: center;
    justify-content: space-between;
    padding: 0.65rem 0;
    border-bottom: 1px solid var(--border);
  }

  .history-row:last-child { border-bottom: none; }

  .history-row .month-name { font-size: 0.8rem; color: var(--text); }
  .history-row .month-hours { font-size: 0.65rem; color: var(--muted); margin-top: 2px; }

  .history-row .month-total {
    font-family: 'Syne', sans-serif;
    font-size: 1rem;
    font-weight: 700;
    color: var(--pink);
  }

  .delete-btn {
    background: none;
    border: none;
    color: var(--muted);
    font-size: 1rem;
    cursor: pointer;
    padding: 4px 8px;
    border-radius: 6px;
  }

  .delete-btn:active { color: var(--pink); }

  .empty-state {
    text-align: center;
    color: var(--muted);
    font-size: 0.75rem;
    padding: 1.5rem 0;
  }
</style>
</head>
<body>

<header>
  <div class="logo">
    <svg viewBox="0 0 24 24" fill="none" xmlns="http://www.w3.org/2000/svg">
      <circle cx="12" cy="12" r="10" fill="#E8173A"/>
      <path d="M8 12h8M12 8v8" stroke="white" stroke-width="2.5" stroke-linecap="round"/>
    </svg>
  </div>
  <div>
    <h1>Foodora Výdělky</h1>
    <span>Měsíční přehled</span>
  </div>
</header>

<main>
  <!-- Summary stats -->
  <div class="stats-grid" id="stats-grid">
    <!-- rendered by JS -->
  </div>

  <!-- Chart -->
  <div class="chart-card">
    <h2>Vývoj výdělků</h2>
    <div class="legend">
      <span class="legend-item"><span class="legend-dot" style="background:#1D9E75"></span>Základ</span>
      <span class="legend-item"><span class="legend-dot" style="background:#378ADD"></span>Questy</span>
      <span class="legend-item"><span class="legend-dot" style="background:#D85A30"></span>Spropitné</span>
      <span class="legend-item"><span class="legend-dot" style="background:#666"></span>Úpravy</span>
    </div>
    <div class="chart-wrap">
      <canvas id="mainChart" role="img" aria-label="Graf měsíčních výdělků z Foodory"></canvas>
    </div>
  </div>

  <!-- Add month -->
  <div class="add-card">
    <h2>Přidat měsíc</h2>
    <div class="form-grid">
      <div class="form-field">
        <label>Měsíc</label>
        <select id="f-month">
          <option value="0">Leden</option>
          <option value="1">Únor</option>
          <option value="2">Březen</option>
          <option value="3">Duben</option>
          <option value="4">Květen</option>
          <option value="5">Červen</option>
          <option value="6">Červenec</option>
          <option value="7">Srpen</option>
          <option value="8">Září</option>
          <option value="9">Říjen</option>
          <option value="10">Listopad</option>
          <option value="11">Prosinec</option>
        </select>
      </div>
      <div class="form-field">
        <label>Rok</label>
        <input type="number" id="f-year" value="2026" min="2020" max="2099">
      </div>
      <div class="form-field">
        <label>Základ (Kč)</label>
        <input type="number" id="f-zaklad" placeholder="0" min="0" step="0.01">
      </div>
      <div class="form-field">
        <label>Questy (Kč)</label>
        <input type="number" id="f-questy" placeholder="0" min="0" step="0.01">
      </div>
      <div class="form-field">
        <label>Spropitné (Kč)</label>
        <input type="number" id="f-spropitne" placeholder="0" min="0" step="0.01">
      </div>
      <div class="form-field">
        <label>Úpravy (Kč)</label>
        <input type="number" id="f-upravy" placeholder="0" step="0.01">
      </div>
      <div class="form-field">
        <label>Hodiny</label>
        <input type="number" id="f-hodiny" placeholder="0" min="0">
      </div>
      <div class="form-field">
        <label>Minuty</label>
        <input type="number" id="f-minuty" placeholder="0" min="0" max="59">
      </div>
    </div>
    <button class="btn-add" onclick="addMonth()">Přidat měsíc</button>
    
  </div>

  <!-- History -->
  <div class="history-card">
    <h2>Historie</h2>
    <div id="history-list"></div>
  </div>
</main>

<script>
const MONTHS_CS = ['Leden','Únor','Březen','Duben','Květen','Červen','Červenec','Srpen','Září','Říjen','Listopad','Prosinec'];

const DEFAULT_DATA = [
  {"month":2,"year":2026,"zaklad":720.62,"questy":0,"spropitne":20,"upravy":0,"hodiny":3,"minuty":10},
  {"month":3,"year":2026,"zaklad":17256.6,"questy":425,"spropitne":1260,"upravy":-999.46,"hodiny":74,"minuty":14}
];

let data = JSON.parse(localStorage.getItem('foodora-data') || 'null') || DEFAULT_DATA;

function save() {
  try { localStorage.setItem('foodora-data', JSON.stringify(data)); } catch(e) {}
}

function total(d) { return d.zaklad + d.questy + d.spropitne + (d.upravy || 0); }

function fmt(n) { return n.toLocaleString('cs-CZ', {minimumFractionDigits: 0, maximumFractionDigits: 0}) + ' Kč'; }

function renderStats() {
  const grid = document.getElementById('stats-grid');
  if (!data.length) { grid.innerHTML = ''; return; }
  const totalAll = data.reduce((s,d) => s + total(d), 0);
  const best = data.reduce((a,b) => total(a) > total(b) ? a : b);
  const totalHours = data.reduce((s,d) => s + d.hodiny + (d.minuty||0)/60, 0);
  const avgHourly = totalAll / totalHours;
  grid.innerHTML = `
    <div class="stat-card accent">
      <div>
        <div class="label">Celkem výdělků</div>
        <div class="value">${fmt(totalAll)}</div>
        <div class="sub">${data.length} ${data.length===1?'měsíc':data.length<5?'měsíce':'měsíců'}</div>
      </div>
      <div style="font-size:2rem; opacity:0.4">🛵</div>
    </div>
    <div class="stat-card">
      <div class="label">Nejlepší měsíc</div>
      <div class="value" style="font-size:1rem">${MONTHS_CS[best.month]}</div>
      <div style="font-size:0.7rem;color:var(--muted);margin-top:2px">${fmt(total(best))}</div>
    </div>
    <div class="stat-card">
      <div class="label">Prům. hod. výdělek</div>
      <div class="value">${isFinite(avgHourly) ? Math.round(avgHourly) + ' Kč' : '—'}</div>
    </div>
  `;
}

let chart = null;
function renderChart() {
  const sorted = [...data].sort((a,b) => a.year !== b.year ? a.year-b.year : a.month-b.month);
  const labels = sorted.map(d => MONTHS_CS[d.month].substring(0,3) + ' ' + d.year);
  const ctx = document.getElementById('mainChart');
  if (chart) chart.destroy();
  if (!sorted.length) return;
  chart = new Chart(ctx, {
    type: 'bar',
    data: {
      labels,
      datasets: [
        { label: 'Základ',    data: sorted.map(d=>d.zaklad),    backgroundColor: '#1D9E75', stack: 's' },
        { label: 'Questy',    data: sorted.map(d=>d.questy),    backgroundColor: '#378ADD', stack: 's' },
        { label: 'Spropitné', data: sorted.map(d=>d.spropitne), backgroundColor: '#D85A30', stack: 's' },
        { label: 'Úpravy',   data: sorted.map(d=>d.upravy||0), backgroundColor: '#555555', stack: 's' }
      ]
    },
    options: {
      responsive: true, maintainAspectRatio: false,
      plugins: {
        legend: { display: false },
        tooltip: { callbacks: { label: ctx => ctx.dataset.label + ': ' + ctx.parsed.y.toLocaleString('cs-CZ',{minimumFractionDigits:2,maximumFractionDigits:2}) + ' Kč' } }
      },
      scales: {
        x: { stacked: true, ticks: { color: '#888', font: { family: 'DM Mono', size: 11 }, autoSkip: false }, grid: { color: 'rgba(255,255,255,0.05)' } },
        y: { stacked: true, ticks: { color: '#888', font: { family: 'DM Mono', size: 10 }, callback: v => v.toLocaleString('cs-CZ') + ' Kč' }, grid: { color: 'rgba(255,255,255,0.05)' } }
      }
    }
  });
}

function renderHistory() {
  const list = document.getElementById('history-list');
  const sorted = [...data].sort((a,b) => b.year !== a.year ? b.year-a.year : b.month-a.month);
  if (!sorted.length) { list.innerHTML = '<div class="empty-state">Žádná data. Přidej první měsíc!</div>'; return; }
  list.innerHTML = sorted.map((d,i) => {
    const idx = data.indexOf(d);
    return `<div class="history-row">
      <div>
        <div class="month-name">${MONTHS_CS[d.month]} ${d.year}</div>
        <div class="month-hours">${d.hodiny}h ${d.minuty||0}m odjeto</div>
      </div>
      <div style="display:flex;align-items:center;gap:8px">
        <div class="month-total">${fmt(total(d))}</div>
        <button class="delete-btn" onclick="deleteMonth(${idx})">✕</button>
      </div>
    </div>`;
  }).join('');
}

function render() { renderStats(); renderChart(); renderHistory(); }

function addMonth() {
  const month = parseInt(document.getElementById('f-month').value);
  const year = parseInt(document.getElementById('f-year').value);
  const zaklad = parseFloat(document.getElementById('f-zaklad').value) || 0;
  const questy = parseFloat(document.getElementById('f-questy').value) || 0;
  const spropitne = parseFloat(document.getElementById('f-spropitne').value) || 0;
  const upravy = parseFloat(document.getElementById('f-upravy').value) || 0;
  const hodiny = parseInt(document.getElementById('f-hodiny').value) || 0;
  const minuty = parseInt(document.getElementById('f-minuty').value) || 0;

  if (!zaklad && !questy && !spropitne) { alert('Zadej alespoň základ.'); return; }

  const existing = data.findIndex(d => d.month === month && d.year === year);
  if (existing >= 0) {
    if (!confirm(`${MONTHS_CS[month]} ${year} již existuje. Přepsat?`)) return;
    data[existing] = { month, year, zaklad, questy, spropitne, upravy, hodiny, minuty };
  } else {
    data.push({ month, year, zaklad, questy, spropitne, upravy, hodiny, minuty });
  }

  save();
  render();

  // Clear inputs
  ['f-zaklad','f-questy','f-spropitne','f-upravy','f-hodiny','f-minuty'].forEach(id => document.getElementById(id).value = '');
}

function deleteMonth(idx) {
  const d = data[idx];
  if (!confirm(`Smazat ${MONTHS_CS[d.month]} ${d.year}?`)) return;
  data.splice(idx, 1);
  save();
  render();
}

// Set default month/year to current
const now = new Date();
document.getElementById('f-month').value = now.getMonth();
document.getElementById('f-year').value = now.getFullYear();

render();
</script>
</body>
</html>
