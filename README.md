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

    html, body { overflow-x: hidden; width: 100%; }

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

    /* PANTALLA DE BLOQUEO CON PIN */
    #pin-overlay {
      position: fixed; inset: 0; background-color: var(--fondo-claro);
      z-index: 9999; display: flex; align-items: center; justify-content: center; padding: 20px;
    }
    .pin-card {
      background: white; padding: 30px 25px; border-radius: 14px;
      box-shadow: 0 10px 30px rgba(0,0,0,0.1); width: 100%; max-width: 360px;
      text-align: center; border: 1px solid #e0e0e0;
    }
    body.dark .pin-card { background: #1e1e1e; border-color: #333; }
    .pin-card img { width: 100%; max-height: 75px; object-fit: cover; border-radius: 8px; margin-bottom: 15px; }
    .pin-card h2 { margin: 0 0 6px; font-size: 1.25rem; color: var(--primario); }
    .pin-card p { margin: 0 0 18px; font-size: 0.88rem; color: #666; }
    body.dark .pin-card p { color: #aaa; }
    #pin-input { text-align: center; font-size: 1.4rem; letter-spacing: 6px; padding: 12px; margin-bottom: 12px; font-weight: 700; }
    #pin-error { color: #e74c3c; font-size: 0.85rem; margin-bottom: 12px; display: none; font-weight: 500; }
    #main-content { display: none; }

    /* LOGO */
    .logo-hero-container {
      width: 100%; max-width: 750px; height: 130px; margin: 15px auto 25px;
      border-radius: 12px; box-shadow: 0 8px 24px rgba(0,0,0,0.08); overflow: hidden; background: white;
    }
    body.dark .logo-hero-container { box-shadow: 0 8px 24px rgba(0,0,0,0.3); }
    .hero-logo { width: 100%; height: 100%; object-fit: cover; object-position: center; transition: all 0.3s ease; }
    .hero-logo:hover { transform: scale(1.02); }

    /* SWITCHER PRINCIPAL */
    .main-app-switcher { display: flex; justify-content: center; gap: 12px; margin-bottom: 20px; flex-wrap: wrap; }
    .app-btn {
      padding: 11px 22px; font-size: 1rem; font-weight: 700; border-radius: 30px; border: 2px solid #ddd;
      background: white; color: #666; cursor: pointer; transition: all 0.3s ease; display: flex; align-items: center; gap: 8px;
    }
    body.dark .app-btn { background: #1e1e1e; border-color: #444; color: #aaa; }
    .app-btn:hover { transform: translateY(-2px); box-shadow: 0 4px 10px rgba(0,0,0,0.1); }
    .app-btn.active-main { border-color: var(--primario); background: var(--terciario); color: var(--primario); }
    body.dark .app-btn.active-main { background: var(--terciario); color: var(--primario); border-color: var(--primario); }

    /* SUB-CATEGORÍAS DE VELAS */
    .velas-type-bar {
      display: flex; gap: 8px; justify-content: center; flex-wrap: wrap;
      margin-bottom: 18px; padding: 12px; background: white; border-radius: 10px;
      border: 1px solid #e0e0e0; box-shadow: 0 2px 6px rgba(0,0,0,0.04);
    }
    body.dark .velas-type-bar { background: #1e1e1e; border-color: #333; }
    .vela-type-btn {
      padding: 8px 14px; border-radius: 20px; border: 1.5px solid var(--secundario);
      background: transparent; color: var(--texto-claro); font-size: 0.88rem; font-weight: 600;
      cursor: pointer; display: flex; align-items: center; gap: 6px; transition: all 0.25s ease; font-family: inherit;
    }
    .vela-type-btn:hover:not(.active) { background: var(--terciario); }
    .vela-type-btn.active { background: var(--primario); border-color: var(--primario); color: white; box-shadow: 0 3px 8px rgba(217, 122, 155, 0.35); }
    body.dark .vela-type-btn.active { color: #121212; }

    .vela-subapp { display: none; animation: fadeIn 0.35s ease; }
    .vela-subapp.active { display: block; }

    .placeholder-card {
      background: white; border-radius: 10px; padding: 40px 20px; text-align: center;
      border: 2px dashed var(--secundario); margin-bottom: 20px;
    }
    body.dark .placeholder-card { background: #1e1e1e; }
    .placeholder-card i { font-size: 2.5rem; color: var(--primario); margin-bottom: 12px; }
    .placeholder-card h3 { margin: 0 0 8px; color: var(--primario); }
    .placeholder-card p { margin: 0; color: #666; font-size: 0.92rem; }
    body.dark .placeholder-card p { color: #aaa; }

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

    .step-section-frasco, .step-section-ramos, .step-section-conc { display: none; animation: fadeIn 0.4s ease; }
    .step-section-frasco.active, .step-section-ramos.active, .step-section-conc.active { display: block; }
    @keyframes fadeIn { from { opacity: 0; transform: translateY(5px); } to { opacity: 1; transform: translateY(0); } }

    .sub-divider {
      margin: 16px 0 10px; padding-top: 12px; border-top: 1px dashed #ddd;
      color: var(--primario); font-size: 0.95rem; font-weight: 600; display: flex; align-items: center; gap: 6px;
    }
    body.dark .sub-divider { border-color: #444; }

    .highlight-box {
      background: rgba(217, 122, 155, 0.08);
      border: 1px dashed var(--primario);
      padding: 10px 12px;
      border-radius: 8px;
      margin-bottom: 12px;
    }
    .highlight-box small { display: block; font-size: 0.78rem; color: #666; margin-top: 3px; }
    body.dark .highlight-box small { color: #bbb; }

    /* BOTONES */
    .export-buttons { display: flex; gap: 12px; margin: 15px 0; flex-wrap: wrap; }
    .export-btn { padding: 12px 18px; border: none; border-radius: 8px; font-weight: 600; cursor: pointer; display: flex; align-items: center; gap: 8px; transition: all 0.3s ease; font-size: 0.9rem; flex: 1; min-width: 150px; justify-content: center; }
    .pdf-btn { background-color: #e74c3c; color: white; } .pdf-btn:hover { background-color: #c0392b; transform: translateY(-2px); }
    .add-cart-btn { background-color: var(--primario); color: white; width: 100%; margin-top: 10px; } .add-cart-btn:hover { background-color: var(--hover); transform: translateY(-2px); }
    .wa-quote-btn { background-color: #25D366; color: white; } .wa-quote-btn:hover { background-color: #128C7E; transform: translateY(-2px); }
    .clear-cart-btn { background-color: #6c757d; color: white; } .clear-cart-btn:hover { background-color: #5a6268; transform: translateY(-2px); }

    /* ESTRUCTURA GENERAL */
    .calculadora-grid { display: grid; grid-template-columns: 1fr; gap: 15px; }
    .cart-grid > div, .calculadora-grid > div { min-width: 0; width: 100%; }

    .input-card, .result-card, .cart-card { background: white; border-radius: 8px; padding: 18px; margin-bottom: 15px; box-shadow: 0 2px 8px rgba(0,0,0,0.05); border: 1px solid #e0e0e0; }
    body.dark .input-card, body.dark .result-card, body.dark .cart-card { background: #1e1e1e; box-shadow: 0 2px 12px rgba(0,0,0,0.1); border-color: #333; }
    .input-card h3, .result-card h3, .cart-card h3 { margin-top: 0; color: var(--primario); padding-bottom: 6px; margin-bottom: 12px; font-size: 1.1rem; display: flex; align-items: center; gap: 8px; font-weight: 600; }

    .input-group { margin-bottom: 11px; }
    .input-group label { display: block; margin-bottom: 4px; font-weight: 500; font-size: 0.88rem; }
    input, select { width: 100%; padding: 9px 11px; border: 1px solid #ddd; border-radius: 6px; background-color: #fff; transition: all 0.3s; font-size: 0.95rem; -webkit-appearance: none; box-sizing: border-box; }
    body.dark input, body.dark select { background-color: #2a2a2a; color: #e0e0e0; border-color: #555; }

    .grid-2-inputs { display: grid; grid-template-columns: 1fr 1fr; gap: 10px; }

    /* UNIDADES */
    .unit-selector { display: flex; gap: 10px; margin-top: 5px; }
    .unit-option { flex: 1; text-align: center; padding: 8px 5px; border: 1px solid #ddd; border-radius: 6px; cursor: pointer; font-size: 0.85rem; transition: all 0.3s; background: #f8f9fa; }
    body.dark .unit-option { background: #2a2a2a; border-color: #555; color: #e0e0e0; }
    .unit-option.active { background-color: var(--primario); color: white; border-color: var(--primario); }
    body.dark .unit-option.active { background-color: var(--secundario); border-color: var(--secundario); color: #121212; }

    /* RESUMEN */
    .summary-grid { display: grid; grid-template-columns: repeat(auto-fit, minmax(175px, 1fr)); gap: 12px; margin-top: 10px; }
    .summary-item { background: white; border-radius: 8px; padding: 13px; display: flex; align-items: center; gap: 11px; box-shadow: 0 2px 8px rgba(0,0,0,0.05); border-left: 4px solid var(--primario); }
    body.dark .summary-item { background: #2a2a2a; }
    .summary-item.total { border-left-color: var(--verde); background-color: rgba(82, 171, 152, 0.05); }
    body.dark .summary-item.total { background-color: rgba(82, 171, 152, 0.1); }
    .summary-icon { width: 36px; height: 36px; border-radius: 50%; background-color: var(--terciario); display: flex; align-items: center; justify-content: center; color: var(--primario); font-size: 0.95rem; flex-shrink: 0; }
    body.dark .summary-icon { background-color: #3a3a3a; }
    .summary-item.total .summary-icon { background-color: rgba(82, 171, 152, 0.2); color: var(--verde); }
    .summary-content { flex-grow: 1; }
    .summary-label { font-size: 0.78rem; color: #666; margin-bottom: 2px; }
    body.dark .summary-label { color: #aaa; }
    .summary-value { font-size: 1.02rem; font-weight: 600; color: var(--texto-claro); }
    .summary-item.total .summary-value { color: var(--verde); font-size: 1.15rem; }

    /* CARRITO */
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
    .top-floating-btns { position: fixed; top: 15px; right: 15px; z-index: 1000; display: flex; gap: 8px; }
    .dark-mode-btn, .logout-btn {
      background-color: #2b2b2b; color: white; border: none; padding: 8px 12px; border-radius: 6px; cursor: pointer;
      box-shadow: 0 2px 8px rgba(0,0,0,0.1); font-size: 0.8rem; display: flex; align-items: center; gap: 5px;
      text-transform: uppercase; font-weight: 500; transition: all 0.3s;
    }
    .logout-btn { background-color: #c0392b; }
    .logout-btn:hover { background-color: #a93226; }
    body.dark .dark-mode-btn { background-color: #f0f0f0; color: #2b2b2b; }

    .whatsapp-btn { position: fixed; bottom: 20px; right: 20px; z-index: 999; background-color: #25D366; color: white; width: 50px; height: 50px; border-radius: 50%; display: flex; align-items: center; justify-content: center; box-shadow: 0 2px 8px rgba(0,0,0,0.2); transition: all 0.3s; text-decoration: none; }
    .whatsapp-btn:hover { background-color: #128C7E; transform: scale(1.1); }
    .whatsapp-btn i { font-size: 25px; }

    /* RESPONSIVE */
    @media (min-width: 768px) {
      .calculadora-grid { grid-template-columns: 1.15fr 1.85fr; }
      .cart-grid { grid-template-columns: 1.5fr 1fr; }
    }
    @media (max-width: 768px) {
      body { padding: 10px; min-width: 100%; overflow-x: hidden; }
      .calculadora-grid { display: flex; flex-direction: column; gap: 10px; }
      .summary-grid { grid-template-columns: 1fr 1fr; gap: 10px; }
      .logo-hero-container { height: 85px; margin: 45px auto 20px; width: calc(100% - 20px); }
      .client-grid, .manual-row-3 { grid-template-columns: 1fr; }
      .export-buttons { flex-direction: column; }
      .export-btn { width: 100%; min-width: 0; }
      .cart-table { font-size: 0.78rem; }
      .cart-table th, .cart-table td { padding: 6px 4px; }
      .top-floating-btns { top: 8px; right: 8px; }
      .dark-mode-btn, .logout-btn { padding: 6px 9px; font-size: 0.72rem; }
      .app-btn { padding: 9px 16px; font-size: 0.92rem; }
      .vela-type-btn { font-size: 0.8rem; padding: 7px 11px; }
    }
    @media (max-width: 480px) { .summary-grid, .grid-2-inputs { grid-template-columns: 1fr; } input, select { font-size: 16px; } }
    * { max-width: 100%; box-sizing: border-box; }
  </style>
</head>
<body>

  <!-- PANTALLA DE BLOQUEO POR PIN -->
  <div id="pin-overlay">
    <div class="pin-card">
      <img src="SOLUNA OFICIAL.jpg" alt="Soluna">
      <h2><i class="fas fa-lock"></i> Acceso Soluna</h2>
      <p>Ingresa tu PIN de acceso para continuar</p>
      <input type="password" id="pin-input" inputmode="numeric" maxlength="8" placeholder="••••" autocomplete="off">
      <div id="pin-error"><i class="fas fa-exclamation-circle"></i> PIN incorrecto. Intenta de nuevo.</div>
      <button onclick="verificarPin()" class="export-btn add-cart-btn" style="margin-top: 0;">
        <i class="fas fa-sign-in-alt"></i> Ingresar
      </button>
    </div>
  </div>

  <!-- CONTENIDO PRINCIPAL -->
  <div id="main-content">
    <div class="top-floating-btns">
      <button class="dark-mode-btn" onclick="toggleDarkMode()"><i class="fas fa-moon"></i> Oscuro</button>
      <button class="logout-btn" onclick="cerrarSesion()" title="Bloquear pantalla"><i class="fas fa-lock"></i> Salir</button>
    </div>

    <div class="logo-hero-container">
      <img src="SOLUNA OFICIAL.jpg" alt="Logo Soluna" class="hero-logo" id="main-logo">
    </div>

    <!-- SELECTOR PRINCIPAL -->
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
         INICIO APP VELAS (5 SUB-CATEGORÍAS)
         ========================================== -->
    <div id="app-velas">
      <div class="velas-type-bar">
        <button class="vela-type-btn active" onclick="switchVelaType('frasco', this)">
          <i class="fas fa-jar"></i> Vela de Frasco
        </button>
        <button class="vela-type-btn" onclick="switchVelaType('ramos', this)">
          <i class="fas fa-spa"></i> Ramos / Bouquet
        </button>
        <button class="vela-type-btn" onclick="switchVelaType('recuerditos', this)">
          <i class="fas fa-gift"></i> Recuerditos
        </button>
        <button class="vela-type-btn" onclick="switchVelaType('decorativas', this)">
          <i class="fas fa-star"></i> Velas Decorativas
        </button>
        <button class="vela-type-btn" onclick="switchVelaType('corporativas', this)">
          <i class="fas fa-building"></i> Corporativas
        </button>
      </div>

      <!-- ==========================================
           1. VELA DE FRASCO (COLUMNA B)
           ========================================== -->
      <div id="vela-sub-frasco" class="vela-subapp active">
        <div class="step-nav-container">
          <h3 class="step-nav-title">Guía paso a paso · Vela de Frasco</h3>
          <div class="step-nav">
            <button class="step-btn step-frasco active" onclick="mostrarSeccion('frasco', 'fr-sec-frasco', this)">1 Frasco y MO</button>
            <button class="step-btn step-frasco" onclick="mostrarSeccion('frasco', 'fr-sec-cera', this)">2 Cera y Fragancia</button>
            <button class="step-btn step-frasco" onclick="mostrarSeccion('frasco', 'fr-sec-deco', this)">3 Decoración</button>
            <button class="step-btn step-frasco" onclick="mostrarSeccion('frasco', 'fr-sec-empaque', this)">4 Empaque</button>
            <button class="step-btn step-frasco" onclick="mostrarSeccion('frasco', 'fr-sec-margen', this)">5 Margen</button>
            <button class="step-btn step-frasco" onclick="mostrarSeccion('frasco', 'fr-sec-cot', this)">6 Cotización</button>
          </div>
        </div>

        <div class="calculadora-grid">
          <div class="input-section">
            <!-- PASO 1: FRASCO -->
            <div id="fr-sec-frasco" class="input-card step-section-frasco active">
              <h3><i class="fas fa-jar"></i> 1. Frasco y Mano de Obra</h3>
              <div class="highlight-box">
                <div class="input-group" style="margin-bottom:0;">
                  <label for="fr-frasco"><strong>Frasco ($) — Se duplica al final (x2):</strong></label>
                  <input type="number" id="fr-frasco" step="0.01" value="0.00">
                </div>
                <small>No entra en MO ni en margen; se suma x2 directamente al precio final.</small>
              </div>
              <div class="grid-2-inputs">
                <div class="input-group"><label for="fr-mecha">Mecha, ficha y pegamento ($):</label><input type="number" id="fr-mecha" step="0.01" value="0.00"></div>
                <div class="input-group"><label for="fr-etiqueta">Etiqueta de frasco ($):</label><input type="number" id="fr-etiqueta" step="0.01" value="0.00"></div>
                <div class="input-group"><label for="fr-tag-cubre">Tag cubre vela ($):</label><input type="number" id="fr-tag-cubre" step="0.01" value="0.00"></div>
                <div class="input-group"><label for="fr-calca-prec">Calca precaución ($):</label><input type="number" id="fr-calca-prec" step="0.01" value="0.00"></div>
                <div class="input-group"><label for="fr-tag-largo">Tag largo para frasco ($):</label><input type="number" id="fr-tag-largo" step="0.01" value="0.00"></div>
                <div class="input-group"><label for="fr-grabado">Grabado ($):</label><input type="number" id="fr-grabado" step="0.01" value="0.00"></div>
                <div class="input-group"><label for="fr-calca-soluna">Calcomanía Soluna ($):</label><input type="number" id="fr-calca-soluna" step="0.01" value="0.00"></div>
                <div class="input-group"><label for="fr-mo">Mano de Obra (%):</label><input type="number" id="fr-mo" step="0.5" value="25"></div>
              </div>
            </div>

            <!-- PASO 2: CERA Y FRAGANCIA -->
            <div id="fr-sec-cera" class="input-card step-section-frasco">
              <h3><i class="fas fa-fire"></i> 2. Cera y Fragancia</h3>
              <div class="grid-2-inputs">
                <div class="input-group"><label for="fr-cera-soya-kg">Cera de soya ($/kg):</label><input type="number" id="fr-cera-soya-kg" step="0.01" value="100.00"></div>
                <div class="input-group"><label for="fr-cera-soya-g">Gramos de soya:</label><input type="number" id="fr-cera-soya-g" step="0.1" value="150"></div>
              </div>
              <div class="input-group"><label for="fr-cera-gel">Cera de gel ($ por vela):</label><input type="number" id="fr-cera-gel" step="0.01" value="0.00"></div>

              <div class="sub-divider"><i class="fas fa-wind"></i> Fragancias y Aceites</div>
              <div class="grid-2-inputs">
                <div class="input-group"><label for="fr-frag-costo">Fragancia normal ($ envase):</label><input type="number" id="fr-frag-costo" step="0.01" value="250.00"></div>
                <div class="input-group"><label for="fr-frag-ml">Tamaño envase (ml):</label><input type="number" id="fr-frag-ml" step="1" value="250"></div>
              </div>
              <div class="input-group"><label for="fr-frag-pct">Porcentaje de fragancia normal (%):</label><input type="number" id="fr-frag-pct" step="0.1" value="10"></div>
              <div class="grid-2-inputs">
                <div class="input-group"><label for="fr-frag-masaje">Fragancia para masaje ($):</label><input type="number" id="fr-frag-masaje" step="0.01" value="0.00"></div>
                <div class="input-group"><label for="fr-aceites">Aceites naturales ($):</label><input type="number" id="fr-aceites" step="0.01" value="0.00"></div>
              </div>
            </div>

            <!-- PASO 3: DECORACIÓN -->
            <div id="fr-sec-deco" class="input-card step-section-frasco">
              <h3><i class="fas fa-sparkles"></i> 3. Decoración</h3>
              <div class="grid-2-inputs">
                <div class="input-group"><label for="fr-cuarzo">Cuarzo ($):</label><input type="number" id="fr-cuarzo" step="0.01" value="0.00"></div>
                <div class="input-group"><label for="fr-naturaleza">Naturaleza ($):</label><input type="number" id="fr-naturaleza" step="0.01" value="0.00"></div>
                <div class="input-group"><label for="fr-purpurina">Purpurina ($):</label><input type="number" id="fr-purpurina" step="0.01" value="0.00"></div>
                <div class="input-group"><label for="fr-lamina">Lámina de oro ($):</label><input type="number" id="fr-lamina" step="0.01" value="0.00"></div>
                <div class="input-group"><label for="fr-mica">Mica ($):</label><input type="number" id="fr-mica" step="0.01" value="0.00"></div>
              </div>
            </div>

            <!-- PASO 4: EMPAQUE -->
            <div id="fr-sec-empaque" class="input-card step-section-frasco">
              <h3><i class="fas fa-box-open"></i> 4. Empaque</h3>
              <div class="grid-2-inputs">
                <div class="input-group"><label for="fr-caja">Caja ($):</label><input type="number" id="fr-caja" step="0.01" value="0.00"></div>
                <div class="input-group"><label for="fr-papel-china">Papel China ($):</label><input type="number" id="fr-papel-china" step="0.01" value="0.00"></div>
                <div class="input-group"><label for="fr-etq-dedic">Etiqueta dedicatoria/temp. ($):</label><input type="number" id="fr-etq-dedic" step="0.01" value="0.00"></div>
                <div class="input-group"><label for="fr-etq-inst">Etiqueta de instrucciones ($):</label><input type="number" id="fr-etq-inst" step="0.01" value="0.00"></div>
                <div class="input-group"><label for="fr-brillos">Brillos Soluna ($):</label><input type="number" id="fr-brillos" step="0.01" value="0.00"></div>
                <div class="input-group"><label for="fr-calca-regalo">Calcomanía de Regalo ($):</label><input type="number" id="fr-calca-regalo" step="0.01" value="0.00"></div>
                <div class="input-group"><label for="fr-liston">Listón ($):</label><input type="number" id="fr-liston" step="0.01" value="0.00"></div>
                <div class="input-group"><label for="fr-tarjeta-agr">Tarjeta de agradecimiento ($):</label><input type="number" id="fr-tarjeta-agr" step="0.01" value="0.00"></div>
                <div class="input-group"><label for="fr-burbuja">Burbuja ($):</label><input type="number" id="fr-burbuja" step="0.01" value="0.00"></div>
                <div class="input-group"><label for="fr-papel-panal">Papel panal ($):</label><input type="number" id="fr-papel-panal" step="0.01" value="0.00"></div>
              </div>
            </div>

            <!-- PASO 5: MARGEN -->
            <div id="fr-sec-margen" class="input-card step-section-frasco">
              <h3><i class="fas fa-percentage"></i> 5. Margen de Utilidad</h3>
              <div class="input-group"><label for="fr-margen">Margen de beneficio deseado (%):</label><input type="number" id="fr-margen" step="0.5" value="35"></div>
              <p style="font-size:0.83rem; color:#666; margin:0;">Este porcentaje se aplica a todos los insumos + MO (el frasco se suma aparte x2 al final).</p>
            </div>

            <!-- PASO 6: COTIZACIÓN -->
            <div id="fr-sec-cot" class="input-card step-section-frasco">
              <h3><i class="fas fa-file-invoice-dollar"></i> 6. Cotización</h3>
              <div class="input-group"><label for="fr-cantidad">Cantidad de velas:</label><input type="number" id="fr-cantidad" step="1" min="1" value="1"></div>
              <div class="input-group"><label for="fr-desc">Descripción para cotización:</label><input type="text" id="fr-desc" value="Vela Artesanal Aromática en Frasco"></div>
              <button onclick="agregarAlCarrito('frasco')" class="export-btn add-cart-btn">
                <i class="fas fa-cart-plus"></i> Agregar a la Cotización
              </button>
            </div>
          </div>

          <div class="results-section">
            <div class="result-card">
              <h3><i class="fas fa-calculator"></i> Resumen Vela de Frasco</h3>
              <div class="summary-grid">
                <div class="summary-item"><div class="summary-icon"><i class="fas fa-fire"></i></div><div class="summary-content"><div class="summary-label">Cera y Fragancia</div><div class="summary-value" id="res-fr-cera">$0.00</div></div></div>
                <div class="summary-item"><div class="summary-icon"><i class="fas fa-tools"></i></div><div class="summary-content"><div class="summary-label">Accesorios / Deco / Emp.</div><div class="summary-value" id="res-fr-otros">$0.00</div></div></div>
                <div class="summary-item"><div class="summary-icon"><i class="fas fa-user-tie"></i></div><div class="summary-content"><div class="summary-label">Mano de Obra</div><div class="summary-value" id="res-fr-mo">$0.00</div></div></div>
                <div class="summary-item"><div class="summary-icon"><i class="fas fa-jar"></i></div><div class="summary-content"><div class="summary-label">Frasco (x2 al final)</div><div class="summary-value" id="res-fr-frasco2">$0.00</div></div></div>
                <div class="summary-item total"><div class="summary-icon"><i class="fas fa-dollar-sign"></i></div><div class="summary-content"><div class="summary-label">Costo Unitario Real</div><div class="summary-value" id="res-fr-costo">$0.00</div></div></div>
                <div class="summary-item total"><div class="summary-icon"><i class="fas fa-tag"></i></div><div class="summary-content"><div class="summary-label">Precio de Venta</div><div class="summary-value" id="res-fr-precio">$0.00</div></div></div>
              </div>
            </div>
          </div>
        </div>
      </div>

      <!-- ==========================================
           2. RAMOS / BOUQUET (COLUMNA C)
           ========================================== -->
      <div id="vela-sub-ramos" class="vela-subapp">
        <div class="step-nav-container">
          <h3 class="step-nav-title">Guía paso a paso · Ramos / Bouquet</h3>
          <div class="step-nav">
            <button class="step-btn step-ramos active" onclick="mostrarSeccion('ramos', 'rm-sec-ramo', this)">1 Ramo y MO</button>
            <button class="step-btn step-ramos" onclick="mostrarSeccion('ramos', 'rm-sec-deco', this)">2 Decoración</button>
            <button class="step-btn step-ramos" onclick="mostrarSeccion('ramos', 'rm-sec-empaque', this)">3 Empaque</button>
            <button class="step-btn step-ramos" onclick="mostrarSeccion('ramos', 'rm-sec-margen', this)">4 Margen</button>
            <button class="step-btn step-ramos" onclick="mostrarSeccion('ramos', 'rm-sec-cot', this)">5 Cotización</button>
          </div>
        </div>

        <div class="calculadora-grid">
          <div class="input-section">
            <!-- PASO 1: RAMO -->
            <div id="rm-sec-ramo" class="input-card step-section-ramos active">
              <h3><i class="fas fa-spa"></i> 1. Ramo y Mano de Obra</h3>
              <div class="highlight-box">
                <div class="input-group" style="margin-bottom:0;">
                  <label for="rm-contenedor"><strong>Contenedor ($) — Se duplica al final (x2):</strong></label>
                  <input type="number" id="rm-contenedor" step="0.01" value="0.00">
                </div>
                <small>No entra en MO ni en margen; se suma x2 directamente al precio final.</small>
              </div>
              <div class="grid-2-inputs">
                <div class="input-group"><label for="rm-palitos">Palitos ($):</label><input type="number" id="rm-palitos" step="0.01" value="0.00"></div>
                <div class="input-group"><label for="rm-mecha">Mecha ($):</label><input type="number" id="rm-mecha" step="0.01" value="0.00"></div>
                <div class="input-group"><label for="rm-alambre">Alambre ($):</label><input type="number" id="rm-alambre" step="0.01" value="0.00"></div>
                <div class="input-group"><label for="rm-cinta">Cinta ($):</label><input type="number" id="rm-cinta" step="0.01" value="0.00"></div>
                <div class="input-group"><label for="rm-oasis">Oasis / Hielo seco ($):</label><input type="number" id="rm-oasis" step="0.01" value="0.00"></div>
                <div class="input-group"><label for="rm-papel-alg">Papel algodón ($):</label><input type="number" id="rm-papel-alg" step="0.01" value="0.00"></div>
                <div class="input-group"><label for="rm-papel-cor">Papel coreano ($):</label><input type="number" id="rm-papel-cor" step="0.01" value="0.00"></div>
                <div class="input-group"><label for="rm-calca-logo">Calcomanía logo ($):</label><input type="number" id="rm-calca-logo" step="0.01" value="0.00"></div>
                <div class="input-group"><label for="rm-moldes">Moldes ($):</label><input type="number" id="rm-moldes" step="0.01" value="0.00"></div>
                <div class="input-group"><label for="rm-mo">Mano de Obra (%):</label><input type="number" id="rm-mo" step="0.5" value="35"></div>
              </div>
            </div>

            <!-- PASO 2: DECORACIÓN -->
            <div id="rm-sec-deco" class="input-card step-section-ramos">
              <h3><i class="fas fa-sparkles"></i> 2. Decoración</h3>
              <div class="highlight-box">
                <div class="input-group" style="margin-bottom:0;">
                  <label for="rm-vela-molde"><strong>Vela de molde ($) — Solo se suma al final:</strong></label>
                  <input type="number" id="rm-vela-molde" step="0.01" value="0.00">
                </div>
                <small>Se suma tal cual al costo y precio final del ramo (sin MO ni margen extra).</small>
              </div>
              <div class="grid-2-inputs">
                <div class="input-group"><label for="rm-naturaleza">Naturaleza ($):</label><input type="number" id="rm-naturaleza" step="0.01" value="0.00"></div>
                <div class="input-group"><label for="rm-mica">Mica / Brillos ($):</label><input type="number" id="rm-mica" step="0.01" value="0.00"></div>
                <div class="input-group"><label for="rm-tarj-inst">Tarjeta instrucciones ramo ($):</label><input type="number" id="rm-tarj-inst" step="0.01" value="0.00"></div>
                <div class="input-group"><label for="rm-ganchito">Ganchito ($):</label><input type="number" id="rm-ganchito" step="0.01" value="0.00"></div>
                <div class="input-group"><label for="rm-tarj-dedic">Tarjeta de dedicatoria ($):</label><input type="number" id="rm-tarj-dedic" step="0.01" value="0.00"></div>
                <div class="input-group"><label for="rm-deco-extra">Decoración extra ($):</label><input type="number" id="rm-deco-extra" step="0.01" value="0.00"></div>
              </div>
            </div>

            <!-- PASO 3: EMPAQUE -->
            <div id="rm-sec-empaque" class="input-card step-section-ramos">
              <h3><i class="fas fa-box-open"></i> 3. Empaque</h3>
              <div class="grid-2-inputs">
                <div class="input-group"><label for="rm-caja">Caja ($):</label><input type="number" id="rm-caja" step="0.01" value="0.00"></div>
                <div class="input-group"><label for="rm-liston">Listón ($):</label><input type="number" id="rm-liston" step="0.01" value="0.00"></div>
              </div>
            </div>

            <!-- PASO 4: MARGEN -->
            <div id="rm-sec-margen" class="input-card step-section-ramos">
              <h3><i class="fas fa-percentage"></i> 4. Margen de Utilidad</h3>
              <div class="input-group"><label for="rm-margen">Margen de beneficio deseado (%):</label><input type="number" id="rm-margen" step="0.5" value="35"></div>
              <p style="font-size:0.83rem; color:#666; margin:0;">El contenedor se suma aparte x2 y la vela de molde se suma tal cual al final.</p>
            </div>

            <!-- PASO 5: COTIZACIÓN -->
            <div id="rm-sec-cot" class="input-card step-section-ramos">
              <h3><i class="fas fa-file-invoice-dollar"></i> 5. Cotización</h3>
              <div class="input-group"><label for="rm-cantidad">Cantidad de ramos:</label><input type="number" id="rm-cantidad" step="1" min="1" value="1"></div>
              <div class="input-group"><label for="rm-desc">Descripción para cotización:</label><input type="text" id="rm-desc" value="Ramo / Bouquet de Velas Artesanales"></div>
              <button onclick="agregarAlCarrito('ramos')" class="export-btn add-cart-btn">
                <i class="fas fa-cart-plus"></i> Agregar a la Cotización
              </button>
            </div>
          </div>

          <div class="results-section">
            <div class="result-card">
              <h3><i class="fas fa-calculator"></i> Resumen Ramos / Bouquet</h3>
              <div class="summary-grid">
                <div class="summary-item"><div class="summary-icon"><i class="fas fa-spa"></i></div><div class="summary-content"><div class="summary-label">Insumos Ramo / Deco / Emp.</div><div class="summary-value" id="res-rm-base">$0.00</div></div></div>
                <div class="summary-item"><div class="summary-icon"><i class="fas fa-user-tie"></i></div><div class="summary-content"><div class="summary-label">Mano de Obra</div><div class="summary-value" id="res-rm-mo">$0.00</div></div></div>
                <div class="summary-item"><div class="summary-icon"><i class="fas fa-box"></i></div><div class="summary-content"><div class="summary-label">Contenedor (x2 al final)</div><div class="summary-value" id="res-rm-cont2">$0.00</div></div></div>
                <div class="summary-item"><div class="summary-icon"><i class="fas fa-fire"></i></div><div class="summary-content"><div class="summary-label">Vela de Molde (+ directo)</div><div class="summary-value" id="res-rm-vela">$0.00</div></div></div>
                <div class="summary-item total"><div class="summary-icon"><i class="fas fa-dollar-sign"></i></div><div class="summary-content"><div class="summary-label">Costo Unitario Real</div><div class="summary-value" id="res-rm-costo">$0.00</div></div></div>
                <div class="summary-item total"><div class="summary-icon"><i class="fas fa-tag"></i></div><div class="summary-content"><div class="summary-label">Precio de Venta</div><div class="summary-value" id="res-rm-precio">$0.00</div></div></div>
              </div>
            </div>
          </div>
        </div>
      </div>

      <!-- 3. RECUERDITOS -->
      <div id="vela-sub-recuerditos" class="vela-subapp">
        <div class="placeholder-card">
          <i class="fas fa-gift"></i>
          <h3>Recuerditos</h3>
          <p>Listo para agregar tus campos cuando me pases la lista.</p>
        </div>
      </div>

      <!-- 4. VELAS DECORATIVAS -->
      <div id="vela-sub-decorativas" class="vela-subapp">
        <div class="placeholder-card">
          <i class="fas fa-star"></i>
          <h3>Velas Decorativas</h3>
          <p>Listo para agregar tus campos cuando me pases la lista.</p>
        </div>
      </div>

      <!-- 5. CORPORATIVAS -->
      <div id="vela-sub-corporativas" class="vela-subapp">
        <div class="placeholder-card">
          <i class="fas fa-building"></i>
          <h3>Corporativas</h3>
          <p>Listo para agregar tus campos cuando me pases la lista.</p>
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
          <button class="step-btn step-conc active" onclick="mostrarSeccion('conc', 'sec-conc-mezcla', this)">1 Mezcla Base</button>
          <button class="step-btn step-conc" onclick="mostrarSeccion('conc', 'sec-conc-quimicos', this)">2 Químicos</button>
          <button class="step-btn step-conc" onclick="mostrarSeccion('conc', 'sec-conc-pigmento', this)">3 Pigmentos</button>
          <button class="step-btn step-conc" onclick="mostrarSeccion('conc', 'sec-conc-acabado', this)">4 Acabado / Molde</button>
          <button class="step-btn step-conc" onclick="mostrarSeccion('conc', 'sec-conc-manoobra', this)">5 Mano de Obra</button>
          <button class="step-btn step-conc" onclick="mostrarSeccion('conc', 'sec-conc-margen', this)">6 Margen</button>
          <button class="step-btn step-conc" onclick="mostrarSeccion('conc', 'sec-conc-cotizacion', this)">7 Cotización</button>
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
        <div class="input-section">
          <div class="input-card">
            <h3><i class="fas fa-plus-circle"></i> Agregar Artículo Manual</h3>
            <div class="input-group"><label for="manArticulo">Artículo / Título:</label><input type="text" id="manArticulo" placeholder="Ej. VELA AROMATICA, RAMO..." value="VELA AROMATICA"></div>
            <div class="input-group"><label for="manDescripcion">Descripción:</label><input type="text" id="manDescripcion" placeholder="Ej. Vela de soya 170g en frasco Mónaco con tapa dorada"></div>
            <div class="manual-row-2">
              <div class="input-group"><label for="manPrecio">Costo unitario ($):</label><input type="number" id="manPrecio" step="0.01" min="0" placeholder="0.00"></div>
              <div class="input-group"><label for="manCantidad">Cantidad:</label><input type="number" id="manCantidad" step="1" min="1" value="1"></div>
            </div>
            <button onclick="agregarItemManual()" class="export-btn add-cart-btn"><i class="fas fa-cart-plus"></i> Agregar al Carrito Manual</button>
          </div>

          <div class="input-card">
            <h3><i class="fas fa-user"></i> Datos del Cliente</h3>
            <div class="input-group"><label for="manFolio">No. Cotización (Folio):</label><input type="text" id="manFolio" value="100" placeholder="Ej. 100"></div>
            <div class="input-group"><label for="manClienteNombre">Nombre del Cliente:</label><input type="text" id="manClienteNombre" placeholder="Nombre o empresa"></div>
            <div class="input-group"><label for="manClienteTel">Teléfono:</label><input type="text" id="manClienteTel" placeholder="Ej. 33 1234 5678"></div>
            <div class="input-group"><label for="manClienteDom">Domicilio / Ciudad:</label><input type="text" id="manClienteDom" placeholder="Dirección, colonia y ciudad"></div>
          </div>
        </div>

        <div class="results-section">
          <div class="cart-card" style="margin-top: 0;">
            <h3><i class="fas fa-shopping-cart"></i> Carrito de Cotización Manual <span class="badge-count" id="man-cart-badge">0 artículos</span></h3>
            <div style="overflow-x: auto;">
              <table class="cart-table">
                <thead><tr><th>Artículo</th><th>Descripción</th><th>Cant.</th><th>P. Unitario</th><th>Importe</th><th></th></tr></thead>
                <tbody id="man-cart-tbody"><tr><td colspan="6" style="text-align:center; color:#888; padding: 20px;">Agrega uno o más artículos desde el formulario para armar tu cotización manual.</td></tr></tbody>
              </table>
            </div>

            <div class="manual-row-3" style="margin-top: 15px;">
              <div class="input-group"><label for="manDescuentoPct">Descuento (%):</label><input type="number" id="manDescuentoPct" step="0.5" min="0" max="100" value="0"></div>
              <div class="input-group"><label for="manEnvio">Costo de envío ($):</label><input type="number" id="manEnvio" step="1" min="0" value="0" placeholder="0.00"></div>
              <div class="input-group">
                <label for="manModoIva">IVA (16%):</label>
                <select id="manModoIva"><option value="sumar">Sumar +16% IVA</option><option value="sin-iva">Sin IVA (0%)</option></select>
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
  </div>

  <script>
    // ==========================================
    // CONFIGURACIÓN DE PINS Y PERMISOS
    // ==========================================
    const USUARIOS_PIN = {
      "1234": { rol: "maestro", permisos: ["velas", "concreto", "manual"] },
      "0000": { rol: "trabajador", permisos: ["manual"] }
    };

    let usuarioActual = null;
    let subVelaActiva = 'frasco';
    let resFrasco = { cantidad: 1, precioVenta: 0 };
    let resRamos = { cantidad: 1, precioVenta: 0 };
    let resultadosConcreto = { cantidad: 1, precioVenta: 0 };
    let carritoCotizacion = [];
    let carritoManual = [];

    function verificarPin() {
      const pinIngresado = document.getElementById('pin-input').value.trim();
      const errorEl = document.getElementById('pin-error');
      if (USUARIOS_PIN[pinIngresado]) {
        usuarioActual = USUARIOS_PIN[pinIngresado];
        sessionStorage.setItem('soluna_pin_activo', pinIngresado);
        errorEl.style.display = 'none';
        document.getElementById('pin-input').value = '';
        aplicarPermisos(usuarioActual);
      } else {
        errorEl.style.display = 'block';
        document.getElementById('pin-input').value = '';
        document.getElementById('pin-input').focus();
      }
    }

    function aplicarPermisos(usuario) {
      document.getElementById('pin-overlay').style.display = 'none';
      document.getElementById('main-content').style.display = 'block';
      const permisos = usuario.permisos;
      document.getElementById('btn-app-velas').style.display = permisos.includes('velas') ? 'flex' : 'none';
      document.getElementById('btn-app-concreto').style.display = permisos.includes('concreto') ? 'flex' : 'none';
      document.getElementById('btn-app-manual').style.display = permisos.includes('manual') ? 'flex' : 'none';
      if (permisos.length > 0) switchApp(permisos[0]);
    }

    function cerrarSesion() {
      sessionStorage.removeItem('soluna_pin_activo');
      usuarioActual = null;
      document.getElementById('main-content').style.display = 'none';
      document.getElementById('pin-overlay').style.display = 'flex';
      document.getElementById('pin-input').focus();
    }

    function switchApp(app) {
      if (usuarioActual && !usuarioActual.permisos.includes(app)) return;
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
        calcularFrasco();
        calcularRamos();
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

    function switchVelaType(tipo, btnElement) {
      subVelaActiva = tipo;
      document.querySelectorAll('.vela-subapp').forEach(el => el.classList.remove('active'));
      document.querySelectorAll('.vela-type-btn').forEach(btn => btn.classList.remove('active'));
      document.getElementById(`vela-sub-${tipo}`).classList.add('active');
      btnElement.classList.add('active');
      actualizarResumenCarrito();
    }

    function mostrarSeccion(grupo, sectionId, btnElement) {
      document.querySelectorAll(`.step-section-${grupo}`).forEach(sec => sec.classList.remove('active'));
      document.querySelectorAll(`.step-${grupo}`).forEach(btn => btn.classList.remove('active'));
      document.getElementById(sectionId).classList.add('active');
      btnElement.classList.add('active');
    }

    function num(id, def = 0) {
      const el = document.getElementById(id);
      if (!el) return def;
      const v = parseFloat(el.value);
      return isNaN(v) || v < 0 ? def : v;
    }

    function formatCurrency(value) {
      const numero = isNaN(value) || value === null ? 0 : value;
      return new Intl.NumberFormat('es-MX', { style: 'currency', currency: 'MXN', minimumFractionDigits: 2 }).format(numero);
    }

    function toggleDarkMode() { document.body.classList.toggle("dark"); }

    // ==========================================
    // 1. CÁLCULO: VELA DE FRASCO (COLUMNA B)
    // ==========================================
    function calcularFrasco() {
      const frasco = num('fr-frasco'); // Se duplica al final
      const accesoriosFrasco = num('fr-mecha') + num('fr-etiqueta') + num('fr-tag-cubre') +
        num('fr-calca-prec') + num('fr-tag-largo') + num('fr-grabado') + num('fr-calca-soluna');

      const ceraSoya = (num('fr-cera-soya-kg') / 1000) * num('fr-cera-soya-g');
      const ceraGel = num('fr-cera-gel');
      const mlFrag = num('fr-frag-ml', 1);
      const fragNormal = (num('fr-cera-soya-g') * (num('fr-frag-pct') / 100)) * (mlFrag > 0 ? num('fr-frag-costo') / mlFrag : 0);
      const ceraYFrag = ceraSoya + ceraGel + fragNormal + num('fr-frag-masaje') + num('fr-aceites');

      const decoracion = num('fr-cuarzo') + num('fr-naturaleza') + num('fr-purpurina') + num('fr-lamina') + num('fr-mica');
      const empaque = num('fr-caja') + num('fr-papel-china') + num('fr-etq-dedic') + num('fr-etq-inst') +
        num('fr-brillos') + num('fr-calca-regalo') + num('fr-liston') + num('fr-tarjeta-agr') +
        num('fr-burbuja') + num('fr-papel-panal');

      const materialesBase = accesoriosFrasco + ceraYFrag + decoracion + empaque;
      const mo = materialesBase * (num('fr-mo', 25) / 100);
      const costoBaseSinFrasco = materialesBase + mo;

      const costoUnitarioReal = costoBaseSinFrasco + frasco;
      const precioVenta = (costoBaseSinFrasco * (1 + num('fr-margen', 35) / 100)) + (frasco * 2);

      resFrasco = { cantidad: num('fr-cantidad', 1), precioVenta, costoUnitarioReal };

      document.getElementById('res-fr-cera').textContent = formatCurrency(ceraYFrag);
      document.getElementById('res-fr-otros').textContent = formatCurrency(accesoriosFrasco + decoracion + empaque);
      document.getElementById('res-fr-mo').textContent = formatCurrency(mo);
      document.getElementById('res-fr-frasco2').textContent = formatCurrency(frasco * 2);
      document.getElementById('res-fr-costo').textContent = formatCurrency(costoUnitarioReal);
      document.getElementById('res-fr-precio').textContent = formatCurrency(precioVenta);

      if (carritoCotizacion.length === 0) actualizarResumenCarrito();
    }

    // ==========================================
    // 2. CÁLCULO: RAMOS / BOUQUET (COLUMNA C)
    // ==========================================
    function calcularRamos() {
      const contenedor = num('rm-contenedor'); // Se duplica al final
      const velaMolde = num('rm-vela-molde');  // Solo se suma al final

      const insumosRamo = num('rm-palitos') + num('rm-mecha') + num('rm-alambre') + num('rm-cinta') +
        num('rm-oasis') + num('rm-papel-alg') + num('rm-papel-cor') + num('rm-calca-logo') + num('rm-moldes');

      const decoracion = num('rm-naturaleza') + num('rm-mica') + num('rm-tarj-inst') +
        num('rm-ganchito') + num('rm-tarj-dedic') + num('rm-deco-extra');

      const empaque = num('rm-caja') + num('rm-liston');

      const materialesBase = insumosRamo + decoracion + empaque;
      const mo = materialesBase * (num('rm-mo', 35) / 100);
      const costoBase = materialesBase + mo;

      const costoUnitarioReal = costoBase + contenedor + velaMolde;
      const precioVenta = (costoBase * (1 + num('rm-margen', 35) / 100)) + (contenedor * 2) + velaMolde;

      resRamos = { cantidad: num('rm-cantidad', 1), precioVenta, costoUnitarioReal };

      document.getElementById('res-rm-base').textContent = formatCurrency(materialesBase);
      document.getElementById('res-rm-mo').textContent = formatCurrency(mo);
      document.getElementById('res-rm-cont2').textContent = formatCurrency(contenedor * 2);
      document.getElementById('res-rm-vela').textContent = formatCurrency(velaMolde);
      document.getElementById('res-rm-costo').textContent = formatCurrency(costoUnitarioReal);
      document.getElementById('res-rm-precio').textContent = formatCurrency(precioVenta);

      if (carritoCotizacion.length === 0) actualizarResumenCarrito();
    }

    // ==========================================
    // 3. CÁLCULO: CONCRETO
    // ==========================================
    function calcularCostosConcreto() {
      const costoMezcla = (num('costoCemento') / 1000 * num('gramosCemento')) + (num('costoArena') / 1000 * num('gramosArena')) + num('costoAgua');
      const costoQuimicos = num('costoAcido') + num('costoCarbonato');
      const costoPigmento = num('costoPigmentoConc');
      const costoAcabados = num('costoSellador') + num('costoBaseCorcho') + num('desgasteMolde');
      const manoObra = num('manoObraConc');
      const indirectos = num('indirectosConc');
      const costoTotalUnitario = costoMezcla + costoQuimicos + costoPigmento + costoAcabados + manoObra + indirectos;
      const precioVenta = costoTotalUnitario * (1 + num('margenConc', 40) / 100);

      resultadosConcreto = { cantidad: num('cantidadConc', 1), precioVenta, costoTotalUnitario };

      document.getElementById('res-conc-mezcla').textContent = formatCurrency(costoMezcla);
      document.getElementById('res-conc-quimicos').textContent = formatCurrency(costoQuimicos);
      document.getElementById('res-conc-acabado').textContent = formatCurrency(costoAcabados);
      document.getElementById('res-conc-manoobra').textContent = formatCurrency(manoObra + indirectos);
      document.getElementById('res-conc-total').textContent = formatCurrency(costoTotalUnitario);
      document.getElementById('res-conc-precio').textContent = formatCurrency(precioVenta);

      if (carritoCotizacion.length === 0) actualizarResumenCarrito();
    }

    // ==========================================
    // TOTALES, CARRITO, PDF Y WHATSAPP
    // ==========================================
    function calcularTotalesGenerico(items, descPct, modoIva, envio = 0) {
      const bruto = items.reduce((acc, item) => acc + (item.precioUnitario * item.cantidad), 0);
      const descuento = bruto * (Math.min(descPct, 100) / 100);
      const subtotal = bruto - descuento;
      const iva = (modoIva === 'sumar') ? subtotal * 0.16 : 0;
      const total = subtotal + iva + envio;
      return { items, bruto, descPct, descuento, subtotal, iva, envio, total, modoIva };
    }

    function agregarAlCarrito(tipo) {
      if (tipo === 'frasco') {
        carritoCotizacion.push({
          articulo: 'VELA DE FRASCO',
          descripcion: document.getElementById('fr-desc').value.trim() || 'Vela Artesanal en Frasco',
          cantidad: resFrasco.cantidad || 1,
          precioUnitario: resFrasco.precioVenta || 0
        });
      } else if (tipo === 'ramos') {
        carritoCotizacion.push({
          articulo: 'RAMO / BOUQUET',
          descripcion: document.getElementById('rm-desc').value.trim() || 'Ramo / Bouquet de Velas',
          cantidad: resRamos.cantidad || 1,
          precioUnitario: resRamos.precioVenta || 0
        });
      } else {
        carritoCotizacion.push({
          articulo: 'PIEZA DE CONCRETO',
          descripcion: document.getElementById('descConc').value.trim() || 'Pieza Artesanal de Concreto',
          cantidad: resultadosConcreto.cantidad || 1,
          precioUnitario: resultadosConcreto.precioVenta || 0
        });
      }
      renderizarCarrito();
      document.getElementById('panel-carrito').scrollIntoView({ behavior: 'smooth' });
    }

    function eliminarDelCarrito(i) { carritoCotizacion.splice(i, 1); renderizarCarrito(); }
    function vaciarCarrito() { if (carritoCotizacion.length && confirm('¿Vaciar cotización?')) { carritoCotizacion = []; renderizarCarrito(); } }

    function obtenerItemsActivos() {
      if (carritoCotizacion.length > 0) return carritoCotizacion;
      const esVelas = document.getElementById('app-velas').style.display !== 'none';
      if (esVelas) {
        if (subVelaActiva === 'ramos') {
          return [{ articulo: 'RAMO / BOUQUET', descripcion: document.getElementById('rm-desc').value.trim() || 'Ramo / Bouquet de Velas', cantidad: resRamos.cantidad || 1, precioUnitario: resRamos.precioVenta || 0 }];
        }
        return [{ articulo: 'VELA DE FRASCO', descripcion: document.getElementById('fr-desc').value.trim() || 'Vela Artesanal en Frasco', cantidad: resFrasco.cantidad || 1, precioUnitario: resFrasco.precioVenta || 0 }];
      }
      return [{ articulo: 'PIEZA DE CONCRETO', descripcion: document.getElementById('descConc').value.trim() || 'Pieza Artesanal de Concreto', cantidad: resultadosConcreto.cantidad || 1, precioUnitario: resultadosConcreto.precioVenta || 0 }];
    }

    function renderizarCarrito() {
      const tbody = document.getElementById('cart-tbody');
      document.getElementById('cart-badge').textContent = `${carritoCotizacion.length} artículo${carritoCotizacion.length === 1 ? '' : 's'}`;
      if (carritoCotizacion.length === 0) {
        tbody.innerHTML = '<tr><td colspan="6" style="text-align:center; color:#888; padding: 15px;">Aún no agregas productos. Puedes agregar varios desde el apartado de Cotización o exportar directamente el cálculo actual.</td></tr>';
      } else {
        tbody.innerHTML = carritoCotizacion.map((item, i) => `
          <tr>
            <td><strong>${item.articulo}</strong></td><td>${item.descripcion}</td><td>${item.cantidad}</td>
            <td>${formatCurrency(item.precioUnitario)}</td><td>${formatCurrency(item.precioUnitario * item.cantidad)}</td>
            <td><button class="btn-del-item" onclick="eliminarDelCarrito(${i})"><i class="fas fa-times"></i></button></td>
          </tr>`).join('');
      }
      actualizarResumenCarrito();
    }

    function actualizarResumenCarrito() {
      const t = calcularTotalesGenerico(obtenerItemsActivos(), num('descuentoPct'), document.getElementById('modoIva').value, 0);
      document.getElementById('cart-bruto').textContent = formatCurrency(t.bruto);
      document.getElementById('lbl-desc-pct').textContent = t.descPct;
      document.getElementById('cart-descuento').textContent = `-${formatCurrency(t.descuento)}`;
      document.getElementById('row-descuento').style.display = t.descPct > 0 ? 'flex' : 'none';
      document.getElementById('cart-subtotal').textContent = formatCurrency(t.subtotal);
      document.getElementById('cart-iva').textContent = formatCurrency(t.iva);
      document.getElementById('row-iva').style.display = t.modoIva === 'sin-iva' ? 'none' : 'flex';
      document.getElementById('cart-total').textContent = formatCurrency(t.total);
    }

    // MANUAL
    function agregarItemManual() {
      const articulo = document.getElementById('manArticulo').value.trim() || 'ARTÍCULO';
      const descripcion = document.getElementById('manDescripcion').value.trim();
      const precio = num('manPrecio');
      const cantidad = num('manCantidad', 1);
      if (!descripcion) { alert('Escribe una descripción.'); return; }
      if (precio <= 0) { alert('Ingresa un costo unitario mayor a 0.'); return; }
      carritoManual.push({ articulo: articulo.toUpperCase(), descripcion, cantidad, precioUnitario: precio });
      document.getElementById('manDescripcion').value = '';
      document.getElementById('manPrecio').value = '';
      document.getElementById('manCantidad').value = '1';
      renderizarCarritoManual();
    }
    function eliminarItemManual(i) { carritoManual.splice(i, 1); renderizarCarritoManual(); }
    function vaciarCarritoManual() { if (carritoManual.length && confirm('¿Vaciar cotización manual?')) { carritoManual = []; renderizarCarritoManual(); } }

    function renderizarCarritoManual() {
      const tbody = document.getElementById('man-cart-tbody');
      document.getElementById('man-cart-badge').textContent = `${carritoManual.length} artículo${carritoManual.length === 1 ? '' : 's'}`;
      if (carritoManual.length === 0) {
        tbody.innerHTML = '<tr><td colspan="6" style="text-align:center; color:#888; padding: 20px;">Agrega uno o más artículos desde el formulario para armar tu cotización manual.</td></tr>';
      } else {
        tbody.innerHTML = carritoManual.map((item, i) => `
          <tr>
            <td><strong>${item.articulo}</strong></td><td>${item.descripcion}</td><td>${item.cantidad}</td>
            <td>${formatCurrency(item.precioUnitario)}</td><td>${formatCurrency(item.precioUnitario * item.cantidad)}</td>
            <td><button class="btn-del-item" onclick="eliminarItemManual(${i})"><i class="fas fa-times"></i></button></td>
          </tr>`).join('');
      }
      actualizarResumenManual();
    }

    function actualizarResumenManual() {
      const t = calcularTotalesGenerico(carritoManual, num('manDescuentoPct'), document.getElementById('manModoIva').value, num('manEnvio'));
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

    // PDF OFICIAL
    async function generarPDFOficial({ t, folio, nombre, domicilio, contacto, primaryColor, rgb }) {
      const { jsPDF } = window.jspdf;
      const doc = new jsPDF({ orientation: 'portrait', unit: 'mm', format: 'letter' });
      doc.setFontSize(8); doc.setTextColor(80);
      doc.text("ANA PAULA VILLASEÑOR GONZALEZ", 15, 20);
      doc.text("VILLA CORAL 21, SANTA ANA TEPETITLÁN, 45230 ZAPOPAN, JAL.", 15, 24);
      doc.text("CORREO: ANAPAULAVIGON@GMAIL.COM", 15, 28);
      doc.text("CEL: 33 3149 0596", 15, 32);
      const fechaActual = new Date().toLocaleDateString('es-MX');
      doc.text(`No. COTIZACION: #${folio}`, 160, 20);
      doc.text(`FECHA: ${fechaActual}`, 160, 24);

      try {
        const logoImg = document.getElementById('main-logo');
        const canvas = document.createElement('canvas');
        canvas.width = logoImg.naturalWidth; canvas.height = logoImg.naturalHeight;
        canvas.getContext('2d').drawImage(logoImg, 0, 0);
        doc.addImage(canvas.toDataURL('image/jpeg'), 'JPEG', 58, 38, 100, 30);
      } catch (e) {}

      doc.setFontSize(10); doc.setFont('helvetica', 'bold'); doc.setTextColor(primaryColor);
      doc.text("DATOS DE CLIENTE", 15, 80);
      doc.setDrawColor(rgb[0], rgb[1], rgb[2]); doc.line(15, 82, 195, 82);
      doc.setFont('helvetica', 'normal'); doc.setTextColor(50);
      doc.text(`NOMBRE: ${nombre || "___________________________"}`, 15, 89);
      doc.text(`Domicilio: ${domicilio || "___________________________"}`, 15, 95);
      doc.text("Lugar de Emisión: Zapopan, Jalisco", 15, 101);
      doc.text(`Contacto: ${contacto || "___________________________"}`, 15, 107);

      doc.setFont('helvetica', 'bold'); doc.setTextColor(primaryColor);
      doc.text("DATOS DE COTIZACION", 15, 118); doc.line(15, 120, 195, 120);

      const bodyRows = t.items.map(item => [item.articulo, item.descripcion, item.cantidad.toString(), formatCurrency(item.precioUnitario), formatCurrency(item.precioUnitario * item.cantidad)]);
      if (t.descPct > 0) bodyRows.push(['DESCUENTO', `Descuento (${t.descPct}%)`, '1', `${t.descPct}%`, `-${formatCurrency(t.descuento)}`]);

      doc.autoTable({
        startY: 125, head: [['Articulo', 'Descripción', 'Cantidad', 'Precio Unitario', 'Importe']], body: bodyRows, theme: 'plain',
        headStyles: { fillColor: [255, 255, 255], textColor: [0, 0, 0], fontStyle: 'bold', lineWidth: 0.1, lineColor: [200, 200, 200] },
        bodyStyles: { textColor: [50, 50, 50], fontSize: 9 }, styles: { cellPadding: 3 }
      });

      let finalY = doc.lastAutoTable.finalY + 8;
      doc.setFontSize(10); doc.setFont('helvetica', 'bold'); doc.setTextColor(50);
      doc.text("SUBTOTAL", 138, finalY); doc.text(formatCurrency(t.subtotal), 195, finalY, { align: 'right' });
      if (t.modoIva !== 'sin-iva') { finalY += 6; doc.text("IVA 16%", 138, finalY); doc.text(formatCurrency(t.iva), 195, finalY, { align: 'right' }); }
      if (t.envio > 0) { finalY += 6; doc.text("ENVÍO", 138, finalY); doc.text(formatCurrency(t.envio), 195, finalY, { align: 'right' }); }
      finalY += 7; doc.setTextColor(primaryColor);
      doc.text("TOTAL", 138, finalY); doc.text(formatCurrency(t.total), 195, finalY, { align: 'right' });

      let cy = finalY + 15;
      if (cy > 220) { doc.addPage(); cy = 20; }
      doc.setTextColor(primaryColor); doc.text("CONDICIONES DE VENTA", 15, cy); doc.line(15, cy + 2, 195, cy + 2);
      doc.setFont('helvetica', 'normal'); doc.setTextColor(80); doc.setFontSize(8);

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
      condiciones.forEach(c => { const lines = doc.splitTextToSize(c, 180); doc.text(lines, 15, cy); cy += (lines.length * 4) + 1.5; });
      doc.setFont('helvetica', 'bold'); doc.text("GRACIAS POR SU PREFERENCIA.", 105, cy + 8, { align: 'center' });
      doc.save(`Cotizacion_Soluna_${folio}.pdf`);
    }

    function exportToPDF() {
      const esVelas = document.getElementById('app-velas').style.display !== 'none';
      const t = calcularTotalesGenerico(obtenerItemsActivos(), num('descuentoPct'), document.getElementById('modoIva').value, 0);
      generarPDFOficial({
        t, folio: document.getElementById('cotFolio').value.trim() || "100",
        nombre: document.getElementById('clienteNombre').value.trim(),
        domicilio: document.getElementById('clienteDomicilio').value.trim(),
        contacto: document.getElementById('clienteContacto').value.trim(),
        primaryColor: esVelas ? '#d97a9b' : '#546e7a', rgb: esVelas ? [217, 122, 155] : [84, 110, 122]
      });
    }

    function exportarPDFManual() {
      if (!carritoManual.length) { alert('Agrega al menos un artículo al carrito manual.'); return; }
      const t = calcularTotalesGenerico(carritoManual, num('manDescuentoPct'), document.getElementById('manModoIva').value, num('manEnvio'));
      generarPDFOficial({
        t, folio: document.getElementById('manFolio').value.trim() || "100",
        nombre: document.getElementById('manClienteNombre').value.trim(),
        domicilio: document.getElementById('manClienteDom').value.trim(),
        contacto: document.getElementById('manClienteTel').value.trim(),
        primaryColor: '#a65d37', rgb: [166, 93, 55]
      });
    }

    function enviarWhatsAppGenerico(t, folio, cliente) {
      let msg = `*Soluna - Velas para regalar* ✨\n*Cotización #${folio}*`;
      if (cliente) msg += ` | Cliente: ${cliente}`;
      msg += `\n\n`;
      t.items.forEach(item => {
        msg += `• *${item.cantidad}x ${item.descripcion}*\n   ${formatCurrency(item.precioUnitario)} c/u = *${formatCurrency(item.precioUnitario * item.cantidad)}*\n`;
      });
      msg += `\n`;
      if (t.descPct > 0) msg += `Descuento (${t.descPct}%): -${formatCurrency(t.descuento)}\n`;
      msg += `Subtotal: ${formatCurrency(t.subtotal)}\n`;
      if (t.modoIva !== 'sin-iva') msg += `IVA (16%): ${formatCurrency(t.iva)}\n`;
      if (t.envio > 0) msg += `Envío: ${formatCurrency(t.envio)}\n`;
      msg += `*TOTAL: ${formatCurrency(t.total)} MXN*\n\n_Vigencia de 30 días. Se requiere 50% de anticipo para confirmar fecha de entrega._`;
      window.open(`https://wa.me/?text=${encodeURIComponent(msg)}`, '_blank');
    }

    function enviarWhatsAppCotizacion() {
      const t = calcularTotalesGenerico(obtenerItemsActivos(), num('descuentoPct'), document.getElementById('modoIva').value, 0);
      enviarWhatsAppGenerico(t, document.getElementById('cotFolio').value.trim() || "100", document.getElementById('clienteNombre').value.trim());
    }

    function enviarWhatsAppManual() {
      if (!carritoManual.length) { alert('Agrega al menos un artículo al carrito manual.'); return; }
      const t = calcularTotalesGenerico(carritoManual, num('manDescuentoPct'), document.getElementById('manModoIva').value, num('manEnvio'));
      enviarWhatsAppGenerico(t, document.getElementById('manFolio').value.trim() || "100", document.getElementById('manClienteNombre').value.trim());
    }

    // ==========================================
    // INICIALIZACIÓN
    // ==========================================
    document.addEventListener('DOMContentLoaded', function() {
      document.getElementById('pin-input').addEventListener('keypress', e => { if (e.key === 'Enter') verificarPin(); });
      const pinGuardado = sessionStorage.getItem('soluna_pin_activo');
      if (pinGuardado && USUARIOS_PIN[pinGuardado]) { usuarioActual = USUARIOS_PIN[pinGuardado]; aplicarPermisos(usuarioActual); }
      else { document.getElementById('pin-input').focus(); }

      document.querySelectorAll('#vela-sub-frasco input').forEach(i => i.addEventListener('input', calcularFrasco));
      document.querySelectorAll('#vela-sub-ramos input').forEach(i => i.addEventListener('input', calcularRamos));
      document.querySelectorAll('#app-concreto input').forEach(i => i.addEventListener('input', calcularCostosConcreto));
      document.getElementById('descuentoPct').addEventListener('input', actualizarResumenCarrito);
      document.getElementById('modoIva').addEventListener('change', actualizarResumenCarrito);
      document.getElementById('manDescuentoPct').addEventListener('input', actualizarResumenManual);
      document.getElementById('manEnvio').addEventListener('input', actualizarResumenManual);
      document.getElementById('manModoIva').addEventListener('change', actualizarResumenManual);

      calcularFrasco();
      calcularRamos();
      calcularCostosConcreto();
    });
  </script>
</body>
</html>
