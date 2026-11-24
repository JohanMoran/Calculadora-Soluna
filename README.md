<!DOCTYPE html>
<html lang="es">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
  <title>Calculadora de Costos - Soluna</title>
  <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.0.0/css/all.min.css">
  <link href="https://fonts.googleapis.com/css2?family=Inter:wght@300;400;500;600;700&display=swap" rel="stylesheet">
  <!-- Librerías para exportación -->
  <script src="https://cdnjs.cloudflare.com/ajax/libs/jspdf/2.5.1/jspdf.umd.min.js"></script>
  <script src="https://cdnjs.cloudflare.com/ajax/libs/jspdf-autotable/3.5.25/jspdf.plugin.autotable.min.js"></script>
  <script src="https://cdn.sheetjs.com/xlsx-0.19.3/package/dist/xlsx.full.min.js"></script>
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

    /* TEMA COTIZACIÓN MANUAL (Terracota / Soluna) */
    body.theme-manual {
      --fondo-claro: #f7f4f2;
      --primario: #a65d37;
      --secundario: #c27d57;
      --terciario: #f9ede6;
      --hover: #8a4b2a;
    }

    body.theme-manual.dark {
      --primario: #d49472;
      --secundario: #b87654;
      --terciario: #2b201a;
    }

    html, body {
      overflow-x: hidden;
      width: 100%;
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

    /* LOGO ENMARCADO Y RECORTADO */
    .logo-hero-container {
      width: 100%;
      max-width: 750px;
      height: 130px;
      margin: 15px auto 25px;
      border-radius: 12px;
      box-shadow: 0 8px 24px rgba(0,0,0,0.08);
      overflow: hidden;
      background: white;
    }

    body.dark .logo-hero-container { box-shadow: 0 8px 24px rgba(0,0,0,0.3); }

    .hero-logo {
      width: 100%;
      height: 100%;
      object-fit: cover;
      object-position: center;
      transition: all 0.3s ease;
    }

    .hero-logo:hover { transform: scale(1.02); }

    /* SWITCHER PRINCIPAL DE APPS */
    .main-app-switcher {
      display: flex; justify-content: center; gap: 12px; margin-bottom: 25px; flex-wrap: wrap;
    }
    .app-btn {
      padding: 11px 22px; font-size: 1rem; font-weight: 700; border-radius: 30px; border: 2px solid #ddd;
      background: white; color: #666; cursor: pointer; transition: all 0.3s ease; display: flex; align-items: center; gap: 8px;
    }
    body.dark .app-btn { background: #1e1e1e; border-color: #444; color: #aaa; }
    .app-btn:hover { transform: translateY(-2px); box-shadow: 0 4px 10px rgba(0,0,0,0.1); }
    .app-btn.active-main { border-color: var(--primario); background: var(--terciario); color: var(--primario); }
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

    .step-section-velas, .step-section-conc { display: none; animation: fadeIn 0.4s ease; }
    .step-section-velas.active, .step-section-conc.active { display: block; }
    @keyframes fadeIn { from { opacity: 0; transform: translateY(5px); } to { opacity: 1; transform: translateY(0); } }

    .sub-divider {
      margin: 18px 0 12px;
      padding-top: 14px;
      border-top: 1px dashed #ddd;
      color: var(--primario);
      font-size: 1rem;
      font-weight: 600;
      display: flex;
      align-items: center;
      gap: 6px;
    }
    body.dark .sub-divider { border-color: #444; }

    /* BOTONES */
    .export-buttons { display: flex; gap: 12px; margin: 15px 0; flex-wrap: wrap; }
    .export-btn { padding: 12px 18px; border: none; border-radius: 8px; font-weight: 600; cursor: pointer; display: flex; align-items: center; gap: 8px; transition: all 0.3s ease; font-size: 0.9rem; flex: 1; min-width: 150px; justify-content: center; }
    .pdf-btn { background-color: #e74c3c; color: white; } .pdf-btn:hover { background-color: #c0392b; transform: translateY(-2px); }
    .excel-btn { background-color: var(--verde); color: white; } .excel-btn:hover { background-color: var(--verde-hover); transform: translateY(-2px); }
    .add-cart-btn { background-color: var(--primario); color: white; width: 100%; margin-top: 10px; } .add-cart-btn:hover { background-color: var(--hover); transform: translateY(-2px); }
    .wa-quote-btn { background-color: #25D366; color: white; } .wa-quote-btn:hover { background-color: #128C7E; transform: translateY(-2px); }
    .clear-cart-btn { background-color: #6c757d; color: white; } .clear-cart-btn:hover { background-color: #5a6268; transform: translateY(-2px); }

    /* ESTRUCTURA GENERAL */
    .calculadora-grid { display: grid; grid-template-columns: 1fr; gap: 15px; }
    .cart-grid > div, .calculadora-grid > div { min-width: 0; width: 100%; }

    .input-card, .result-card, .cart-card { background: white; border-radius: 8px; padding: 18px; margin-bottom: 15px; box-shadow: 0 2px 8px rgba(0,0,0,0.05); border: 1px solid #e0e0e0; transition: transform 0.3s ease, box-shadow 0.3s ease; }
    body.dark .input-card, body.dark .result-card, body.dark .cart-card { background: #1e1e1e; box-shadow: 0 2px 12px rgba(0,0,0,0.1); border-color: #333; }
    .input-card h3, .result-card h3, .cart-card h3 { margin-top: 0; color: var(--primario); padding-bottom: 6px; margin-bottom: 12px; font-size: 1.1rem; display: flex; align-items: center; gap: 8px; font-weight: 600; }

    .input-group { margin-bottom: 12px; }
    .input-group label { display: block; margin-bottom: 4px; font-weight: 500; font-size: 0.9rem; }
    input, select { width: 100%; padding: 10px 12px; border: 1px solid #ddd; border-radius: 6px; background-color: #fff; transition: all 0.3s; font-size: 0.95rem; -webkit-appearance: none; box-sizing: border-box; }
    body.dark input, body.dark select { background-color: #2a2a2a; color: #e0e0e0; border-color: #555; }

    /* UNIDADES Y SWITCHES */
    .unit-selector { display: flex; gap: 10px; margin-top: 5px; }
    .unit-option { flex: 1; text-align: center; padding: 8px 5px; border: 1px solid #ddd; border-radius: 6px; cursor: pointer; font-size: 0.85rem; transition: all 0.3s; background: #f8f9fa; }
    body.dark .unit-option { background: #2a2a2a; border-color: #555; color: #e0e0e0; }
    .unit-option.active { background-color: var(--primario); color: white; border-color: var(--primario); }
    body.dark .unit-option.active { background-color: var(--secundario); border-color: var(--secundario); color: #121212; }

    /* RESUMEN DE RESULTADOS */
    .summary-grid { display: grid; grid-template-columns: repeat(auto-fit, minmax(180px, 1fr)); gap: 12px; margin-top: 10px; }
    .summary-item { background: white; border-radius: 8px; padding: 14px; display: flex; align-items: center; gap: 12px; box-shadow: 0 2px 8px rgba(0,0,0,0.05); border-left: 4px solid var(--primario); }
    body.dark .summary-item { background: #2a2a2a; }
    .summary-item.total { border-left-color: var(--verde); background-color: rgba(82, 171, 152, 0.05); }
    body.dark .summary-item.total { background-color: rgba(82, 171, 152, 0.1); }
    .summary-icon { width: 38px; height: 38px; border-radius: 50%; background-color: var(--terciario); display: flex; align-items: center; justify-content: center; color: var(--primario); font-size: 1rem; flex-shrink: 0; }
    body.dark .summary-icon { background-color: #3a3a3a; }
    .summary-item.total .summary-icon { background-color: rgba(82, 171, 152, 0.2); color: var(--verde); }
    .summary-content { flex-grow: 1; }
    .summary-label { font-size: 0.8rem; color: #666; margin-bottom: 3px; }
    body.dark .summary-label { color: #aaa; }
    .summary-value { font-size: 1.05rem; font-weight: 600; color: var(--texto-claro); }
    .summary-item.total .summary-value { color: var(--verde); font-size: 1.15rem; }

    /* CARRITO DE COTIZACIÓN */
    .cart-card { margin-top: 25px; border-top: 4px solid var(--primario); }
    .cart-grid { display: grid; grid-template-columns: 1fr; gap: 20px; }
    .cart-table { width: 100%; border-collapse: collapse; font-size: 0.88rem; margin-bottom: 15px; }
    .cart-table th, .cart-table td { padding: 10px 8px; border-bottom: 1px solid #eee; text-align: left; }
    .cart-table th { background-color: var(--terciario); color: var(--primario); font-weight: 600; }
    body.dark .cart-table th { background-color: #2a2a2a; }
    body.dark .cart-table td { border-color: #333; }
    .btn-del-item { background: #e74c3c; color: white; border: none; border-radius: 4px; padding: 4px 8px; cursor: pointer; font-size: 0.8rem; }
    .btn-del-item:hover { background: #c0392b; }

    .cart-totals-box { background: var(--terciario); padding: 15px; border-radius: 8px; margin-top: 10px; }
    body.dark .cart-totals-box { background: #262626; }
    .total-row { display: flex; justify-content: space-between; margin-bottom: 6px; font-size: 0.95rem; }
    .total-row.grand-total { font-size: 1.2rem; font-weight: 700; color: var(--primario); border-top: 1px solid #ccc; padding-top: 8px; margin-top: 8px; }
    body.dark .total-row.grand-total { border-color: #444; }

    .client-grid { display: grid; grid-template-columns: repeat(auto-fit, minmax(200px, 1fr)); gap: 10px; }
    .manual-row-2 { display: grid; grid-template-columns: 1fr 1fr; gap: 10px; }
    .manual-row-3 { display: grid; grid-template-columns: repeat(3, 1fr); gap: 10px; }
    .badge-count { background: var(--primario); color: white; font-size: 0.8rem; padding: 2px 8px; border-radius: 12px; }

    /* BOTONES FLOTANTES */
    .dark-mode-btn { position: fixed; top: 20px; right: 20px; z-index: 1000; background-color: #2b2b2b; color: white; border: none; padding: 8px 16px; border-radius: 6px; cursor: pointer; box-shadow: 0 2px 8px rgba(0,0,0,0.1); font-size: 0.85rem; display: flex; align-items: center; gap: 4px; text-transform: uppercase; font-weight: 500; transition: all 0.3s; }
    body.dark .dark-mode-btn { background-color: #f0f0f0; color: #2b2b2b; }
    .whatsapp-btn { position: fixed; bottom: 20px; right: 20px; z-index: 999; background-color: #25D366; color: white; width: 50px; height: 50px; border-radius: 50%; display: flex; align-items: center; justify-content: center; box-shadow: 0 2px 8px rgba(0,0,0,0.2); transition: all 0.3s; text-decoration: none; }
    .whatsapp-btn:hover { background-color: #128C7E; transform: scale(1.1); }
    .whatsapp-btn i { font-size: 25px; }

    /* RESPONSIVE */
    @media (min-width: 768px) {
      .calculadora-grid { grid-template-columns: 1.1fr 2fr; }
      .cart-grid { grid-template-columns: 1.5fr 1fr; }
    }
    @media (max-width: 768px) {
      body { padding: 10px; min-width: 100%; overflow-x: hidden; }
      .calculadora-grid { display: flex; flex-direction: column; gap: 10px; }
      .summary-grid { grid-template-columns: 1fr 1fr; gap: 10px; }
      .logo-hero-container { height: 85px; margin: 10px auto 20px; width: calc(100% - 20px); }
      .client-grid, .manual-row-3 { grid-template-columns: 1fr; }
      .export-buttons { flex-direction: column; }
      .export-btn { width: 100%; min-width: 0; }
      .cart-table { font-size: 0.78rem; }
      .cart-table th, .cart-table td { padding: 6px 4px; }
      .dark-mode-btn { top: 10px; right: 10px; padding: 6px 10px; font-size: 0.75rem; }
      .app-btn { padding: 9px 16px; font-size: 0.92rem; }
    }
    @media (max-width: 480px) { .summary-grid { grid-template-columns: 1fr; } input, select { font-size: 16px; } }
    * { max-width: 100%; box-sizing: border-box; }
  </style>
</head>
<body>
  <button class="dark-mode-btn" onclick="toggleDarkMode()"><i class="fas fa-moon"></i> Modo Oscuro</button>

  <div class="logo-hero-container">
    <img src="SOLUNA OFICIAL.jpg" alt="Logo Soluna" class="hero-logo" id="main-logo">
  </div>

  <!-- SELECTOR PRINCIPAL: VELAS | CONCRETO | COTIZACIÓN MANUAL -->
  <div class="main-app-switcher">
    <button id="btn-app-velas" class="app-btn active-main" onclick="switchApp('velas')">
      <i class="fas fa-fire"></i> Velas
    </button>
    <button id="btn-app-concreto" class="app-btn" onclick="switchApp('concreto')">
      <i class="fas fa-cubes"></i> Concreto
    </button>
    <button id="btn-app-manual" class="app-btn" onclick="switchApp('manual')">
      <i class="fas fa-file-signature"></i> Cotización Manual
    </button>
  </div>

  <!-- ==========================================
       INICIO APP VELAS
       ========================================== -->
  <div id="app-velas">
    <div class="step-nav-container">
      <h3 class="step-nav-title">Guía paso a paso · Velas</h3>
      <div class="step-nav">
        <button class="step-btn step-velas active" onclick="mostrarSeccionVelas('sec-cera-fragancia', this)">1 Cera y Fragancia</button>
        <button class="step-btn step-velas" onclick="mostrarSeccionVelas('sec-colorante', this)">2 Colorante</button>
        <button class="step-btn step-velas" onclick="mostrarSeccionVelas('sec-materiales', this)">3 Materiales</button>
        <button class="step-btn step-velas" onclick="mostrarSeccionVelas('sec-manoobra', this)">4 Mano de Obra</button>
        <button class="step-btn step-velas" onclick="mostrarSeccionVelas('sec-margen', this)">5 Margen</button>
        <button class="step-btn step-velas" onclick="mostrarSeccionVelas('sec-cotizacion', this)">6 Cotización</button>
      </div>
    </div>

    <div class="calculadora-grid">
      <div class="input-section">
        <div id="sec-cera-fragancia" class="input-card step-section-velas active">
          <h3><i class="fas fa-fire"></i> 1. Cera y Fragancia</h3>
          <div class="input-group"><label for="costoCera">Costo de 1kg de cera ($):</label><input type="number" id="costoCera" step="0.01" value="100.00"></div>
          <div class="input-group"><label for="gramosCera">Cera por vela (gramos):</label><input type="number" id="gramosCera" step="0.1" value="150"></div>

          <div class="sub-divider"><i class="fas fa-wind"></i> Fragancia</div>
          <div class="input-group"><label for="costoFragancia">Costo de la fragancia ($):</label><input type="number" id="costoFragancia" step="0.01" value="250.00"></div>
          <div class="input-group">
            <label>Unidad de medida:</label>
            <div class="unit-selector">
              <div class="unit-option unit-velas active" data-unit="ml" data-target="fragancia">Mililitros (ml)</div>
              <div class="unit-option unit-velas" data-unit="litros" data-target="fragancia">Litros (L)</div>
            </div>
          </div>
          <div class="input-group"><label for="tamanoFragancia">Tamaño del envase:</label><input type="number" id="tamanoFragancia" step="1" value="250"></div>
          <div class="input-group"><label for="porcentajeFragancia">Porcentaje de fragancia (%):</label><input type="number" id="porcentajeFragancia" step="0.1" value="10"></div>
        </div>

        <div id="sec-colorante" class="input-card step-section-velas">
          <h3><i class="fas fa-palette"></i> 2. Colorante</h3>
          <div class="input-group"><label for="costoColorante">Costo del colorante ($):</label><input type="number" id="costoColorante" step="0.01" value="10.00"></div>
          <div class="input-group">
            <label>Unidad de medida:</label>
            <div class="unit-selector">
              <div class="unit-option unit-velas active" data-unit="ml" data-target="colorante">Mililitros (ml)</div>
              <div class="unit-option unit-velas" data-unit="gramos" data-target="colorante">Gramos (g)</div>
            </div>
          </div>
          <div class="input-group"><label for="cantidadColorante">Tamaño del envase:</label><input type="number" id="cantidadColorante" step="1" value="10"></div>
          <div class="input-group"><label for="usoColorante">Uso por vela:</label><input type="number" id="usoColorante" step="0.1" value="0.1"></div>
        </div>

        <div id="sec-materiales" class="input-card step-section-velas">
          <h3><i class="fas fa-tools"></i> 3. Materiales y Empaque</h3>
          <div class="input-group"><label for="costoFrasco">Costo del frasco/envase por vela ($):</label><input type="number" id="costoFrasco" step="0.01" value="8.00"></div>
          <div class="input-group"><label for="costoMecha">Costo de mecha por vela ($):</label><input type="number" id="costoMecha" step="0.01" value="2.50"></div>
          <div class="input-group"><label for="costoEtiqueta">Costo de etiqueta principal ($):</label><input type="number" id="costoEtiqueta" step="0.01" value="1.50"></div>
          <div class="input-group"><label for="costoRotulacion">Rotulación de tapa/frasco ($):</label><input type="number" id="costoRotulacion" step="0.01" value="0.50"></div>
          <div class="input-group"><label for="costoEmpaque">Coste de empaque ($):</label><input type="number" id="costoEmpaque" step="0.01" value="2.00"></div>
          <div class="input-group"><label for="costoProtector">Protectores de cera ($):</label><input type="number" id="costoProtector" step="0.01" value="0.75"></div>
        </div>

        <div id="sec-manoobra" class="input-card step-section-velas">
          <h3><i class="fas fa-user-tie"></i> 4. Mano de Obra e Indirectos</h3>
          <div class="input-group">
            <label>Tipo de cálculo de mano de obra:</label>
            <div class="unit-selector">
              <div class="unit-option unit-velas" data-unit="fijo" data-target="manoObra">Monto Fijo</div>
              <div class="unit-option unit-velas active" data-unit="porcentaje" data-target="manoObra">Porcentaje</div>
            </div>
          </div>
          <div class="input-group" id="manoObraFijo-group" style="display:none;"><label for="manoObraFijo">Mano de obra fija por vela ($):</label><input type="number" id="manoObraFijo" step="0.01" value="0.00"></div>
          <div class="input-group" id="manoObraPorcentaje-group"><label for="manoObraPorcentaje">Porcentaje de mano de obra (%):</label><input type="number" id="manoObraPorcentaje" step="0.1" value="25"></div>
          <div class="input-group"><label for="costoIndirecto">Costos indirectos por vela ($):</label><input type="number" id="costoIndirecto" step="0.01" value="3.00"></div>
        </div>

        <div id="sec-margen" class="input-card step-section-velas">
          <h3><i class="fas fa-percentage"></i> 5. Margen y Utilidad</h3>
          <div class="input-group"><label for="margen">Margen de beneficio deseado (%):</label><input type="number" id="margen" step="0.1" value="35"></div>
          <div class="input-group" style="margin-top: 12px; margin-bottom: 12px; background: rgba(82,171,152,0.05); padding: 10px; border-radius: 6px; border: 1px solid rgba(82,171,152,0.2);">
            <label style="color: var(--primario); font-weight: 600;">¿Aplicar margen de utilidad al frasco?</label>
            <span style="display:block; font-size: 0.8rem; color:#666; margin-bottom: 8px;">Si eliges "No", el frasco se cobrará al costo neto.</span>
            <div class="unit-selector">
              <div class="unit-option unit-velas active" data-unit="con-utilidad" data-target="utilidadEnvase">Sí (Todo)</div>
              <div class="unit-option unit-velas" data-unit="sin-utilidad" data-target="utilidadEnvase">No (Frasco al costo)</div>
            </div>
          </div>
        </div>

        <div id="sec-cotizacion" class="input-card step-section-velas">
          <h3><i class="fas fa-file-invoice-dollar"></i> 6. Cotización</h3>
          <div class="input-group"><label for="cantidadVelas">Cantidad de velas a cotizar:</label><input type="number" id="cantidadVelas" step="1" value="1"></div>
          <div class="input-group"><label for="descVelas">Descripción para la cotización:</label><input type="text" id="descVelas" value="Vela Artesanal Aromática"></div>
          <button onclick="agregarAlCarrito('velas')" class="export-btn add-cart-btn">
            <i class="fas fa-cart-plus"></i> Agregar a la Cotización
          </button>
        </div>
      </div>

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
        <button class="step-btn step-conc" onclick="mostrarSeccionConcreto('sec-conc-quimicos', this)">2 Químicos</button>
        <button class="step-btn step-conc" onclick="mostrarSeccionConcreto('sec-conc-pigmento', this)">3 Pigmentos</button>
        <button class="step-btn step-conc" onclick="mostrarSeccionConcreto('sec-conc-acabado', this)">4 Acabado / Molde</button>
        <button class="step-btn step-conc" onclick="mostrarSeccionConcreto('sec-conc-manoobra', this)">5 Mano de Obra</button>
        <button class="step-btn step-conc" onclick="mostrarSeccionConcreto('sec-conc-margen', this)">6 Margen</button>
        <button class="step-btn step-conc" onclick="mostrarSeccionConcreto('sec-conc-cotizacion', this)">7 Cotización</button>
      </div>
    </div>

    <div class="calculadora-grid">
      <div class="input-section">
        <div id="sec-conc-mezcla" class="input-card step-section-conc active">
          <h3><i class="fas fa-cubes"></i> 1. Mezcla Base</h3>
          <div class="input-group"><label for="costoCemento">Costo de 1kg de Cemento/Jesmonite ($):</label><input type="number" id="costoCemento" step="0.01" value="20.00"></div>
          <div class="input-group"><label for="gramosCemento">Gramos de Cemento por pieza:</label><input type="number" id="gramosCemento" step="1" value="300"></div>
          <div class="input-group"><label for="costoArena">Costo de 1kg de Arena/Marmolina ($):</label><input type="number" id="costoArena" step="0.01" value="5.00"></div>
          <div class="input-group"><label for="gramosArena">Gramos de Arena/Marmolina por pieza:</label><input type="number" id="gramosArena" step="1" value="200"></div>
          <div class="input-group"><label for="costoAgua">Costo de Aditivo/Líquido por pieza ($):</label><input type="number" id="costoAgua" step="0.01" value="2.00"></div>
        </div>

        <div id="sec-conc-quimicos" class="input-card step-section-conc">
          <h3><i class="fas fa-flask"></i> 2. Químicos</h3>
          <div class="input-group"><label for="costoAcido">Ácido muriático por pieza ($):</label><input type="number" id="costoAcido" step="0.01" value="1.00"></div>
          <div class="input-group"><label for="costoCarbonato">Carbonato de sodio por pieza ($):</label><input type="number" id="costoCarbonato" step="0.01" value="0.50"></div>
        </div>

        <div id="sec-conc-pigmento" class="input-card step-section-conc">
          <h3><i class="fas fa-palette"></i> 3. Pigmento / Color</h3>
          <div class="input-group"><label for="costoPigmentoConc">Costo del Pigmento por pieza ($):</label><input type="number" id="costoPigmentoConc" step="0.01" value="1.50"></div>
        </div>

        <div id="sec-conc-acabado" class="input-card step-section-conc">
          <h3><i class="fas fa-paint-roller"></i> 4. Acabado, Sellador y Moldes</h3>
          <div class="input-group"><label for="costoSellador">Sellador / Barniz por pieza ($):</label><input type="number" id="costoSellador" step="0.01" value="3.00"></div>
          <div class="input-group"><label for="costoBaseCorcho">Base protectora (Corcho/Fieltro) ($):</label><input type="number" id="costoBaseCorcho" step="0.01" value="1.50"></div>
          <div class="input-group"><label for="desgasteMolde">Desgaste de molde por pieza ($):</label><input type="number" id="desgasteMolde" step="0.01" value="5.00"></div>
        </div>

        <div id="sec-conc-manoobra" class="input-card step-section-conc">
          <h3><i class="fas fa-user-tie"></i> 5. Mano de Obra e Indirectos</h3>
          <div class="input-group"><label for="manoObraConc">Mano de obra fija por pieza ($):</label><input type="number" id="manoObraConc" step="0.01" value="15.00"></div>
          <div class="input-group"><label for="indirectosConc">Costos indirectos (Lijas, luz, agua) ($):</label><input type="number" id="indirectosConc" step="0.01" value="4.00"></div>
        </div>

        <div id="sec-conc-margen" class="input-card step-section-conc">
          <h3><i class="fas fa-percentage"></i> 6. Margen y Utilidad</h3>
          <div class="input-group"><label for="margenConc">Margen de beneficio deseado (%):</label><input type="number" id="margenConc" step="0.1" value="40"></div>
        </div>

        <div id="sec-conc-cotizacion" class="input-card step-section-conc">
          <h3><i class="fas fa-file-invoice-dollar"></i> 7. Cotización</h3>
          <div class="input-group"><label for="cantidadConc">Cantidad de piezas a cotizar:</label><input type="number" id="cantidadConc" step="1" value="1"></div>
          <div class="input-group"><label for="descConc">Descripción para la cotización:</label><input type="text" id="descConc" value="Pieza Artesanal de Concreto"></div>
          <button onclick="agregarAlCarrito('concreto')" class="export-btn add-cart-btn">
            <i class="fas fa-cart-plus"></i> Agregar a la Cotización
          </button>
        </div>
      </div>

      <div class="results-section">
        <div class="result-card">
          <h3><i class="fas fa-calculator"></i> Resumen Concreto</h3>
          <div class="summary-grid">
            <div class="summary-item"><div class="summary-icon"><i class="fas fa-cubes"></i></div><div class="summary-content"><div class="summary-label">Costo Mezcla</div><div class="summary-value" id="res-conc-mezcla">$0.00</div></div></div>
            <div class="summary-item"><div class="summary-icon"><i class="fas fa-flask"></i></div><div class="summary-content"><div class="summary-label">Químicos</div><div class="summary-value" id="res-conc-quimicos">$0.00</div></div></div>
            <div class="summary-item"><div class="summary-icon"><i class="fas fa-paint-roller"></i></div><div class="summary-content"><div class="summary-label">Acabados/Molde</div><div class="summary-value" id="res-conc-acabado">$0.00</div></div></div>
            <div class="summary-item"><div class="summary-icon"><i class="fas fa-user-tie"></i></div><div class="summary-content"><div class="summary-label">Mano de Obra</div><div class="summary-value" id="res-conc-manoobra">$0.00</div></div></div>
            <div class="summary-item total"><div class="summary-icon"><i class="fas fa-dollar-sign"></i></div><div class="summary-content"><div class="summary-label">Costo Unitario</div><div class="summary-value" id="res-conc-total">$0.00</div></div></div>
            <div class="summary-item total"><div class="summary-icon"><i class="fas fa-tag"></i></div><div class="summary-content"><div class="summary-label">Precio Sugerido</div><div class="summary-value" id="res-conc-precio">$0.00</div></div></div>
          </div>
        </div>
      </div>
    </div>
  </div>

  <!-- ==========================================
       CARRITO DE COTIZACIÓN PARA CALCULADORAS
       ========================================== -->
  <div class="cart-card" id="panel-carrito">
    <h3>
      <i class="fas fa-file-invoice-dollar"></i> Cotización Final (Carrito)
      <span class="badge-count" id="cart-badge">0 artículos</span>
    </h3>

    <div class="cart-grid">
      <div>
        <div style="overflow-x: auto;">
          <table class="cart-table">
            <thead>
              <tr>
                <th>Artículo</th>
                <th>Descripción</th>
                <th>Cant.</th>
                <th>P. Unitario</th>
                <th>Importe</th>
                <th></th>
              </tr>
            </thead>
            <tbody id="cart-tbody">
              <tr><td colspan="6" style="text-align:center; color:#888; padding: 15px;">Aún no agregas productos. Puedes agregar varios desde el apartado de Cotización o exportar directamente el cálculo actual.</td></tr>
            </tbody>
          </table>
        </div>

        <h4 style="margin: 15px 0 8px 0; color: var(--primario); font-size: 0.95rem;">Datos del Cliente y Folio</h4>
        <div class="client-grid">
          <div class="input-group"><input type="text" id="cotFolio" placeholder="No. Cotización (ej. 100)" value="100"></div>
          <div class="input-group"><input type="text" id="clienteNombre" placeholder="Nombre del Cliente"></div>
          <div class="input-group"><input type="text" id="clienteContacto" placeholder="Teléfono o Correo"></div>
          <div class="input-group"><input type="text" id="clienteDomicilio" placeholder="Domicilio / Ciudad"></div>
        </div>
      </div>

      <div>
        <div class="input-group">
          <label for="descuentoPct">Descuento por volumen (%):</label>
          <input type="number" id="descuentoPct" step="0.5" min="0" max="100" value="0">
        </div>

        <div class="input-group">
          <label for="modoIva">Configuración de IVA (16%):</label>
          <select id="modoIva">
            <option value="sumar">Sumar +16% de IVA al subtotal</option>
            <option value="sin-iva">Sin IVA (0%)</option>
          </select>
        </div>

        <div class="cart-totals-box">
          <div class="total-row"><span>Subtotal bruto:</span><span id="cart-bruto">$0.00</span></div>
          <div class="total-row" id="row-descuento" style="display:none; color:#e74c3c;"><span>Descuento (<span id="lbl-desc-pct">0</span>%):</span><span id="cart-descuento">-$0.00</span></div>
          <div class="total-row"><span>Subtotal neto:</span><span id="cart-subtotal">$0.00</span></div>
          <div class="total-row" id="row-iva"><span>IVA (16%):</span><span id="cart-iva">$0.00</span></div>
          <div class="total-row grand-total"><span>TOTAL:</span><span id="cart-total">$0.00</span></div>
        </div>

        <div class="export-buttons">
          <button onclick="exportToPDF()" class="export-btn pdf-btn"><i class="fas fa-file-pdf"></i> PDF Oficial</button>
          <button onclick="enviarWhatsAppCotizacion()" class="export-btn wa-quote-btn"><i class="fab fa-whatsapp"></i> Enviar WhatsApp</button>
          <button onclick="vaciarCarrito()" class="export-btn clear-cart-btn"><i class="fas fa-trash"></i> Vaciar</button>
        </div>
      </div>
    </div>
  </div>

  <!-- ==========================================
       INICIO APP COTIZACIÓN MANUAL (INDEPENDIENTE)
       ========================================== -->
  <div id="app-manual" style="display: none;">
    <div class="calculadora-grid">
      <!-- Columna izquierda: Agregar artículo manual -->
      <div class="input-section">
        <div class="input-card">
          <h3><i class="fas fa-plus-circle"></i> Agregar Artículo Manual</h3>
          <div class="input-group">
            <label for="manArticulo">Artículo / Título:</label>
            <input type="text" id="manArticulo" placeholder="Ej. VELA AROMATICA, BANDEJA..." value="VELA AROMATICA">
          </div>
          <div class="input-group">
            <label for="manDescripcion">Descripción:</label>
            <input type="text" id="manDescripcion" placeholder="Ej. Vela de soya 170g en frasco Mónaco con tapa dorada">
          </div>
          <div class="manual-row-2">
            <div class="input-group">
              <label for="manPrecio">Costo unitario ($):</label>
              <input type="number" id="manPrecio" step="0.01" min="0" placeholder="0.00">
            </div>
            <div class="input-group">
              <label for="manCantidad">Cantidad:</label>
              <input type="number" id="manCantidad" step="1" min="1" value="1">
            </div>
          </div>
          <button onclick="agregarItemManual()" class="export-btn add-cart-btn">
            <i class="fas fa-cart-plus"></i> Agregar al Carrito Manual
          </button>
        </div>

        <div class="input-card">
          <h3><i class="fas fa-user"></i> Datos del Cliente</h3>
          <div class="input-group">
            <label for="manFolio">No. Cotización (Folio):</label>
            <input type="text" id="manFolio" value="100" placeholder="Ej. 100">
          </div>
          <div class="input-group">
            <label for="manClienteNombre">Nombre del Cliente:</label>
            <input type="text" id="manClienteNombre" placeholder="Nombre o empresa">
          </div>
          <div class="input-group">
            <label for="manClienteTel">Teléfono:</label>
            <input type="text" id="manClienteTel" placeholder="Ej. 33 1234 5678">
          </div>
          <div class="input-group">
            <label for="manClienteDom">Domicilio / Ciudad:</label>
            <input type="text" id="manClienteDom" placeholder="Dirección, colonia y ciudad">
          </div>
        </div>
      </div>

      <!-- Columna derecha: Carrito manual y totales -->
      <div class="results-section">
        <div class="cart-card" style="margin-top: 0;">
          <h3>
            <i class="fas fa-shopping-cart"></i> Carrito de Cotización Manual
            <span class="badge-count" id="man-cart-badge">0 artículos</span>
          </h3>

          <div style="overflow-x: auto;">
            <table class="cart-table">
              <thead>
                <tr>
                  <th>Artículo</th>
                  <th>Descripción</th>
                  <th>Cant.</th>
                  <th>P. Unitario</th>
                  <th>Importe</th>
                  <th></th>
                </tr>
              </thead>
              <tbody id="man-cart-tbody">
                <tr><td colspan="6" style="text-align:center; color:#888; padding: 20px;">Agrega uno o más artículos desde el formulario para armar tu cotización manual.</td></tr>
              </tbody>
            </table>
          </div>

          <div class="manual-row-3" style="margin-top: 15px;">
            <div class="input-group">
              <label for="manDescuentoPct">Descuento (%):</label>
              <input type="number" id="manDescuentoPct" step="0.5" min="0" max="100" value="0">
            </div>
            <div class="input-group">
              <label for="manEnvio">Costo de envío ($):</label>
              <input type="number" id="manEnvio" step="1" min="0" value="0" placeholder="0.00">
            </div>
            <div class="input-group">
              <label for="manModoIva">IVA (16%):</label>
              <select id="manModoIva">
                <option value="sumar">Sumar +16% IVA</option>
                <option value="sin-iva">Sin IVA (0%)</option>
              </select>
            </div>
          </div>

          <div class="cart-totals-box">
            <div class="total-row"><span>Subtotal bruto:</span><span id="man-bruto">$0.00</span></div>
            <div class="total-row" id="man-row-descuento" style="display:none; color:#e74c3c;"><span>Descuento (<span id="man-lbl-desc">0</span>%):</span><span id="man-descuento">-$0.00</span></div>
            <div class="total-row"><span>Subtotal neto:</span><span id="man-subtotal">$0.00</span></div>
            <div class="total-row" id="man-row-iva"><span>IVA (16%):</span><span id="man-iva">$0.00</span></div>
            <div class="total-row" id="man-row-envio" style="display:none;"><span>Costo de envío:</span><span id="man-envio">$0.00</span></div>
            <div class="total-row grand-total"><span>TOTAL:</span><span id="man-total">$0.00</span></div>
          </div>

          <div class="export-buttons">
            <button onclick="exportarPDFManual()" class="export-btn pdf-btn"><i class="fas fa-file-pdf"></i> PDF Oficial</button>
            <button onclick="enviarWhatsAppManual()" class="export-btn wa-quote-btn"><i class="fab fa-whatsapp"></i> Enviar WhatsApp</button>
            <button onclick="vaciarCarritoManual()" class="export-btn clear-cart-btn"><i class="fas fa-trash"></i> Vaciar</button>
          </div>
        </div>
      </div>
    </div>
  </div>

  <a href="https://wa.me/523331490596" class="whatsapp-btn" target="_blank" title="Contactar por WhatsApp">
    <i class="fab fa-whatsapp"></i>
  </a>

  <script>
    let resultadosCalculo = {};
    const unidadesConfig = {
      fragancia: { unidad: 'ml', factor: 1 },
      colorante: { unidad: 'ml', factor: 1 },
      manoObra: { tipo: 'porcentaje' },
      utilidadEnvase: { tipo: 'con-utilidad' }
    };

    let resultadosConcreto = {};
    let carritoCotizacion = [];
    let carritoManual = [];

    function switchApp(app) {
      document.getElementById('app-velas').style.display = 'none';
      document.getElementById('app-concreto').style.display = 'none';
      document.getElementById('app-manual').style.display = 'none';
      document.body.classList.remove('theme-concreto', 'theme-manual');

      document.getElementById('btn-app-velas').classList.remove('active-main');
      document.getElementById('btn-app-concreto').classList.remove('active-main');
      document.getElementById('btn-app-manual').classList.remove('active-main');

      if (app === 'velas') {
        document.getElementById('app-velas').style.display = 'block';
        document.getElementById('panel-carrito').style.display = 'block';
        document.getElementById('btn-app-velas').classList.add('active-main');
        calcularCostos();
        actualizarResumenCarrito();
      } else if (app === 'concreto') {
        document.getElementById('app-concreto').style.display = 'block';
        document.getElementById('panel-carrito').style.display = 'block';
        document.body.classList.add('theme-concreto');
        document.getElementById('btn-app-concreto').classList.add('active-main');
        calcularCostosConcreto();
        actualizarResumenCarrito();
      } else if (app === 'manual') {
        document.getElementById('app-manual').style.display = 'block';
        document.getElementById('panel-carrito').style.display = 'none';
        document.body.classList.add('theme-manual');
        document.getElementById('btn-app-manual').classList.add('active-main');
        actualizarResumenManual();
      }
    }

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

    function obtenerValorNumerico(id, valorPorDefecto = 0) {
      const elemento = document.getElementById(id);
      if (!elemento) return valorPorDefecto;
      const valor = parseFloat(elemento.value);
      return isNaN(valor) || valor < 0 ? valorPorDefecto : valor;
    }

    function formatCurrency(value) {
      const numero = isNaN(value) || value === null ? 0 : value;
      return new Intl.NumberFormat('es-MX', { style: 'currency', currency: 'MXN', minimumFractionDigits: 2 }).format(numero);
    }

    function toggleDarkMode() {
      document.body.classList.toggle("dark");
    }

    document.addEventListener('DOMContentLoaded', function() {
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
          } else {
            unidadesConfig[target].unidad = unit;
            unidadesConfig[target].factor = (unit === 'litros') ? 1000 : 1;
          }
          calcularCostos();
        });
      });

      document.querySelectorAll('#app-velas input').forEach(input => input.addEventListener('input', calcularCostos));
      document.querySelectorAll('#app-concreto input').forEach(input => input.addEventListener('input', calcularCostosConcreto));
      document.getElementById('descuentoPct').addEventListener('input', actualizarResumenCarrito);
      document.getElementById('modoIva').addEventListener('change', actualizarResumenCarrito);

      document.getElementById('manDescuentoPct').addEventListener('input', actualizarResumenManual);
      document.getElementById('manEnvio').addEventListener('input', actualizarResumenManual);
      document.getElementById('manModoIva').addEventListener('change', actualizarResumenManual);

      calcularCostos();
      calcularCostosConcreto();
    });

    function calcularCostosConcreto() {
      const costoCemento = obtenerValorNumerico('costoCemento', 0);
      const gramosCemento = obtenerValorNumerico('gramosCemento', 0);
      const costoArena = obtenerValorNumerico('costoArena', 0);
      const gramosArena = obtenerValorNumerico('gramosArena', 0);
      const costoAgua = obtenerValorNumerico('costoAgua', 0);
      const costoAcido = obtenerValorNumerico('costoAcido', 0);
      const costoCarbonato = obtenerValorNumerico('costoCarbonato', 0);
      const costoPigmento = obtenerValorNumerico('costoPigmentoConc', 0);
      const costoSellador = obtenerValorNumerico('costoSellador', 0);
      const costoBaseCorcho = obtenerValorNumerico('costoBaseCorcho', 0);
      const desgasteMolde = obtenerValorNumerico('desgasteMolde', 0);
      const manoObra = obtenerValorNumerico('manoObraConc', 0);
      const indirectos = obtenerValorNumerico('indirectosConc', 0);
      const margen = obtenerValorNumerico('margenConc', 40);
      const cantidad = obtenerValorNumerico('cantidadConc', 1);

      const costoMezcla = (costoCemento / 1000 * gramosCemento) + (costoArena / 1000 * gramosArena) + costoAgua;
      const costoQuimicos = costoAcido + costoCarbonato;
      const costoAcabados = costoSellador + costoBaseCorcho + desgasteMolde;
      const costoTotalUnitario = costoMezcla + costoQuimicos + costoPigmento + costoAcabados + manoObra + indirectos;
      const precioVenta = costoTotalUnitario * (1 + margen / 100);

      resultadosConcreto = { cantidad, costoMezcla, costoQuimicos, costoPigmento, costoAcabados, manoObra, indirectos, costoTotalUnitario, precioVenta };

      document.getElementById('res-conc-mezcla').textContent = formatCurrency(costoMezcla);
      document.getElementById('res-conc-quimicos').textContent = formatCurrency(costoQuimicos);
      document.getElementById('res-conc-acabado').textContent = formatCurrency(costoAcabados);
      document.getElementById('res-conc-manoobra').textContent = formatCurrency(manoObra + indirectos);
      document.getElementById('res-conc-total').textContent = formatCurrency(costoTotalUnitario);
      document.getElementById('res-conc-precio').textContent = formatCurrency(precioVenta);

      if (carritoCotizacion.length === 0) actualizarResumenCarrito();
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
      const cantidad = obtenerValorNumerico('cantidadVelas', 1);

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

      resultadosCalculo = { cantidad, costoCeraPorVela, costoFraganciaPorVela, costoColorantePorVela, costosMateriales, costoManoObraPorVela, costoIndirecto, costoTotalUnitario, precioVentaUnitario };

      document.getElementById('res-cera').textContent = formatCurrency(costoCeraPorVela);
      document.getElementById('res-fragancia').textContent = formatCurrency(costoFraganciaPorVela);
      document.getElementById('res-colorante').textContent = formatCurrency(costoColorantePorVela);
      document.getElementById('res-materiales').textContent = formatCurrency(costosMateriales);
      document.getElementById('res-total-unitario').textContent = formatCurrency(costoTotalUnitario);
      document.getElementById('res-precio-venta').textContent = formatCurrency(precioVentaUnitario);

      if (carritoCotizacion.length === 0) actualizarResumenCarrito();
    }

    // ==========================================
    // HELPER DE CÁLCULO DE TOTALES (COMPARTIDO)
    // ==========================================
    function calcularTotalesGenerico(items, descPct, modoIva, envio = 0) {
      const bruto = items.reduce((acc, item) => acc + (item.precioUnitario * item.cantidad), 0);
      const descuento = bruto * (Math.min(descPct, 100) / 100);
      const subtotal = bruto - descuento;
      const iva = (modoIva === 'sumar') ? subtotal * 0.16 : 0;
      const total = subtotal + iva + envio;

      return { items, bruto, descPct, descuento, subtotal, iva, envio, total, modoIva };
    }

    // ==========================================
    // CARRITO DE CALCULADORAS (VELAS / CONCRETO)
    // ==========================================
    function agregarAlCarrito(tipo) {
      if (tipo === 'velas') {
        const gramos = obtenerValorNumerico('gramosCera', 0);
        const desc = document.getElementById('descVelas').value.trim() || `Vela Artesanal Aromática (${gramos}g)`;
        carritoCotizacion.push({
          articulo: 'VELA AROMATICA',
          descripcion: desc,
          cantidad: resultadosCalculo.cantidad || 1,
          precioUnitario: resultadosCalculo.precioVentaUnitario || 0
        });
      } else {
        const desc = document.getElementById('descConc').value.trim() || 'Pieza Artesanal de Concreto';
        carritoCotizacion.push({
          articulo: 'PIEZA DE CONCRETO',
          descripcion: desc,
          cantidad: resultadosConcreto.cantidad || 1,
          precioUnitario: resultadosConcreto.precioVenta || 0
        });
      }
      renderizarCarrito();
      document.getElementById('panel-carrito').scrollIntoView({ behavior: 'smooth' });
    }

    function eliminarDelCarrito(index) {
      carritoCotizacion.splice(index, 1);
      renderizarCarrito();
    }

    function vaciarCarrito() {
      if (carritoCotizacion.length === 0) return;
      if (confirm('¿Vaciar todos los artículos de la cotización?')) {
        carritoCotizacion = [];
        renderizarCarrito();
      }
    }

    function obtenerItemsActivos() {
      if (carritoCotizacion.length > 0) return carritoCotizacion;
      const esVelas = document.getElementById('app-velas').style.display !== 'none';
      if (esVelas) {
        return [{
          articulo: 'VELA AROMATICA',
          descripcion: document.getElementById('descVelas').value.trim() || 'Vela Artesanal Aromática',
          cantidad: resultadosCalculo.cantidad || 1,
          precioUnitario: resultadosCalculo.precioVentaUnitario || 0
        }];
      } else {
        return [{
          articulo: 'PIEZA DE CONCRETO',
          descripcion: document.getElementById('descConc').value.trim() || 'Pieza Artesanal de Concreto',
          cantidad: resultadosConcreto.cantidad || 1,
          precioUnitario: resultadosConcreto.precioVenta || 0
        }];
      }
    }

    function renderizarCarrito() {
      const tbody = document.getElementById('cart-tbody');
      const badge = document.getElementById('cart-badge');
      badge.textContent = `${carritoCotizacion.length} artículo${carritoCotizacion.length === 1 ? '' : 's'}`;

      if (carritoCotizacion.length === 0) {
        tbody.innerHTML = '<tr><td colspan="6" style="text-align:center; color:#888; padding: 15px;">Aún no agregas productos. Puedes agregar varios desde el apartado de Cotización o exportar directamente el cálculo actual.</td></tr>';
      } else {
        tbody.innerHTML = carritoCotizacion.map((item, i) => `
          <tr>
            <td><strong>${item.articulo}</strong></td>
            <td>${item.descripcion}</td>
            <td>${item.cantidad}</td>
            <td>${formatCurrency(item.precioUnitario)}</td>
            <td>${formatCurrency(item.precioUnitario * item.cantidad)}</td>
            <td><button class="btn-del-item" onclick="eliminarDelCarrito(${i})" title="Quitar"><i class="fas fa-times"></i></button></td>
          </tr>
        `).join('');
      }
      actualizarResumenCarrito();
    }

    function actualizarResumenCarrito() {
      const t = calcularTotalesGenerico(
        obtenerItemsActivos(),
        obtenerValorNumerico('descuentoPct', 0),
        document.getElementById('modoIva').value,
        0
      );
      document.getElementById('cart-bruto').textContent = formatCurrency(t.bruto);
      document.getElementById('lbl-desc-pct').textContent = t.descPct;
      document.getElementById('cart-descuento').textContent = `-${formatCurrency(t.descuento)}`;
      document.getElementById('row-descuento').style.display = t.descPct > 0 ? 'flex' : 'none';
      document.getElementById('cart-subtotal').textContent = formatCurrency(t.subtotal);
      document.getElementById('cart-iva').textContent = formatCurrency(t.iva);
      document.getElementById('row-iva').style.display = t.modoIva === 'sin-iva' ? 'none' : 'flex';
      document.getElementById('cart-total').textContent = formatCurrency(t.total);
    }

    // ==========================================
    // CARRITO DE COTIZACIÓN MANUAL (INDEPENDIENTE)
    // ==========================================
    function agregarItemManual() {
      const articulo = document.getElementById('manArticulo').value.trim() || 'ARTÍCULO';
      const descripcion = document.getElementById('manDescripcion').value.trim();
      const precio = obtenerValorNumerico('manPrecio', 0);
      const cantidad = obtenerValorNumerico('manCantidad', 1);

      if (!descripcion) {
        alert('Por favor escribe una descripción para el artículo.');
        return;
      }
      if (precio <= 0) {
        alert('Por favor ingresa un costo unitario mayor a 0.');
        return;
      }

      carritoManual.push({
        articulo: articulo.toUpperCase(),
        descripcion: descripcion,
        cantidad: cantidad,
        precioUnitario: precio
      });

      document.getElementById('manDescripcion').value = '';
      document.getElementById('manPrecio').value = '';
      document.getElementById('manCantidad').value = '1';

      renderizarCarritoManual();
    }

    function eliminarItemManual(index) {
      carritoManual.splice(index, 1);
      renderizarCarritoManual();
    }

    function vaciarCarritoManual() {
      if (carritoManual.length === 0) return;
      if (confirm('¿Vaciar todos los artículos de la cotización manual?')) {
        carritoManual = [];
        renderizarCarritoManual();
      }
    }

    function renderizarCarritoManual() {
      const tbody = document.getElementById('man-cart-tbody');
      const badge = document.getElementById('man-cart-badge');
      badge.textContent = `${carritoManual.length} artículo${carritoManual.length === 1 ? '' : 's'}`;

      if (carritoManual.length === 0) {
        tbody.innerHTML = '<tr><td colspan="6" style="text-align:center; color:#888; padding: 20px;">Agrega uno o más artículos desde el formulario para armar tu cotización manual.</td></tr>';
      } else {
        tbody.innerHTML = carritoManual.map((item, i) => `
          <tr>
            <td><strong>${item.articulo}</strong></td>
            <td>${item.descripcion}</td>
            <td>${item.cantidad}</td>
            <td>${formatCurrency(item.precioUnitario)}</td>
            <td>${formatCurrency(item.precioUnitario * item.cantidad)}</td>
            <td><button class="btn-del-item" onclick="eliminarItemManual(${i})" title="Quitar"><i class="fas fa-times"></i></button></td>
          </tr>
        `).join('');
      }
      actualizarResumenManual();
    }

    function actualizarResumenManual() {
      const t = calcularTotalesGenerico(
        carritoManual,
        obtenerValorNumerico('manDescuentoPct', 0),
        document.getElementById('manModoIva').value,
        obtenerValorNumerico('manEnvio', 0)
      );
      document.getElementById('man-bruto').textContent = formatCurrency(t.bruto);
      document.getElementById('man-lbl-desc').textContent = t.descPct;
      document.getElementById('man-descuento').textContent = `-${formatCurrency(t.descuento)}`;
      document.getElementById('man-row-descuento').style.display = t.descPct > 0 ? 'flex' : 'none';
      document.getElementById('man-subtotal').textContent = formatCurrency(t.subtotal);
      document.getElementById('man-iva').textContent = formatCurrency(t.iva);
      document.getElementById('man-row-iva').style.display = t.modoIva === 'sin-iva' ? 'none' : 'flex';
      document.getElementById('man-envio').textContent = formatCurrency(t.envio);
      document.getElementById('man-row-envio').style.display = t.envio > 0 ? 'flex' : 'none';
      document.getElementById('man-total').textContent = formatCurrency(t.total);
    }

    // ==========================================
    // EXPORTACIÓN A PDF OFICIAL (COMPARTIDA)
    // ==========================================
    async function generarPDFOficial({ t, folio, nombre, domicilio, contacto, primaryColor, rgb }) {
      const { jsPDF } = window.jspdf;
      const doc = new jsPDF({ orientation: 'portrait', unit: 'mm', format: 'letter' });

      // 1. HEADER
      doc.setFontSize(8);
      doc.setTextColor(80);
      doc.text("ANA PAULA VILLASEÑOR GONZALEZ", 15, 20);
      doc.text("VILLA CORAL 21, SANTA ANA TEPETITLÁN, 45230 ZAPOPAN, JAL.", 15, 24);
      doc.text("CORREO: ANAPAULAVIGON@GMAIL.COM", 15, 28);
      doc.text("CEL: 33 3149 0596", 15, 32);

      const fechaActual = new Date().toLocaleDateString('es-MX');
      doc.text(`No. COTIZACION: #${folio}`, 160, 20);
      doc.text(`FECHA: ${fechaActual}`, 160, 24);

      // LOGO CENTRADO
      try {
        const logoImg = document.getElementById('main-logo');
        const canvas = document.createElement('canvas');
        canvas.width = logoImg.naturalWidth;
        canvas.height = logoImg.naturalHeight;
        canvas.getContext('2d').drawImage(logoImg, 0, 0);
        const logoData = canvas.toDataURL('image/jpeg');
        doc.addImage(logoData, 'JPEG', 58, 38, 100, 30);
      } catch (e) { console.error("No se pudo cargar el logo al PDF."); }

      // 2. DATOS DEL CLIENTE
      doc.setFontSize(10);
      doc.setFont('helvetica', 'bold');
      doc.setTextColor(primaryColor);
      doc.text("DATOS DE CLIENTE", 15, 80);
      doc.setDrawColor(rgb[0], rgb[1], rgb[2]);
      doc.line(15, 82, 195, 82);

      doc.setFont('helvetica', 'normal');
      doc.setTextColor(50);
      doc.text(`NOMBRE: ${nombre || "___________________________"}`, 15, 89);
      doc.text(`Domicilio: ${domicilio || "___________________________"}`, 15, 95);
      doc.text("Lugar de Emisión: Zapopan, Jalisco", 15, 101);
      doc.text(`Contacto: ${contacto || "___________________________"}`, 15, 107);

      // 3. DATOS DE COTIZACIÓN
      doc.setFont('helvetica', 'bold');
      doc.setTextColor(primaryColor);
      doc.text("DATOS DE COTIZACION", 15, 118);
      doc.line(15, 120, 195, 120);

      const bodyRows = t.items.map(item => [
        item.articulo,
        item.descripcion,
        item.cantidad.toString(),
        formatCurrency(item.precioUnitario),
        formatCurrency(item.precioUnitario * item.cantidad)
      ]);

      if (t.descPct > 0) {
        bodyRows.push([
          'DESCUENTO',
          `Descuento (${t.descPct}%)`,
          '1',
          `${t.descPct}%`,
          `-${formatCurrency(t.descuento)}`
        ]);
      }

      doc.autoTable({
        startY: 125,
        head: [['Articulo', 'Descripción', 'Cantidad', 'Precio Unitario', 'Importe']],
        body: bodyRows,
        theme: 'plain',
        headStyles: { fillColor: [255, 255, 255], textColor: [0, 0, 0], fontStyle: 'bold', lineWidth: 0.1, lineColor: [200, 200, 200] },
        bodyStyles: { textColor: [50, 50, 50], fontSize: 9 },
        styles: { cellPadding: 3 }
      });

      let finalY = doc.lastAutoTable.finalY + 8;
      doc.setFontSize(10);
      doc.setFont('helvetica', 'bold');
      doc.setTextColor(50);

      doc.text("SUBTOTAL", 138, finalY);
      doc.text(formatCurrency(t.subtotal), 195, finalY, { align: 'right' });

      if (t.modoIva !== 'sin-iva') {
        finalY += 6;
        doc.text("IVA 16%", 138, finalY);
        doc.text(formatCurrency(t.iva), 195, finalY, { align: 'right' });
      }

      if (t.envio > 0) {
        finalY += 6;
        doc.text("ENVÍO", 138, finalY);
        doc.text(formatCurrency(t.envio), 195, finalY, { align: 'right' });
      }

      finalY += 7;
      doc.setTextColor(primaryColor);
      doc.text("TOTAL", 138, finalY);
      doc.text(formatCurrency(t.total), 195, finalY, { align: 'right' });

      // 4. CONDICIONES DE VENTA
      let cy = finalY + 15;
      if (cy > 220) { doc.addPage(); cy = 20; }

      doc.setTextColor(primaryColor);
      doc.text("CONDICIONES DE VENTA", 15, cy);
      doc.line(15, cy + 2, 195, cy + 2);

      doc.setFont('helvetica', 'normal');
      doc.setTextColor(80);
      doc.setFontSize(8);

      const condiciones = [
        "Estimado Cliente se le informa que:",
        "La presente cotización tiene vigencia de 30 días naturales.",
        "En caso de seleccionar el frasco marmoleado, se aplicará un costo adicional de $15.00 MXN por unidad.",
        "La fecha de entrega se confirmará toda vez que se realice el pago del anticipo del 50%.",
        "El tiempo de producción del pedido comienza a correr a partir del envío del comprobante de pago correspondiente.",
        "Cualquier cambio o modificación en el pedido posterior a la fecha de confirmación podrá causar demoras en el tiempo de entrega, así como generar un costo adicional.",
        "La manufactura de nuestras piezas es completamente artesanal, por lo que pueden presentarse sutiles variaciones en su apariencia final, sin que ello afecte su calidad. Estas diferencias naturales no se consideran defectos, sino parte del valor y la autenticidad que distingue a cada pieza."
      ];

      cy += 8;
      condiciones.forEach(c => {
        const lines = doc.splitTextToSize(c, 180);
        doc.text(lines, 15, cy);
        cy += (lines.length * 4) + 1.5;
      });

      doc.setFont('helvetica', 'bold');
      doc.text("GRACIAS POR SU PREFERENCIA.", 105, cy + 8, { align: 'center' });

      doc.save(`Cotizacion_Soluna_${folio}.pdf`);
    }

    function exportToPDF() {
      const esVelas = document.getElementById('app-velas').style.display !== 'none';
      const t = calcularTotalesGenerico(
        obtenerItemsActivos(),
        obtenerValorNumerico('descuentoPct', 0),
        document.getElementById('modoIva').value,
        0
      );
      generarPDFOficial({
        t,
        folio: document.getElementById('cotFolio').value.trim() || "100",
        nombre: document.getElementById('clienteNombre').value.trim(),
        domicilio: document.getElementById('clienteDomicilio').value.trim(),
        contacto: document.getElementById('clienteContacto').value.trim(),
        primaryColor: esVelas ? '#d97a9b' : '#546e7a',
        rgb: esVelas ? [217, 122, 155] : [84, 110, 122]
      });
    }

    function exportarPDFManual() {
      if (carritoManual.length === 0) {
        alert('Agrega al menos un artículo al carrito manual antes de generar el PDF.');
        return;
      }
      const t = calcularTotalesGenerico(
        carritoManual,
        obtenerValorNumerico('manDescuentoPct', 0),
        document.getElementById('manModoIva').value,
        obtenerValorNumerico('manEnvio', 0)
      );
      generarPDFOficial({
        t,
        folio: document.getElementById('manFolio').value.trim() || "100",
        nombre: document.getElementById('manClienteNombre').value.trim(),
        domicilio: document.getElementById('manClienteDom').value.trim(),
        contacto: document.getElementById('manClienteTel').value.trim(),
        primaryColor: '#a65d37',
        rgb: [166, 93, 55]
      });
    }

    // ==========================================
    // WHATSAPP (COMPARTIDO)
    // ==========================================
    function enviarWhatsAppGenerico(t, folio, cliente) {
      let msg = `*Soluna - Velas para regalar* ✨\n`;
      msg += `*Cotización #${folio}*`;
      if (cliente) msg += ` | Cliente: ${cliente}`;
      msg += `\n\n`;

      t.items.forEach(item => {
        msg += `• *${item.cantidad}x ${item.descripcion}*\n`;
        msg += `   ${formatCurrency(item.precioUnitario)} c/u = *${formatCurrency(item.precioUnitario * item.cantidad)}*\n`;
      });

      msg += `\n`;
      if (t.descPct > 0) {
        msg += `Descuento (${t.descPct}%): -${formatCurrency(t.descuento)}\n`;
      }
      msg += `Subtotal: ${formatCurrency(t.subtotal)}\n`;
      if (t.modoIva !== 'sin-iva') {
        msg += `IVA (16%): ${formatCurrency(t.iva)}\n`;
      }
      if (t.envio > 0) {
        msg += `Envío: ${formatCurrency(t.envio)}\n`;
      }
      msg += `*TOTAL: ${formatCurrency(t.total)} MXN*\n\n`;
      msg += `_Vigencia de 30 días. Se requiere 50% de anticipo para confirmar fecha de entrega._`;

      window.open(`https://wa.me/?text=${encodeURIComponent(msg)}`, '_blank');
    }

    function enviarWhatsAppCotizacion() {
      const t = calcularTotalesGenerico(
        obtenerItemsActivos(),
        obtenerValorNumerico('descuentoPct', 0),
        document.getElementById('modoIva').value,
        0
      );
      enviarWhatsAppGenerico(
        t,
        document.getElementById('cotFolio').value.trim() || "100",
        document.getElementById('clienteNombre').value.trim()
      );
    }

    function enviarWhatsAppManual() {
      if (carritoManual.length === 0) {
        alert('Agrega al menos un artículo al carrito manual antes de enviar por WhatsApp.');
        return;
      }
      const t = calcularTotalesGenerico(
        carritoManual,
        obtenerValorNumerico('manDescuentoPct', 0),
        document.getElementById('manModoIva').value,
        obtenerValorNumerico('manEnvio', 0)
      );
      enviarWhatsAppGenerico(
        t,
        document.getElementById('manFolio').value.trim() || "100",
        document.getElementById('manClienteNombre').value.trim()
      );
    }
  </script>
</body>
</html>
