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

    /* Logo hero - MEJORADO Y MÁS COMPACTO */
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

    /* ========================================================
       NUEVOS ESTILOS: NAVEGACIÓN PASO A PASO (TIPO PESTAÑAS)
       ======================================================== */
    .step-nav-container {
      margin-bottom: 20px;
      background: white;
      padding: 15px;
      border-radius: 8px;
      box-shadow: 0 2px 8px rgba(0,0,0,0.05);
      border: 1px solid #e0e0e0;
    }

    body.dark .step-nav-container {
      background: #1e1e1e;
      border-color: #333;
    }

    .step-nav-title {
      margin-top: 0;
      margin-bottom: 12px;
      font-size: 1rem;
      color: var(--texto-claro);
      font-weight: 600;
    }

    .step-nav {
      display: flex;
      gap: 8px;
      overflow-x: auto;
      padding-bottom: 8px; /* Espacio para barra de scroll en móviles */
      scrollbar-width: thin;
      scrollbar-color: var(--primario) transparent;
    }

    .step-nav::-webkit-scrollbar {
      height: 6px;
    }
    .step-nav::-webkit-scrollbar-thumb {
      background-color: var(--primario);
      border-radius: 10px;
    }

    .step-btn {
      background-color: transparent;
      border: 1px solid #c9a091; /* Color de contorno inspirado en tu imagen */
      color: #6d4c41; /* Color de texto inspirado en tu imagen */
      padding: 8px 16px;
      border-radius: 6px;
      cursor: pointer;
      white-space: nowrap;
      font-weight: 500;
      font-size: 0.95rem;
      transition: all 0.3s;
      font-family: inherit;
    }

    /* Estilo del botón activo (como el "1 Comprar" de tu imagen) */
    .step-btn.active {
      background-color: #5d3a43; 
      border-color: #5d3a43;
      color: white;
    }

    .step-btn:hover:not(.active) {
      background-color: #f5ece9;
    }

    body.dark .step-btn {
      border-color: var(--secundario);
      color: var(--texto-claro);
    }
    body.dark .step-btn.active {
      background-color: var(--secundario);
      border-color: var(--secundario);
      color: #121212;
    }
    body.dark .step-btn:hover:not(.active) {
      background-color: #2a2a2a;
    }

    /* Ocultar secciones no activas */
    .step-section {
      display: none;
      animation: fadeIn 0.4s ease;
    }
    .step-section.active {
      display: block;
    }

    @keyframes fadeIn {
      from { opacity: 0; transform: translateY(5px); }
      to { opacity: 1; transform: translateY(0); }
    }
    /* ======================================================== */

    /* Botones de exportación */
    .export-buttons {
      display: flex;
      gap: 15px;
      margin: 20px 0;
    }
    
    .export-btn {
      padding: 12px 20px;
      border: none;
      border-radius: 8px;
      font-weight: 600;
      cursor: pointer;
      display: flex;
      align-items: center;
      gap: 8px;
      transition: all 0.3s ease;
      font-size: 0.9rem;
      width: 100%;
      justify-content: center;
    }
    
    .pdf-btn { background-color: #e74c3c; color: white; }
    .pdf-btn:hover { background-color: #c0392b; transform: translateY(-2px); }
    .excel-btn { background-color: var(--verde); color: white; }
    .excel-btn:hover { background-color: var(--verde-hover); transform: translateY(-2px); }

    /* Estilos mejorados para tooltips */
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

    @media (max-width: 768px) {
      .tooltip-text {
        width: 180px; font-size: 0.75rem; bottom: auto; top: 100%; left: 50%;
        transform: translateX(-50%); margin-top: 8px; max-width: 80vw;
      }
      .tooltip-container .tooltip-text.right { left: auto; right: 0; transform: none; }
      .tooltip-container .tooltip-text.left { left: 0; transform: none; }
    }
  
    .calculadora-grid {
      display: grid;
      grid-template-columns: 1fr;
      gap: 15px;
    }
  
    .input-card, .result-card {
      background: white; border-radius: 8px; padding: 15px; margin-bottom: 15px;
      box-shadow: 0 2px 8px rgba(0,0,0,0.05); border: 1px solid #e0e0e0;
      transition: transform 0.3s ease, box-shadow 0.3s ease;
    }
    
    .result-card:hover { transform: translateY(-2px); box-shadow: 0 4px 12px rgba(0,0,0,0.1); }
  
    body.dark .input-card, body.dark .result-card, body.dark .chart-container, body.dark .table-wrapper {
      background: #1e1e1e; box-shadow: 0 2px 12px rgba(0,0,0,0.1); border-color: #333;
    }
  
    .input-card h3, .result-card h3 {
      margin-top: 0; color: var(--primario); padding-bottom: 6px; margin-bottom: 12px;
      font-size: 1.1rem; display: flex; align-items: center; gap: 6px; font-weight: 500; letter-spacing: 0.2px;
    }
  
    .input-group { margin-bottom: 12px; }
    .input-group label { display: block; margin-bottom: 4px; font-weight: 500; font-size: 0.9rem; }
  
    input, select {
      width: 100%; padding: 10px 12px; border: 1px solid #ddd; border-radius: 6px;
      background-color: #fff; transition: all 0.3s; font-size: 0.95rem; -webkit-appearance: none; box-sizing: border-box;
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
      box-shadow: 0 2px 8px rgba(0,0,0,0.05); border-left: 4px solid var(--primario); transition: transform 0.3s ease, box-shadow 0.3s ease;
    }
    body.dark .summary-item { background: #2a2a2a; box-shadow: 0 2px 12px rgba(0,0,0,0.1); }
    .summary-item:hover { transform: translateY(-3px); box-shadow: 0 4px 12px rgba(0,0,0,0.1); }
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
      font-size: 0.85rem; display: flex; align-items: center; gap: 4px; letter-spacing: 0.5px; text-transform: uppercase; font-weight: 500; transition: all 0.3s;
    }
    body.dark .dark-mode-btn { background-color: #f0f0f0; color: #2b2b2b; }
    .dark-mode-btn:hover { transform: translateY(-2px); box-shadow: 0 4px 12px rgba(0,0,0,0.15); }
  
    .whatsapp-btn {
      position: fixed; bottom: 20px; right: 20px; z-index: 999; background-color: #25D366; color: white;
      width: 50px; height: 50px; border-radius: 50%; display: flex; align-items: center; justify-content: center;
      box-shadow: 0 2px 8px rgba(0,0,0,0.2); transition: all 0.3s;
    }
    .whatsapp-btn:hover { background-color: #128C7E; transform: scale(1.1); }
    .whatsapp-btn i { font-size: 25px; }
  
    .results-table-container { margin-top: 15px; }
    .table-wrapper {
      overflow-x: auto; max-height: 250px; overflow-y: auto; margin-top: 12px; border-radius: 6px;
      box-shadow: 0 1px 2px rgba(0,0,0,0.1); -webkit-overflow-scrolling: touch; border: 1px solid #e0e0e0;
    }
    
    #tablaResultados { width: 100%; border-collapse: collapse; font-size: 0.85rem; }
    #tablaResultados th, #tablaResultados td { padding: 10px 12px; text-align: right; border-bottom: 1px solid #eee; }
    #tablaResultados th { background-color: var(--primario); color: white; position: sticky; top: 0; text-align: center; font-size: 0.85rem; font-weight: 500; }
    #tablaResultados tr:nth-child(even) { background-color: #f9f9f9; }
    #tablaResultados tr:hover { background-color: #f1f1f1; }
    
    body.dark #tablaResultados th { background-color: var(--secundario); }
    body.dark #tablaResultados tr:nth-child(even) { background-color: #2a2a2a; }
    body.dark #tablaResultados tr:hover { background-color: #333; }
    body.dark #tablaResultados th, body.dark #tablaResultados td { border-color: #444; }
    
    /* FAQ */
    .faq-item { margin-bottom: 10px; border-radius: 6px; overflow: hidden; border: 1px solid #e0e0e0; }
    body.dark .faq-item { border-color: #444; }
    .faq-question {
      width: 100%; padding: 12px 15px; text-align: left; background-color: var(--terciario); border: none; cursor: pointer;
      font-weight: 500; font-size: 0.95rem; display: flex; justify-content: space-between; align-items: center; transition: background-color 0.3s;
    }
    body.dark .faq-question { background-color: #3a3a3a; }
    .faq-question:hover { background-color: #f8d7e5; }
    body.dark .faq-question:hover { background-color: #4a4a4a; }
    .faq-question::after { content: '+'; font-size: 1.2rem; transition: transform 0.3s; }
    .faq-question.active::after { content: '-'; }
    .faq-answer { padding: 0; max-height: 0; overflow: hidden; transition: max-height 0.3s ease-out, padding 0.3s ease; background-color: white; }
    body.dark .faq-answer { background-color: #1e1e1e; }
    .faq-answer.show { padding: 15px; max-height: 1000px; }
    .faq-answer p { margin-top: 0; margin-bottom: 10px; }
    
    /* Responsive */
    @media (min-width: 768px) {
      .calculadora-grid { grid-template-columns: 1.1fr 2fr; }
      .chart-container { height: 400px; }
      .export-buttons { flex-direction: row; }
    }
    
    @media (max-width: 768px) {
      body { padding: 10px; min-width: 100%; overflow-x: hidden; }
      .calculadora-grid { display: flex; flex-direction: column; gap: 10px; }
      .input-card, .result-card, .chart-container { width: 100%; box-sizing: border-box; }
      .hero-logo { max-height: 70px; }
      .table-wrapper { width: 100%; display: block; }
      #tablaResultados { min-width: 600px; }
      .chart-container { height: 300px; padding: 10px; }
      .summary-grid { grid-template-columns: 1fr 1fr; gap: 10px; }
      .export-buttons { flex-direction: column; }
      .export-btn { width: 100%; margin-bottom: 10px; }
    }

    @media (max-width: 480px) {
      .summary-grid { grid-template-columns: 1fr; }
      .chart-container { height: 250px; }
      input, select { font-size: 16px; } /* Ajuste de zoom iOS */
    }

    * { max-width: 100%; box-sizing: border-box; }
    .calculadora-grid { width: 100%; overflow: hidden; }
  </style>
</head>
<body>
  <button class="dark-mode-btn" onclick="toggleDarkMode()"><i class="fas fa-moon"></i> Modo Oscuro</button>

  <div class="logo-hero">
    <img src="SOLUNA OFICIAL.jpg" alt="Logo Soluna" class="hero-logo">
  </div>

  <!-- NAVEGACIÓN PASO A PASO (TIPO PESTAÑAS) -->
  <div class="step-nav-container">
    <h3 class="step-nav-title">Guía paso a paso · Selecciona la característica</h3>
    <div class="step-nav">
      <button class="step-btn active" onclick="mostrarSeccion('sec-cera', this)">1 Cera</button>
      <button class="step-btn" onclick="mostrarSeccion('sec-fragancia', this)">2 Fragancia</button>
      <button class="step-btn" onclick="mostrarSeccion('sec-colorante', this)">3 Colorante</button>
      <button class="step-btn" onclick="mostrarSeccion('sec-materiales', this)">4 Materiales</button>
      <button class="step-btn" onclick="mostrarSeccion('sec-manoobra', this)">5 Mano de Obra</button>
      <button class="step-btn" onclick="mostrarSeccion('sec-produccion', this)">6 Producción</button>
    </div>
  </div>

  <div class="calculadora-grid">
    <!-- Columna izquierda - Inputs -->
    <div class="input-section">
      
      <!-- SECCIÓN 1: CERA -->
      <div id="sec-cera" class="input-card step-section active">
        <h3><i class="fas fa-fire"></i> 1. Cera
          <div class="tooltip-container"><i class="fas fa-question-circle tooltip-icon"></i><span class="tooltip-text">Información sobre los costos de cera para tus velas</span></div>
        </h3>
        <div class="input-group">
          <label for="costoCera">Costo de 1kg de cera ($):</label>
          <input type="number" id="costoCera" step="0.01" placeholder="0.00" value="100.00">
        </div>
        <div class="input-group">
          <label for="gramosCera">Cera por vela (gramos):</label>
          <input type="number" id="gramosCera" step="0.1" placeholder="0" value="150">
        </div>
      </div>

      <!-- SECCIÓN 2: FRAGANCIA -->
      <div id="sec-fragancia" class="input-card step-section">
        <h3><i class="fas fa-wind"></i> 2. Fragancia
          <div class="tooltip-container"><i class="fas fa-question-circle tooltip-icon"></i><span class="tooltip-text">Costos relacionados con las fragancias</span></div>
        </h3>
        <div class="input-group">
          <label for="costoFragancia">Costo de la fragancia ($):</label>
          <input type="number" id="costoFragancia" step="0.01" placeholder="0.00" value="200.00">
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
          <input type="number" id="tamanoFragancia" step="1" placeholder="0" value="250">
          <div id="fragancia-unit-display" style="font-size: 0.8rem; color: #666; margin-top: 5px;">ml</div>
        </div>
        <div class="input-group">
          <label for="porcentajeFragancia">Porcentaje de fragancia (%):</label>
          <input type="number" id="porcentajeFragancia" step="0.1" placeholder="0" value="10">
        </div>
      </div>

      <!-- SECCIÓN 3: COLORANTE -->
      <div id="sec-colorante" class="input-card step-section">
        <h3><i class="fas fa-palette"></i> 3. Colorante
          <div class="tooltip-container"><i class="fas fa-question-circle tooltip-icon"></i><span class="tooltip-text">Costos de colorantes para tus velas</span></div>
        </h3>
        <div class="input-group">
          <label for="costoColorante">Costo del colorante ($):</label>
          <input type="number" id="costoColorante" step="0.01" placeholder="0.00" value="10.00">
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
          <input type="number" id="cantidadColorante" step="1" placeholder="0" value="10">
          <div id="colorante-unit-display" style="font-size: 0.8rem; color: #666; margin-top: 5px;">ml</div>
        </div>
        <div class="input-group">
          <label for="usoColorante">Uso por vela:</label>
          <input type="number" id="usoColorante" step="0.1" placeholder="0" value="0.1">
          <div id="uso-colorante-unit-display" style="font-size: 0.8rem; color: #666; margin-top: 5px;">ml</div>
        </div>
      </div>

      <!-- SECCIÓN 4: MATERIALES -->
      <div id="sec-materiales" class="input-card step-section">
        <h3><i class="fas fa-tools"></i> 4. Materiales y Empaque
          <div class="tooltip-container"><i class="fas fa-question-circle tooltip-icon"></i><span class="tooltip-text">Costos de materiales y empaque</span></div>
        </h3>
        <div class="input-group">
          <label for="costoMecha">Costo de mecha por vela ($):</label>
          <input type="number" id="costoMecha" step="0.01" placeholder="0.00" value="2.50">
        </div>
        <div class="input-group">
          <label for="costoFrasco">Costo del frasco por vela ($):</label>
          <input type="number" id="costoFrasco" step="0.01" placeholder="0.00" value="8.00">
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

      <!-- SECCIÓN 5: MANO DE OBRA -->
      <div id="sec-manoobra" class="input-card step-section">
        <h3><i class="fas fa-user-tie"></i> 5. Mano de Obra e Indirectos
          <div class="tooltip-container"><i class="fas fa-question-circle tooltip-icon"></i><span class="tooltip-text">Configuración de mano de obra y costos indirectos</span></div>
        </h3>
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
          <input type="number" id="costoIndirecto" step="0.01" value="3.00" placeholder="0.00">
        </div>
      </div>

      <!-- SECCIÓN 6: PRODUCCIÓN Y EXPORTACIÓN -->
      <div id="sec-produccion" class="input-card step-section">
        <h3><i class="fas fa-chart-line"></i> 6. Producción y Margen
          <div class="tooltip-container"><i class="fas fa-question-circle tooltip-icon"></i><span class="tooltip-text">Configuración de producción y márgenes de ganancia</span></div>
        </h3>
        <div class="input-group">
          <label for="margen">Margen de beneficio deseado (%):</label>
          <input type="number" id="margen" step="0.1" value="35" placeholder="35">
        </div>
        <div class="input-group">
          <label for="cantidadVelas">Número de velas a producir:</label>
          <input type="number" id="cantidadVelas" step="1" value="1" placeholder="1">
        </div>
        
        <div class="export-buttons">
          <button onclick="exportToPDF()" class="export-btn pdf-btn">
            <i class="fas fa-file-pdf"></i> Exportar a PDF
          </button>
          <button onclick="exportToExcel()" class="export-btn excel-btn">
            <i class="fas fa-file-excel"></i> Exportar a Excel
          </button>
        </div>
      </div>
    </div>

    <!-- Columna derecha - Resultados (Siempre visible) -->
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
          <div class="summary-item total"><div class="summary-icon"><i class="fas fa-tag"></i></div><div class="summary-content"><div class="summary-label">Precio de venta sugerido</div><div class="summary-value" id="res-precio-venta">$0.00</div></div></div>
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
              <tbody><!-- Filas dinámicas --></tbody>
            </table>
          </div>
        </div>
      </div>
    </div>
  </div>

  <div class="faq-section" style="margin-top: 40px; margin-bottom: 60px;">
    <div class="input-card">
      <h3><i class="fas fa-question-circle"></i> Preguntas frecuentes</h3>
      <div class="faq-item">
        <button class="faq-question">¿Cómo calcular el porcentaje de fragancia?</button>
        <div class="faq-answer">
          <p>El porcentaje de fragancia se calcula en relación al peso de la cera. Por ejemplo:</p>
          <ul>
            <li>Si usas 150g de cera y 8% de fragancia: 150g × 8% = 12g de fragancia por vela</li>
            <li>Para un bote de 500ml de fragancia a $15: $15 ÷ 500ml = $0.03 por ml</li>
            <li>Costo de fragancia por vela: 12g × $0.03 = $0.36</li>
          </ul>
        </div>
      </div>
      <div class="faq-item">
        <button class="faq-question">¿Qué incluyen los costos indirectos?</button>
        <div class="faq-answer">
          <p>Los costos indirectos son gastos generales de producción que no están directamente vinculados a una vela específica, pero que son necesarios para el funcionamiento de tu negocio:</p>
          <ul>
            <li>Electricidad para derretir la cera</li>
            <li>Materiales de empaque adicionales</li>
            <li>Mantenimiento de equipos</li>
            <li>Gastos de taller o espacio de trabajo</li>
            <li>Herramientas y utensilios</li>
          </ul>
        </div>
      </div>
      <div class="faq-item">
        <button class="faq-question">¿Cómo determinar el margen de beneficio adecuado?</button>
        <div class="faq-answer">
          <p>El margen de beneficio depende de varios factores:</p>
          <ul>
            <li><strong>30-40%:</strong> Margen estándar para productos artesanales</li>
            <li><strong>40-60%:</strong> Para productos premium o especializados</li>
            <li><strong>20-30%:</strong> Para productos de entrada o promocionales</li>
          </ul>
          <p>Considera tus costos fijos, competencia y valor percibido al establecer tu margen.</p>
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
    
    const unidadesConfig = {
      fragancia: { unidad: 'ml', factor: 1 },
      colorante: { unidad: 'ml', factor: 1 },
      manoObra: { tipo: 'porcentaje' }
    };

    // Función de Lógica de Pestañas (Navegación)
    function mostrarSeccion(sectionId, btnElement) {
      // Ocultar todos los formularios
      document.querySelectorAll('.step-section').forEach(sec => {
        sec.classList.remove('active');
      });
      // Quitar el color activo de todos los botones
      document.querySelectorAll('.step-btn').forEach(btn => {
        btn.classList.remove('active');
      });
      
      // Mostrar el formulario deseado y pintar su botón
      document.getElementById(sectionId).classList.add('active');
      btnElement.classList.add('active');
    }

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
          
          document.querySelectorAll(`.unit-option[data-target="${target}"]`).forEach(opt => {
            opt.classList.remove('active');
          });
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

    document.addEventListener('DOMContentLoaded', function() {
      configurarSelectoresUnidad();
      actualizarVisibilidadManoObra();
      actualizarDisplayUnidades('fragancia');
      actualizarDisplayUnidades('colorante');
      
      document.querySelectorAll('input').forEach(input => {
        input.addEventListener('input', calcularCostos);
      });

      document.querySelectorAll('.faq-question').forEach(button => {
        button.addEventListener('click', () => {
          const answer = button.nextElementSibling;
          document.querySelectorAll('.faq-answer').forEach(item => {
            if (item !== answer && item.classList.contains('show')) {
              item.classList.remove('show');
              item.previousElementSibling.classList.remove('active');
            }
          });
          button.classList.toggle('active');
          answer.classList.toggle('show');
        });
      });

      calcularCostos();
    });

    function toggleDarkMode() {
      document.body.classList.toggle("dark");
      const icon = document.querySelector('.dark-mode-btn i');
      if (document.body.classList.contains("dark")) {
        icon.classList.remove('fa-moon');
        icon.classList.add('fa-sun');
      } else {
        icon.classList.remove('fa-sun');
        icon.classList.add('fa-moon');
      }
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
      
      let costoManoObraPorVela = 0;
      if (unidadesConfig.manoObra.tipo === 'porcentaje') {
        costoManoObraPorVela = costosMaterialesTotales * (manoObraPorcentaje / 100);
      } else {
        costoManoObraPorVela = manoObraFijo;
      }

      const costoTotalUnitario = costosMaterialesTotales + costoManoObraPorVela + costoIndirecto;
      const precioVentaUnitario = costoTotalUnitario * (1 + margen / 100);
      const gananciaUnitario = precioVentaUnitario - costoTotalUnitario;

      const costoTotal = costoTotalUnitario * cantidadVelas;
      const precioTotalVenta = precioVentaUnitario * cantidadVelas;
      const gananciaTotal = gananciaUnitario * cantidadVelas;

      resultadosCalculo = {
        cantidadVelas, costoCeraPorVela, costoFraganciaPorVela, costoColorantePorVela,
        costosMateriales, costoManoObraPorVela, costoIndirecto, costoTotalUnitario,
        margen, precioVentaUnitario, gananciaUnitario, costoTotal, precioTotalVenta, gananciaTotal
      };

      document.getElementById('res-cera').textContent = formatCurrency(costoCeraPorVela);
      document.getElementById('res-fragancia').textContent = formatCurrency(costoFraganciaPorVela);
      document.getElementById('res-colorante').textContent = formatCurrency(costoColorantePorVela);
      document.getElementById('res-materiales').textContent = formatCurrency(costosMateriales);
      document.getElementById('res-mano-obra').textContent = formatCurrency(costoManoObraPorVela);
      document.getElementById('res-total-unitario').textContent = formatCurrency(costoTotalUnitario);
      document.getElementById('res-precio-venta').textContent = formatCurrency(precioVentaUnitario);

      generarGraficoCostos();
      generarTablaCostos();
    }

    function generarGraficoCostos() {
      const ctx = document.getElementById('graficaCostos').getContext('2d');
      const esMovil = window.innerWidth < 768;
      if (chartCostos) chartCostos.destroy();

      const data = {
        labels: ['Cera', 'Fragancia', 'Colorante', 'Materiales', 'Mano Obra', 'Indirectos'],
        datasets: [{
          data: [
            resultadosCalculo.costoCeraPorVela || 0, resultadosCalculo.costoFraganciaPorVela || 0,
            resultadosCalculo.costoColorantePorVela || 0, resultadosCalculo.costosMateriales || 0,
            resultadosCalculo.costoManoObraPorVela || 0, resultadosCalculo.costoIndirecto || 0
          ],
          backgroundColor: [
            'rgba(217, 122, 155, 0.8)', 'rgba(227, 141, 168, 0.8)', 'rgba(255, 234, 240, 0.8)',
            'rgba(82, 171, 152, 0.8)', 'rgba(155, 89, 182, 0.8)', 'rgba(52, 152, 219, 0.8)'
          ],
          borderColor: [
            'rgba(217, 122, 155, 1)', 'rgba(227, 141, 168, 1)', 'rgba(255, 234, 240, 1)',
            'rgba(82, 171, 152, 1)', 'rgba(155, 89, 182, 1)', 'rgba(52, 152, 219, 1)'
          ],
          borderWidth: 1
        }]
      };

      chartCostos = new Chart(ctx, {
        type: 'pie',
        data: data,
        options: {
          responsive: true, maintainAspectRatio: false,
          plugins: { legend: { position: esMovil ? 'bottom' : 'right', labels: { boxWidth: 12, padding: 20, usePointStyle: true } } }
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
        { nombre: 'Etiqueta principal', valor: obtenerValorNumerico('costoEtiqueta', 0) },
        { nombre: 'Rotulación tapa/frasco', valor: obtenerValorNumerico('costoRotulacion', 0) },
        { nombre: 'Empaque', valor: obtenerValorNumerico('costoEmpaque', 0) },
        { nombre: 'Protectores de cera', valor: obtenerValorNumerico('costoProtector', 0) },
        { nombre: 'Mano de obra', valor: resultadosCalculo.costoManoObraPorVela || 0 },
        { nombre: 'Costos indirectos', valor: obtenerValorNumerico('costoIndirecto', 0) }
      ];

      conceptos.forEach(concepto => {
        if (concepto.valor > 0 || total > 0) {
          const porcentaje = total > 0 ? ((concepto.valor / total) * 100).toFixed(1) : '0.0';
          const fila = document.createElement('tr');
          fila.innerHTML = `<td style="text-align: left;">${concepto.nombre}</td><td>${formatCurrency(concepto.valor)}</td><td>${porcentaje}%</td>`;
          tbody.appendChild(fila);
        }
      });

      const filaTotal = document.createElement('tr');
      filaTotal.style.fontWeight = 'bold';
      filaTotal.style.backgroundColor = 'rgba(82, 171, 152, 0.1)';
      filaTotal.innerHTML = `<td style="text-align: left;">TOTAL</td><td>${formatCurrency(total)}</td><td>100%</td>`;
      tbody.appendChild(filaTotal);
    }

    function formatCurrency(value) {
      const numero = isNaN(value) || value === null ? 0 : value;
      return new Intl.NumberFormat('es-MX', { style: 'currency', currency: 'MXN', minimumFractionDigits: 2, maximumFractionDigits: 2 }).format(numero);
    }

    async function exportToPDF() {
      const { jsPDF } = window.jspdf;
      const doc = new jsPDF({ orientation: 'portrait', unit: 'mm', format: 'a4' });
      const primaryColor = '#d97a9b'; const accentColor = '#52ab98'; const textColor = '#333333';
      const margin = 15; const pageWidth = doc.internal.pageSize.getWidth(); const contentWidth = pageWidth - margin * 2; let yPosition = margin;

      doc.setFontSize(18); doc.setTextColor(primaryColor); doc.setFont('helvetica', 'bold');
      doc.text('Reporte de Costos de Velas - Soluna', pageWidth / 2, yPosition, { align: 'center' });
      doc.setFontSize(10); doc.setTextColor(100); doc.setFont('helvetica', 'normal'); yPosition += 8;
      doc.text(`Generado el: ${new Date().toLocaleDateString('es-MX')}`, pageWidth / 2, yPosition, { align: 'center' });
      yPosition += 15; doc.setFontSize(14); doc.setTextColor(primaryColor);
      doc.text('Resumen de Costos', margin, yPosition); doc.setDrawColor(primaryColor); doc.setLineWidth(0.3);
      doc.line(margin, yPosition + 2, margin + 40, yPosition + 2); yPosition += 10;

      const summaryData = [
        { label: 'Costo unitario total', value: resultadosCalculo.costoTotalUnitario || 0 },
        { label: 'Precio de venta sugerido', value: resultadosCalculo.precioVentaUnitario || 0 },
        { label: 'Ganancia por unidad', value: resultadosCalculo.gananciaUnitario || 0 },
        { label: `Total para ${resultadosCalculo.cantidadVelas || 0} velas`, value: resultadosCalculo.costoTotal || 0 },
        { label: 'Venta total sugerida', value: resultadosCalculo.precioTotalVenta || 0 },
        { label: 'Ganancia total', value: resultadosCalculo.gananciaTotal || 0 }
      ];

      summaryData.forEach((item, i) => {
        const isTotal = i >= 3;
        doc.setFontSize(isTotal ? 11 : 10); doc.setFont('helvetica', isTotal ? 'bold' : 'normal'); doc.setTextColor(isTotal ? accentColor : textColor);
        doc.text(item.label, margin, yPosition); doc.text(formatCurrency(item.value), pageWidth - margin, yPosition, { align: 'right' }); yPosition += 6;
      });
      yPosition += 10;

      try {
        const canvas = document.getElementById('graficaCostos');
        const canvasImage = await html2canvas(canvas, { scale: 2, logging: false, useCORS: true });
        const imgData = canvasImage.toDataURL('image/png');
        const imgProps = doc.getImageProperties(imgData);
        const imgWidth = contentWidth * 0.6; const imgHeight = (imgProps.height * imgWidth) / imgProps.width;
        doc.addImage(imgData, 'PNG', margin + (contentWidth - imgWidth) / 2, yPosition, imgWidth, imgHeight); yPosition += imgHeight + 10;
      } catch (error) { console.error("Error al generar gráfico:", error); }

      doc.setFontSize(14); doc.setTextColor(primaryColor); doc.text('Desglose de Costos Unitarios', margin, yPosition);
      doc.setDrawColor(primaryColor); doc.line(margin, yPosition + 2, margin + 60, yPosition + 2); yPosition += 10;

      const tableData = []; const headers = ['Concepto', 'Costo Unitario', 'Porcentaje'];
      const rows = document.querySelectorAll('#tablaResultados tbody tr');
      rows.forEach(row => { const cells = row.querySelectorAll('td'); if (cells.length === 3) { tableData.push([cells[0].textContent.trim(), cells[1].textContent.trim(), cells[2].textContent.trim()]); } });

      doc.autoTable({
        head: [headers], body: tableData, startY: yPosition, theme: 'grid',
        headStyles: { fillColor: primaryColor, textColor: 255, fontStyle: 'bold', fontSize: 9 }, bodyStyles: { fontSize: 8, cellPadding: 2 },
        columnStyles: { 0: { cellWidth: 'auto', halign: 'left' }, 1: { cellWidth: 'auto', halign: 'right' }, 2: { cellWidth: 'auto', halign: 'right' } },
        margin: { left: margin, right: margin }, tableWidth: contentWidth
      });
      doc.save(`Reporte_Costos_Velas_${new Date().toISOString().slice(0,10)}.pdf`);
    }

    function exportToExcel() {
      const wb = XLSX.utils.book_new();
      const summaryData = [
        ['REPORTE DE COSTOS DE VELAS - SOLUNA'], ['Generado el:', new Date().toLocaleDateString()], [],
        ['RESUMEN DE COSTOS'], ['Costo unitario total:', formatCurrency(resultadosCalculo.costoTotalUnitario || 0)],
        ['Precio de venta sugerido:', formatCurrency(resultadosCalculo.precioVentaUnitario || 0)], ['Ganancia por unidad:', formatCurrency(resultadosCalculo.gananciaUnitario || 0)],
        [`Total para ${resultadosCalculo.cantidadVelas || 0} velas:`, formatCurrency(resultadosCalculo.costoTotal || 0)], ['Venta total sugerida:', formatCurrency(resultadosCalculo.precioTotalVenta || 0)],
        ['Ganancia total:', formatCurrency(resultadosCalculo.gananciaTotal || 0)], ['Margen aplicado:', (resultadosCalculo.margen || 35) + '%'], [], ['DESGLOSE DE COSTOS UNITARIOS']
      ];
      const tableHeaders = ['Concepto', 'Costo Unitario', 'Porcentaje']; const tableData = [tableHeaders];
      document.querySelectorAll('#tablaResultados tbody tr').forEach(row => { const rowData = []; row.querySelectorAll('td').forEach(cell => { rowData.push(cell.textContent.trim()); }); tableData.push(rowData); });
      const ws = XLSX.utils.aoa_to_sheet([...summaryData, ...tableData]);
      if(!ws['!merges']) ws['!merges'] = [];
      ws['!merges'].push({ s: { r: 0, c: 0 }, e: { r: 0, c: 2 } }); ws['!merges'].push({ s: { r: 3, c: 0 }, e: { r: 3, c: 2 } }); ws['!merges'].push({ s: { r: 12, c: 0 }, e: { r: 12, c: 2 } });
      ws['!cols'] = [{ wch: 25 }, { wch: 20 }, { wch: 15 }];
      XLSX.utils.book_append_sheet(wb, ws, "Reporte de Costos"); XLSX.writeFile(wb, 'Reporte_Costos_Velas.xlsx');
    }
  </script>
</body>
</html>
