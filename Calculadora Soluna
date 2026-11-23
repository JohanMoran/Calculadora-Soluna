<html lang="es">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
  <title>Calculadora de Costos - Soluna</title>
  <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
  <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.0.0/css/all.min.css">
  <link href="https://fonts.googleapis.com/css2?family=Inter:wght@300;400;500;600;700&display=swap" rel="stylesheet">
  <!-- Librerías para exportación -->
  <script src="https://cdnjs.cloudflare.com/ajax/libs/jspdf/2.5.1/jspdf.umd.min.js"></script>
  <script src="https://cdnjs.cloudflare.com/ajax/libs/jspdf-autotable/3.5.25/jspdf.plugin.autotable.min.js"></script>
  <script src="https://cdn.sheetjs.com/xlsx-0.19.3/package/dist/xlsx.full.min.js"></script>
  <script src="https://cdnjs.cloudflare.com/ajax/libs/html2canvas/1.4.1/html2canvas.min.js"></script>
  <style>
    :root {
      /* TEMA VELAS (Por defecto) */
      --fondo-claro: #f4f6f8;
      --texto-claro: #333;
      --primario: #d97a9b;
      --secundario: #e38da8;
      --terciario: #ffeaf0;
      --hover: #c25d7e;
      --boton-texto: #fff;
      --verde: #52ab98;
      --verde-hover: #429084;
    }
  
    body.dark {
      --fondo-claro: #121212;
      --texto-claro: #e0e0e0;
      --primario: #e38da8;
      --secundario: #d97a9b;
      --terciario: #2a1a21;
    }

    /* TEMA CONCRETO (Gris Pizarra/Piedra) */
    body.theme-concreto {
      --fondo-claro: #f0f2f5;
      --primario: #546e7a;
      --secundario: #78909c;
      --terciario: #eceff1;
      --hover: #455a64;
    }

    body.theme-concreto.dark {
      --primario: #90a4ae;
      --secundario: #b0bec5;
      --terciario: #263238;
    }
  
    body {
      font-family: 'Inter', -apple-system, BlinkMacSystemFont, sans-serif;
      background-color: var(--fondo-claro);
      color: var(--texto-claro);
      padding: 15px;
      max-width: 1200px;
      margin: 0 auto;
      transition: background-color 0.4s, color 0.4s;
      font-size: 15px;
      line-height: 1.6;
      -webkit-text-size-adjust: 100%;
    }

    .logo-hero { width: 100%; max-width: 1200px; margin: 0 auto 15px; padding: 10px; display: flex; justify-content: center; align-items: center; background: transparent; }
    .hero-logo { width: 100%; max-width: 900px; height: auto; max-height: 80px; object-fit: cover; object-position: center; border-radius: 8px; box-shadow: 0 4px 15px rgba(0, 0, 0, 0.1); transition: all 0.3s ease; }
    body.dark .hero-logo { box-shadow: 0 4px 15px rgba(0, 0, 0, 0.25); }
    .hero-logo:hover { transform: translateY(-1px); box-shadow: 0 6px 20px rgba(0, 0, 0, 0.15); }

    /* SWITCHER PRINCIPAL DE APPS */
    .main-app-switcher {
      display: flex; justify-content: center; gap: 15px; margin-bottom: 25px;
    }
    .app-btn {
      padding: 12px 25px; font-size: 1.1rem; font-weight: 700; border-radius: 30px; border: 2px solid #ddd;
      background: white; color: #666; cursor: pointer; transition: all 0.3s ease; display: flex; align-items: center; gap: 8px;
    }
    body.dark .app-btn { background: #1e1e1e; border-color: #444; color: #aaa; }
    
    .app-btn:hover { transform: translateY(-2px); box-shadow: 0 4px 10px rgba(0,0,0,0.1); }
    
    .app-btn.active-main {
      border-color: var(--primario); background: var(--terciario); color: var(--primario);
    }
    body.dark .app-btn.active-main { background: var(--terciario); color: var(--primario); border-color: var(--primario); }

    /* NAVEGACIÓN PASO A PASO */
    .step-nav-container { margin-bottom: 20px; background: white; padding: 15px; border-radius: 8px; box-shadow: 0 2px 8px rgba(0,0,0,0.05); border: 1px solid #e0e0e0; }
    body.dark .step-nav-container { background: #1e1e1e; border-color: #333; }
    .step-nav-title { margin-top: 0; margin-bottom: 12px; font-size: 1rem; color: var(--texto-claro); font-weight: 600; }
    .step-nav { display: flex; gap: 8px; overflow-x: auto; padding-bottom: 8px; scrollbar-width: thin; scrollbar-color: var(--primario) transparent; }
    .step-nav::-webkit-scrollbar { height: 6px; }
    .step-nav::-webkit-scrollbar-thumb { background-color: var(--primario); border-radius: 10px; }

    .step-btn {
      background-color: transparent; border: 1px solid var(--secundario); color: var(--texto-claro);
      padding: 8px 16px; border-radius: 6px; cursor: pointer; white-space: nowrap;
      font-weight: 500; font-size: 0.95rem; transition: all 0.3s; font-family: inherit;
    }
    .step-btn.active { background-color: var(--primario); border-color: var(--primario); color: white; }
    .step-btn:hover:not(.active) { background-color: var(--terciario); }
    
    body.dark .step-btn { border-color: var(--secundario); color: var(--texto-claro); }
    body.dark .step-btn.active { background-color: var(--secundario); border-color: var(--secundario); color: #121212; }
    body.dark .step-btn:hover:not(.active) { background-color: #2a2a2a; }

    .step-section { display: none; animation: fadeIn 0.4s ease; }
    .step-section.active { display: block; }
    @keyframes fadeIn { from { opacity: 0; transform: translateY(5px); } to { opacity: 1; transform: translateY(0); } }

    /* BOTONES */
    .export-buttons { display: flex; gap: 15px; margin: 20px 0; }
    .export-btn { padding: 12px 20px; border: none; border-radius: 8px; font-weight: 600; cursor: pointer; display: flex; align-items: center; gap: 8px; transition: all 0.3s ease; font-size: 0.9rem; width: 100%; justify-content: center; }
    .pdf-btn { background-color: #e74c3c; color: white; } .pdf-btn:hover { background-color: #c0392b; transform: translateY(-2px); }
    .excel-btn { background-color: var(--verde); color: white; } .excel-btn:hover { background-color: var(--verde-hover); transform: translateY(-2px); }

    /* TOOLTIPS */
    .tooltip-container { position: relative; display: inline-block; margin-left: 5px; }
    .tooltip-icon { color: var(--primario); cursor: help; font-size: 0.9rem; touch-action: manipulation; }
    .tooltip-text { visibility: hidden; width: 200px; background-color: var(--primario); color: white; text-align: left; border-radius: 6px; padding: 10px; position: absolute; z-index: 1000; bottom: 125%; left: 50%; transform: translateX(-50%); opacity: 0; transition: opacity 0.3s; font-size: 0.8rem; font-weight: normal; line-height: 1.4; box-shadow: 0 2px 10px rgba(0,0,0,0.2); word-wrap: break-word; white-space: normal; }
    body.dark .tooltip-text { background-color: var(--secundario); }
    .tooltip-container:hover .tooltip-text, .tooltip-container:focus .tooltip-text { visibility: visible; opacity: 1; }

    /* ESTRUCTURA GENERAL */
    .calculadora-grid { display: grid; grid-template-columns: 1fr; gap: 15px; }
    .input-card, .result-card { background: white; border-radius: 8px; padding: 15px; margin-bottom: 15px; box-shadow: 0 2px 8px rgba(0,0,0,0.05); border: 1px solid #e0e0e0; transition: transform 0.3s ease, box-shadow 0.3s ease; }
    .result-card:hover { transform: translateY(-2px); box-shadow: 0 4px 12px rgba(0,0,0,0.1); }
    body.dark .input-card, body.dark .result-card, body.dark .chart-container, body.dark .table-wrapper { background: #1e1e1e; box-shadow: 0 2px 12px rgba(0,0,0,0.1); border-color: #333; }
    .input-card h3, .result-card h3 { margin-top: 0; color: var(--primario); padding-bottom: 6px; margin-bottom: 12px; font-size: 1.1rem; display: flex; align-items: center; gap: 6px; font-weight: 500; }
  
    .input-group { margin-bottom: 12px; }
    .input-group label { display: block; margin-bottom: 4px; font-weight: 500; font-size: 0.9rem; }
    input, select { width: 100%; padding: 10px 12px; border: 1px solid #ddd; border-radius: 6px; background-color: #fff; transition: all 0.3s; font-size: 0.95rem; -webkit-appearance: none; box-sizing: border-box; }
    body.dark input, body.dark select { background-color: #2a2a2a; color: #e0e0e0; border-color: #555; }

    /* UNIDADES Y SWITCHES */
    .unit-selector { display: flex; gap: 10px; margin-top: 5px; }
    .unit-option { flex: 1; text-align: center; padding: 8px 5px; border: 1px solid #ddd; border-radius: 6px; cursor: pointer; font-size: 0.85rem; transition: all 0.3s; background: #f8f9fa; }
    body.dark .unit-option { background: #2a2a2a; border-color: #555; color: #e0e0e0; }
    .unit-option.active { background-color: var(--primario); color: white; border-color: var(--primario); }
    body.dark .unit-option.active { background-color: var(--secundario); border-color: var(--secundario); }
  
    /* RESUMEN DE RESULTADOS */
    .summary-grid { display: grid; grid-template-columns: repeat(auto-fit, minmax(200px, 1fr)); gap: 15px; margin-top: 15px; }
    .summary-item { background: white; border-radius: 8px; padding: 15px; display: flex; align-items: center; gap: 12px; box-shadow: 0 2px 8px rgba(0,0,0,0.05); border-left: 4px solid var(--primario); transition: transform 0.3s ease; }
    body.dark .summary-item { background: #2a2a2a; box-shadow: 0 2px 12px rgba(0,0,0,0.1); }
    .summary-item.total { border-left-color: var(--verde); background-color: rgba(82, 171, 152, 0.05); }
    body.dark .summary-item.total { background-color: rgba(82, 171, 152, 0.1); }
    
    .summary-icon { width: 40px; height: 40px; border-radius: 50%; background-color: var(--terciario); display: flex; align-items: center; justify-content: center; color: var(--primario); font-size: 1.1rem; flex-shrink: 0; }
    body.dark .summary-icon { background-color: #3a3a3a; }
    .summary-item.total .summary-icon { background-color: rgba(82, 171, 152, 0.2); color: var(--verde); }
    
    .summary-content { flex-grow: 1; }
    .summary-label { font-size: 0.85rem; color: #666; margin-bottom: 5px; }
    body.dark .summary-label { color: #aaa; }
    .summary-value { font-size: 1.1rem; font-weight: 600; color: var(--texto-claro); }
    .summary-item.total .summary-value { color: var(--verde); font-size: 1.2rem; }
  
    .chart-container { background: white; padding: 15px; border-radius: 8px; box-shadow: 0 2px 8px rgba(0,0,0,0.05); height: 300px; margin-bottom: 15px; border: 1px solid #e0e0e0; }
    canvas { width: 100% !important; height: 100% !important; }
  
    /* BOTONES FLOTANTES */
    .dark-mode-btn { position: fixed; top: 20px; right: 20px; z-index: 1000; background-color: #2b2b2b; color: white; border: none; padding: 8px 16px; border-radius: 6px; cursor: pointer; box-shadow: 0 2px 8px rgba(0,0,0,0.1); font-size: 0.85rem; display: flex; align-items: center; gap: 4px; text-transform: uppercase; font-weight: 500; transition: all 0.3s; }
    body.dark .dark-mode-btn { background-color: #f0f0f0; color: #2b2b2b; }
    .whatsapp-btn { position: fixed; bottom: 20px; right: 20px; z-index: 999; background-color: #25D366; color: white; width: 50px; height: 50px; border-radius: 50%; display: flex; align-items: center; justify-content: center; box-shadow: 0 2px 8px rgba(0,0,0,0.2); transition: all 0.3s; }
    .whatsapp-btn:hover { background-color: #128C7E; transform: scale(1.1); }
    .whatsapp-btn i { font-size: 25px; }
  
    /* TABLAS Y FAQ */
    .table-wrapper { overflow-x: auto; max-height: 250px; margin-top: 12px; border-radius: 6px; border: 1px solid #e0e0e0; }
    #tablaResultados, #tablaResultadosConcreto { width: 100%; border-collapse: collapse; font-size: 0.85rem; }
    #tablaResultados th, #tablaResultados td, #tablaResultadosConcreto th, #tablaResultadosConcreto td { padding: 10px 12px; text-align: right; border-bottom: 1px solid #eee; }
    #tablaResultados th, #tablaResultadosConcreto th { background-color: var(--primario); color: white; position: sticky; top: 0; text-align: center; font-weight: 500; }
    #tablaResultados tr:nth-child(even), #tablaResultadosConcreto tr:nth-child(even) { background-color: #f9f9f9; }
    body.dark #tablaResultados th, body.dark #tablaResultadosConcreto th { background-color: var(--secundario); }
    body.dark #tablaResultados tr:nth-child(even), body.dark #tablaResultadosConcreto tr:nth-child(even) { background-color: #2a2a2a; }
    body.dark #tablaResultados th, body.dark #tablaResultados td, body.dark #tablaResultadosConcreto th, body.dark #tablaResultadosConcreto td { border-color: #444; }
    
    /* RESPONSIVE */
    @media (min-width: 768px) { .calculadora-grid { grid-template-columns: 1.1fr 2fr; } .chart-container { height: 400px; } .export-buttons { flex-direction: row; } }
    @media (max-width: 768px) { body { padding: 10px; min-width: 100%; overflow-x: hidden; } .calculadora-grid { display: flex; flex-direction: column; gap: 10px; } .input-card, .result-card, .chart-container { width: 100%; box-sizing: border-box; } .hero-logo { max-height: 70px; } .table-wrapper { width: 100%; display: block; } .chart-container { height: 300px; padding: 10px; } .summary-grid { grid-template-columns: 1fr 1fr; gap: 10px; } .export-buttons { flex-direction: column; } }
    @media (max-width: 480px) { .summary-grid { grid-template-columns: 1fr; } input, select { font-size: 16px; } }
    * { max-width: 100%; box-sizing: border-box; } .calculadora-grid { width: 100%; overflow: hidden; }
  </style>
</head>
<body>
  <button class="dark-mode-btn" onclick="toggleDarkMode()"><i class="fas fa-moon"></i> Modo Oscuro</button>

  <div class="logo-hero">
    <img src="SOLUNA OFICIAL.jpg" alt="Logo Soluna" class="hero-logo">
  </div>

  <!-- SELECTOR PRINCIPAL VELAS VS CONCRETO -->
  <div class="main-app-switcher">
    <button id="btn-app-velas" class="app-btn active-main" onclick="switchApp('velas')">
      <i class="fas fa-fire"></i> Velas
    </button>
    <button id="btn-app-concreto" class="app-btn" onclick="switchApp('concreto')">
      <i class="fas fa-cubes"></i> Concreto
    </button>
  </div>

  <!-- ==========================================
       INICIO APP VELAS
       ========================================== -->
  <div id="app-velas">
    <!-- NAVEGACIÓN PASO A PASO VELAS -->
    <div class="step-nav-container">
      <h3 class="step-nav-title">Guía paso a paso · Velas</h3>
      <div class="step-nav">
        <button class="step-btn step-velas active" onclick="mostrarSeccionVelas('sec-cera', this)">1 Cera</button>
        <button class="step-btn step-velas" onclick="mostrarSeccionVelas('sec-fragancia', this)">2 Fragancia</button>
        <button class="step-btn step-velas" onclick="mostrarSeccionVelas('sec-colorante', this)">3 Colorante</button>
        <button class="step-btn step-velas" onclick="mostrarSeccionVelas('sec-materiales', this)">4 Materiales</button>
        <button class="step-btn step-velas" onclick="mostrarSeccionVelas('sec-manoobra', this)">5 Mano de Obra</button>
        <button class="step-btn step-velas" onclick="mostrarSeccionVelas('sec-produccion', this)">6 Producción</button>
      </div>
    </div>

    <div class="calculadora-grid">
      <!-- Columna izquierda - Inputs Velas -->
      <div class="input-section">
        
        <div id="sec-cera" class="input-card step-section-velas active">
          <h3><i class="fas fa-fire"></i> 1. Cera</h3>
          <div class="input-group">
            <label for="costoCera">Costo de 1kg de cera ($):</label>
            <input type="number" id="costoCera" step="0.01" placeholder="0.00" value="100.00">
          </div>
          <div class="input-group">
            <label for="gramosCera">Cera por vela (gramos):</label>
            <input type="number" id="gramosCera" step="0.1" placeholder="0" value="150">
          </div>
        </div>

        <div id="sec-fragancia" class="input-card step-section-velas">
          <h3><i class="fas fa-wind"></i> 2. Fragancia</h3>
          <div class="input-group">
            <label for="costoFragancia">Costo de la fragancia ($):</label>
            <input type="number" id="costoFragancia" step="0.01" placeholder="0.00" value="200.00">
          </div>
          <div class="input-group">
            <label>Unidad de medida:</label>
            <div class="unit-selector">
              <div class="unit-option unit-velas active" data-unit="ml" data-target="fragancia">Mililitros (ml)</div>
              <div class="unit-option unit-velas" data-unit="litros" data-target="fragancia">Litros (L)</div>
            </div>
          </div>
          <div class="input-group">
            <label for="tamanoFragancia">Tamaño del envase:</label>
            <input type="number" id="tamanoFragancia" step="1" placeholder="0" value="250">
          </div>
          <div class="input-group">
            <label for="porcentajeFragancia">Porcentaje de fragancia (%):</label>
            <input type="number" id="porcentajeFragancia" step="0.1" placeholder="0" value="10">
          </div>
        </div>

        <div id="sec-colorante" class="input-card step-section-velas">
          <h3><i class="fas fa-palette"></i> 3. Colorante</h3>
          <div class="input-group">
            <label for="costoColorante">Costo del colorante ($):</label>
            <input type="number" id="costoColorante" step="0.01" placeholder="0.00" value="10.00">
          </div>
          <div class="input-group">
            <label>Unidad de medida:</label>
            <div class="unit-selector">
              <div class="unit-option unit-velas active" data-unit="ml" data-target="colorante">Mililitros (ml)</div>
              <div class="unit-option unit-velas" data-unit="gramos" data-target="colorante">Gramos (g)</div>
            </div>
          </div>
          <div class="input-group">
            <label for="cantidadColorante">Tamaño del envase:</label>
            <input type="number" id="cantidadColorante" step="1" placeholder="0" value="10">
          </div>
          <div class="input-group">
            <label for="usoColorante">Uso por vela:</label>
            <input type="number" id="usoColorante" step="0.1" placeholder="0" value="0.1">
          </div>
        </div>

        <div id="sec-materiales" class="input-card step-section-velas">
          <h3><i class="fas fa-tools"></i> 4. Materiales y Empaque</h3>
          <div class="input-group">
            <label for="costoFrasco">Costo del frasco/envase por vela ($):</label>
            <input type="number" id="costoFrasco" step="0.01" placeholder="0.00" value="8.00">
          </div>
          <div class="input-group">
            <label for="costoMecha">Costo de mecha por vela ($):</label>
            <input type="number" id="costoMecha" step="0.01" placeholder="0.00" value="2.50">
          </div>
          <div class="input-group">
            <label for="costoEtiqueta">Costo de etiqueta principal ($):</label>
            <input type="number" id="costoEtiqueta" step="0.01" placeholder="0.00" value="1.50">
          </div>
          <div class="input-group">
            <label for="costoRotulacion">Rotulación de tapa/frasco ($):</label>
            <input type="number" id="costoRotulacion" step="0.01" placeholder="0.00" value="0.50">
          </div>
          <div class="input-group">
            <label for="costoEmpaque">Coste de empaque ($):</label>
            <input type="number" id="costoEmpaque" step="0.01" placeholder="0.00" value="2.00">
          </div>
          <div class="input-group">
            <label for="costoProtector">Protectores de cera ($):</label>
            <input type="number" id="costoProtector" step="0.01" placeholder="0.00" value="0.75">
          </div>
        </div>

        <div id="sec-manoobra" class="input-card step-section-velas">
          <h3><i class="fas fa-user-tie"></i> 5. Mano de Obra e Indirectos</h3>
          <div class="input-group">
            <label>Tipo de cálculo de mano de obra:</label>
            <div class="unit-selector">
              <div class="unit-option unit-velas" data-unit="fijo" data-target="manoObra">Monto Fijo</div>
              <div class="unit-option unit-velas active" data-unit="porcentaje" data-target="manoObra">Porcentaje</div>
            </div>
          </div>
          <div class="input-group" id="manoObraFijo-group">
            <label for="manoObraFijo">Mano de obra fija por vela ($):</label>
            <input type="number" id="manoObraFijo" step="0.01" value="0.00">
          </div>
          <div class="input-group" id="manoObraPorcentaje-group">
            <label for="manoObraPorcentaje">Porcentaje de mano de obra (%):</label>
            <input type="number" id="manoObraPorcentaje" step="0.1" value="25">
          </div>
          <div class="input-group">
            <label for="costoIndirecto">Costos indirectos por vela ($):</label>
            <input type="number" id="costoIndirecto" step="0.01" value="3.00">
          </div>
        </div>

        <div id="sec-produccion" class="input-card step-section-velas">
          <h3><i class="fas fa-chart-line"></i> 6. Producción y Margen</h3>
          <div class="input-group">
            <label for="margen">Margen de beneficio deseado (%):</label>
            <input type="number" id="margen" step="0.1" value="35">
          </div>
          
          <div class="input-group" style="margin-top: 15px; margin-bottom: 15px; background: rgba(82,171,152,0.05); padding: 10px; border-radius: 6px; border: 1px solid rgba(82,171,152,0.2);">
            <label style="color: var(--primario); font-weight: 600;">¿Aplicar margen de utilidad al frasco?</label>
            <span style="display:block; font-size: 0.8rem; color:#666; margin-bottom: 8px;">Si eliges "No", el frasco se cobrará al costo neto.</span>
            <div class="unit-selector">
              <div class="unit-option unit-velas active" data-unit="con-utilidad" data-target="utilidadEnvase">Sí (Todo)</div>
              <div class="unit-option unit-velas" data-unit="sin-utilidad" data-target="utilidadEnvase">No (Frasco al costo)</div>
            </div>
          </div>

          <div class="input-group">
            <label for="cantidadVelas">Número de velas a producir:</label>
            <input type="number" id="cantidadVelas" step="1" value="1">
          </div>
          
          <div class="export-buttons">
            <button onclick="exportToPDF()" class="export-btn pdf-btn"><i class="fas fa-file-pdf"></i> PDF</button>
            <button onclick="exportToExcel()" class="export-btn excel-btn"><i class="fas fa-file-excel"></i> Excel</button>
          </div>
        </div>
      </div>

      <!-- Columna derecha - Resultados Velas -->
      <div class="results-section">
        <div class="result-card">
          <h3><i class="fas fa-calculator"></i> Resumen Velas</h3>
          <div class="summary-grid">
            <div class="summary-item"><div class="summary-icon"><i class="fas fa-fire"></i></div><div class="summary-content"><div class="summary-label">Cera</div><div class="summary-value" id="res-cera">$0.00</div></div></div>
            <div class="summary-item"><div class="summary-icon"><i class="fas fa-wind"></i></div><div class="summary-content"><div class="summary-label">Fragancia</div><div class="summary-value" id="res-fragancia">$0.00</div></div></div>
            <div class="summary-item"><div class="summary-icon"><i class="fas fa-palette"></i></div><div class="summary-content"><div class="summary-label">Colorante</div><div class="summary-value" id="res-colorante">$0.00</div></div></div>
            <div class="summary-item"><div class="summary-icon"><i class="fas fa-box"></i></div><div class="summary-content"><div class="summary-label">Materiales</div><div class="summary-value" id="res-materiales">$0.00</div></div></div>
            <div class="summary-item total"><div class="summary-icon"><i class="fas fa-dollar-sign"></i></div><div class="summary-content"><div class="summary-label">Costo Unitario</div><div class="summary-value" id="res-total-unitario">$0.00</div></div></div>
            <div class="summary-item total"><div class="summary-icon"><i class="fas fa-tag"></i></div><div class="summary-content"><div class="summary-label">Precio de venta</div><div class="summary-value" id="res-precio-venta">$0.00</div></div></div>
          </div>
        </div>
        <div class="chart-container"><canvas id="graficaCostos"></canvas></div>
        <div class="results-table-container">
          <div class="input-card">
            <h3><i class="fas fa-table"></i> Desglose</h3>
            <div class="table-wrapper">
              <table id="tablaResultados">
                <thead><tr><th>Concepto</th><th>Costo</th><th>%</th></tr></thead>
                <tbody></tbody>
              </table>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>


  <!-- ==========================================
       INICIO APP CONCRETO
       ========================================== -->
  <div id="app-concreto" style="display: none;">
    
    <div class="step-nav-container">
      <h3 class="step-nav-title">Guía paso a paso · Concreto</h3>
      <div class="step-nav">
        <button class="step-btn step-conc active" onclick="mostrarSeccionConcreto('sec-conc-mezcla', this)">1 Mezcla Base</button>
        <button class="step-btn step-conc" onclick="mostrarSeccionConcreto('sec-conc-pigmento', this)">2 Pigmentos</button>
        <button class="step-btn step-conc" onclick="mostrarSeccionConcreto('sec-conc-acabado', this)">3 Acabado / Molde</button>
        <button class="step-btn step-conc" onclick="mostrarSeccionConcreto('sec-conc-manoobra', this)">4 Mano de Obra</button>
        <button class="step-btn step-conc" onclick="mostrarSeccionConcreto('sec-conc-produccion', this)">5 Producción</button>
      </div>
    </div>

    <div class="calculadora-grid">
      <!-- Columna izquierda - Inputs Concreto -->
      <div class="input-section">
        
        <div id="sec-conc-mezcla" class="input-card step-section-conc active">
          <h3><i class="fas fa-cubes"></i> 1. Mezcla Base</h3>
          <div class="input-group">
            <label for="costoCemento">Costo de 1kg de Cemento/Jesmonite ($):</label>
            <input type="number" id="costoCemento" step="0.01" value="20.00">
          </div>
          <div class="input-group">
            <label for="gramosCemento">Gramos por pieza:</label>
            <input type="number" id="gramosCemento" step="1" value="300">
          </div>
          <div class="input-group">
            <label for="costoAgua">Costo de Aditivo/Líquido por pieza ($) (Opcional):</label>
            <input type="number" id="costoAgua" step="0.01" value="2.00">
          </div>
        </div>

        <div id="sec-conc-pigmento" class="input-card step-section-conc">
          <h3><i class="fas fa-palette"></i> 2. Pigmento / Color</h3>
          <div class="input-group">
            <label for="costoPigmentoConc">Costo del Pigmento por pieza ($):</label>
            <input type="number" id="costoPigmentoConc" step="0.01" value="1.50">
          </div>
        </div>

        <div id="sec-conc-acabado" class="input-card step-section-conc">
          <h3><i class="fas fa-paint-roller"></i> 3. Acabado, Sellador y Moldes</h3>
          <div class="input-group">
            <label for="costoSellador">Sellador / Barniz por pieza ($):</label>
            <input type="number" id="costoSellador" step="0.01" value="3.00">
          </div>
          <div class="input-group">
            <label for="costoBaseCorcho">Base protectora (Corcho/Fieltro) ($):</label>
            <input type="number" id="costoBaseCorcho" step="0.01" value="1.50">
          </div>
          <div class="input-group">
            <label for="desgasteMolde">Desgaste de molde por pieza ($):
              <div class="tooltip-container"><i class="fas fa-question-circle tooltip-icon"></i><span class="tooltip-text">Si un molde cuesta $100 y dura 20 usos, el costo es $5 por pieza.</span></div>
            </label>
            <input type="number" id="desgasteMolde" step="0.01" value="5.00">
          </div>
        </div>

        <div id="sec-conc-manoobra" class="input-card step-section-conc">
          <h3><i class="fas fa-user-tie"></i> 4. Mano de Obra e Indirectos</h3>
          <div class="input-group">
            <label for="manoObraConc">Mano de obra fija por pieza ($):</label>
            <input type="number" id="manoObraConc" step="0.01" value="15.00">
          </div>
          <div class="input-group">
            <label for="indirectosConc">Costos indirectos (Lijas, luz, agua) ($):</label>
            <input type="number" id="indirectosConc" step="0.01" value="4.00">
          </div>
        </div>

        <div id="sec-conc-produccion" class="input-card step-section-conc">
          <h3><i class="fas fa-chart-line"></i> 5. Producción y Margen</h3>
          <div class="input-group">
            <label for="margenConc">Margen de beneficio deseado (%):</label>
            <input type="number" id="margenConc" step="0.1" value="40">
          </div>
          <div class="input-group">
            <label for="cantidadConc">Piezas a producir:</label>
            <input type="number" id="cantidadConc" step="1" value="1">
          </div>
        </div>
      </div>

      <!-- Columna derecha - Resultados Concreto -->
      <div class="results-section">
        <div class="result-card">
          <h3><i class="fas fa-calculator"></i> Resumen Concreto</h3>
          <div class="summary-grid">
            <div class="summary-item"><div class="summary-icon"><i class="fas fa-cubes"></i></div><div class="summary-content"><div class="summary-label">Costo Mezcla</div><div class="summary-value" id="res-conc-mezcla">$0.00</div></div></div>
            <div class="summary-item"><div class="summary-icon"><i class="fas fa-paint-roller"></i></div><div class="summary-content"><div class="summary-label">Acabados/Molde</div><div class="summary-value" id="res-conc-acabado">$0.00</div></div></div>
            <div class="summary-item"><div class="summary-icon"><i class="fas fa-user-tie"></i></div><div class="summary-content"><div class="summary-label">Mano de Obra</div><div class="summary-value" id="res-conc-manoobra">$0.00</div></div></div>
            <div class="summary-item total"><div class="summary-icon"><i class="fas fa-dollar-sign"></i></div><div class="summary-content"><div class="summary-label">Costo Unitario</div><div class="summary-value" id="res-conc-total">$0.00</div></div></div>
            <div class="summary-item total"><div class="summary-icon"><i class="fas fa-tag"></i></div><div class="summary-content"><div class="summary-label">Precio Sugerido</div><div class="summary-value" id="res-conc-precio">$0.00</div></div></div>
          </div>
        </div>
        
        <div class="chart-container"><canvas id="graficaCostosConcreto"></canvas></div>
        
        <div class="results-table-container">
          <div class="input-card">
            <h3><i class="fas fa-table"></i> Desglose Concreto</h3>
            <div class="table-wrapper">
              <table id="tablaResultadosConcreto">
                <thead><tr><th>Concepto</th><th>Costo</th><th>%</th></tr></thead>
                <tbody></tbody>
              </table>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>


  <a href="https://wa.me/523331490596" class="whatsapp-btn" target="_blank" title="Contactar por WhatsApp">
    <i class="fab fa-whatsapp"></i>
  </a>

  <script>
    // Variables Velas
    let chartCostos = null;
    let resultadosCalculo = {};
    const unidadesConfig = {
      fragancia: { unidad: 'ml', factor: 1 }, colorante: { unidad: 'ml', factor: 1 },
      manoObra: { tipo: 'porcentaje' }, utilidadEnvase: { tipo: 'con-utilidad' }
    };

    // Variables Concreto
    let chartCostosConcreto = null;
    let resultadosConcreto = {};

    // ==========================================
    // LÓGICA DEL SWITCHER PRINCIPAL
    // ==========================================
    function switchApp(app) {
      if(app === 'velas') {
        document.getElementById('app-velas').style.display = 'block';
        document.getElementById('app-concreto').style.display = 'none';
        document.body.classList.remove('theme-concreto');
        document.getElementById('btn-app-velas').classList.add('active-main');
        document.getElementById('btn-app-concreto').classList.remove('active-main');
        calcularCostos();
      } else {
        document.getElementById('app-velas').style.display = 'none';
        document.getElementById('app-concreto').style.display = 'block';
        document.body.classList.add('theme-concreto');
        document.getElementById('btn-app-concreto').classList.add('active-main');
        document.getElementById('btn-app-velas').classList.remove('active-main');
        calcularCostosConcreto();
      }
    }

    // ==========================================
    // LÓGICA DE PESTAÑAS (VELAS Y CONCRETO)
    // ==========================================
    function mostrarSeccionVelas(sectionId, btnElement) {
      document.querySelectorAll('.step-section-velas').forEach(sec => sec.classList.remove('active'));
      document.querySelectorAll('.step-velas').forEach(btn => btn.classList.remove('active'));
      document.getElementById(sectionId).classList.add('active');
      btnElement.classList.add('active');
    }

    function mostrarSeccionConcreto(sectionId, btnElement) {
      document.querySelectorAll('.step-section-conc').forEach(sec => sec.classList.remove('active'));
      document.querySelectorAll('.step-conc').forEach(btn => btn.classList.remove('active'));
      document.getElementById(sectionId).classList.add('active');
      btnElement.classList.add('active');
    }

    // ==========================================
    // UTILIDADES
    // ==========================================
    function obtenerValorNumerico(id, valorPorDefecto = 0) {
      const elemento = document.getElementById(id);
      if(!elemento) return valorPorDefecto;
      const valor = parseFloat(elemento.value);
      return isNaN(valor) || valor < 0 ? valorPorDefecto : valor;
    }

    function formatCurrency(value) {
      const numero = isNaN(value) || value === null ? 0 : value;
      return new Intl.NumberFormat('es-MX', { style: 'currency', currency: 'MXN', minimumFractionDigits: 2 }).format(numero);
    }

    function toggleDarkMode() {
      document.body.classList.toggle("dark");
      if (chartCostos) chartCostos.update();
      if (chartCostosConcreto) chartCostosConcreto.update();
    }

    // ==========================================
    // EVENTOS INICIALES
    // ==========================================
    document.addEventListener('DOMContentLoaded', function() {
      // Configs de Velas (Selectores)
      document.querySelectorAll('.unit-velas').forEach(option => {
        option.addEventListener('click', function() {
          const target = this.getAttribute('data-target');
          const unit = this.getAttribute('data-unit');
          document.querySelectorAll(`.unit-velas[data-target="${target}"]`).forEach(opt => opt.classList.remove('active'));
          this.classList.add('active');
          
          if (target === 'manoObra') {
            unidadesConfig.manoObra.tipo = unit;
            document.getElementById('manoObraFijo-group').style.display = (unit === 'porcentaje') ? 'none' : 'block';
            document.getElementById('manoObraPorcentaje-group').style.display = (unit === 'porcentaje') ? 'block' : 'none';
          } else if (target === 'utilidadEnvase') {
            unidadesConfig.utilidadEnvase.tipo = unit;
          }
          calcularCostos();
        });
      });

      // Listeners Globales
      document.querySelectorAll('#app-velas input').forEach(input => input.addEventListener('input', calcularCostos));
      document.querySelectorAll('#app-concreto input').forEach(input => input.addEventListener('input', calcularCostosConcreto));

      calcularCostos();
      calcularCostosConcreto();
    });

    // ==========================================
    // CALCULADORA CONCRETO
    // ==========================================
    function calcularCostosConcreto() {
      const costoCemento = obtenerValorNumerico('costoCemento', 0);
      const gramosCemento = obtenerValorNumerico('gramosCemento', 0);
      const costoAgua = obtenerValorNumerico('costoAgua', 0);
      const costoPigmento = obtenerValorNumerico('costoPigmentoConc', 0);
      
      const costoSellador = obtenerValorNumerico('costoSellador', 0);
      const costoBaseCorcho = obtenerValorNumerico('costoBaseCorcho', 0);
      const desgasteMolde = obtenerValorNumerico('desgasteMolde', 0);
      
      const manoObra = obtenerValorNumerico('manoObraConc', 0);
      const indirectos = obtenerValorNumerico('indirectosConc', 0);
      const margen = obtenerValorNumerico('margenConc', 40);

      // Math
      const costoMezcla = (costoCemento / 1000 * gramosCemento) + costoAgua;
      const costoAcabados = costoSellador + costoBaseCorcho + desgasteMolde;
      const costoTotalUnitario = costoMezcla + costoPigmento + costoAcabados + manoObra + indirectos;
      const precioVenta = costoTotalUnitario * (1 + margen / 100);

      resultadosConcreto = { costoMezcla, costoPigmento, costoAcabados, manoObra, indirectos, costoTotalUnitario };

      // DOM
      document.getElementById('res-conc-mezcla').textContent = formatCurrency(costoMezcla + costoPigmento);
      document.getElementById('res-conc-acabado').textContent = formatCurrency(costoAcabados);
      document.getElementById('res-conc-manoobra').textContent = formatCurrency(manoObra + indirectos);
      document.getElementById('res-conc-total').textContent = formatCurrency(costoTotalUnitario);
      document.getElementById('res-conc-precio').textContent = formatCurrency(precioVenta);

      generarGraficoConcreto();
      generarTablaConcreto(costoTotalUnitario, costoMezcla, costoPigmento, costoAcabados, manoObra, indirectos);
    }

    function generarGraficoConcreto() {
      const ctx = document.getElementById('graficaCostosConcreto').getContext('2d');
      if (chartCostosConcreto) chartCostosConcreto.destroy();
      chartCostosConcreto = new Chart(ctx, {
        type: 'pie',
        data: {
          labels: ['Mezcla', 'Pigmento', 'Acabados/Molde', 'Mano Obra', 'Indirectos'],
          datasets: [{
            data: [ resultadosConcreto.costoMezcla, resultadosConcreto.costoPigmento, resultadosConcreto.costoAcabados, resultadosConcreto.manoObra, resultadosConcreto.indirectos ],
            backgroundColor: ['#546e7a', '#78909c', '#90a4ae', '#b0bec5', '#cfd8dc']
          }]
        },
        options: { responsive: true, maintainAspectRatio: false }
      });
    }

    function generarTablaConcreto(total, mezcla, pigmento, acabados, mano, ind) {
      const tbody = document.querySelector('#tablaResultadosConcreto tbody');
      tbody.innerHTML = '';
      const datos = [
        { nombre: 'Mezcla (Cemento + Líquido)', valor: mezcla }, { nombre: 'Pigmentos', valor: pigmento },
        { nombre: 'Sellador / Molde / Corcho', valor: acabados }, { nombre: 'Mano de Obra', valor: mano }, { nombre: 'Indirectos', valor: ind }
      ];
      datos.forEach(d => {
        if(d.valor > 0) {
          const pct = total > 0 ? ((d.valor / total) * 100).toFixed(1) : '0';
          tbody.innerHTML += `<tr><td style="text-align:left">${d.nombre}</td><td>${formatCurrency(d.valor)}</td><td>${pct}%</td></tr>`;
        }
      });
      tbody.innerHTML += `<tr style="font-weight:bold; background:rgba(82,171,152,0.1);"><td style="text-align:left">TOTAL</td><td>${formatCurrency(total)}</td><td>100%</td></tr>`;
    }

    // ==========================================
    // CALCULADORA VELAS (Ya existente)
    // ==========================================
    function calcularCostos() {
      // (Se mantiene la lógica matemática exacta de tu versión de velas que me pasaste antes)
      const costoCera = obtenerValorNumerico('costoCera', 0);
      const gramosCera = obtenerValorNumerico('gramosCera', 0);
      const costoFragancia = obtenerValorNumerico('costoFragancia', 0);
      const tamanoFragancia = obtenerValorNumerico('tamanoFragancia', 1);
      const porcentajeFragancia = obtenerValorNumerico('porcentajeFragancia', 0);
      const costoColorante = obtenerValorNumerico('costoColorante', 0);
      const usoColorante = obtenerValorNumerico('usoColorante', 0);
      const cantidadColorante = obtenerValorNumerico('cantidadColorante', 1);
      const costoMecha = obtenerValorNumerico('costoMecha', 0);
      const costoFrasco = obtenerValorNumerico('costoFrasco', 0);
      const costoEtiqueta = obtenerValorNumerico('costoEtiqueta', 0);
      const costoRotulacion = obtenerValorNumerico('costoRotulacion', 0);
      const costoEmpaque = obtenerValorNumerico('costoEmpaque', 0);
      const costoProtector = obtenerValorNumerico('costoProtector', 0);
      const manoObraFijo = obtenerValorNumerico('manoObraFijo', 0);
      const manoObraPorcentaje = obtenerValorNumerico('manoObraPorcentaje', 0);
      const costoIndirecto = obtenerValorNumerico('costoIndirecto', 0);
      const margen = obtenerValorNumerico('margen', 35);

      const costoCeraPorVela = (costoCera / 1000) * gramosCera;
      const costoFraganciaPorVela = (gramosCera * (porcentajeFragancia / 100)) * (tamanoFragancia > 0 ? costoFragancia / (tamanoFragancia * unidadesConfig.fragancia.factor) : 0);
      const costoColorantePorVela = usoColorante * (cantidadColorante > 0 ? costoColorante / (cantidadColorante * unidadesConfig.colorante.factor) : 0);
      const costosMateriales = costoMecha + costoFrasco + costoEtiqueta + costoRotulacion + costoEmpaque + costoProtector;
      const costosTotalesSinMano = costoCeraPorVela + costoFraganciaPorVela + costoColorantePorVela + costosMateriales;
      
      const costoManoObraPorVela = (unidadesConfig.manoObra.tipo === 'porcentaje') ? (costosTotalesSinMano * (manoObraPorcentaje / 100)) : manoObraFijo;
      const costoTotalUnitario = costosTotalesSinMano + costoManoObraPorVela + costoIndirecto;
      
      let precioVentaUnitario = 0;
      if (unidadesConfig.utilidadEnvase.tipo === 'sin-utilidad') {
        precioVentaUnitario = ((costoTotalUnitario - costoFrasco) * (1 + margen / 100)) + costoFrasco;
      } else {
        precioVentaUnitario = costoTotalUnitario * (1 + margen / 100);
      }

      resultadosCalculo = { costoCeraPorVela, costoFraganciaPorVela, costoColorantePorVela, costosMateriales, costoManoObraPorVela, costoIndirecto, costoTotalUnitario };

      document.getElementById('res-cera').textContent = formatCurrency(costoCeraPorVela);
      document.getElementById('res-fragancia').textContent = formatCurrency(costoFraganciaPorVela);
      document.getElementById('res-colorante').textContent = formatCurrency(costoColorantePorVela);
      document.getElementById('res-materiales').textContent = formatCurrency(costosMateriales);
      document.getElementById('res-total-unitario').textContent = formatCurrency(costoTotalUnitario);
      document.getElementById('res-precio-venta').textContent = formatCurrency(precioVentaUnitario);

      generarGraficoVelas();
      generarTablaVelas(costoTotalUnitario, costoCeraPorVela, costoFraganciaPorVela, costoColorantePorVela, costosMateriales, costoManoObraPorVela, costoIndirecto);
    }

    function generarGraficoVelas() {
      const ctx = document.getElementById('graficaCostos').getContext('2d');
      if (chartCostos) chartCostos.destroy();
      chartCostos = new Chart(ctx, {
        type: 'pie',
        data: {
          labels: ['Cera', 'Fragancia', 'Colorante', 'Materiales', 'Mano Obra', 'Indirectos'],
          datasets: [{
            data: [ resultadosCalculo.costoCeraPorVela, resultadosCalculo.costoFraganciaPorVela, resultadosCalculo.costoColorantePorVela, resultadosCalculo.costosMateriales, resultadosCalculo.costoManoObraPorVela, resultadosCalculo.costoIndirecto ],
            backgroundColor: ['rgba(217, 122, 155, 0.8)', 'rgba(227, 141, 168, 0.8)', 'rgba(255, 234, 240, 0.8)', 'rgba(82, 171, 152, 0.8)', 'rgba(155, 89, 182, 0.8)', 'rgba(52, 152, 219, 0.8)']
          }]
        },
        options: { responsive: true, maintainAspectRatio: false }
      });
    }

    function generarTablaVelas(total, cera, fragancia, colorante, materiales, mano, ind) {
      const tbody = document.querySelector('#tablaResultados tbody');
      tbody.innerHTML = '';
      const datos = [ { nombre: 'Cera', valor: cera }, { nombre: 'Fragancia', valor: fragancia }, { nombre: 'Colorante', valor: colorante }, { nombre: 'Materiales/Frasco', valor: materiales }, { nombre: 'Mano de Obra', valor: mano }, { nombre: 'Indirectos', valor: ind } ];
      datos.forEach(d => {
        if(d.valor > 0) {
          const pct = total > 0 ? ((d.valor / total) * 100).toFixed(1) : '0';
          tbody.innerHTML += `<tr><td style="text-align:left">${d.nombre}</td><td>${formatCurrency(d.valor)}</td><td>${pct}%</td></tr>`;
        }
      });
      tbody.innerHTML += `<tr style="font-weight:bold; background:rgba(82,171,152,0.1);"><td style="text-align:left">TOTAL</td><td>${formatCurrency(total)}</td><td>100%</td></tr>`;
    }
    
    // (Mantener las funciones exportToPDF y exportToExcel intactas para Velas)
  </script>
</body>
</html>
