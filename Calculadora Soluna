<html lang="es">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
  <title>Calculadora de Costos de Velas - Soluna</title>
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

    .logo-hero {
      width: 100%;
      max-width: 1200px;
      margin: 0 auto 15px;
      padding: 10px;
      display: flex;
      justify-content: center;
      align-items: center;
      background: transparent;
    }
    
    .hero-logo {
      width: 100%;
      max-width: 900px;
      height: auto;
      max-height: 80px;
      object-fit: cover;
      object-position: center;
      border-radius: 8px;
      box-shadow: 0 4px 15px rgba(0, 0, 0, 0.1);
      transition: all 0.3s ease;
    }
    
    body.dark .hero-logo {
      box-shadow: 0 4px 15px rgba(0, 0, 0, 0.25);
    }
    
    .hero-logo:hover {
      transform: translateY(-1px);
      box-shadow: 0 6px 20px rgba(0, 0, 0, 0.15);
    }

    /* Botones de Presets y Exportación */
    .quick-presets {
      background: white;
      padding: 15px;
      border-radius: 8px;
      margin-bottom: 20px;
      box-shadow: 0 2px 8px rgba(0,0,0,0.05);
      text-align: center;
      border: 1px solid #e0e0e0;
    }

    body.dark .quick-presets {
      background: #1e1e1e;
      border-color: #333;
    }

    .export-buttons, .action-buttons {
      display: flex;
      flex-wrap: wrap;
      gap: 10px;
      margin: 15px 0;
    }
    
    .export-btn {
      flex: 1;
      min-width: 140px;
      padding: 12px 15px;
      border: none;
      border-radius: 8px;
      font-weight: 600;
      cursor: pointer;
      display: flex;
      align-items: center;
      justify-content: center;
      gap: 8px;
      transition: all 0.3s ease;
      font-size: 0.9rem;
    }
    
    .pdf-btn { background-color: #e74c3c; color: white; }
    .pdf-btn:hover { background-color: #c0392b; transform: translateY(-2px); }
    .excel-btn { background-color: var(--verde); color: white; }
    .excel-btn:hover { background-color: var(--verde-hover); transform: translateY(-2px); }
    .client-btn { background-color: #2c3e50; color: white; }
    .client-btn:hover { background-color: #1a252f; transform: translateY(-2px); }
    .clear-btn { background-color: #6c757d; color: white; }
    .clear-btn:hover { background-color: #5a6268; transform: translateY(-2px); }
    
    .preset-btn { background-color: var(--terciario); color: var(--primario); border: 1px solid var(--primario); }
    .preset-btn:hover { background-color: var(--primario); color: white; transform: translateY(-2px); }
    body.dark .preset-btn { background-color: var(--terciario); color: var(--secundario); border-color: var(--secundario); }
    body.dark .preset-btn:hover { background-color: var(--secundario); color: #121212; }

    /* Tooltips */
    .tooltip-container { position: relative; display: inline-block; margin-left: 5px; }
    .tooltip-icon { color: var(--primario); cursor: help; font-size: 0.9rem; touch-action: manipulation; }
    .tooltip-text {
      visibility: hidden; width: 200px; background-color: var(--primario); color: white;
      text-align: left; border-radius: 6px; padding: 10px; position: absolute;
      z-index: 1000; bottom: 125%; left: 50%; transform: translateX(-50%);
      opacity: 0; transition: opacity 0.3s; font-size: 0.8rem; font-weight: normal;
      line-height: 1.4; box-shadow: 0 2px 10px rgba(0,0,0,0.2); word-wrap: break-word; white-space: normal;
    }
    body.dark .tooltip-text { background-color: var(--secundario); }
    .tooltip-container:hover .tooltip-text, .tooltip-container:focus .tooltip-text { visibility: visible; opacity: 1; }

    /* Contenedores Principales */
    .calculadora-grid { display: grid; grid-template-columns: 1fr; gap: 15px; }
  
    .input-card, .result-card {
      background: white; border-radius: 8px; padding: 15px; margin-bottom: 15px;
      box-shadow: 0 2px 8px rgba(0,0,0,0.05); border: 1px solid #e0e0e0;
      transition: transform 0.3s ease, box-shadow 0.3s ease;
    }
    
    body.dark .input-card, body.dark .result-card, body.dark .chart-container, body.dark .table-wrapper {
      background: #1e1e1e; box-shadow: 0 2px 12px rgba(0,0,0,0.1); border-color: #333;
    }

    /* Elementos details y summary para compactar la vista */
    details { margin-bottom: 15px; }
    summary {
      cursor: pointer; list-style: none; display: flex; align-items: center; gap: 6px;
      color: var(--primario); font-size: 1.1rem; font-weight: 500; padding: 5px 0;
    }
    summary::-webkit-details-marker { display: none; }
    summary::after {
      content: '\f078'; font-family: 'Font Awesome 5 Free'; font-weight: 900;
      margin-left: auto; transition: transform 0.3s; font-size: 0.9rem;
    }
    details[open] summary::after { transform: rotate(180deg); }
    .details-content { margin-top: 15px; animation: fadeIn 0.3s ease; }
    @keyframes fadeIn { from { opacity: 0; transform: translateY(-5px); } to { opacity: 1; transform: translateY(0); } }

    .input-card h3, .result-card h3 {
      margin-top: 0; color: var(--primario); padding-bottom: 6px; margin-bottom: 12px;
      font-size: 1.1rem; display: flex; align-items: center; gap: 6px; font-weight: 500; letter-spacing: 0.2px;
    }
  
    .input-group { margin-bottom: 12px; }
    .input-group label { display: block; margin-bottom: 4px; font-weight: 500; font-size: 0.9rem; }
  
    input, select {
      width: 100%; padding: 10px 12px; border: 1px solid #ddd; border-radius: 6px;
      background-color: #fff; transition: all 0.3s; font-size: 16px; /* 16px evita zoom en iOS */
      -webkit-appearance: none; box-sizing: border-box;
    }
  
    body.dark input, body.dark select { background-color: #2a2a2a; color: #e0e0e0; border-color: #555; }

    /* Selectores de unidad */
    .unit-selector { display: flex; gap: 10px; margin-top: 5px; }
    .unit-option {
      flex: 1; text-align: center; padding: 8px 5px; border: 1px solid #ddd; border-radius: 6px;
      cursor: pointer; font-size: 0.85rem; transition: all 0.3s; background: #f8f9fa;
    }
    body.dark .unit-option { background: #2a2a2a; border-color: #555; color: #e0e0e0; }
    .unit-option.active { background-color: var(--primario); color: white; border-color: var(--primario); }
    body.dark .unit-option.active { background-color: var(--secundario); border-color: var(--secundario); }
  
    /* Resumen tipo infografía */
    .summary-grid { display: grid; grid-template-columns: repeat(auto-fit, minmax(200px, 1fr)); gap: 15px; margin-top: 15px; }
    .summary-item {
      background: white; border-radius: 8px; padding: 15px; display: flex; align-items: center; gap: 12px;
      box-shadow: 0 2px 8px rgba(0,0,0,0.05); border-left: 4px solid var(--primario); transition: transform 0.3s ease;
    }
    body.dark .summary-item { background: #2a2a2a; box-shadow: 0 2px 12px rgba(0,0,0,0.1); }
    .summary-item.total { border-left-color: var(--verde); background-color: rgba(82, 171, 152, 0.05); }
    body.dark .summary-item.total { background-color: rgba(82, 171, 152, 0.1); }
    .summary-icon {
      width: 40px; height: 40px; border-radius: 50%; background-color: var(--terciario);
      display: flex; align-items: center; justify-content: center; color: var(--primario); font-size: 1.1rem; flex-shrink: 0;
    }
    body.dark .summary-icon { background-color: #3a3a3a; }
    .summary-item.total .summary-icon { background-color: rgba(82, 171, 152, 0.2); color: var(--verde); }
    .summary-content { flex-grow: 1; }
    .summary-label { font-size: 0.85rem; color: #666; margin-bottom: 5px; }
    body.dark .summary-label { color: #aaa; }
    .summary-value { font-size: 1.1rem; font-weight: 600; color: var(--texto-claro); }
    .summary-item.total .summary-value { color: var(--verde); font-size: 1.2rem; }
  
    .chart-container {
      background: white; padding: 15px; border-radius: 8px; box-shadow: 0 2px 8px rgba(0,0,0,0.05);
      height: 300px; margin-bottom: 15px; border: 1px solid #e0e0e0;
    }
  
    canvas { width: 100% !important; height: 100% !important; }
  
    .dark-mode-btn {
      position: fixed; top: 20px; right: 20px; z-index: 1000; background-color: #2b2b2b; color: white;
      border: none; padding: 8px 16px; border-radius: 6px; cursor: pointer; box-shadow: 0 2px 8px rgba(0,0,0,0.1);
      font-size: 0.85rem; display: flex; align-items: center; gap: 4px; text-transform: uppercase; font-weight: 500; transition: all 0.3s;
    }
    body.dark .dark-mode-btn { background-color: #f0f0f0; color: #2b2b2b; }
  
    .whatsapp-btn {
      position: fixed; bottom: 20px; right: 20px; z-index: 999; background-color: #25D366; color: white;
      width: 50px; height: 50px; border-radius: 50%; display: flex; align-items: center; justify-content: center;
      box-shadow: 0 2px 8px rgba(0,0,0,0.2); transition: all 0.3s;
    }
    .whatsapp-btn:hover { background-color: #128C7E; transform: scale(1.1); }
    .whatsapp-btn i { font-size: 25px; }
  
    .table-wrapper {
      overflow-x: auto; max-height: 250px; overflow-y: auto; margin-top: 12px; border-radius: 6px;
      box-shadow: 0 1px 2px rgba(0,0,0,0.1); border: 1px solid #e0e0e0; -webkit-overflow-scrolling: touch;
    }
    
    #tablaResultados { width: 100%; border-collapse: collapse; font-size: 0.85rem; }
    #tablaResultados th, #tablaResultados td { padding: 10px 12px; text-align: right; border-bottom: 1px solid #eee; }
    #tablaResultados th { background-color: var(--primario); color: white; position: sticky; top: 0; text-align: center; font-weight: 500; }
    #tablaResultados tr:nth-child(even) { background-color: #f9f9f9; }
    body.dark #tablaResultados th { background-color: var(--secundario); }
    body.dark #tablaResultados tr:nth-child(even) { background-color: #2a2a2a; }
    body.dark #tablaResultados th, body.dark #tablaResultados td { border-color: #444; }

    @media (min-width: 768px) {
      .calculadora-grid { grid-template-columns: 1.1fr 2fr; }
      .chart-container { height: 400px; }
    }
    
    @media (max-width: 768px) {
      body { padding: 10px; }
      .calculadora-grid { display: flex; flex-direction: column; gap: 10px; }
      .summary-grid { grid-template-columns: 1fr 1fr; }
      .export-btn { width: 100%; }
    }
    
    @media print {
      .dark-mode-btn, .whatsapp-btn, .quick-presets, .action-buttons { display: none !important; }
    }
  </style>
</head>
<body>
  <button class="dark-mode-btn" onclick="toggleDarkMode()"><i class="fas fa-moon"></i> Modo Oscuro</button>

  <div class="logo-hero">
    <img src="SOLUNA OFICIAL.jpg" alt="Logo Soluna" class="hero-logo" onerror="this.src='https://via.placeholder.com/900x80/d97a9b/ffffff?text=SOLUNA'">
  </div>

  <!-- SECCIÓN DE CARGA RÁPIDA -->
  <div class="quick-presets">
    <h3 style="margin-top: 0; color: var(--primario); font-size: 1.1rem; margin-bottom: 15px;">
      <i class="fas fa-bolt"></i> Carga Rápida de Tamaños
    </h3>
    <div style="display: flex; gap: 10px; justify-content: center; flex-wrap: wrap;">
      <button onclick="cargarReceta(100, 6.00, 'Vela Pequeña')" class="export-btn preset-btn">
        Vela Pequeña (100g)
      </button>
      <button onclick="cargarReceta(170, 8.00, 'Vela Mónaco')" class="export-btn preset-btn">
        Vela Mónaco (170g)
      </button>
      <button onclick="cargarReceta(250, 12.00, 'Vela Grande')" class="export-btn preset-btn">
        Vela Grande (250g)
      </button>
    </div>
  </div>

  <div class="calculadora-grid">
    <!-- Columna izquierda - Inputs -->
    <div class="input-section">
      
      <div class="input-card">
        <details open>
          <summary><i class="fas fa-fire"></i> Cera</summary>
          <div class="details-content">
            <div class="input-group">
              <label for="costoCera">Costo de 1kg de cera ($):</label>
              <input type="number" id="costoCera" step="0.01" placeholder="100.00" value="100.00">
            </div>
            <div class="input-group">
              <label for="gramosCera">Cera por vela (gramos):</label>
              <input type="number" id="gramosCera" step="0.1" placeholder="150" value="150">
            </div>
          </div>
        </details>
      </div>

      <div class="input-card">
        <details>
          <summary><i class="fas fa-wind"></i> Fragancia</summary>
          <div class="details-content">
            <div class="input-group">
              <label for="costoFragancia">Costo de la fragancia ($):</label>
              <input type="number" id="costoFragancia" step="0.01" placeholder="200.00" value="200.00">
            </div>
            <div class="input-group">
              <label>Unidad de medida:</label>
              <div class="unit-selector">
                <div class="unit-option active" data-unit="ml" data-target="fragancia">Mililitros (ml)</div>
                <div class="unit-option" data-unit="litros" data-target="fragancia">Litros (L)</div>
              </div>
            </div>
            <div class="input-group">
              <label for="tamanoFragancia">Tamaño del envase:</label>
              <input type="number" id="tamanoFragancia" step="1" placeholder="250" value="250">
              <div id="fragancia-unit-display" style="font-size: 0.8rem; color: #666; margin-top: 5px;">ml</div>
            </div>
            <div class="input-group">
              <label for="porcentajeFragancia">Porcentaje de fragancia (%):</label>
              <input type="number" id="porcentajeFragancia" step="0.1" placeholder="10" value="10">
            </div>
          </div>
        </details>
      </div>

      <div class="input-card">
        <details>
          <summary><i class="fas fa-palette"></i> Colorante</summary>
          <div class="details-content">
            <div class="input-group">
              <label for="costoColorante">Costo del colorante ($):</label>
              <input type="number" id="costoColorante" step="0.01" placeholder="10.00" value="10.00">
            </div>
            <div class="input-group">
              <label>Unidad de medida:</label>
              <div class="unit-selector">
                <div class="unit-option active" data-unit="ml" data-target="colorante">Mililitros (ml)</div>
                <div class="unit-option" data-unit="gramos" data-target="colorante">Gramos (g)</div>
              </div>
            </div>
            <div class="input-group">
              <label for="cantidadColorante">Tamaño del envase:</label>
              <input type="number" id="cantidadColorante" step="1" placeholder="10" value="10">
              <div id="colorante-unit-display" style="font-size: 0.8rem; color: #666; margin-top: 5px;">ml</div>
            </div>
            <div class="input-group">
              <label for="usoColorante">Uso por vela:</label>
              <input type="number" id="usoColorante" step="0.1" placeholder="0.1" value="0.1">
              <div id="uso-colorante-unit-display" style="font-size: 0.8rem; color: #666; margin-top: 5px;">ml</div>
            </div>
          </div>
        </details>
      </div>

      <div class="input-card">
        <details>
          <summary><i class="fas fa-tools"></i> Materiales y Empaque</summary>
          <div class="details-content">
            <div class="input-group">
              <label for="costoMecha">Costo de mecha por vela ($):</label>
              <input type="number" id="costoMecha" step="0.01" placeholder="2.50" value="2.50">
            </div>
            <div class="input-group">
              <label for="costoFrasco">Costo del frasco por vela ($):</label>
              <input type="number" id="costoFrasco" step="0.01" placeholder="8.00" value="8.00">
            </div>
            <div class="input-group">
              <label for="costoEtiqueta">Costo de etiqueta principal ($):</label>
              <input type="number" id="costoEtiqueta" step="0.01" placeholder="1.50" value="1.50">
            </div>
            <div class="input-group">
              <label for="costoRotulacion">Rotulación de tapa/frasco ($):</label>
              <input type="number" id="costoRotulacion" step="0.01" placeholder="0.50" value="0.50">
            </div>
            <div class="input-group">
              <label for="costoEmpaque">Coste de empaque ($):</label>
              <input type="number" id="costoEmpaque" step="0.01" placeholder="2.00" value="2.00">
            </div>
            <div class="input-group">
              <label for="costoProtector">Protectores de cera ($):</label>
              <input type="number" id="costoProtector" step="0.01" placeholder="0.75" value="0.75">
            </div>
          </div>
        </details>
      </div>

      <div class="input-card">
        <details>
          <summary><i class="fas fa-user-tie"></i> Mano de Obra e Indirectos</summary>
          <div class="details-content">
            <div class="input-group">
              <label>Tipo de cálculo de mano de obra:</label>
              <div class="unit-selector">
                <div class="unit-option" data-unit="fijo" data-target="manoObra">Monto Fijo</div>
                <div class="unit-option active" data-unit="porcentaje" data-target="manoObra">Porcentaje</div>
              </div>
            </div>
            <div class="input-group" id="manoObraFijo-group">
              <label for="manoObraFijo">Mano de obra fija por vela ($):</label>
              <input type="number" id="manoObraFijo" step="0.01" value="0.00" placeholder="0.00">
            </div>
            <div class="input-group" id="manoObraPorcentaje-group">
              <label for="manoObraPorcentaje">Porcentaje de mano de obra (%):</label>
              <input type="number" id="manoObraPorcentaje" step="0.1" value="25" placeholder="25">
            </div>
            <div class="input-group">
              <label for="costoIndirecto">Costos indirectos por vela ($):</label>
              <input type="number" id="costoIndirecto" step="0.01" value="3.00" placeholder="3.00">
            </div>
          </div>
        </details>
      </div>

      <div class="input-card">
        <details open>
          <summary><i class="fas fa-chart-line"></i> Producción y Cotización</summary>
          <div class="details-content">
            <div class="input-group">
              <label for="margen">Margen de beneficio deseado (%):</label>
              <input type="number" id="margen" step="0.1" value="35" placeholder="35">
            </div>
            <div class="input-group">
              <label for="cantidadVelas">Número de velas a producir / cotizar:</label>
              <input type="number" id="cantidadVelas" step="1" value="1" placeholder="1">
            </div>
          </div>
        </details>
        
        <!-- Botones de Acción (Cotización, Excel, Limpiar) -->
        <div class="action-buttons">
          <button onclick="generarCotizacionCliente()" class="export-btn client-btn">
            <i class="fas fa-file-invoice-dollar"></i> Cotización Cliente (PDF)
          </button>
          <button onclick="exportToPDF()" class="export-btn pdf-btn">
            <i class="fas fa-file-pdf"></i> Reporte Interno (PDF)
          </button>
          <button onclick="exportToExcel()" class="export-btn excel-btn">
            <i class="fas fa-file-excel"></i> Excel Interno
          </button>
          <button onclick="limpiarDatos()" class="export-btn clear-btn">
            <i class="fas fa-trash-alt"></i> Limpiar Datos
          </button>
        </div>
      </div>
    </div>

    <!-- Columna derecha - Resultados -->
    <div class="results-section">
      <div class="result-card">
        <h3><i class="fas fa-calculator"></i> Resumen de Costos</h3>
        <div class="summary-grid">
          <div class="summary-item"><div class="summary-icon"><i class="fas fa-fire"></i></div><div class="summary-content"><div class="summary-label">Costo de cera</div><div class="summary-value" id="res-cera">$0.00</div></div></div>
          <div class="summary-item"><div class="summary-icon"><i class="fas fa-wind"></i></div><div class="summary-content"><div class="summary-label">Costo de fragancia</div><div class="summary-value" id="res-fragancia">$0.00</div></div></div>
          <div class="summary-item"><div class="summary-icon"><i class="fas fa-palette"></i></div><div class="summary-content"><div class="summary-label">Costo de colorante</div><div class="summary-value" id="res-colorante">$0.00</div></div></div>
          <div class="summary-item"><div class="summary-icon"><i class="fas fa-box"></i></div><div class="summary-content"><div class="summary-label">Materiales y empaque</div><div class="summary-value" id="res-materiales">$0.00</div></div></div>
          <div class="summary-item"><div class="summary-icon"><i class="fas fa-user-tie"></i></div><div class="summary-content"><div class="summary-label">Mano de obra</div><div class="summary-value" id="res-mano-obra">$0.00</div></div></div>
          
          <div class="summary-item total"><div class="summary-icon"><i class="fas fa-dollar-sign"></i></div><div class="summary-content"><div class="summary-label">Costo total unitario</div><div class="summary-value" id="res-total-unitario">$0.00</div></div></div>
          <div class="summary-item total"><div class="summary-icon"><i class="fas fa-tag"></i></div><div class="summary-content"><div class="summary-label">Precio de venta (1 vela)</div><div class="summary-value" id="res-precio-venta">$0.00</div></div></div>
          <div class="summary-item total"><div class="summary-icon"><i class="fas fa-shopping-cart"></i></div><div class="summary-content"><div class="summary-label">Total Cotización</div><div class="summary-value" id="res-precio-total">$0.00</div></div></div>
        </div>
      </div>

      <div class="chart-container">
        <canvas id="graficaCostos"></canvas>
      </div>
      
      <div class="results-table-container">
        <div class="input-card">
          <h3><i class="fas fa-table"></i> Desglose de Costos por Vela</h3>
          <div class="table-wrapper">
            <table id="tablaResultados">
              <thead><tr><th>Concepto</th><th>Costo Unitario</th><th>Porcentaje</th></tr></thead>
              <tbody></tbody>
            </table>
          </div>
        </div>
      </div>
    </div>
  </div>

  <a href="https://wa.me/523331490596?text=Hola,%20me%20interesa%20saber%20m%C3%A1s%20sobre%20la%20calculadora%20de%20costos%20de%20velas%20%F0%9F%94%A5" class="whatsapp-btn" target="_blank" title="Contactar por WhatsApp">
    <i class="fab fa-whatsapp"></i>
  </a>

  <script>
    let chartCostos = null;
    let resultadosCalculo = {};
    let timeoutGrafico = null;
    
    const unidadesConfig = {
      fragancia: { unidad: 'ml', factor: 1 },
      colorante: { unidad: 'ml', factor: 1 },
      manoObra: { tipo: 'porcentaje' }
    };

    function obtenerValorNumerico(id, valorPorDefecto = 0) {
      const elemento = document.getElementById(id);
      const valor = parseFloat(elemento.value);
      return isNaN(valor) || valor < 0 ? valorPorDefecto : valor;
    }

    function configurarSelectoresUnidad() {
      document.querySelectorAll('.unit-option').forEach(option => {
        option.addEventListener('click', function() {
          const target = this.getAttribute('data-target');
          const unit = this.getAttribute('data-unit');
          
          document.querySelectorAll(`.unit-option[data-target="${target}"]`).forEach(opt => opt.classList.remove('active'));
          this.classList.add('active');
          
          if (target === 'manoObra') {
            unidadesConfig.manoObra.tipo = unit;
            actualizarVisibilidadManoObra();
          } else {
            unidadesConfig[target].unidad = unit;
            unidadesConfig[target].factor = unit === 'litros' ? 1000 : 1;
            actualizarDisplayUnidades(target);
          }
          calcularCostos();
          guardarDatos();
        });
      });
    }

    function actualizarVisibilidadManoObra() {
      const esPorcentaje = unidadesConfig.manoObra.tipo === 'porcentaje';
      document.getElementById('manoObraFijo-group').style.display = esPorcentaje ? 'none' : 'block';
      document.getElementById('manoObraPorcentaje-group').style.display = esPorcentaje ? 'block' : 'none';
    }

    function actualizarDisplayUnidades(target) {
      const config = unidadesConfig[target];
      if (target === 'fragancia') {
        document.getElementById('fragancia-unit-display').textContent = config.unidad;
      } else if (target === 'colorante') {
        document.getElementById('colorante-unit-display').textContent = config.unidad;
        document.getElementById('uso-colorante-unit-display').textContent = config.unidad;
      }
    }

    // Funciones de Guardado Local (LocalStorage)
    function guardarDatos() {
      const datos = {};
      document.querySelectorAll('input[type="number"]').forEach(input => datos[input.id] = input.value);
      localStorage.setItem('soluna_calc_datos', JSON.stringify(datos));
    }

    function cargarDatosGuardados() {
      const datosGuardados = localStorage.getItem('soluna_calc_datos');
      if (datosGuardados) {
        const datos = JSON.parse(datosGuardados);
        for (const [id, valor] of Object.entries(datos)) {
          const input = document.getElementById(id);
          if (input) input.value = valor;
        }
      }
    }

    function limpiarDatos() {
      if(confirm('¿Estás seguro de restablecer todos los valores?')) {
        localStorage.removeItem('soluna_calc_datos');
        document.querySelectorAll('input[type="number"]').forEach(input => {
          input.value = input.getAttribute('placeholder') || 0;
        });
        calcularCostos();
      }
    }

    // Carga Rápida de Presets
    function cargarReceta(gramos, costoFrasco, tipo) {
      document.getElementById('gramosCera').value = gramos;
      document.getElementById('costoFrasco').value = costoFrasco;
      // Expandir la sección de cera si estaba cerrada
      document.querySelector('details:has(#gramosCera)').open = true;
      document.querySelector('details:has(#costoFrasco)').open = true;
      
      calcularCostos();
      guardarDatos();
    }

    document.addEventListener('DOMContentLoaded', function() {
      configurarSelectoresUnidad();
      actualizarVisibilidadManoObra();
      actualizarDisplayUnidades('fragancia');
      actualizarDisplayUnidades('colorante');
      
      cargarDatosGuardados();

      document.querySelectorAll('input').forEach(input => {
        input.addEventListener('input', () => {
          calcularCostos();
          guardarDatos();
        });
      });

      calcularCostos();
    });

    function toggleDarkMode() {
      document.body.classList.toggle("dark");
      const icon = document.querySelector('.dark-mode-btn i');
      icon.className = document.body.classList.contains("dark") ? 'fas fa-sun' : 'fas fa-moon';
      if (chartCostos) chartCostos.update();
    }

    function calcularCostos() {
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
      const cantidadVelas = obtenerValorNumerico('cantidadVelas', 1);

      const costoPorGramoCera = costoCera / 1000;
      const costoCeraPorVela = costoPorGramoCera * gramosCera;

      const gramosFragancia = gramosCera * (porcentajeFragancia / 100);
      const tamanoFraganciaMl = tamanoFragancia * unidadesConfig.fragancia.factor;
      const costoPorMlFragancia = tamanoFraganciaMl > 0 ? costoFragancia / tamanoFraganciaMl : 0;
      const costoFraganciaPorVela = gramosFragancia * costoPorMlFragancia;

      const cantidadColoranteMl = cantidadColorante * unidadesConfig.colorante.factor;
      const costoColorantePorUnidad = cantidadColoranteMl > 0 ? costoColorante / cantidadColoranteMl : 0;
      const costoColorantePorVela = usoColorante * costoColorantePorUnidad;

      const costosMateriales = costoMecha + costoFrasco + costoEtiqueta + costoRotulacion + costoEmpaque + costoProtector;
      const costosMaterialesTotales = costoCeraPorVela + costoFraganciaPorVela + costoColorantePorVela + costosMateriales;
      
      let costoManoObraPorVela = unidadesConfig.manoObra.tipo === 'porcentaje' 
        ? costosMaterialesTotales * (manoObraPorcentaje / 100) 
        : manoObraFijo;

      const costoTotalUnitario = costosMaterialesTotales + costoManoObraPorVela + costoIndirecto;
      const precioVentaUnitario = costoTotalUnitario * (1 + margen / 100);
      
      const precioTotalVenta = precioVentaUnitario * cantidadVelas;

      resultadosCalculo = {
        cantidadVelas, costoCeraPorVela, costoFraganciaPorVela, costoColorantePorVela,
        costosMateriales, costoManoObraPorVela, costoIndirecto, costoTotalUnitario,
        margen, precioVentaUnitario, precioTotalVenta
      };

      document.getElementById('res-cera').textContent = formatCurrency(costoCeraPorVela);
      document.getElementById('res-fragancia').textContent = formatCurrency(costoFraganciaPorVela);
      document.getElementById('res-colorante').textContent = formatCurrency(costoColorantePorVela);
      document.getElementById('res-materiales').textContent = formatCurrency(costosMateriales);
      document.getElementById('res-mano-obra').textContent = formatCurrency(costoManoObraPorVela);
      document.getElementById('res-total-unitario').textContent = formatCurrency(costoTotalUnitario);
      document.getElementById('res-precio-venta').textContent = formatCurrency(precioVentaUnitario);
      document.getElementById('res-precio-total').textContent = formatCurrency(precioTotalVenta);

      generarTablaCostos();

      clearTimeout(timeoutGrafico);
      timeoutGrafico = setTimeout(() => generarGraficoCostos(), 300);
    }

    function formatCurrency(value) {
      return new Intl.NumberFormat('es-MX', { 
        style: 'currency', currency: 'MXN', minimumFractionDigits: 2
      }).format(isNaN(value) ? 0 : value);
    }

    function generarGraficoCostos() {
      const ctx = document.getElementById('graficaCostos').getContext('2d');
      if (chartCostos) chartCostos.destroy();

      chartCostos = new Chart(ctx, {
        type: 'pie',
        data: {
          labels: ['Cera', 'Fragancia', 'Colorante', 'Materiales', 'Mano Obra', 'Indirectos'],
          datasets: [{
            data: [
              resultadosCalculo.costoCeraPorVela || 0, resultadosCalculo.costoFraganciaPorVela || 0,
              resultadosCalculo.costoColorantePorVela || 0, resultadosCalculo.costosMateriales || 0,
              resultadosCalculo.costoManoObraPorVela || 0, resultadosCalculo.costoIndirecto || 0
            ],
            backgroundColor: ['rgba(217,122,155,0.8)', 'rgba(227,141,168,0.8)', 'rgba(255,234,240,0.8)', 'rgba(82,171,152,0.8)', 'rgba(155,89,182,0.8)', 'rgba(52,152,219,0.8)'],
            borderColor: ['#d97a9b', '#e38da8', '#ffeaf0', '#52ab98', '#9b59b6', '#3498db'],
            borderWidth: 1
          }]
        },
        options: {
          responsive: true, maintainAspectRatio: false,
          plugins: { legend: { position: window.innerWidth < 768 ? 'bottom' : 'right' } }
        }
      });
    }

    function generarTablaCostos() {
      const tbody = document.querySelector('#tablaResultados tbody');
      tbody.innerHTML = '';
      
      const total = resultadosCalculo.costoTotalUnitario || 0;
      const conceptos = [
        { nombre: 'Cera', valor: resultadosCalculo.costoCeraPorVela || 0 },
        { nombre: 'Fragancia', valor: resultadosCalculo.costoFraganciaPorVela || 0 },
        { nombre: 'Colorante', valor: resultadosCalculo.costoColorantePorVela || 0 },
        { nombre: 'Mecha', valor: obtenerValorNumerico('costoMecha', 0) },
        { nombre: 'Frasco', valor: obtenerValorNumerico('costoFrasco', 0) },
        { nombre: 'Etiquetas y Empaque', valor: obtenerValorNumerico('costoEtiqueta',0) + obtenerValorNumerico('costoRotulacion',0) + obtenerValorNumerico('costoEmpaque',0) + obtenerValorNumerico('costoProtector',0) },
        { nombre: 'Mano de obra', valor: resultadosCalculo.costoManoObraPorVela || 0 },
        { nombre: 'Costos indirectos', valor: obtenerValorNumerico('costoIndirecto', 0) }
      ];

      conceptos.forEach(c => {
        if (c.valor > 0) {
          const pct = total > 0 ? ((c.valor / total) * 100).toFixed(1) : '0.0';
          tbody.innerHTML += `<tr><td style="text-align: left;">${c.nombre}</td><td>${formatCurrency(c.valor)}</td><td>${pct}%</td></tr>`;
        }
      });
      tbody.innerHTML += `<tr style="font-weight:bold; background:rgba(82,171,152,0.1);"><td style="text-align:left;">TOTAL COSTO</td><td>${formatCurrency(total)}</td><td>100%</td></tr>`;
    }

    // NUEVA FUNCIÓN: Generar Cotización en PDF para el Cliente
    function generarCotizacionCliente() {
      const { jsPDF } = window.jspdf;
      const doc = new jsPDF();
      
      const cantidad = resultadosCalculo.cantidadVelas || 1;
      const precioUnitario = resultadosCalculo.precioVentaUnitario || 0;
      const subtotal = precioUnitario * cantidad;
      const iva = subtotal * 0.16;
      const total = subtotal + iva;
      const gramos = document.getElementById('gramosCera').value;
      const fecha = new Date().toLocaleDateString('es-MX');

      // Encabezado
      doc.setFontSize(22); doc.setTextColor(217, 122, 155); doc.setFont('helvetica', 'bold');
      doc.text('SOLUNA', 105, 20, { align: 'center' });[cite: 1]
      
      doc.setFontSize(10); doc.setTextColor(100); doc.setFont('helvetica', 'normal');
      doc.text('RECUERDOS Y DECORACIÓN PARA EVENTOS', 105, 26, { align: 'center' });[cite: 1]

      doc.setFontSize(14); doc.setTextColor(0); doc.setFont('helvetica', 'bold');
      doc.text('COTIZACIÓN VELAS ARTESANALES AROMÁTICAS', 105, 36, { align: 'center' });

      // Datos Emisor[cite: 1]
      doc.setFontSize(9); doc.setFont('helvetica', 'normal');
      doc.text('Lugar de Emisión: Zapopan, Jalisco', 14, 50);[cite: 1]
      doc.text(`Fecha: ${fecha}`, 160, 50);
      doc.text('Contacto: Soluna - 33 3149 0596', 14, 55);

      // Tabla de productos[cite: 1]
      doc.autoTable({
        startY: 65,
        head: [['Artículo', 'Descripción', 'Cantidad', 'Precio Unitario', 'Importe']],
        body: [[
          'Vela Aromática', 
          `Vela artesanal aromática de cera de soya (${gramos}g).\nPresentación personalizada sujeta a disponibilidad.`, 
          cantidad.toString(), 
          formatCurrency(precioUnitario), 
          formatCurrency(subtotal)
        ]],
        theme: 'grid',
        headStyles: { fillColor: [217, 122, 155], textColor: 255 },
        styles: { fontSize: 9, cellPadding: 4 }
      });

      // Totales (Subtotal, IVA, Total)[cite: 1]
      const finalY = doc.lastAutoTable.finalY + 10;
      doc.setFont('helvetica', 'bold');
      doc.text('SUBTOTAL:', 140, finalY);
      doc.text(formatCurrency(subtotal), 170, finalY);[cite: 1]
      
      doc.text('IVA 16%:', 140, finalY + 7);[cite: 1]
      doc.text(formatCurrency(iva), 170, finalY + 7);
      
      doc.setTextColor(217, 122, 155);
      doc.text('TOTAL:', 140, finalY + 14);[cite: 1]
      doc.text(formatCurrency(total), 170, finalY + 14);[cite: 1]

      // Condiciones de Venta[cite: 1]
      doc.setTextColor(0); doc.setFontSize(10);
      doc.text('CONDICIONES DE VENTA', 14, finalY + 30);[cite: 1]
      
      doc.setFontSize(8); doc.setFont('helvetica', 'normal');
      const condiciones = [
        "La presente cotización tiene vigencia de 30 días naturales.",[cite: 1]
        "La manufactura de nuestras piezas es completamente artesanal, por lo que pueden presentarse sutiles",[cite: 1]
        "variaciones en su apariencia final, sin que ello afecte su calidad.",[cite: 1]
        "El tiempo de producción comienza a partir del anticipo del 50%."[cite: 1]
      ];
      
      let condY = finalY + 36;
      condiciones.forEach(line => { doc.text(line, 14, condY); condY += 5; });

      doc.setFont('helvetica', 'bold');
      doc.text('GRACIAS POR SU PREFERENCIA.', 105, condY + 10, { align: 'center' });[cite: 1]

      doc.save(`Cotizacion_Cliente_Soluna_${fecha.replace(/\//g, '-')}.pdf`);
    }

    // Funciones de Reporte Interno (Mantenidas del original)
    function exportToPDF() {
      const { jsPDF } = window.jspdf;
      const doc = new jsPDF();
      doc.setFontSize(16); doc.text('Reporte Interno de Costos - Soluna', 14, 20);
      doc.setFontSize(10); doc.text(`Generado: ${new Date().toLocaleDateString()}`, 14, 28);
      
      doc.autoTable({ html: '#tablaResultados', startY: 35, theme: 'grid', headStyles: { fillColor: [82, 171, 152] } });
      doc.save('Reporte_Costos_Interno.pdf');
    }

    function exportToExcel() {
      const wb = XLSX.utils.table_to_book(document.getElementById('tablaResultados'), {sheet:"Costos"});
      XLSX.writeFile(wb, 'Reporte_Costos_Interno.xlsx');
    }
  </script>
</body>
</html>
