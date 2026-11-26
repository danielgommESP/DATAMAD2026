Este es el repositorio para el participante número 20
proyecto enlace: https://peach-marigold-4.tiiny.site
[Presentación sin título.pdf](https://github.com/user-attachments/files/35470054/Presentacion.sin.titulo.pdf)
[metro dinamico proyecto.html](https://github.com/user-attachments/files/35470063/metro.dinamico.proyecto.html)

<!DOCTYPE html>
<html lang="es">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Metro de Madrid — Red, Frecuencias, Demanda, Aire y Accesibilidad PMR</title>
  <link rel="stylesheet" href="https://unpkg.com/leaflet@1.9.4/dist/leaflet.css" crossorigin="" />
  <style>
    :root{--bg:#05070c;--p1:#0a0e17;--p2:#111827;--p3:#182234;--bd:#263248;--tx:#f8fafc;--mu:#94a3b8;--ac:#38bdf8}
    *{box-sizing:border-box}
    html,body{margin:0;padding:0;height:100%;width:100%;background:var(--bg);color:var(--tx);font-family:-apple-system,BlinkMacSystemFont,"Segoe UI",Roboto,Inter,sans-serif;overflow:hidden}
    .app{display:grid;grid-template-columns:390px 1fr;grid-template-rows:1fr auto;height:100vh;width:100vw}
    .sidebar{grid-row:1/3;background:var(--bg);border-right:1px solid var(--bd);display:flex;flex-direction:column;overflow-y:auto;z-index:1000;box-shadow:4px 0 18px rgba(0,0,0,.45)}
    .header{padding:12px 15px 10px;border-bottom:1px solid var(--bd);background:linear-gradient(135deg,#060a14 0%,#0b1b36 100%);color:#fff}
    .header-top{display:flex;align-items:center;justify-content:space-between;gap:8px}
    .header h1{margin:0;font-size:15.5px;font-weight:800;display:flex;align-items:center;gap:8px}
    .metro-diamond{display:inline-flex;align-items:center;justify-content:center;background:#004B87;color:#fff;border:2.5px solid #E51923;font-size:10px;font-weight:900;padding:2px 7px;border-radius:4px}
    .header p{margin:5px 0 0;font-size:11px;color:#cbd5e1;line-height:1.35}
    .btn-info{background:rgba(255,255,255,.1);border:1px solid rgba(255,255,255,.28);color:#fff;font-size:11px;padding:4px 10px;border-radius:6px}
    .btn-info:hover{background:rgba(56,189,248,.22);border-color:var(--ac)}
    .badge-row{display:flex;flex-wrap:wrap;gap:5px;margin-top:8px}
    .status-badge{font-size:10px;font-weight:600;padding:2px 8px;border-radius:999px;background:rgba(56,189,248,.14);border:1px solid rgba(56,189,248,.35);color:#bae6fd}
    .status-badge.live{background:rgba(34,197,94,.18);border-color:rgba(74,222,128,.45);color:#86efac}
    .status-badge.datamad{background:rgba(245,158,11,.16);border-color:rgba(251,191,36,.42);color:#fde68a}
    .section{padding:10px 15px;border-bottom:1px solid var(--bd);background:var(--bg)}
    .section-title{font-size:10.5px;text-transform:uppercase;letter-spacing:.6px;color:var(--mu);font-weight:800;margin:0 0 7px;display:flex;justify-content:space-between;align-items:center;gap:6px}
    .btn-group{display:flex;gap:6px;flex-wrap:wrap}
    button{cursor:pointer;border:1px solid var(--bd);background:var(--p2);color:var(--tx);border-radius:7px;padding:6px 10px;font-size:12px;font-weight:600;transition:all .14s ease}
    button:hover{border-color:var(--ac);background:var(--p3)}
    button.active{background:#0284c7;border-color:#38bdf8;color:#fff}
    button.eco-active{background:#15803d;border-color:#4ade80;color:#fff}
    button.pmr-active{background:#0369a1;border-color:#38bdf8;color:#fff}
    button.small{padding:4px 9px;font-size:11px}
    .mode-grid{display:grid;grid-template-columns:repeat(2,1fr);gap:6px;margin-bottom:8px}
    .mode-grid button{padding:7px 5px;font-size:11.5px;text-align:center}
    .impact-box{background:linear-gradient(135deg,rgba(20,83,45,.34) 0%,rgba(10,14,23,.95) 100%);border:1px solid rgba(74,222,128,.34);border-radius:9px;padding:7px 9px;display:flex;flex-direction:column;gap:6px}
    .eco-top{display:flex;align-items:center;justify-content:space-between;gap:8px}
    .eco-stats{display:grid;grid-template-columns:repeat(3,1fr);gap:5px}
    .eco-stat-item{background:rgba(5,7,12,.68);border:1px solid rgba(74,222,128,.22);border-radius:6px;padding:4px 6px}
    .eco-stat-label{font-size:9px;color:#bbf7d0}
    .eco-stat-val{font-size:11.5px;font-weight:800;color:#fff;margin-top:1px}
    .day-pills{display:grid;grid-template-columns:repeat(7,1fr);gap:4px;margin-bottom:7px}
    .day-pill{padding:5px 0;text-align:center;font-size:11.5px}
    .preset-select{width:100%;padding:6px 8px;border-radius:7px;border:1px solid var(--bd);background:var(--p2);color:var(--tx);font-size:11px;margin-bottom:7px;outline:none}
    .hour-row{display:flex;align-items:center;gap:8px}
    .hour-row input[type=range]{flex:1;accent-color:var(--ac);cursor:pointer}
    .hour-display{min-width:54px;text-align:center;font-variant-numeric:tabular-nums;font-weight:800;font-size:13.5px;background:var(--p2);border:1px solid var(--bd);padding:4px 6px;border-radius:6px}
    .kpi-strip{display:grid;grid-template-columns:1fr 1fr;gap:6px;margin-top:7px}
    .kpi-box{background:var(--p1);border:1px solid var(--bd);border-radius:7px;padding:5px 8px}
    .kpi-label{font-size:9.5px;color:var(--mu);font-weight:600}
    .kpi-val{font-size:12px;font-weight:800;color:#f8fafc;margin-top:1px}
    .lines-grid{display:grid;grid-template-columns:repeat(2,1fr);gap:5px;margin-top:7px}
    .line-card{display:flex;align-items:center;justify-content:space-between;padding:5px 6px;border-radius:8px;border:1px solid var(--bd);background:var(--p2);cursor:pointer;transition:all .14s ease;user-select:none}
    .line-card:hover{border-color:var(--ac);background:var(--p3)}
    .line-card.selected{background:#131c2e;border-color:#64748b}
    .line-card:not(.selected){background:#080b12;border-color:#1e293b;opacity:.42}
    .line-left{display:flex;align-items:center;gap:6px;min-width:0}
    .line-pill{min-width:26px;height:21px;padding:0 4px;border-radius:5px;display:inline-flex;align-items:center;justify-content:center;font-weight:900;font-size:11px;flex-shrink:0;border:1.5px solid rgba(255,255,255,.35)}
    .line-info{min-width:0;display:flex;flex-direction:column}
    .line-name{font-size:11px;font-weight:700;color:#f8fafc;white-space:nowrap;overflow:hidden;text-overflow:ellipsis}
    .line-sub{font-size:9px;color:#94a3b8;white-space:nowrap;overflow:hidden;text-overflow:ellipsis}
    .line-zoom{background:transparent;border:1px solid transparent;color:#94a3b8;padding:2px 5px;font-size:12px;border-radius:5px}
    .line-zoom:hover{background:rgba(255,255,255,.12);color:#fff;border-color:#475569}
    .search-row{display:flex;gap:6px;align-items:center}
    .search-input{flex:1;padding:7px 9px;border-radius:7px;border:1px solid var(--bd);background:var(--p2);color:var(--tx);font-size:12px;outline:none}
    .search-input:focus{border-color:var(--ac)}
    .stations-list{max-height:195px;overflow-y:auto;margin-top:7px;display:flex;flex-direction:column;gap:4px}
    .station-item{display:flex;align-items:center;justify-content:space-between;padding:5px 8px;border-radius:6px;background:var(--p1);font-size:11.5px;font-weight:600;color:#f1f5f9;cursor:pointer;border:1px solid #1e293b;gap:6px;transition:transform .12s ease,border-color .12s ease}
    .station-item:hover{border-color:var(--ac);background:var(--p3);transform:translateX(2px)}
    .station-item.selected-item{border-color:#38bdf8;background:#102542;box-shadow:inset 3px 0 0 #38bdf8}
    .station-dot{width:10px;height:10px;border-radius:50%;margin-right:6px;flex-shrink:0;border:1.5px solid rgba(255,255,255,.55)}
    .top10-tag{font-size:9px;font-weight:800;padding:1px 4px;border-radius:4px;background:rgba(245,158,11,.2);color:#fcd34d;border:1px solid rgba(251,191,36,.45);margin-left:4px}
    .renfe-tag{font-size:9px;font-weight:800;padding:1px 4px;border-radius:4px;background:#831843;color:#fbcfe8;border:1px solid #db2777;margin-left:4px}
    .pmr-mini{font-size:9px;font-weight:800;padding:1px 4px;border-radius:4px;margin-left:4px}
    .pmr-T{background:rgba(22,163,74,.22);color:#86efac;border:1px solid rgba(74,222,128,.45)}
    .pmr-P{background:rgba(245,158,11,.22);color:#fde68a;border:1px solid rgba(251,191,36,.45)}
    .pmr-N{background:rgba(220,38,38,.22);color:#fca5a5;border:1px solid rgba(248,113,113,.45)}
    .map-wrap{position:relative;grid-column:2;grid-row:1;min-height:300px;background:#cbd5e1;overflow:hidden}
    #map{width:100%;height:100%;background:#d5dde5}
    .map-overlay-topleft{position:absolute;top:12px;left:12px;z-index:800;display:flex;gap:6px;align-items:center}
    .map-btn-light{background:#0a0e17;color:#f8fafc;border:1px solid #334155;box-shadow:0 3px 10px rgba(0,0,0,.28)}
    .map-overlay-top{position:absolute;top:12px;right:12px;z-index:800;display:flex;flex-direction:column;gap:8px;align-items:flex-end;pointer-events:none}
    .map-card{pointer-events:auto;background:rgba(10,14,23,.93);backdrop-filter:blur(8px);border:1px solid #334155;border-radius:9px;padding:9px 12px;font-size:11.5px;color:#f1f5f9;box-shadow:0 6px 20px rgba(0,0,0,.35);max-width:280px}
    .legend-bar{width:205px;height:10px;border-radius:999px;background:linear-gradient(90deg,#0ea5e9 0%,#22c55e 28%,#eab308 54%,#f97316 78%,#dc2626 100%);margin:6px 0 4px;border:1px solid rgba(255,255,255,.22)}
    .legend-labels{display:flex;justify-content:space-between;font-size:10px;color:#cbd5e1;font-weight:600}
    .freq-legend-list{margin-top:6px;display:flex;flex-direction:column;gap:4px;font-size:11px}
    .freq-legend-row{display:flex;align-items:center;gap:7px;color:#e2e8f0}
    .freq-swatch{width:14px;height:5px;border-radius:3px;flex-shrink:0}
    .air-badge-icon{display:flex;align-items:center;justify-content:center;width:27px;height:27px;border-radius:7px;color:#fff;font-size:10px;font-weight:900;border:2px solid #fff;box-shadow:0 3px 10px rgba(0,0,0,.45)}
    .pulse-marker-wrap{position:relative;width:44px;height:44px;pointer-events:none}
    .pulse-ring{position:absolute;inset:0;border-radius:50%;border:3px solid #38bdf8;background:rgba(56,189,248,.18);animation:metroRipple 1.5s cubic-bezier(.16,1,.3,1) infinite}
    .pulse-ring.delay{border-color:#f59e0b;animation-delay:.55s}
    .pulse-core{position:absolute;top:50%;left:50%;width:14px;height:14px;transform:translate(-50%,-50%);border-radius:50%;background:#fff;border:3px solid #0284c7;box-shadow:0 0 12px rgba(56,189,248,.95)}
    @keyframes metroRipple{0%{transform:scale(.35);opacity:.95}100%{transform:scale(1.65);opacity:0}}
    .station-spotlight{position:absolute;left:16px;bottom:16px;z-index:850;width:435px;max-width:calc(100% - 32px);background:rgba(5,7,12,.96);backdrop-filter:blur(12px);border:1.5px solid #334155;border-radius:14px;padding:14px 16px;color:#f8fafc;box-shadow:0 18px 46px rgba(0,0,0,.62);opacity:0;transform:translateY(22px) scale(.96);pointer-events:none;transition:opacity .28s cubic-bezier(.16,1,.3,1),transform .32s cubic-bezier(.16,1,.3,1)}
    .station-spotlight.visible{opacity:1;transform:translateY(0) scale(1);pointer-events:auto}
    .spotlight-header{display:flex;justify-content:space-between;align-items:flex-start;gap:10px}
    .spotlight-title{font-size:19px;font-weight:900;margin:0;color:#fff;line-height:1.15}
    .spotlight-sub{font-size:11px;color:#94a3b8;margin-top:3px}
    .spotlight-actions{display:flex;gap:4px;flex-shrink:0}
    .spotlight-lines{display:flex;flex-wrap:wrap;gap:5px;margin:9px 0 8px}
    .spotlight-lines .line-pill{height:23px;min-width:29px;font-size:11.5px;cursor:pointer}
    .spotlight-badges{display:flex;flex-direction:column;gap:5px;margin-bottom:9px}
    .spotlight-grid{display:grid;grid-template-columns:repeat(3,1fr);gap:7px;margin-bottom:9px}
    .spot-kpi{background:#0f172a;border:1px solid #1e293b;border-radius:8px;padding:6px 8px}
    .spot-kpi-label{font-size:9px;color:#94a3b8;text-transform:uppercase;font-weight:700}
    .spot-kpi-val{font-size:13px;font-weight:800;color:#f8fafc;margin-top:2px}
    .spotlight-chart-wrap{background:#090d16;border:1px solid #1e293b;border-radius:9px;padding:7px 9px 5px}
    .spotlight-chart-title{font-size:10.5px;font-weight:700;color:#cbd5e1;margin-bottom:4px;display:flex;justify-content:space-between}
    .spotlight-chart-box{height:75px;position:relative}
    .chart-drawer{grid-column:2;grid-row:2;background:var(--bg);border-top:1px solid var(--bd);padding:9px 15px 11px;z-index:900}
    .chart-header{display:flex;flex-wrap:wrap;justify-content:space-between;align-items:center;gap:8px;margin-bottom:5px}
    .chart-title{font-size:12.5px;font-weight:800;color:#f8fafc}
    .chart-sub{font-size:10.5px;color:#94a3b8}
    .chart-container{position:relative;height:165px;width:100%}
    .chart-drawer.collapsed .chart-container{display:none}
    .leaflet-popup-content-wrapper{background:#0a0e17;color:#f8fafc;border:1px solid #334155;border-radius:10px}
    .leaflet-popup-tip{background:#0a0e17}
    .leaflet-container a.leaflet-popup-close-button{color:#94a3b8}
    .popup-renfe-box{padding:5px 8px;border-radius:7px;background:rgba(157,23,77,.25);border:1px solid #db2777;color:#fce7f3;font-size:11px}
    .popup-top10-box{padding:5px 8px;border-radius:7px;background:rgba(245,158,11,.16);border:1px solid rgba(251,191,36,.45);color:#fde68a;font-size:11px;font-weight:700}
    .popup-air-box{padding:5px 8px;border-radius:7px;background:rgba(34,197,94,.14);border:1px solid rgba(74,222,128,.4);color:#dcfce7;font-size:11px;line-height:1.35}
    .popup-pmr-box{padding:5px 8px;border-radius:7px;font-size:11px;font-weight:700;line-height:1.35}
    .popup-pmr-T{background:rgba(22,163,74,.22);border:1px solid #4ade80;color:#dcfce7}
    .popup-pmr-P{background:rgba(245,158,11,.2);border:1px solid #fbbf24;color:#fef3c7}
    .popup-pmr-N{background:rgba(220,38,38,.2);border:1px solid #f87171;color:#fee2e2}
    .modal-backdrop{display:none;position:fixed;inset:0;background:rgba(0,0,0,.72);z-index:2000;align-items:center;justify-content:center;padding:16px}
    .modal-backdrop.open{display:flex}
    .modal-box{background:#0a0e17;color:#f1f5f9;border-radius:12px;max-width:650px;width:100%;max-height:86vh;overflow-y:auto;padding:18px 20px;border:1px solid #334155}
    .modal-box h2{margin:0 0 10px;font-size:17px;color:#38bdf8}
    .modal-box h3{margin:12px 0 5px;font-size:13px;color:#f8fafc}
    .modal-box p,.modal-box li{font-size:12px;line-height:1.48;color:#cbd5e1}
    .tag-real{background:rgba(34,197,94,.2);color:#86efac;border:1px solid rgba(74,222,128,.4);font-weight:700;font-size:10px;padding:1px 6px;border-radius:4px}
    .tag-est{background:rgba(245,158,11,.2);color:#fde68a;border:1px solid rgba(251,191,36,.4);font-weight:700;font-size:10px;padding:1px 6px;border-radius:4px}
    .mobile-toggle{display:none}
    @media(max-width:920px){
      .app{grid-template-columns:1fr;grid-template-rows:1fr auto}
      .sidebar{position:fixed;top:0;left:0;bottom:0;width:335px;max-width:90vw;transform:translateX(-100%);transition:transform .22s ease}
      .sidebar.open{transform:translateX(0)}
      .map-wrap,.chart-drawer{grid-column:1}
      .mobile-toggle{display:inline-block}
      .station-spotlight{left:10px;right:10px;bottom:10px;width:auto;max-width:none}
    }
  </style>
</head>
<body>
<div class="app">
  <aside class="sidebar" id="sidebar">
    <div class="header">
      <div class="header-top">
        <h1><span class="metro-diamond">METRO</span> Red de Madrid</h1>
        <button class="btn-info" onclick="toggleModal(true)">Fuentes</button>
      </div>
      <p>Red oficial CRTM, accesibilidad PMR real, frecuencias, conexiones Renfe, demanda y Calidad del Aire de la Comunidad de Madrid.</p>
      <div class="badge-row">
        <span class="status-badge" id="crtmStatusBadge">CRTM M4_Lineas...</span>
        <span class="status-badge" id="pmrStatusBadge">PMR CRTM...</span>
        <span class="status-badge" id="airStatusBadge">Aire CAM/Madrid...</span>
      </div>
    </div>

    <div class="section">
      <div class="section-title">Visualización del mapa de Madrid</div>
      <div class="mode-grid">
        <button id="btnModeOfficial" class="active" onclick="setColorMode('official')">Colores Metro</button>
        <button id="btnModePmr" onclick="setColorMode('pmr')">♿ Accesibilidad PMR</button>
        <button id="btnModeFreq" onclick="setColorMode('freq')">Frecuencia Real</button>
        <button id="btnModeHeat" onclick="setColorMode('heat')">Demanda / Uso</button>
      </div>
      <div class="impact-box">
        <div class="eco-top">
          <span style="font-size:11px;font-weight:800;color:#86efac;">🌿 Impacto Ambiental y Social</span>
          <button id="btnToggleAir" class="small eco-active" onclick="toggleAirLayer()">Aire: ON</button>
        </div>
        <div class="eco-stats">
          <div class="eco-stat-item"><div class="eco-stat-label">Accesibles (T)</div><div class="eco-stat-val" id="pmrSummaryKpi">—</div></div>
          <div class="eco-stat-item"><div class="eco-stat-label">CO₂ evitado / año</div><div class="eco-stat-val" id="ecoCo2Year">—</div></div>
          <div class="eco-stat-item"><div class="eco-stat-label">Coches / día</div><div class="eco-stat-val" id="ecoCarsDay">—</div></div>
        </div>
      </div>
    </div>

    <div class="section">
      <div class="section-title">
        <span>Servicio y día de análisis</span>
        <span id="serviceStateTag" style="color:#4ade80;">En servicio</span>
      </div>
      <select id="realDayPreset" class="preset-select" onchange="applyRealDayPreset(this.value)">
        <option value="normal">Semana tipo 2025 (media oficial CRTM)</option>
        <option value="bf2025">Hito CRTM: Black Friday 28/11/2025 (2.819.020 viajes)</option>
        <option value="sat2025">Hito CRTM: Sábado récord 29/11/2025 (2.064.116 viajes)</option>
        <option value="sun2025">Hito CRTM: Domingo récord 07/12/2025 (1.620.360 viajes)</option>
      </select>
      <div class="day-pills" id="dayPills"></div>
      <div class="hour-row">
        <button id="btnPlay" class="small" onclick="togglePlay()">▶ Animar</button>
        <input type="range" id="hourSlider" min="0" max="23" value="8" oninput="setHour(this.value)" />
        <div class="hour-display" id="hourDisplay">08:00</div>
      </div>
      <div class="kpi-strip">
        <div class="kpi-box"><div class="kpi-label">Intervalo medio / trenes·h</div><div class="kpi-val" id="kpiHeadway">—</div></div>
        <div class="kpi-box"><div class="kpi-label">Demanda estimada (hora)</div><div class="kpi-val" id="hourlyNetworkLoad">—</div></div>
      </div>
    </div>

    <div class="section">
      <div class="section-title">
        <span>Líneas de Metro (L1–L12 + Ramal)</span>
        <span id="selectedCountLabel">13/13</span>
      </div>
      <div class="btn-group">
        <button class="small" onclick="selectAllLines()">Todas</button>
        <button class="small" onclick="clearLines()">Limpiar</button>
      </div>
      <div class="lines-grid" id="linesGrid"></div>
    </div>

    <div class="section" style="border-bottom:none; flex:1;">
      <div class="section-title">
        <span>Estaciones visibles (PMR, Top 10 y Renfe)</span>
        <span id="stationsCountLabel">0</span>
      </div>
      <div class="search-row">
        <input type="text" id="stationSearch" class="search-input" placeholder="Buscar parada o 'renfe'..." oninput="renderStationList()" />
        <button id="btnFilterPmr" class="small" title="Solo totalmente accesibles (T)" onclick="togglePmrOnlyFilter()">♿ Solo T</button>
      </div>
      <div class="stations-list" id="stationsList"></div>
    </div>
  </aside>

  <main class="map-wrap">
    <div class="map-overlay-topleft">
      <button class="mobile-toggle map-btn-light" onclick="document.getElementById('sidebar').classList.toggle('open')">☰ Controles</button>
      <button class="small map-btn-light" onclick="resetMadridView()">⌖ Centrar Madrid</button>
    </div>

    <div id="map"></div>

    <div class="map-overlay-top">
      <div class="map-card" id="legendCard">
        <div style="font-weight:800; font-size:12px;" id="legendTitle">Colores oficiales de Metro de Madrid</div>
        <div id="legendOfficialWrap" style="font-size:11px; color:#cbd5e1; margin-top:4px; line-height:1.4;">
          Trazado real (<span id="geomSourceTag">CRTM</span>) sobre mapa IGN/OSM. Blanco = transbordo. Rosa = Renfe. Dorado = Top 10 (2025).
        </div>
        <div id="legendPmrWrap" style="display:none;">
          <div style="font-size:10.5px; color:#94a3b8; margin-top:3px;">Campo oficial CRTM <code>GRADOACCESIBILIDAD</code>:</div>
          <div class="freq-legend-list">
            <div class="freq-legend-row"><span class="freq-swatch" style="background:#16a34a;height:9px;width:9px;border-radius:50%;"></span><span><strong>T · Totalmente accesible:</strong> ascensores calle–andén</span></div>
            <div class="freq-legend-row"><span class="freq-swatch" style="background:#f59e0b;height:9px;width:9px;border-radius:50%;"></span><span><strong>P · Parcialmente accesible:</strong> en parte de sus líneas</span></div>
            <div class="freq-legend-row"><span class="freq-swatch" style="background:#dc2626;height:9px;width:9px;border-radius:50%;"></span><span><strong>N · No accesible PMR:</strong> sin itinerario sin escaleras</span></div>
          </div>
        </div>
        <div id="legendFreqWrap" style="display:none;">
          <div style="font-size:10.5px; color:#94a3b8; margin-top:3px;">Mide <strong>cuánto esperas por un tren</strong>: en punta pasa rápido (verde).</div>
          <div class="freq-legend-list">
            <div class="freq-legend-row"><span class="freq-swatch" style="background:#16a34a;"></span><span><strong>2,5 – 4,5 min:</strong> Máxima (punta)</span></div>
            <div class="freq-legend-row"><span class="freq-swatch" style="background:#0284c7;"></span><span><strong>4,6 – 7,5 min:</strong> Media (valle/finde)</span></div>
            <div class="freq-legend-row"><span class="freq-swatch" style="background:#f59e0b;"></span><span><strong>7,6 – 10,5 min:</strong> Reducida</span></div>
            <div class="freq-legend-row"><span class="freq-swatch" style="background:#dc2626;"></span><span><strong>11 – 15 min:</strong> Mínima (noche)</span></div>
            <div class="freq-legend-row"><span class="freq-swatch" style="background:#64748b;"></span><span><strong>02:00 – 05:59:</strong> Cerrado</span></div>
          </div>
        </div>
        <div id="legendHeatWrap" style="display:none;">
          <div class="legend-bar"></div>
          <div class="legend-labels"><span>Poca gente</span><span>Media</span><span>Hora punta</span></div>
          <div style="font-size:10px; color:#94a3b8; margin-top:4px;">Ocupación calibrada con datos reales CRTM 2025 (736,9 M).</div>
        </div>
        <div id="legendAirWrap" style="margin-top:8px; padding-top:7px; border-top:1px solid #334155; font-size:10.5px;">
          <div style="font-weight:700; color:#86efac; margin-bottom:3px;">🌿 Cuadrados: Calidad del Aire (NO₂ µg/m³)</div>
          <div style="display:flex; gap:8px; flex-wrap:wrap; color:#cbd5e1;">
            <span><strong style="color:#16a34a;">■</strong> ≤20 Buena</span>
            <span><strong style="color:#65a30d;">■</strong> 21–40 Razonable</span>
            <span><strong style="color:#f59e0b;">■</strong> &gt;40 Alta</span>
          </div>
        </div>
      </div>
    </div>

    <div class="station-spotlight" id="stationSpotlight">
      <div class="spotlight-header">
        <div>
          <h2 class="spotlight-title" id="spotName">Estación</h2>
          <div class="spotlight-sub" id="spotSub">Detalle de parada</div>
        </div>
        <div class="spotlight-actions">
          <button class="small" id="btnSpotPrev" onclick="stepSelectedStation(-1)" style="display:none;">◀</button>
          <button class="small" id="btnSpotNext" onclick="stepSelectedStation(1)" style="display:none;">▶</button>
          <button class="small" onclick="centerSelectedStation()">⌖</button>
          <button class="small" onclick="closeStationCard()">✕</button>
        </div>
      </div>
      <div class="spotlight-lines" id="spotLines"></div>
      <div class="spotlight-badges" id="spotBadges"></div>
      <div class="spotlight-grid">
        <div class="spot-kpi"><div class="spot-kpi-label">Frecuencia</div><div class="spot-kpi-val" id="spotHeadway">—</div></div>
        <div class="spot-kpi"><div class="spot-kpi-label">Afluencia (hora)</div><div class="spot-kpi-val" id="spotPax">—</div></div>
        <div class="spot-kpi"><div class="spot-kpi-label">Ocupación rel.</div><div class="spot-kpi-val" id="spotLoad">—</div></div>
      </div>
      <div class="spotlight-chart-wrap">
        <div class="spotlight-chart-title">
          <span id="spotChartTitle">Perfil 24h</span>
          <span id="spotHourTag" style="color:#38bdf8;">08:00</span>
        </div>
        <div class="spotlight-chart-box"><canvas id="spotMiniChart"></canvas></div>
      </div>
    </div>
  </main>

  <section class="chart-drawer" id="chartDrawer">
    <div class="chart-header">
      <div>
        <div class="chart-title" id="chartTitle">Viajeros anuales oficiales (2019–2025) — Red completa</div>
        <div class="chart-sub" id="chartSub">Cifras oficiales de Metro de Madrid / CRTM (millones de viajes). Récord 2025: 736,87 M.</div>
      </div>
      <div class="btn-group">
        <button id="btnChartAnnual" class="small active" onclick="setChartMode('annual')">Anual (2019–2025)</button>
        <button id="btnChartRanking25" class="small" onclick="setChartMode('ranking25')">Líneas 2025</button>
        <button id="btnChartPmr" class="small" onclick="setChartMode('pmr')">Accesibilidad PMR</button>
        <button id="btnChartHourly" class="small" onclick="setChartMode('hourly')">Perfil 24h</button>
        <button id="btnChartCo2" class="small" onclick="setChartMode('co2')">CO₂ evitado</button>
        <button class="small" onclick="toggleChartDrawer()" id="btnToggleChart">Ocultar</button>
      </div>
    </div>
    <div class="chart-container"><canvas id="mainChart"></canvas></div>
  </section>
</div>

<div class="modal-backdrop" id="sourcesModal" onclick="if(event.target===this) toggleModal(false)">
  <div class="modal-box">
    <div style="display:flex; justify-content:space-between; align-items:center;">
      <h2>Metodología y Fuentes de Datos Abiertos</h2>
      <button class="small" onclick="toggleModal(false)">✕ Cerrar</button>
    </div>
    <p>Combina datos oficiales del <strong>CRTM</strong>, <strong>Comunidad de Madrid</strong> (<code>datos.comunidad.madrid</code>), <strong>Metro de Madrid</strong>, <strong>Ayuntamiento de Madrid</strong>, <strong>IGN</strong> y <strong>Renfe</strong>:</p>
    <h3>1. Datos oficiales integrados <span class="tag-real">DATO OFICIAL</span></h3>
    <ul>
      <li><strong>Accesibilidad PMR real (CRTM <code>M4_Estaciones</code> / <code>GRADOACCESIBILIDAD</code>):</strong> consulta en vivo el grado oficial de cada estación (<code>T</code> Totalmente accesible, <code>P</code> Parcial, <code>N</code> No accesible), con respaldo oficial de Metro de Madrid.</li>
      <li><strong>Geometría de red (CRTM <code>M4_Lineas</code> y <code>M4_Red</code>):</strong> tramos reales de vía y estaciones georreferenciadas en vivo.</li>
      <li><strong>Calidad del Aire (<code>datos.comunidad.madrid</code> + <code>ciudadesabiertas.madrid.es</code>):</strong> consulta en vivo de NO₂ (µg/m³) en 18 estaciones de la Comunidad de Madrid y capital.</li>
      <li><strong>Demanda oficial (2019–2025) e hitos diarios CRTM:</strong> 736,87 M viajes en 2025, ranking L1/L10/L3/L5/L6, Top 10 estaciones 2025 y récords diarios (Black Friday 28/11/2025: 2.819.020 viajes).</li>
      <li><strong>Correspondencias Renfe Cercanías Madrid:</strong> 24 estaciones oficiales (C-1 a C-10), incluida El Casar (C-3).</li>
    </ul>
    <h3>2. Indicadores derivados <span class="tag-est">ESTIMACIÓN CALIBRADA</span></h3>
    <ul>
      <li><strong>CO₂ evitado:</strong> desplazamiento urbano medio de <code>5,5 km</code> × <code>135 g CO₂/km</code> × <code>38%</code> sustitución de coche privado = <code>0,282 kg CO₂</code> ahorrado por viaje en Metro.</li>
      <li><strong>Desglose horario por parada:</strong> calibrado con demanda oficial CRTM, intervalos por franja horaria y peso Top 10.</li>
    </ul>
  </div>
</div>

<script src="https://unpkg.com/leaflet@1.9.4/dist/leaflet.js" crossorigin=""></script>
<script src="https://cdn.jsdelivr.net/npm/chart.js@4.4.3/dist/chart.umd.min.js" crossorigin=""></script>
<script>
const OFFICIAL_TOTAL_ANNUAL_M={2019:677.4,2020:349.8,2021:442.3,2022:571.0,2023:662.3,2024:715.2,2025:736.9};
const KG_CO2_SAVED_PER_TRIP=0.28215;

const TOP_10_STATIONS_2025={
  "Sol":{rank:1,note:"21,96 M entradas oficiales (récord 2025)"},"Moncloa":{rank:2,note:"Top 2 oficial 2025"},
  "Nuevos Ministerios":{rank:3,note:"Top 3 oficial 2025"},"Plaza de Castilla":{rank:4,note:"Top 4 oficial 2025"},
  "Príncipe Pío":{rank:5,note:"Top 5 oficial 2025"},"Avenida de América":{rank:6,note:"Top 6 oficial 2025"},
  "Plaza de España":{rank:7,note:"Top 7 oficial 2025"},"Atocha":{rank:8,note:"Top 8 oficial 2025"},
  "Legazpi":{rank:9,note:"Top 9 oficial 2025"},"Argüelles":{rank:10,note:"Top 10 oficial 2025"}
};

const RENFE_CONNECTIONS={
  "Chamartín":{lines:"C-1, C-2, C-3, C-4, C-7, C-8, C-10",extra:"AVE / Larga y Media Distancia"},
  "Nuevos Ministerios":{lines:"C-1, C-2, C-3, C-4, C-7, C-8, C-10"},"Sol":{lines:"C-3, C-4"},
  "Atocha":{lines:"C-1, C-2, C-3, C-4, C-5, C-7, C-8, C-10",extra:"AVE / Larga y Media Distancia"},
  "Príncipe Pío":{lines:"C-1, C-7, C-10",extra:"Media Distancia"},"Embajadores":{lines:"C-5"},
  "Méndez Álvaro":{lines:"C-1, C-5, C-10"},"Pirámides":{lines:"C-1, C-10"},"Laguna":{lines:"C-5"},
  "Aluche":{lines:"C-5"},"Cuatro Vientos":{lines:"C-5"},"Villaverde Alto":{lines:"C-4, C-5"},"El Casar":{lines:"C-3"},
  "Sierra de Guadalupe":{lines:"C-2, C-7, C-8",renfeName:"Vallecas"},"Puerta de Arganda":{lines:"C-2, C-7, C-8",renfeName:"Vicálvaro"},
  "Coslada Central":{lines:"C-2, C-7, C-8",renfeName:"Coslada"},"Aeropuerto T4":{lines:"C-1, C-10"},"Pitis":{lines:"C-7, C-8"},
  "Paco de Lucía":{lines:"C-7, C-8",renfeName:"Mirasierra-Paco de Lucía"},"Alcorcón Central":{lines:"C-5",renfeName:"Alcorcón"},
  "Móstoles Central":{lines:"C-5",renfeName:"Móstoles"},"Fuenlabrada Central":{lines:"C-5",renfeName:"Fuenlabrada"},
  "Getafe Central":{lines:"C-4",renfeName:"Getafe Centro"},"Leganés Central":{lines:"C-5",renfeName:"Leganés"}
};

const PMR_N=new Set(["Valdeacederas","Tetuán","Estrecho","Alvarado","Ríos Rosas","Tirso de Molina","Antón Martín","Estación del Arte","Puente de Vallecas","Nueva Numancia","Buenos Aires","Retiro","Banco de España","Santo Domingo","Noviciado","San Bernardo","Quevedo","Colón","Serrano","Velázquez","Lista","Prosperidad","Alfonso XIII","Avenida de la Paz","Arturo Soria","Esperanza","Canillas","San Lorenzo","Parque de Santa María","Hortaleza","Manoteras","Canillejas","Torre Arias","Suanzes","Quintana","El Carmen","Rubén Darío","Chueca","La Latina","Puerta de Toledo","Acacias","Pirámides","Marqués de Vadillo","Urgel","Vista Alegre","Carabanchel","Eugenia de Montijo","Empalme","Campamento","Laguna","Carpetana","Opañel","Usera","Arganzuela-Planetario","Méndez Álvaro","O'Donnell","República Argentina","Vicente Aleixandre","Ciudad Universitaria","Puerta del Ángel","Alto de Extremadura","Lucero","Las Musas","San Blas","Simancas","García Noblejas","Ascao","Parque de las Avenidas","Cartagena","Alonso Cano","Islas Filipinas","Francos Rodríguez","Valdezarza","Antonio Machado","Peñagrande","Avenida de la Ilustración","Lacoma","Herrera Oria","Barrio del Pilar","Ventilla","Duque de Pastrana","Pío XII","Concha Espina","Cruz del Rayo","Ibiza","Estrella","Vinateros","Artilleros","Pavones","Valdebernardo","Vicálvaro","San Cipriano","Begoña","Cuzco","Santiago Bernabéu"]);
const PMR_P=new Set(["Manuel Becerra","Diego de León","Avenida de América","Oporto","Ventas","Núñez de Balboa","Alonso Martínez","Guzmán el Bueno","Príncipe de Vergara","Canal","Pueblo Nuevo"]);

function getStationPMR(n){
  const m=STATION_META[n];
  if(m&&m.accesibilidad){const v=String(m.accesibilidad).trim().toUpperCase();if(v==='T'||v==='P'||v==='N')return v;}
  return PMR_N.has(n)?'N':(PMR_P.has(n)?'P':'T');
}
function pmrInfo(c){
  if(c==='T')return{code:'T',color:'#16a34a',label:'Totalmente accesible (T)',short:'T · Accesible'};
  if(c==='P')return{code:'P',color:'#f59e0b',label:'Parcialmente accesible (P)',short:'P · Parcial'};
  return{code:'N',color:'#dc2626',label:'No accesible PMR (N)',short:'N · No accesible'};
}

const AIR_QUALITY_STATIONS=[
  {id:"28065014",name:"Getafe",red:"Comunidad de Madrid",tipo:"Tráfico",lat:40.3047,lng:-3.7294,no2:26,pm10:19,metroNear:"Getafe Central"},
  {id:"28074007",name:"Leganés",red:"Comunidad de Madrid",tipo:"Tráfico",lat:40.3282,lng:-3.7589,no2:24,pm10:18,metroNear:"Casa del Reloj"},
  {id:"28007004",name:"Alcorcón",red:"Comunidad de Madrid",tipo:"Fondo urbano",lat:40.3435,lng:-3.8295,no2:20,pm10:16,metroNear:"Alcorcón Central"},
  {id:"28092005",name:"Móstoles",red:"Comunidad de Madrid",tipo:"Fondo urbano",lat:40.3242,lng:-3.8768,no2:19,pm10:15,metroNear:"Móstoles Central"},
  {id:"28058004",name:"Fuenlabrada",red:"Comunidad de Madrid",tipo:"Industrial",lat:40.2855,lng:-3.8012,no2:21,pm10:18,metroNear:"Fuenlabrada Central"},
  {id:"28049003",name:"Coslada",red:"Comunidad de Madrid",tipo:"Tráfico",lat:40.4239,lng:-3.5535,no2:27,pm10:20,metroNear:"Coslada Central"},
  {id:"28006004",name:"Alcobendas",red:"Comunidad de Madrid",tipo:"Industrial",lat:40.5414,lng:-3.6435,no2:22,pm10:17,metroNear:"Marqués de la Valdavia"},
  {id:"28123002",name:"Rivas-Vaciamadrid",red:"Comunidad de Madrid",tipo:"Fondo urbano",lat:40.3519,lng:-3.5414,no2:16,pm10:14,metroNear:"Rivas Futura"},
  {id:"28014002",name:"Arganda del Rey",red:"Comunidad de Madrid",tipo:"Industrial",lat:40.3008,lng:-3.4408,no2:18,pm10:16,metroNear:"Arganda del Rey"},
  {id:"28079056",name:"Plaza Elíptica",red:"Ayto. de Madrid",tipo:"Tráfico",lat:40.3850,lng:-3.7188,no2:34,pm10:21,metroNear:"Plaza Elíptica"},
  {id:"28079008",name:"Escuelas Aguirre",red:"Ayto. de Madrid",tipo:"Tráfico",lat:40.4216,lng:-3.6823,no2:28,pm10:18,metroNear:"Retiro"},
  {id:"28079038",name:"Cuatro Caminos",red:"Ayto. de Madrid",tipo:"Tráfico",lat:40.4455,lng:-3.7071,no2:25,pm10:17,metroNear:"Cuatro Caminos"},
  {id:"28079047",name:"Méndez Álvaro",red:"Ayto. de Madrid",tipo:"Fondo urbano",lat:40.3981,lng:-3.6868,no2:23,pm10:16,metroNear:"Méndez Álvaro"},
  {id:"28079004",name:"Plaza de España",red:"Ayto. de Madrid",tipo:"Tráfico",lat:40.4239,lng:-3.7123,no2:29,pm10:18,metroNear:"Plaza de España"},
  {id:"28079040",name:"Vallecas",red:"Ayto. de Madrid",tipo:"Fondo urbano",lat:40.3881,lng:-3.6515,no2:22,pm10:17,metroNear:"Alto del Arenal"},
  {id:"28079024",name:"Casa de Campo",red:"Ayto. de Madrid",tipo:"Suburbana",lat:40.4193,lng:-3.7473,no2:12,pm10:11,metroNear:"Casa de Campo"},
  {id:"28079027",name:"Barajas Pueblo",red:"Ayto. de Madrid",tipo:"Fondo urbano",lat:40.4769,lng:-3.5800,no2:21,pm10:16,metroNear:"Barajas"},
  {id:"28079057",name:"Sanchinarro",red:"Ayto. de Madrid",tipo:"Fondo urbano",lat:40.4942,lng:-3.6605,no2:18,pm10:15,metroNear:"Las Tablas"}
];

const LINES_META=[
  {id:'L1',code:'1',name:'Pinar de Chamartín - Valdecarros',color:'#2CA6E0',textColor:'#fff',pax2025M:112.66,official2025:true,shareNormal:.145,circular:false,freqTier:'trunk'},
  {id:'L2',code:'2',name:'Las Rosas - Cuatro Caminos',color:'#E51923',textColor:'#fff',pax2025M:43.20,official2025:false,shareNormal:.056,circular:false,freqTier:'urban'},
  {id:'L3',code:'3',name:'El Casar - Moncloa',color:'#FFD100',textColor:'#111827',pax2025M:83.00,official2025:true,shareNormal:.105,circular:false,freqTier:'trunk'},
  {id:'L4',code:'4',name:'Argüelles - Pinar de Chamartín',color:'#A85928',textColor:'#fff',pax2025M:47.10,official2025:false,shareNormal:.062,circular:false,freqTier:'urban'},
  {id:'L5',code:'5',name:'Alameda de Osuna - Casa de Campo',color:'#82C341',textColor:'#111827',pax2025M:82.50,official2025:true,shareNormal:.107,circular:false,freqTier:'trunk'},
  {id:'L6',code:'6',name:'Circular',color:'#73787E',textColor:'#fff',pax2025M:76.00,official2025:true,shareNormal:.162,circular:true,freqTier:'trunk'},
  {id:'L7',code:'7',name:'Hospital del Henares - Pitis',color:'#F28C22',textColor:'#111827',pax2025M:41.50,official2025:false,shareNormal:.054,circular:false,freqTier:'urban'},
  {id:'L8',code:'8',name:'Nuevos Ministerios - Aeropuerto T4',color:'#E36DA7',textColor:'#111827',pax2025M:18.40,official2025:false,shareNormal:.024,circular:false,freqTier:'airport'},
  {id:'L9',code:'9',name:'Paco de Lucía - Arganda del Rey',color:'#9B26B6',textColor:'#fff',pax2025M:45.20,official2025:false,shareNormal:.059,circular:false,freqTier:'urban'},
  {id:'L10',code:'10',name:'Hosp. Infanta Sofía - Puerta del Sur',color:'#004B87',textColor:'#fff',pax2025M:91.60,official2025:true,shareNormal:.120,circular:false,freqTier:'trunk'},
  {id:'L11',code:'11',name:'Plaza Elíptica - La Fortuna',color:'#008542',textColor:'#fff',pax2025M:9.80,official2025:false,shareNormal:.013,circular:false,freqTier:'feeder'},
  {id:'L12',code:'12',name:'MetroSur (Circular Sur)',color:'#A39121',textColor:'#fff',pax2025M:39.10,official2025:false,shareNormal:.051,circular:true,freqTier:'metrosur'},
  {id:'R',code:'R',name:'Ramal Ópera - Príncipe Pío',color:'#1D4ED8',textColor:'#fff',pax2025M:6.81,official2025:false,shareNormal:.009,circular:false,freqTier:'ramal'}
];
(function(){
  const t=736.87,f=LINES_META.filter(l=>l.official2025).reduce((a,b)=>a+b.pax2025M,0),r=t-f,c=LINES_META.filter(l=>!l.official2025).reduce((a,b)=>a+b.pax2025M,0);
  LINES_META.forEach(l=>{if(!l.official2025)l.pax2025M=Number(((l.pax2025M/c)*r).toFixed(2));l.share2025=l.pax2025M/t;});
})();

const STATIONS={
  "Pinar de Chamartín":[40.4801,-3.6668,1.15],"Bambú":[40.4767,-3.6763,.55],"Chamartín":[40.4721,-3.6825,1.55],
  "Plaza de Castilla":[40.4669,-3.6892,1.78],"Valdeacederas":[40.4639,-3.6952,.7],"Tetuán":[40.4602,-3.6983,.75],
  "Estrecho":[40.4539,-3.7026,.8],"Alvarado":[40.4501,-3.7034,.7],"Cuatro Caminos":[40.4468,-3.7038,1.52],
  "Ríos Rosas":[40.4419,-3.7016,.8],"Iglesia":[40.4348,-3.6986,.8],"Bilbao":[40.4291,-3.7022,1.25],
  "Tribunal":[40.4262,-3.7011,1.38],"Gran Vía":[40.4201,-3.7017,1.58],"Sol":[40.4169,-3.7035,2.25],
  "Tirso de Molina":[40.4123,-3.7046,.9],"Antón Martín":[40.4122,-3.6993,.85],"Estación del Arte":[40.4087,-3.6927,1.1],
  "Atocha":[40.4063,-3.6895,1.66],"Menéndez Pelayo":[40.4044,-3.6805,.8],"Pacífico":[40.4014,-3.6752,1.35],
  "Puente de Vallecas":[40.3979,-3.6693,1.05],"Nueva Numancia":[40.3951,-3.6629,.85],"Portazgo":[40.3931,-3.6572,.75],
  "Buenos Aires":[40.3915,-3.6526,.7],"Alto del Arenal":[40.3899,-3.6451,.7],"Miguel Hernández":[40.3875,-3.6406,.75],
  "Sierra de Guadalupe":[40.3822,-3.6249,.9],"Villa de Vallecas":[40.3795,-3.6213,.75],"Congosto":[40.3724,-3.6186,.6],
  "La Gavia":[40.3692,-3.6124,.55],"Las Suertes":[40.3634,-3.5997,.65],"Valdecarros":[40.3599,-3.5929,.6],
  "Las Rosas":[40.424,-3.6037,.65],"Avenida de Guadalajara":[40.422,-3.612,.6],"Alsacia":[40.4184,-3.6234,.75],
  "La Almudena":[40.4237,-3.6391,.55],"La Elipa":[40.4267,-3.6502,.7],"Ventas":[40.4311,-3.6634,1.25],
  "Manuel Becerra":[40.4281,-3.6693,1.3],"Goya":[40.4245,-3.6758,1.4],"Príncipe de Vergara":[40.4231,-3.6801,1.1],
  "Retiro":[40.4212,-3.6859,.85],"Banco de España":[40.4192,-3.6951,1.05],"Sevilla":[40.4181,-3.6989,.95],
  "Ópera":[40.4179,-3.7093,1.42],"Santo Domingo":[40.4216,-3.7082,.8],"Noviciado":[40.4251,-3.7076,.85],
  "San Bernardo":[40.4301,-3.7058,1.15],"Quevedo":[40.4337,-3.7048,.9],"Canal":[40.4384,-3.7043,1.2],
  "Moncloa":[40.4347,-3.7192,1.95],"Argüelles":[40.4306,-3.716,1.56],"Ventura Rodríguez":[40.4272,-3.7137,.75],
  "Plaza de España":[40.4235,-3.7116,1.68],"Callao":[40.4201,-3.7059,1.55],"Lavapiés":[40.4086,-3.7009,.95],
  "Embajadores":[40.405,-3.7026,1.4],"Palos de la Frontera":[40.4031,-3.6946,.85],"Delicias":[40.3983,-3.6941,.9],
  "Legazpi":[40.3913,-3.695,1.6],"Almendrales":[40.3834,-3.6983,.75],"Hospital 12 de Octubre":[40.3752,-3.696,.95],
  "San Fermín-Orcasur":[40.3698,-3.6952,.65],"Ciudad de los Ángeles":[40.3601,-3.6938,.75],"Villaverde Bajo-Cruce":[40.3507,-3.6926,.85],
  "San Cristóbal":[40.3422,-3.6934,.6],"Villaverde Alto":[40.3413,-3.7118,.95],"El Casar":[40.3186,-3.7102,1.08],
  "Alonso Martínez":[40.4275,-3.6958,1.48],"Colón":[40.4254,-3.6909,.95],"Serrano":[40.4252,-3.6867,.85],
  "Velázquez":[40.4252,-3.6836,.85],"Lista":[40.4293,-3.6754,.8],"Diego de León":[40.4349,-3.6746,1.48],
  "Avenida de América":[40.4384,-3.6766,1.72],"Prosperidad":[40.4441,-3.6747,.85],"Alfonso XIII":[40.4481,-3.6681,.8],
  "Avenida de la Paz":[40.4539,-3.6619,.75],"Arturo Soria":[40.4576,-3.6563,.8],"Esperanza":[40.4593,-3.6455,.65],
  "Canillas":[40.4641,-3.6357,.65],"Mar de Cristal":[40.4692,-3.6389,1.25],"San Lorenzo":[40.4744,-3.6396,.6],
  "Parque de Santa María":[40.477,-3.6454,.55],"Hortaleza":[40.4754,-3.6531,.6],"Manoteras":[40.4771,-3.6626,.55],
  "Alameda de Osuna":[40.4576,-3.5877,.65],"El Capricho":[40.4535,-3.5955,.55],"Canillejas":[40.4489,-3.6087,1.05],
  "Torre Arias":[40.4442,-3.6172,.7],"Suanzes":[40.4408,-3.627,.85],"Ciudad Lineal":[40.438,-3.6377,1.1],
  "Pueblo Nuevo":[40.4357,-3.6426,1.25],"Quintana":[40.4337,-3.6476,.85],"El Carmen":[40.4317,-3.6569,.75],
  "Núñez de Balboa":[40.4328,-3.6844,1.25],"Rubén Darío":[40.4332,-3.689,.8],"Chueca":[40.4226,-3.6976,.9],
  "La Latina":[40.4112,-3.7082,.95],"Puerta de Toledo":[40.4067,-3.7116,.8],"Acacias":[40.4041,-3.7069,.95],
  "Pirámides":[40.4017,-3.7115,1],"Marqués de Vadillo":[40.3979,-3.7166,.9],"Urgel":[40.3935,-3.7229,.8],
  "Oporto":[40.3883,-3.7315,1.35],"Vista Alegre":[40.3889,-3.7399,.8],"Carabanchel":[40.3879,-3.7453,.85],
  "Eugenia de Montijo":[40.3853,-3.7534,.7],"Aluche":[40.3854,-3.761,1.3],"Empalme":[40.3911,-3.767,.65],
  "Campamento":[40.3948,-3.7681,.7],"Casa de Campo":[40.4033,-3.7611,1.15],"Laguna":[40.3989,-3.7443,1.05],
  "Carpetana":[40.3924,-3.7412,.95],"Opañel":[40.3895,-3.7235,.8],"Plaza Elíptica":[40.3849,-3.7186,1.48],
  "Usera":[40.3869,-3.7073,1.05],"Arganzuela-Planetario":[40.3933,-3.6887,.75],"Méndez Álvaro":[40.3954,-3.6776,1.45],
  "Conde de Casal":[40.4071,-3.6708,1.2],"Sainz de Baranda":[40.4153,-3.6696,1.3],"O'Donnell":[40.4231,-3.6687,1.05],
  "República Argentina":[40.4443,-3.6854,.9],"Nuevos Ministerios":[40.4463,-3.692,1.88],"Guzmán el Bueno":[40.4461,-3.7124,1.35],
  "Vicente Aleixandre":[40.4459,-3.7198,.9],"Ciudad Universitaria":[40.4437,-3.7266,1.45],"Príncipe Pío":[40.421,-3.7204,1.75],
  "Puerta del Ángel":[40.4139,-3.7273,.95],"Alto de Extremadura":[40.4096,-3.7389,.85],"Lucero":[40.405,-3.7454,.85],
  "Hospital del Henares":[40.4177,-3.5082,.45],"Henares":[40.4177,-3.5269,.4],"Jarama":[40.4222,-3.5331,.4],
  "San Fernando":[40.4241,-3.5419,.55],"La Rambla":[40.4245,-3.5494,.55],"Coslada Central":[40.4236,-3.5621,.75],
  "Barrio del Puerto":[40.4226,-3.5692,.55],"Estadio Metropolitano":[40.4331,-3.5994,.9],"Las Musas":[40.4331,-3.6079,.65],
  "San Blas":[40.4281,-3.6152,.75],"Simancas":[40.4281,-3.6258,.75],"García Noblejas":[40.429,-3.6331,.8],
  "Ascao":[40.4301,-3.6409,.75],"Barrio de la Concepción":[40.4384,-3.6522,.8],"Parque de las Avenidas":[40.4394,-3.6626,.75],
  "Cartagena":[40.4393,-3.6716,.75],"Gregorio Marañón":[40.4384,-3.6914,1.35],"Alonso Cano":[40.4385,-3.6992,.75],
  "Islas Filipinas":[40.4389,-3.7136,.85],"Francos Rodríguez":[40.4567,-3.7121,.75],"Valdezarza":[40.4646,-3.7161,.65],
  "Antonio Machado":[40.4701,-3.7183,.65],"Peñagrande":[40.4761,-3.7164,.7],"Avenida de la Ilustración":[40.4795,-3.7185,.75],
  "Lacoma":[40.4847,-3.7244,.6],"Arroyofresno":[40.491,-3.7259,.5],"Pitis":[40.4945,-3.7267,.65],
  "Colombia":[40.4563,-3.6768,1.15],"Pinar del Rey":[40.4678,-3.6486,.65],"Feria de Madrid":[40.4638,-3.6164,.9],
  "Aeropuerto T1-T2-T3":[40.4686,-3.5696,1],"Barajas":[40.4757,-3.5829,.65],"Aeropuerto T4":[40.4916,-3.5933,1.05],
  "Paco de Lucía":[40.4998,-3.7102,.75],"Mirasierra":[40.4911,-3.7145,.6],"Herrera Oria":[40.4844,-3.7082,.75],
  "Barrio del Pilar":[40.4773,-3.7037,.85],"Ventilla":[40.4701,-3.6956,.75],"Duque de Pastrana":[40.4673,-3.679,.65],
  "Pío XII":[40.4623,-3.6759,.75],"Concha Espina":[40.4525,-3.6751,.75],"Cruz del Rayo":[40.4454,-3.6778,.7],
  "Ibiza":[40.4184,-3.6736,.85],"Estrella":[40.4112,-3.6615,.75],"Vinateros":[40.4102,-3.6526,.75],
  "Artilleros":[40.4063,-3.6442,.65],"Pavones":[40.4009,-3.6353,.8],"Valdebernardo":[40.3999,-3.6213,.65],
  "Vicálvaro":[40.4044,-3.6096,.7],"San Cipriano":[40.4041,-3.5979,.6],"Puerta de Arganda":[40.4014,-3.596,.85],
  "Rivas Urbanizaciones":[40.3652,-3.5471,.65],"Rivas Futura":[40.3461,-3.5219,.65],"Rivas Vaciamadrid":[40.3284,-3.5194,.55],
  "La Poveda":[40.3193,-3.4748,.45],"Arganda del Rey":[40.3036,-3.4472,.65],"Hospital Infanta Sofía":[40.5594,-3.6114,.6],
  "Reyes Católicos":[40.5504,-3.6231,.6],"Baunatal":[40.5539,-3.635,.55],"Manuel de Falla":[40.5504,-3.6467,.55],
  "Marqués de la Valdavia":[40.5416,-3.6373,.65],"La Moraleja":[40.532,-3.6354,.7],"La Granja":[40.5256,-3.6584,.55],
  "Ronda de la Comunicación":[40.5159,-3.6626,.85],"Las Tablas":[40.5082,-3.6693,.85],"Montecarmelo":[40.5055,-3.6956,.7],
  "Tres Olivos":[40.5008,-3.6952,.9],"Fuencarral":[40.4944,-3.6929,.75],"Begoña":[40.4796,-3.6862,1.05],
  "Cuzco":[40.4586,-3.6901,1.05],"Santiago Bernabéu":[40.4521,-3.6908,1.25],"Lago":[40.4163,-3.7351,.6],
  "Batán":[40.4079,-3.7532,.75],"Colonia Jardín":[40.3969,-3.7747,1.05],"Aviación Española":[40.3836,-3.7834,.6],
  "Cuatro Vientos":[40.377,-3.7916,.85],"Joaquín Vilumbrales":[40.3499,-3.8071,.6],"Puerta del Sur":[40.3452,-3.8123,1.3],
  "Abrantes":[40.3809,-3.7279,.6],"Pan Bendito":[40.3759,-3.734,.55],"San Francisco":[40.3732,-3.7393,.55],
  "Carabanchel Alto":[40.3698,-3.751,.65],"La Peseta":[40.3642,-3.7568,.6],"La Fortuna":[40.3586,-3.7804,.55],
  "Parque Lisboa":[40.3497,-3.8206,.55],"Alcorcón Central":[40.3499,-3.8316,.9],"Parque Oeste":[40.3456,-3.8492,.55],
  "Universidad Rey Juan Carlos":[40.3352,-3.8726,.85],"Móstoles Central":[40.3287,-3.8634,.95],"Pradillo":[40.3235,-3.8648,.6],
  "Hospital de Móstoles":[40.3177,-3.8672,.65],"Manuela Malasaña":[40.3095,-3.8639,.55],"Loranca":[40.2966,-3.8388,.6],
  "Hospital de Fuenlabrada":[40.2874,-3.8165,.65],"Parque Europa":[40.2845,-3.8053,.55],"Fuenlabrada Central":[40.2827,-3.7988,.9],
  "Parque de los Estados":[40.2856,-3.7885,.6],"Arroyo Culebro":[40.2896,-3.7568,.45],"Conservatorio":[40.2937,-3.7457,.5],
  "Alonso de Mendoza":[40.3005,-3.7364,.55],"Getafe Central":[40.3099,-3.7335,.95],"Juan de la Cierva":[40.3119,-3.7235,.65],
  "Los Espartales":[40.3247,-3.7186,.55],"El Bercial":[40.3324,-3.7306,.55],"El Carrascal":[40.3364,-3.7485,.6],
  "Julián Besteiro":[40.3354,-3.7582,.55],"Casa del Reloj":[40.3302,-3.7604,.6],"Hospital Severo Ochoa":[40.3226,-3.7686,.65],
  "Leganés Central":[40.3292,-3.7717,.9],"San Nicasio":[40.3355,-3.7764,.6]
};

const LINE_STATIONS={
  L1:["Pinar de Chamartín","Bambú","Chamartín","Plaza de Castilla","Valdeacederas","Tetuán","Estrecho","Alvarado","Cuatro Caminos","Ríos Rosas","Iglesia","Bilbao","Tribunal","Gran Vía","Sol","Tirso de Molina","Antón Martín","Estación del Arte","Atocha","Menéndez Pelayo","Pacífico","Puente de Vallecas","Nueva Numancia","Portazgo","Buenos Aires","Alto del Arenal","Miguel Hernández","Sierra de Guadalupe","Villa de Vallecas","Congosto","La Gavia","Las Suertes","Valdecarros"],
  L2:["Las Rosas","Avenida de Guadalajara","Alsacia","La Almudena","La Elipa","Ventas","Manuel Becerra","Goya","Príncipe de Vergara","Retiro","Banco de España","Sevilla","Sol","Ópera","Santo Domingo","Noviciado","San Bernardo","Quevedo","Canal","Cuatro Caminos"],
  L3:["Moncloa","Argüelles","Ventura Rodríguez","Plaza de España","Callao","Sol","Lavapiés","Embajadores","Palos de la Frontera","Delicias","Legazpi","Almendrales","Hospital 12 de Octubre","San Fermín-Orcasur","Ciudad de los Ángeles","Villaverde Bajo-Cruce","San Cristóbal","Villaverde Alto","El Casar"],
  L4:["Argüelles","San Bernardo","Bilbao","Alonso Martínez","Colón","Serrano","Velázquez","Goya","Lista","Diego de León","Avenida de América","Prosperidad","Alfonso XIII","Avenida de la Paz","Arturo Soria","Esperanza","Canillas","Mar de Cristal","San Lorenzo","Parque de Santa María","Hortaleza","Manoteras","Pinar de Chamartín"],
  L5:["Alameda de Osuna","El Capricho","Canillejas","Torre Arias","Suanzes","Ciudad Lineal","Pueblo Nuevo","Quintana","El Carmen","Ventas","Diego de León","Núñez de Balboa","Rubén Darío","Alonso Martínez","Chueca","Gran Vía","Callao","Ópera","La Latina","Puerta de Toledo","Acacias","Pirámides","Marqués de Vadillo","Urgel","Oporto","Vista Alegre","Carabanchel","Eugenia de Montijo","Aluche","Empalme","Campamento","Casa de Campo"],
  L6:["Laguna","Carpetana","Oporto","Opañel","Plaza Elíptica","Usera","Legazpi","Arganzuela-Planetario","Méndez Álvaro","Pacífico","Conde de Casal","Sainz de Baranda","O'Donnell","Manuel Becerra","Diego de León","Avenida de América","República Argentina","Nuevos Ministerios","Cuatro Caminos","Guzmán el Bueno","Vicente Aleixandre","Ciudad Universitaria","Moncloa","Argüelles","Príncipe Pío","Puerta del Ángel","Alto de Extremadura","Lucero"],
  L7:["Hospital del Henares","Henares","Jarama","San Fernando","La Rambla","Coslada Central","Barrio del Puerto","Estadio Metropolitano","Las Musas","San Blas","Simancas","García Noblejas","Ascao","Pueblo Nuevo","Barrio de la Concepción","Parque de las Avenidas","Cartagena","Avenida de América","Gregorio Marañón","Alonso Cano","Canal","Islas Filipinas","Guzmán el Bueno","Francos Rodríguez","Valdezarza","Antonio Machado","Peñagrande","Avenida de la Ilustración","Lacoma","Arroyofresno","Pitis"],
  L8:["Nuevos Ministerios","Colombia","Pinar del Rey","Mar de Cristal","Feria de Madrid","Aeropuerto T1-T2-T3","Barajas","Aeropuerto T4"],
  L9:["Paco de Lucía","Mirasierra","Herrera Oria","Barrio del Pilar","Ventilla","Plaza de Castilla","Duque de Pastrana","Pío XII","Colombia","Concha Espina","Cruz del Rayo","Avenida de América","Núñez de Balboa","Príncipe de Vergara","Ibiza","Sainz de Baranda","Estrella","Vinateros","Artilleros","Pavones","Valdebernardo","Vicálvaro","San Cipriano","Puerta de Arganda","Rivas Urbanizaciones","Rivas Futura","Rivas Vaciamadrid","La Poveda","Arganda del Rey"],
  L10:["Hospital Infanta Sofía","Reyes Católicos","Baunatal","Manuel de Falla","Marqués de la Valdavia","La Moraleja","La Granja","Ronda de la Comunicación","Las Tablas","Montecarmelo","Tres Olivos","Fuencarral","Begoña","Chamartín","Plaza de Castilla","Cuzco","Santiago Bernabéu","Nuevos Ministerios","Gregorio Marañón","Alonso Martínez","Tribunal","Plaza de España","Príncipe Pío","Lago","Batán","Casa de Campo","Colonia Jardín","Aviación Española","Cuatro Vientos","Joaquín Vilumbrales","Puerta del Sur"],
  L11:["Plaza Elíptica","Abrantes","Pan Bendito","San Francisco","Carabanchel Alto","La Peseta","La Fortuna"],
  L12:["Puerta del Sur","Parque Lisboa","Alcorcón Central","Parque Oeste","Universidad Rey Juan Carlos","Móstoles Central","Pradillo","Hospital de Móstoles","Manuela Malasaña","Loranca","Hospital de Fuenlabrada","Parque Europa","Fuenlabrada Central","Parque de los Estados","Arroyo Culebro","Conservatorio","Alonso de Mendoza","Getafe Central","Juan de la Cierva","El Casar","Los Espartales","El Bercial","El Carrascal","Julián Besteiro","Casa del Reloj","Hospital Severo Ochoa","Leganés Central","San Nicasio"],
  R:["Ópera","Príncipe Pío"]
};

const STATION_LINES={};
Object.entries(LINE_STATIONS).forEach(([lid,list])=>{list.forEach(n=>{if(!STATION_LINES[n])STATION_LINES[n]=[];if(!STATION_LINES[n].includes(lid))STATION_LINES[n].push(lid);});});
const REAL_CRTM_TRACKS={},STATION_META={};

const DAYS=[
  {id:0,short:'L',name:'Lunes',factor:.99,profile:'weekday',avgDailyM:2.38},
  {id:1,short:'M',name:'Martes',factor:1.03,profile:'weekday',avgDailyM:2.48},
  {id:2,short:'X',name:'Miércoles',factor:1.04,profile:'weekday',avgDailyM:2.51},
  {id:3,short:'J',name:'Jueves',factor:1.05,profile:'weekday',avgDailyM:2.53},
  {id:4,short:'V',name:'Viernes',factor:1.09,profile:'friday',avgDailyM:2.63},
  {id:5,short:'S',name:'Sábado',factor:.71,profile:'saturday',avgDailyM:1.71},
  {id:6,short:'D',name:'Domingo',factor:.51,profile:'sunday',avgDailyM:1.23}
];
const HOURLY_PROFILES={
  weekday:[.09,.04,0,0,0,0,.24,.68,1,.84,.56,.5,.54,.64,.78,.72,.62,.74,.9,.88,.66,.44,.28,.16],
  friday:[.11,.05,0,0,0,0,.22,.64,.95,.8,.56,.52,.58,.7,.86,.78,.68,.78,.92,.94,.8,.62,.44,.26],
  saturday:[.19,.09,0,0,0,0,.08,.16,.26,.38,.52,.66,.76,.82,.78,.64,.66,.76,.84,.88,.8,.64,.46,.32],
  sunday:[.19,.09,0,0,0,0,.06,.12,.18,.28,.4,.54,.66,.7,.64,.56,.62,.72,.8,.82,.68,.48,.3,.16]
};

function isMetroClosed(h){return h>=2&&h<=5;}
function getLineHeadwayMinutes(lid,dIdx,h){
  if(isMetroClosed(h))return null;
  const m=LINES_META.find(l=>l.id===lid),t=m?m.freqTier:'urban',p=DAYS[dIdx].profile,wk=(p==='saturday'||p==='sunday');
  if(h===0||h===1)return t==='ramal'?12:15;
  if(h===23)return t==='trunk'?10.5:13;
  if(h===22)return t==='trunk'?7.5:10;
  if(h===6)return wk?(t==='trunk'?7.5:9.5):(t==='trunk'?5.5:(t==='ramal'?5:8));
  if(wk){
    if(t==='trunk')return(h>=11&&h<=21)?5.5:7;
    if(t==='urban')return(h>=11&&h<=21)?7:8.5;
    if(t==='metrosur')return(h>=11&&h<=21)?7.5:9.5;
    if(t==='airport')return 9;
    if(t==='ramal')return 7.5;
    return 8.5;
  }
  const pA=(h>=7&&h<=9),pP=(h>=18&&h<=20),md=(h>=13&&h<=15);
  if(t==='trunk')return pA?(lid==='L6'||lid==='L1'?2.8:3.4):pP?3.8:md?4.8:5.8;
  if(t==='urban')return pA?4.2:pP?4.8:md?6:7;
  if(t==='metrosur')return pA?5.5:pP?6:7.5;
  if(t==='airport')return(h>=8&&h<=20)?6.5:8.5;
  if(t==='ramal')return(pA||pP)?5:6.5;
  return(pA||pP)?6:8;
}
function headwayToColor(mins,cl){if(cl||mins===null)return'#64748b';if(mins<=4.5)return'#16a34a';if(mins<=7.5)return'#0284c7';if(mins<=10.5)return'#f59e0b';return'#dc2626';}
function getEffectiveDailyM(dIdx){return state.dayOverrideM!==null?state.dayOverrideM:DAYS[dIdx].avgDailyM;}
function getLineIntensity(lid,dIdx,h){
  if(isMetroClosed(h))return 0;
  const l=LINES_META.find(x=>x.id===lid),d=DAYS[dIdx],hw=HOURLY_PROFILES[d.profile][h]||0,sc=getEffectiveDailyM(dIdx)/2.42;
  let lf=l.share2025/.145;
  if(lid==='L8'&&h>=10&&h<=17)lf*=1.35;
  if(lid==='L12'&&(h===7||h===8||h===18))lf*=1.25;
  return Math.max(.04,Math.min(1,hw*sc*(.45+.55*Math.min(1.2,lf))));
}
function getStationIntensity(st,act,dIdx,h){
  if(isMetroClosed(h))return 0;
  const lh=(STATION_LINES[st]||[]).filter(l=>act.includes(l));
  if(!lh.length)return 0;
  const mx=Math.max(...lh.map(l=>getLineIntensity(l,dIdx,h))),w=STATIONS[st]?STATIONS[st][2]:1;
  return Math.max(.05,Math.min(1,mx*(.58+.3*w)*(1+(lh.length-1)*.11)));
}
function getStationHourlyPassengers(st,act,dIdx,h){if(isMetroClosed(h))return 0;const i=getStationIntensity(st,act,dIdx,h),w=STATIONS[st]?STATIONS[st][2]:1;return Math.round(i*w*4950);}
function getLineHourlyPassengers(lid,dIdx,h){
  if(isMetroClosed(h))return 0;
  const l=LINES_META.find(x=>x.id===lid),d=DAYS[dIdx],hw=HOURLY_PROFILES[d.profile][h]||0,dt=getEffectiveDailyM(dIdx)*1e6*l.share2025,sh=HOURLY_PROFILES[d.profile].reduce((a,b)=>a+b,0);
  return Math.round((dt*hw)/sh);
}
function intensityToColor(v,cl){if(cl||v<=0)return'#64748b';if(v<.22)return'#0ea5e9';if(v<.42)return'#22c55e';if(v<.62)return'#eab308';if(v<.8)return'#f97316';return'#dc2626';}
function getAnnualSeriesForLines(sel){
  const yrs=[2019,2020,2021,2022,2023,2024,2025];
  if(sel.length===LINES_META.length)return yrs.map(y=>OFFICIAL_TOTAL_ANNUAL_M[y]);
  return yrs.map(y=>{
    const nt=OFFICIAL_TOTAL_ANNUAL_M[y];let sum=0;
    sel.forEach(id=>{const m=LINES_META.find(l=>l.id===id);if(!m)return;if(y===2025)sum+=m.pax2025M;else{let s=m.shareNormal;if(y===2020&&id==='L8')s*=.6;if(y===2024&&id==='L6')s=116.05/715.2;sum+=nt*s;}});
    return Number(sum.toFixed(1));
  });
}

let state={selectedLines:LINES_META.map(l=>l.id),colorMode:'official',dayIdx:2,dayOverrideM:null,hour:8,chartMode:'annual',playing:false,playTimer:null,selectedStation:null,showAirLayer:true,onlyPmrAccessible:false};
const MADRID_BOUNDS=L.latLngBounds([40.24,-3.96],[40.60,-3.38]);
const map=L.map('map',{center:[40.421,-3.695],zoom:12,minZoom:11,maxZoom:17,maxBounds:MADRID_BOUNDS,maxBoundsViscosity:1.0,zoomControl:false});
L.control.zoom({position:'bottomright'}).addTo(map);
const ignLayer=L.tileLayer('https://tms-ign-base.idee.es/1.0.0/IGNBaseTodo/{z}/{x}/{-y}.jpeg',{attribution:'&copy; IGN España | CRTM / Comunidad de Madrid / Renfe',maxZoom:18});
const osmFallbackLayer=L.tileLayer('https://tile.openstreetmap.org/{z}/{x}/{y}.png',{attribution:'&copy; OpenStreetMap | CRTM / Comunidad de Madrid / Renfe',maxZoom:18});
let switchedToOsm=false;
ignLayer.on('tileerror',()=>{if(!switchedToOsm){switchedToOsm=true;map.removeLayer(ignLayer);osmFallbackLayer.addTo(map);osmFallbackLayer.bringToBack();}});
ignLayer.addTo(map);

const casingLayerGroup=L.layerGroup().addTo(map),linesLayerGroup=L.layerGroup().addTo(map),stationsLayerGroup=L.layerGroup().addTo(map),airLayerGroup=L.layerGroup().addTo(map),pulseLayerGroup=L.layerGroup().addTo(map);
function resetMadridView(){map.setView([40.421,-3.695],12,{animate:true});}

async function tryLoadAirQualityOpenData(){
  const badge=document.getElementById('airStatusBadge');let live=0;
  try{
    const[camData,madData]=await Promise.all([
      fetch('https://datos.comunidad.madrid/api/3/action/package_show?id=calidad_aire_datos_dia').then(r=>r.ok?r.json():null).catch(()=>null),
      fetch('https://ciudadesabiertas.madrid.es/dynamicAPI/API/query/calair_tiemporeal_ult.json?pageSize=2000').then(r=>r.ok?r.json():null).catch(()=>null)
    ]);
    if(madData&&Array.isArray(madData.records)){
      const mp={};
      madData.records.forEach(rec=>{
        if(String(rec.MAGNITUD)==='8'||String(rec.magnitud)==='8'){
          const code=String(rec.PUNTO_MUESTREO||rec.punto_muestreo||'').slice(0,8);
          for(let h=24;h>=1;h--){const kv='V'+String(h).padStart(2,'0'),kh='H'+String(h).padStart(2,'0');if((rec[kv]==='V'||rec[kv]==='v')&&rec[kh]!=null){mp[code]=Math.round(Number(rec[kh]));break;}}
        }
      });
      AIR_QUALITY_STATIONS.forEach(st=>{if(mp[st.id]!=null&&!isNaN(mp[st.id])){st.no2=mp[st.id];st.live=true;live++;}});
    }
    if(camData&&camData.success&&camData.result&&camData.result.resources){
      const jr=camData.result.resources.find(r=>(r.format||'').toUpperCase()==='JSON');
      if(jr&&jr.url){try{const rc=await fetch(jr.url);if(rc.ok){const cj=await rc.json(),rows=Array.isArray(cj)?cj:(cj.data||[]);rows.forEach(row=>{if(String(row.magnitud)==='8'){const sc=String(row.estacion||'').slice(0,8),st=AIR_QUALITY_STATIONS.find(x=>x.id===sc||x.id.endsWith(String(row.estacion)));if(st){for(let h=24;h>=1;h--){const v=row['v'+String(h).padStart(2,'0')],val=row['h'+String(h).padStart(2,'0')];if((v==='V'||v==='v')&&val!=null){st.no2=Math.round(Number(val));st.live=true;live++;break;}}}}});}}catch(_){}}
    }
  }catch(_){}
  if(live>0){badge.textContent=`Aire en vivo (${live} est.)`;badge.className='status-badge live';}
  else{badge.textContent='Aire CAM/Madrid (18 est.)';badge.className='status-badge datamad';}
  renderAirLayer();
}

function mapCrtmLayerNameToLineId(name){
  const n=(name||'').toUpperCase();
  if(n.includes('L10'))return'L10';if(n.includes('L11'))return'L11';if(n.includes('L12'))return'L12';
  if(n.includes('L1'))return'L1';if(n.includes('L2'))return'L2';if(n.includes('L3'))return'L3';
  if(n.includes('L4'))return'L4';if(n.includes('L5'))return'L5';if(n.includes('L6'))return'L6';
  if(n.includes('L7'))return'L7';if(n.includes('L8'))return'L8';if(n.includes('L9'))return'L9';
  if(n.includes('_LR_')||n.includes('RAMAL')||n.includes('_R_'))return'R';
  return null;
}
function findNearestStationName(lat,lng,list){let b=null,bd=Infinity;list.forEach(n=>{const c=STATIONS[n];if(!c)return;const d=(c[0]-lat)**2+(c[1]-lng)**2;if(d<bd){bd=d;b=n;}});return b;}
function mergeStationAccessibility(ex,inc){const v=String(inc||'').trim().toUpperCase();if(v!=='T'&&v!=='P'&&v!=='N')return ex||null;if(!ex)return v;if(ex===v)return ex;return'P';}

async function tryLoadCRTMOpenData(){
  const badge=document.getElementById('crtmStatusBadge'),pmrBadge=document.getElementById('pmrStatusBadge'),geomTag=document.getElementById('geomSourceTag');
  const baseUrl='https://services5.arcgis.com/UxADft6QPcvFyDU1/arcgis/rest/services/M4_Lineas/FeatureServer';
  const redUrl='https://services5.arcgis.com/UxADft6QPcvFyDU1/arcgis/rest/services/M4_Red/FeatureServer';
  const norm=s=>(s||'').toLowerCase().normalize('NFD').replace(/[\u0300-\u036f]/g,'').replace(/[^a-z0-9]/g,'');
  const lookup={};Object.keys(STATIONS).forEach(k=>{lookup[norm(k)]=k;});
  let pmrLiveCount=0;

  fetch(`${redUrl}?f=json`).then(r=>r.ok?r.json():null).then(async rm=>{
    let eid=0;if(rm&&Array.isArray(rm.layers)){const f=rm.layers.find(l=>/ESTACION/i.test(l.name));if(f)eid=f.id;}
    const r=await fetch(`${redUrl}/${eid}/query?where=1%3D1&outFields=DENOMINACION,GRADOACCESIBILIDAD,CORONATARIFARIA,CODIGOESTACION,SITUACIONCALLE&outSR=4326&f=json`);
    if(!r.ok)return;const d=await r.json();
    (d.features||[]).forEach(f=>{
      const p=f.attributes||{},m=lookup[norm(p.DENOMINACION||p.denominacion||'')];
      if(m){const cur=STATION_META[m]||{},acc=mergeStationAccessibility(cur.accesibilidad,p.GRADOACCESIBILIDAD);if(acc)pmrLiveCount++;STATION_META[m]=Object.assign(cur,{codigo:p.CODIGOESTACION||cur.codigo||null,corona:p.CORONATARIFARIA||cur.corona||null,calle:p.SITUACIONCALLE||cur.calle||null,accesibilidad:acc,pmrLive:Boolean(acc)});}
    });
    if(pmrLiveCount>0){pmrBadge.textContent='PMR CRTM en vivo';pmrBadge.className='status-badge live';renderMap();}
  }).catch(()=>{pmrBadge.textContent='PMR oficial integrado';pmrBadge.className='status-badge datamad';});

  try{
    const ctrl=new AbortController(),tm=setTimeout(()=>ctrl.abort(),7000);
    const mr=await fetch(`${baseUrl}?f=json`,{signal:ctrl.signal});clearTimeout(tm);
    if(!mr.ok)throw new Error(mr.status);
    const meta=await mr.json();if(!meta||!Array.isArray(meta.layers))throw new Error('no layers');
    const tramos=meta.layers.filter(l=>/_S1_TRAMO$/i.test(l.name)),ests=meta.layers.filter(l=>/_S1_ESTACION$/i.test(l.name));
    let estUpd=0;
    await Promise.allSettled(ests.map(async l=>{
      const r=await fetch(`${baseUrl}/${l.id}/query?where=1%3D1&outFields=*&outSR=4326&f=geojson`);if(!r.ok)return;
      const g=await r.json();
      (g.features||[]).forEach(f=>{
        if(!f.geometry||!f.geometry.coordinates)return;
        const[lng,lat]=f.geometry.coordinates,p=f.properties||{},m=lookup[norm(p.DENOMINACION||p.denominacion||p.NOMBRE||'')];
        if(m&&lat>40.1&&lat<40.7&&lng>-4.1&&lng<-3.3){
          STATIONS[m][0]=lat;STATIONS[m][1]=lng;
          const cur=STATION_META[m]||{},acc=mergeStationAccessibility(cur.accesibilidad,p.GRADOACCESIBILIDAD);if(acc)pmrLiveCount++;
          STATION_META[m]=Object.assign(cur,{codigo:p.CODIGOESTACION||cur.codigo||null,corona:p.CORONATARIFARIA||cur.corona||null,accesibilidad:acc||cur.accesibilidad||null,pmrLive:Boolean(acc||cur.pmrLive)});
          estUpd++;
        }
      });
    }));
    let trk=0;
    await Promise.allSettled(tramos.map(async l=>{
      const lid=mapCrtmLayerNameToLineId(l.name);if(!lid)return;
      const r=await fetch(`${baseUrl}/${l.id}/query?where=1%3D1&outFields=*&outSR=4326&f=geojson`);if(!r.ok)return;
      const g=await r.json(),feats=g.features||[];if(!feats.length)return;
      if(!REAL_CRTM_TRACKS[lid])REAL_CRTM_TRACKS[lid]=[];
      const ls=LINE_STATIONS[lid]||Object.keys(STATIONS);
      feats.forEach(f=>{
        if(!f.geometry)return;
        const pts=f.geometry.type==='MultiLineString'?f.geometry.coordinates:(f.geometry.type==='LineString'?[f.geometry.coordinates]:[]);
        pts.forEach(arr=>{if(!arr||arr.length<2)return;const ll=arr.map(pt=>[pt[1],pt[0]]);REAL_CRTM_TRACKS[lid].push({coords:ll,a:findNearestStationName(ll[0][0],ll[0][1],ls),b:findNearestStationName(ll[ll.length-1][0],ll[ll.length-1][1],ls)});trk++;});
      });
    }));
    if(trk>0||estUpd>0){
      badge.textContent=`CRTM en vivo (${trk} tramos)`;badge.className='status-badge live';geomTag.textContent='CRTM M4_Lineas en vivo';
      if(pmrLiveCount>0){pmrBadge.textContent='PMR CRTM en vivo';pmrBadge.className='status-badge live';}
      renderMap();
    }else throw new Error('empty');
  }catch(_){
    badge.textContent='CRTM M4 integrado';geomTag.textContent='CRTM integrado';
    if(pmrLiveCount===0){pmrBadge.textContent='PMR oficial integrado';pmrBadge.className='status-badge datamad';}
  }
}

function getAirColorAndLabel(n){if(n<=20)return{color:'#16a34a',label:'Buena'};if(n<=40)return{color:'#65a30d',label:'Razonablemente buena'};if(n<=60)return{color:'#f59e0b',label:'Regular'};return{color:'#dc2626',label:'Desfavorable'};}
function toggleAirLayer(){state.showAirLayer=!state.showAirLayer;const b=document.getElementById('btnToggleAir');b.textContent=state.showAirLayer?'Aire: ON':'Aire: OFF';b.classList.toggle('eco-active',state.showAirLayer);document.getElementById('legendAirWrap').style.display=state.showAirLayer?'block':'none';renderAirLayer();}
function renderAirLayer(){
  airLayerGroup.clearLayers();if(!state.showAirLayer)return;
  AIR_QUALITY_STATIONS.forEach(st=>{
    const info=getAirColorAndLabel(st.no2),icon=L.divIcon({className:'',iconSize:[27,27],iconAnchor:[13,13],html:`<div class="air-badge-icon" style="background:${info.color};">${st.no2}</div>`});
    const m=L.marker([st.lat,st.lng],{icon,zIndexOffset:300});
    m.bindTooltip(`🌿 Aire ${st.name}: ${st.no2} µg/m³ NO₂ (${info.label})`,{direction:'top',offset:[0,-10]});
    m.bindPopup(`<div style="min-width:220px;"><div style="font-size:14px;font-weight:800;color:#86efac;">🌿 Estación Aire: ${st.name}</div><div style="font-size:11px;color:#94a3b8;margin-bottom:6px;">${st.red} · ${st.tipo}</div><div class="popup-air-box"><strong>NO₂:</strong> ${st.no2} µg/m³ (${info.label})<br/><strong>PM10:</strong> ~${st.pm10} µg/m³ · <strong>Fuente:</strong> ${st.live?'API en vivo':'Ref. CAM/Madrid'}</div><div style="font-size:11.5px;color:#cbd5e1;margin-top:6px;">Metro cercano: <strong style="color:#38bdf8;cursor:pointer;text-decoration:underline;" onclick="selectStation('${st.metroNear}',true)">${st.metroNear}</strong></div></div>`);
    m.addTo(airLayerGroup);
  });
}
function getNearestAirStation(lat,lng){let b=null,bd=Infinity;AIR_QUALITY_STATIONS.forEach(s=>{const d=(s.lat-lat)**2+(s.lng-lng)**2;if(d<bd){bd=d;b=s;}});return b;}

let spotChartInstance=null;
function selectStation(stName,fly=true){
  const c=STATIONS[stName];if(!c)return;state.selectedStation=stName;
  pulseLayerGroup.clearLayers();
  L.marker([c[0],c[1]],{icon:L.divIcon({className:'',iconSize:[44,44],iconAnchor:[22,22],html:'<div class="pulse-marker-wrap"><div class="pulse-ring"></div><div class="pulse-ring delay"></div><div class="pulse-core"></div></div>'}),interactive:false,zIndexOffset:900}).addTo(pulseLayerGroup);
  if(fly)map.flyTo([c[0]-.008,c[1]],Math.max(map.getZoom(),13),{duration:.65,easeLinearity:.25});
  const card=document.getElementById('stationSpotlight');card.classList.remove('visible');void card.offsetWidth;updateStationCardContent();card.classList.add('visible');renderStationList();
}
function centerSelectedStation(){if(!state.selectedStation)return;const c=STATIONS[state.selectedStation];if(c)map.flyTo([c[0]-.005,c[1]],14,{duration:.55});}
function stepSelectedStation(delta){if(!state.selectedStation||state.selectedLines.length!==1)return;const l=LINE_STATIONS[state.selectedLines[0]]||[],i=l.indexOf(state.selectedStation);if(i!==-1)selectStation(l[(i+delta+l.length)%l.length],true);}
function closeStationCard(){state.selectedStation=null;pulseLayerGroup.clearLayers();document.getElementById('stationSpotlight').classList.remove('visible');renderStationList();}
document.addEventListener('keydown',e=>{if(e.key==='Escape'&&state.selectedStation)closeStationCard();});

function updateStationCardContent(){
  const st=state.selectedStation;if(!st)return;
  const c=STATIONS[st],all=STATION_LINES[st]||[],act=all.filter(l=>state.selectedLines.includes(l)),ev=act.length?act:all,cl=isMetroClosed(state.hour);
  const sInt=getStationIntensity(st,ev,state.dayIdx,state.hour),sPax=getStationHourlyPassengers(st,ev,state.dayIdx,state.hour);
  const hws=ev.map(l=>getLineHeadwayMinutes(l,state.dayIdx,state.hour)).filter(x=>x!==null),bhw=hws.length?Math.min(...hws):null;
  const top10=TOP_10_STATIONS_2025[st],renfe=RENFE_CONNECTIONS[st],meta=STATION_META[st],air=c?getNearestAirStation(c[0],c[1]):null,pmr=pmrInfo(getStationPMR(st));

  document.getElementById('spotName').textContent=st;
  const sub=[`${DAYS[state.dayIdx].name}, ${String(state.hour).padStart(2,'0')}:00`];
  if(meta&&meta.calle)sub.push(meta.calle);if(meta&&meta.corona)sub.push(`Zona ${meta.corona}`);if(meta&&meta.codigo)sub.push(`Cód. CRTM ${meta.codigo}`);
  document.getElementById('spotSub').textContent=sub.join(' · ');

  const ss=state.selectedLines.length===1&&(LINE_STATIONS[state.selectedLines[0]]||[]).includes(st);
  document.getElementById('btnSpotPrev').style.display=ss?'inline-block':'none';
  document.getElementById('btnSpotNext').style.display=ss?'inline-block':'none';

  document.getElementById('spotLines').innerHTML=all.map(lid=>{const m=LINES_META.find(x=>x.id===lid);return`<span class="line-pill" style="background:${m.color};color:${m.textColor};" title="Ver solo Línea ${m.code}" onclick="focusLine('${lid}')">${m.code}</span>`;}).join('');
  const b=[];
  b.push(`<div class="popup-pmr-box popup-pmr-${pmr.code}">♿ Accesibilidad PMR: ${pmr.label} · <span style="font-weight:500;opacity:.9;">${meta&&meta.pmrLive?'CRTM M4_Red en vivo':'Oficial CRTM/Metro'}</span></div>`);
  if(renfe)b.push(`<div class="popup-renfe-box"><strong>🚆 Renfe Cercanías${renfe.renfeName?` (${renfe.renfeName})`:''}:</strong> ${renfe.lines}${renfe.extra?` · <span style="color:#fbcfe8;">${renfe.extra}</span>`:''}</div>`);
  if(top10)b.push(`<div class="popup-top10-box">★ #${top10.rank} de la red en 2025 · ${top10.note}</div>`);
  if(air){const ai=getAirColorAndLabel(air.no2);b.push(`<div class="popup-air-box">🌿 <strong>Aire cercano (${air.name}):</strong> ${air.no2} µg/m³ NO₂ · <strong>${ai.label}</strong></div>`);}
  document.getElementById('spotBadges').innerHTML=b.join('');

  document.getElementById('spotHeadway').textContent=(cl||bhw===null)?'Cerrado':`~${bhw.toFixed(1)} min`;
  document.getElementById('spotPax').textContent=cl?'0 / h':`~${sPax.toLocaleString('es-ES')} / h`;
  document.getElementById('spotLoad').textContent=cl?'0%':`${Math.round(sInt*100)}%`;
  document.getElementById('spotChartTitle').textContent=`Afluencia 24h en ${st} (${DAYS[state.dayIdx].name})`;
  document.getElementById('spotHourTag').textContent=`${String(state.hour).padStart(2,'0')}:00`;

  const hrs=Array.from({length:24},(_,h)=>`${String(h).padStart(2,'0')}h`),vals=hrs.map((_,h)=>getStationHourlyPassengers(st,ev,state.dayIdx,h));
  const ctx=document.getElementById('spotMiniChart').getContext('2d');if(spotChartInstance)spotChartInstance.destroy();
  spotChartInstance=new Chart(ctx,{type:'bar',data:{labels:hrs,datasets:[{data:vals,backgroundColor:vals.map((_,h)=>h===state.hour?'#E51923':'rgba(56,189,248,.72)'),borderRadius:3}]},options:{responsive:true,maintainAspectRatio:false,animation:{duration:380},plugins:{legend:{display:false},tooltip:{callbacks:{label:c=>`~${c.parsed.y.toLocaleString('es-ES')} viajeros/h`}}},scales:{x:{ticks:{color:'#94a3b8',font:{size:9},maxTicksLimit:8},grid:{display:false}},y:{beginAtZero:true,ticks:{color:'#64748b',font:{size:9},maxTicksLimit:3,callback:v=>v>=1000?(v/1000).toFixed(0)+'k':v},grid:{color:'rgba(148,163,184,.12)'}}}}});
}

function renderMap(){
  casingLayerGroup.clearLayers();linesLayerGroup.clearLayers();stationsLayerGroup.clearLayers();
  const cl=isMetroClosed(state.hour),act=state.selectedLines;

  act.forEach(lid=>{
    const m=LINES_META.find(l=>l.id===lid),list=LINE_STATIONS[lid];if(!m||!list||list.length<2)return;
    const hw=getLineHeadwayMinutes(lid,state.dayIdx,state.hour),tph=hw?Math.round((60/hw)*10)/10:0,lp=getLineHourlyPassengers(lid,state.dayIdx,state.hour);
    let segs=[];
    if(REAL_CRTM_TRACKS[lid]&&REAL_CRTM_TRACKS[lid].length>0)segs=REAL_CRTM_TRACKS[lid];
    else{
      for(let i=0;i<list.length-1;i++){const ca=STATIONS[list[i]],cb=STATIONS[list[i+1]];if(ca&&cb)segs.push({coords:[[ca[0],ca[1]],[cb[0],cb[1]]],a:list[i],b:list[i+1]});}
      if(m.circular){const ca=STATIONS[list[list.length-1]],cb=STATIONS[list[0]];if(ca&&cb)segs.push({coords:[[ca[0],ca[1]],[cb[0],cb[1]]],a:list[list.length-1],b:list[0]});}
    }
    segs.forEach(s=>{
      const si=(s.a&&s.b)?(getStationIntensity(s.a,[lid],state.dayIdx,state.hour)+getStationIntensity(s.b,[lid],state.dayIdx,state.hour))/2:getLineIntensity(lid,state.dayIdx,state.hour);
      let sc,wt,op=cl?.7:.98;
      if(state.colorMode==='official'){sc=m.color;wt=5.5;}
      else if(state.colorMode==='pmr'){sc='#475569';wt=3.8;op=.55;}
      else if(state.colorMode==='freq'){sc=headwayToColor(hw,cl);wt=cl?3.5:(hw<=4.5?6.5:hw<=7.5?5.2:4.2);}
      else{sc=intensityToColor(si,cl);wt=cl?3.5:4.2+si*3.8;}
      const cc=(state.colorMode==='official'&&(lid==='L3'||lid==='L5'))?'#1e293b':'#ffffff';
      L.polyline(s.coords,{color:cc,weight:wt+3,opacity:state.colorMode==='pmr'?.45:(cl?.45:.94),lineJoin:'round',lineCap:'round',interactive:false}).addTo(casingLayerGroup);
      const poly=L.polyline(s.coords,{color:sc,weight:wt,opacity:op,lineJoin:'round',lineCap:'round'});
      const sl=(s.a&&s.b&&s.a!==s.b)?`${s.a} ↔ ${s.b}`:m.name;
      poly.bindTooltip(`<div style="font-size:12px;"><strong>Línea ${m.code} (${sl})</strong><br/>`+(cl?'Servicio cerrado (02:00–05:59)':state.colorMode==='freq'?`Pasa 1 tren cada <strong>${hw.toFixed(1)} min</strong> (~${tph} trenes/h)`:`Demanda estimada: ~${lp.toLocaleString('es-ES')} viajeros/h · Intervalo ~${hw.toFixed(1)} min`)+`</div>`,{sticky:true});
      poly.addTo(linesLayerGroup);
    });
  });

  const vis=new Set();act.forEach(lid=>(LINE_STATIONS[lid]||[]).forEach(st=>vis.add(st)));
  vis.forEach(st=>{
    const c=STATIONS[st];if(!c)return;
    const pc=getStationPMR(st);if(state.onlyPmrAccessible&&pc!=='T')return;
    const lh=(STATION_LINES[st]||[]).filter(l=>act.includes(l)),all=STATION_LINES[st]||[],inter=all.length>1,t10=TOP_10_STATIONS_2025[st],rn=RENFE_CONNECTIONS[st],si=getStationIntensity(st,act,state.dayIdx,state.hour);
    const hws=lh.map(l=>getLineHeadwayMinutes(l,state.dayIdx,state.hour)).filter(x=>x!==null),bhw=hws.length?Math.min(...hws):null,pmr=pmrInfo(pc);
    let fc,bc,rad;
    if(state.colorMode==='official'){const pm=LINES_META.find(l=>l.id===lh[0]);fc=inter?'#fff':(pm?pm.color:'#004B87');bc=t10?'#f59e0b':(rn?'#db2777':(inter?'#0f172a':'#fff'));rad=t10?8:(inter||rn?6.8:4.8);}
    else if(state.colorMode==='pmr'){fc=pmr.color;bc='#fff';rad=inter?7.6:6;}
    else if(state.colorMode==='freq'){fc=headwayToColor(bhw,cl);bc=rn?'#db2777':(inter?'#0f172a':'#fff');rad=cl?4.2:(inter||rn?7.2:5.2);}
    else{fc=intensityToColor(si,cl);bc=t10?'#f59e0b':(rn?'#db2777':(inter?'#0f172a':'#fff'));rad=cl?4.2:(inter||rn?6.4+si*3:4.5+si*2.5);}
    const mk=L.circleMarker([c[0],c[1]],{radius:rad,fillColor:fc,color:bc,weight:state.colorMode==='pmr'?2.2:(t10||rn?2.6:(inter?2.1:1.5)),fillOpacity:.96});
    const ex=[state.colorMode==='pmr'?`♿ ${pmr.short}`:'',t10?`★ Top ${t10.rank}`:'',rn?`🚆 Renfe ${rn.lines}`:''].filter(Boolean).join(' · ');
    mk.bindTooltip(`${st}${ex?` (${ex})`:''}`,{direction:'top',offset:[0,-6]});
    mk.on('click',()=>selectStation(st,true));
    mk.addTo(stationsLayerGroup);
  });

  if(state.selectedStation)updateStationCardContent();
  updateHeaderStats(vis);renderStationList();renderChart();
}

function togglePmrOnlyFilter(){state.onlyPmrAccessible=!state.onlyPmrAccessible;document.getElementById('btnFilterPmr').classList.toggle('pmr-active',state.onlyPmrAccessible);renderMap();}
function applyRealDayPreset(v){if(v==='normal')state.dayOverrideM=null;else if(v==='bf2025'){state.dayIdx=4;state.dayOverrideM=2.819;}else if(v==='sat2025'){state.dayIdx=5;state.dayOverrideM=2.064;}else if(v==='sun2025'){state.dayIdx=6;state.dayOverrideM=1.620;}buildDayPills();renderMap();}
function buildDayPills(){const c=document.getElementById('dayPills');c.innerHTML='';DAYS.forEach(d=>{const b=document.createElement('button');b.className='day-pill'+(d.id===state.dayIdx?' active':'');b.textContent=d.short;b.title=`${d.name} (~${d.avgDailyM.toFixed(2)} M/día)`;b.onclick=()=>{state.dayIdx=d.id;state.dayOverrideM=null;document.getElementById('realDayPreset').value='normal';buildDayPills();renderMap();};c.appendChild(b);});}
function buildLinesGrid(){
  const g=document.getElementById('linesGrid');g.innerHTML='';
  LINES_META.forEach(m=>{
    const sel=state.selectedLines.includes(m.id),card=document.createElement('div');
    card.className='line-card'+(sel?' selected':'');card.onclick=e=>{if(!e.target.closest('.line-zoom'))toggleLine(m.id);};
    const sub=m.official2025?`${m.pax2025M}M (2025 oficial)`:`~${m.pax2025M}M (est. 2025)`;
    card.innerHTML=`<div class="line-left"><span class="line-pill" style="background:${m.color};color:${m.textColor};">${m.code}</span><div class="line-info"><span class="line-name" title="${m.name}">${m.id==='L12'?'MetroSur':m.id==='R'?'Ramal':m.name}</span><span class="line-sub">${sub}</span></div></div><button class="line-zoom" title="Ver solo ${m.id} y centrar" onclick="focusLine('${m.id}')">⌖</button>`;
    g.appendChild(card);
  });
  document.getElementById('selectedCountLabel').textContent=`${state.selectedLines.length}/${LINES_META.length}`;
}
function toggleLine(lid){state.selectedLines=state.selectedLines.includes(lid)?state.selectedLines.filter(x=>x!==lid):[...state.selectedLines,lid];buildLinesGrid();renderMap();}
function selectAllLines(){state.selectedLines=LINES_META.map(l=>l.id);buildLinesGrid();renderMap();resetMadridView();}
function clearLines(){state.selectedLines=[];buildLinesGrid();renderMap();}
function focusLine(lid){state.selectedLines=[lid];buildLinesGrid();renderMap();const pts=(LINE_STATIONS[lid]||[]).map(n=>STATIONS[n]).filter(Boolean).map(c=>[c[0],c[1]]);if(pts.length)map.fitBounds(L.latLngBounds(pts),{padding:[45,45],maxZoom:14});}

function setColorMode(mode){
  state.colorMode=mode;
  ['Official','Pmr','Freq','Heat'].forEach(k=>document.getElementById('btnMode'+k).classList.toggle('active',mode===k.toLowerCase()));
  document.getElementById('legendOfficialWrap').style.display=mode==='official'?'block':'none';
  document.getElementById('legendPmrWrap').style.display=mode==='pmr'?'block':'none';
  document.getElementById('legendFreqWrap').style.display=mode==='freq'?'block':'none';
  document.getElementById('legendHeatWrap').style.display=mode==='heat'?'block':'none';
  document.getElementById('legendTitle').textContent=mode==='official'?'Colores oficiales de Metro de Madrid':mode==='pmr'?'Accesibilidad PMR oficial (CRTM)':mode==='freq'?'Frecuencia de paso (espera entre trenes)':'Intensidad de ocupación (viajeros/h)';
  renderMap();
}
function setHour(v){state.hour=parseInt(v,10);document.getElementById('hourSlider').value=state.hour;document.getElementById('hourDisplay').textContent=`${String(state.hour).padStart(2,'0')}:00`;renderMap();}
function togglePlay(){state.playing=!state.playing;const b=document.getElementById('btnPlay');if(state.playing){b.textContent='⏸ Pausar';b.classList.add('active');state.playTimer=setInterval(()=>setHour((state.hour+1)%24),900);}else{b.textContent='▶ Animar';b.classList.remove('active');clearInterval(state.playTimer);}}

function updateHeaderStats(vis){
  const cl=isMetroClosed(state.hour),tag=document.getElementById('serviceStateTag');
  if(cl){tag.textContent='Cerrado (02:00–05:59)';tag.style.color='#94a3b8';}
  else if(state.hour>=22||state.hour<=1){tag.textContent='Servicio nocturno (sin SIRAT)';tag.style.color='#fbbf24';}
  else{tag.textContent='En servicio (SIRAT activo)';tag.style.color='#4ade80';}
  let thp=0,t25=0,tsh=0;const hws=[];
  state.selectedLines.forEach(lid=>{thp+=getLineHourlyPassengers(lid,state.dayIdx,state.hour);const m=LINES_META.find(l=>l.id===lid);if(m){t25+=m.pax2025M;tsh+=m.share2025;}const hw=getLineHeadwayMinutes(lid,state.dayIdx,state.hour);if(hw!==null)hws.push(hw);});
  document.getElementById('hourlyNetworkLoad').textContent=cl?'0 viajeros/h':`~${thp.toLocaleString('es-ES')} viajeros/h`;
  if(cl||!hws.length)document.getElementById('kpiHeadway').textContent='Cerrado';
  else{const a=hws.reduce((x,y)=>x+y,0)/hws.length;document.getElementById('kpiHeadway').textContent=`~${a.toFixed(1)} min (${(60/a).toFixed(1)}/h)`;}
  const arr=Array.from(vis||[]),tot=arr.length,acc=arr.filter(s=>getStationPMR(s)==='T').length,pct=tot?Math.round((acc/tot)*100):0;
  document.getElementById('pmrSummaryKpi').textContent=tot?`${acc}/${tot} (${pct}%)`:'0/0';
  const tCo2=Math.round(t25*1e6*KG_CO2_SAVED_PER_TRIP/1000),cars=Math.round((getEffectiveDailyM(state.dayIdx)*1e6*tsh*.38)/2.4);
  document.getElementById('ecoCo2Year').textContent=`${(tCo2/1000).toFixed(1)}k t`;
  document.getElementById('ecoCarsDay').textContent=`~${(cars/1000).toFixed(0)}k/día`;
}

function renderStationList(){
  const cont=document.getElementById('stationsList'),q=(document.getElementById('stationSearch').value||'').trim().toLowerCase().normalize('NFD').replace(/[\u0300-\u036f]/g,'');
  let ord=[];
  if(state.selectedLines.length===1)ord=[...(LINE_STATIONS[state.selectedLines[0]]||[])];
  else{const s=new Set();state.selectedLines.forEach(lid=>(LINE_STATIONS[lid]||[]).forEach(x=>s.add(x)));ord=Array.from(s).sort((a,b)=>a.localeCompare(b,'es'));}
  const flt=ord.filter(n=>{
    const pc=getStationPMR(n);if(state.onlyPmrAccessible&&pc!=='T')return false;
    if(!q)return true;
    const nm=n.toLowerCase().normalize('NFD').replace(/[\u0300-\u036f]/g,''),rn=RENFE_CONNECTIONS[n];
    if(q==='renfe'||q==='cercanias')return Boolean(rn);
    return nm.includes(q)||(rn&&(rn.lines.toLowerCase().includes(q)||(rn.renfeName&&rn.renfeName.toLowerCase().normalize('NFD').replace(/[\u0300-\u036f]/g,'').includes(q))));
  });
  document.getElementById('stationsCountLabel').textContent=`${flt.length}`;cont.innerHTML='';const cl=isMetroClosed(state.hour);
  flt.forEach(n=>{
    const item=document.createElement('div');item.className='station-item'+(state.selectedStation===n?' selected-item':'');
    const si=getStationIntensity(n,state.selectedLines,state.dayIdx,state.hour),pl=(STATION_LINES[n]||[]).find(x=>state.selectedLines.includes(x)),pc=getStationPMR(n),pmr=pmrInfo(pc);
    const dc=state.colorMode==='pmr'?pmr.color:state.colorMode==='heat'?intensityToColor(si,cl):state.colorMode==='freq'?headwayToColor(pl?getLineHeadwayMinutes(pl,state.dayIdx,state.hour):null,cl):(LINES_META.find(l=>l.id===pl)||{color:'#38bdf8'}).color;
    const t10=TOP_10_STATIONS_2025[n],rn=RENFE_CONNECTIONS[n];
    item.innerHTML=`<div style="display:flex;align-items:center;min-width:0;flex:1;"><span class="station-dot" style="background:${dc};"></span><span style="white-space:nowrap;overflow:hidden;text-overflow:ellipsis;">${n}</span><span class="pmr-mini pmr-${pc}" title="${pmr.label}">${pc}</span>${t10?`<span class="top10-tag">#${t10.rank}</span>`:''}${rn?`<span class="renfe-tag">Renfe</span>`:''}</div><span style="color:#94a3b8;font-size:10px;flex-shrink:0;">${(STATION_LINES[n]||[]).join('·')}</span>`;
    item.onclick=()=>selectStation(n,true);cont.appendChild(item);
  });
}

function toggleModal(o){document.getElementById('sourcesModal').classList.toggle('open',o);}

let chartInstance=null;
function setChartMode(m){
  state.chartMode=m;
  ['Annual','Ranking25','Pmr','Hourly','Co2'].forEach(k=>document.getElementById('btnChart'+k).classList.toggle('active',m===k.toLowerCase()));
  renderChart();
}
function toggleChartDrawer(){const d=document.getElementById('chartDrawer');d.classList.toggle('collapsed');document.getElementById('btnToggleChart').textContent=d.classList.contains('collapsed')?'Mostrar gráfica':'Ocultar';setTimeout(()=>map.invalidateSize(),200);}

function renderChart(){
  const ctx=document.getElementById('mainChart').getContext('2d');if(chartInstance)chartInstance.destroy();
  const sel=state.selectedLines,selLbl=sel.length===LINES_META.length?'Red completa (L1–L12 + R)':(sel.length===0?'Ninguna':sel.join(', '));
  if(state.chartMode==='annual'){
    const yrs=['2019','2020','2021','2022','2023','2024','2025'];
    document.getElementById('chartTitle').textContent=`Viajeros anuales (2019–2025) — ${selLbl}`;
    document.getElementById('chartSub').textContent=sel.length===LINES_META.length?'Totales anuales oficiales de Metro de Madrid / CRTM (millones). Récord 2025: 736,87 M.':'Serie anual de las líneas seleccionadas (millones).';
    chartInstance=new Chart(ctx,{type:'bar',data:{labels:yrs,datasets:[{data:getAnnualSeriesForLines(sel),backgroundColor:sel.length===1?LINES_META.find(l=>l.id===sel[0]).color:'#38bdf8',borderColor:'rgba(255,255,255,.45)',borderWidth:1,borderRadius:5,maxBarThickness:48}]},options:{responsive:true,maintainAspectRatio:false,plugins:{legend:{display:false},tooltip:{callbacks:{label:c=>`${c.parsed.y.toLocaleString('es-ES')} millones`}}},scales:{x:{ticks:{color:'#e2e8f0'},grid:{display:false}},y:{beginAtZero:true,ticks:{color:'#94a3b8',callback:v=>v+' M'},grid:{color:'rgba(148,163,184,.16)'}}}}});
  }else if(state.chartMode==='ranking25'){
    const s=[...LINES_META].sort((a,b)=>b.pax2025M-a.pax2025M);
    document.getElementById('chartTitle').textContent='Demanda por línea en 2025 (Millones de viajes · Total 736,87 M)';
    document.getElementById('chartSub').textContent='L1 (112,66 M), L10 (91,6 M), L3 (83,0 M), L5 (82,5 M) y L6 (76,0 M) son cifras oficiales publicadas.';
    chartInstance=new Chart(ctx,{type:'bar',data:{labels:s.map(l=>`${l.id}${l.official2025?' ★':''}`),datasets:[{data:s.map(l=>l.pax2025M),backgroundColor:s.map(l=>sel.includes(l.id)?l.color:'#1e293b'),borderColor:s.map(l=>sel.includes(l.id)?'rgba(255,255,255,.55)':'#334155'),borderWidth:1,borderRadius:4}]},options:{responsive:true,maintainAspectRatio:false,plugins:{legend:{display:false},tooltip:{callbacks:{label:c=>`${c.parsed.y.toLocaleString('es-ES')} M`}}},scales:{x:{ticks:{color:'#f1f5f9',font:{weight:'700'}},grid:{display:false}},y:{beginAtZero:true,ticks:{color:'#94a3b8',callback:v=>v+' M'},grid:{color:'rgba(148,163,184,.16)'}}}}});
  }else if(state.chartMode==='pmr'){
    const lbls=LINES_META.map(l=>l.id),dT=[],dP=[],dN=[];
    LINES_META.forEach(l=>{const sts=LINE_STATIONS[l.id]||[];dT.push(sts.filter(s=>getStationPMR(s)==='T').length);dP.push(sts.filter(s=>getStationPMR(s)==='P').length);dN.push(sts.filter(s=>getStationPMR(s)==='N').length);});
    document.getElementById('chartTitle').textContent='Impacto Social: Accesibilidad PMR por línea (Estaciones T / P / N)';
    document.getElementById('chartSub').textContent='Clasificación oficial CRTM GRADOACCESIBILIDAD: L3, L8, L11, L12 y Ramal son 100% accesibles (T).';
    chartInstance=new Chart(ctx,{type:'bar',data:{labels:lbls,datasets:[{label:'Totalmente accesible (T)',data:dT,backgroundColor:'#16a34a',borderRadius:2},{label:'Parcial (P)',data:dP,backgroundColor:'#f59e0b',borderRadius:2},{label:'No accesible (N)',data:dN,backgroundColor:'#dc2626',borderRadius:2}]},options:{responsive:true,maintainAspectRatio:false,plugins:{legend:{display:true,labels:{color:'#e2e8f0',boxWidth:11,font:{size:11}}}},scales:{x:{stacked:true,ticks:{color:'#f1f5f9',font:{weight:'700'}},grid:{display:false}},y:{stacked:true,beginAtZero:true,ticks:{color:'#94a3b8',stepSize:5},grid:{color:'rgba(148,163,184,.16)'}}}}});
  }else if(state.chartMode==='co2'){
    const s=[...LINES_META].sort((a,b)=>b.pax2025M-a.pax2025M),co2=s.map(l=>Math.round(l.pax2025M*1e6*KG_CO2_SAVED_PER_TRIP/1000));
    document.getElementById('chartTitle').textContent='Impacto Ambiental: Toneladas de CO₂ evitadas al año por línea (2025)';
    document.getElementById('chartSub').textContent='Ahorro frente a vehículo privado (5,5 km medios × 135 g CO₂/km × 38% sustitución coche).';
    chartInstance=new Chart(ctx,{type:'bar',data:{labels:s.map(l=>l.id),datasets:[{data:co2,backgroundColor:s.map(l=>sel.includes(l.id)?'#22c55e':'#1e293b'),borderColor:s.map(l=>sel.includes(l.id)?'#86efac':'#334155'),borderWidth:1,borderRadius:4}]},options:{responsive:true,maintainAspectRatio:false,plugins:{legend:{display:false},tooltip:{callbacks:{label:c=>`${c.parsed.y.toLocaleString('es-ES')} t CO₂/año evitadas`}}},scales:{x:{ticks:{color:'#f1f5f9',font:{weight:'700'}},grid:{display:false}},y:{beginAtZero:true,ticks:{color:'#86efac',callback:v=>(v/1000).toFixed(0)+'k t'},grid:{color:'rgba(148,163,184,.16)'}}}}});
  }else{
    const hrs=Array.from({length:24},(_,h)=>`${String(h).padStart(2,'0')}:00`),vals=hrs.map((_,h)=>{let s=0;sel.forEach(lid=>{s+=getLineHourlyPassengers(lid,state.dayIdx,h);});return Math.round(s/1000);});
    const rc=sel.length===1?LINES_META.find(l=>l.id===sel[0]).color:'#38bdf8',lc=(rc==='#004B87'||rc==='#73787E')?'#38bdf8':rc;
    document.getElementById('chartTitle').textContent=`Perfil horario (${state.dayOverrideM!==null?`Hito CRTM ${state.dayOverrideM.toFixed(3)}M`:DAYS[state.dayIdx].name}) — ${selLbl}`;
    document.getElementById('chartSub').textContent='Distribución horaria (miles de viajeros/h) calibrada con demanda diaria CRTM y cierre nocturno (02:00–05:59).';
    chartInstance=new Chart(ctx,{type:'line',data:{labels:hrs,datasets:[{data:vals,borderColor:lc,backgroundColor:'rgba(56,189,248,.16)',fill:true,tension:.35,pointRadius:c=>c.dataIndex===state.hour?6:2.5,pointBackgroundColor:c=>c.dataIndex===state.hour?'#E51923':lc}]},options:{responsive:true,maintainAspectRatio:false,plugins:{legend:{display:false},tooltip:{callbacks:{label:c=>`~${c.parsed.y.toLocaleString('es-ES')} mil viajeros/h`}}},scales:{x:{ticks:{color:'#e2e8f0',maxTicksLimit:12},grid:{display:false}},y:{beginAtZero:true,ticks:{color:'#94a3b8',callback:v=>v+'k'},grid:{color:'rgba(148,163,184,.16)'}}}}});
  }
}

buildDayPills();
buildLinesGrid();
setHour(8);
renderAirLayer();
tryLoadCRTMOpenData();
tryLoadAirQualityOpenData();
</script>
</body>
</html>
