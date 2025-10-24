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

    /* Logo hero */
    .logo-hero {
      width: 100%;
      max-width: 1200px;
      margin: 0 auto 30px;
      padding: 20px;
      display: flex;
      justify-content: center;
      align-items: center;
      background: transparent;
    }
    
    .hero-logo {
      width: 90%;
      max-width: 1000px;
      height: auto;
      object-fit: contain;
      border-radius: 20px;
      box-shadow: 0 10px 30px rgba(0, 0, 0, 0.15);
      transition: all 0.3s ease;
      border: 1px solid rgba(0, 0, 0, 0.1);
    }
    
    body.dark .hero-logo {
      box-shadow: 0 10px 30px rgba(0, 0, 0, 0.3);
      border-color: rgba(255, 255, 255, 0.1);
    }
    
    .hero-logo:hover {
      transform: translateY(-3px);
      box-shadow: 0 15px 35px rgba(0, 0, 0, 0.2);
    }

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
    
    .pdf-btn {
      background-color: #e74c3c;
      color: white;
    }
    
    .pdf-btn:hover {
      background-color: #c0392b;
      transform: translateY(-2px);
    }
    
    .excel-btn {
      background-color: var(--verde);
      color: white;
    }
    
    .excel-btn:hover {
      background-color: var(--verde-hover);
      transform: translateY(-2px);
    }

    /* Estilos mejorados para tooltips */
    .tooltip-container {
      position: relative;
      display: inline-block;
      margin-left: 5px;
    }
    
    .tooltip-icon {
      color: var(--primario);
      cursor: help;
      font-size: 0.9rem;
      touch-action: manipulation;
    }
    
    .tooltip-text {
      visibility: hidden;
      width: 200px;
      background-color: var(--primario);
      color: white;
      text-align: left;
      border-radius: 6px;
      padding: 10px;
      position: absolute;
      z-index: 1000;
      bottom: 125%;
      left: 50%;
      transform: translateX(-50%);
      opacity: 0;
      transition: opacity 0.3s;
      font-size: 0.8rem;
      font-weight: normal;
      line-height: 1.4;
      box-shadow: 0 2px 10px rgba(0,0,0,0.2);
      word-wrap: break-word;
      white-space: normal;
    }
    
    body.dark .tooltip-text {
      background-color: var(--secundario);
    }
    
    .tooltip-container:hover .tooltip-text,
    .tooltip-container:focus .tooltip-text {
      visibility: visible;
      opacity: 1;
    }

    /* Estilos específicos para tooltips en móviles */
    @media (max-width: 768px) {
      .tooltip-text {
        width: 180px;
        font-size: 0.75rem;
        bottom: auto;
        top: 100%;
        left: 50%;
        transform: translateX(-50%);
        margin-top: 8px;
        max-width: 80vw;
      }
      
      .tooltip-container .tooltip-text.right {
        left: auto;
        right: 0;
        transform: none;
      }
      
      .tooltip-container .tooltip-text.left {
        left: 0;
        transform: none;
      }
    }
  
    .calculadora-grid {
      display: grid;
      grid-template-columns: 1fr;
      gap: 15px;
    }
  
    .input-card, .result-card {
      background: white;
      border-radius: 8px;
      padding: 15px;
      margin-bottom: 15px;
      box-shadow: 0 2px 8px rgba(0,0,0,0.05);
      border: 1px solid #e0e0e0;
      transition: transform 0.3s ease, box-shadow 0.3s ease;
    }
    
    .input-card:hover, .result-card:hover {
      transform: translateY(-2px);
      box-shadow: 0 4px 12px rgba(0,0,0,0.1);
    }
  
    body.dark .input-card,
    body.dark .result-card,
    body.dark .chart-container,
    body.dark .table-wrapper {
      background: #1e1e1e;
      box-shadow: 0 2px 12px rgba(0,0,0,0.1);
      border-color: #333;
    }
  
    .input-card h3, .result-card h3 {
      margin-top: 0;
      color: var(--primario);
      padding-bottom: 6px;
      margin-bottom: 12px;
      font-size: 1.1rem;
      display: flex;
      align-items: center;
      gap: 6px;
      font-weight: 500;
      letter-spacing: 0.2px;
    }
  
    .input-card h3 i {
      font-size: 1rem;
    }
  
    .input-group {
      margin-bottom: 12px;
    }
  
    .input-group label {
      display: block;
      margin-bottom: 4px;
      font-weight: 500;
      font-size: 0.9rem;
    }
  
    input, select {
      width: 100%;
      padding: 10px 12px;
      border: 1px solid #ddd;
      border-radius: 6px;
      background-color: #fff;
      transition: all 0.3s;
      font-size: 0.95rem;
      -webkit-appearance: none;
    }
  
    body.dark input,
    body.dark select {
      background-color: #2a2a2a;
      color: #e0e0e0;
      border-color: #555;
    }
  
    /* Estilos para el resumen tipo infografía */
    .summary-grid {
      display: grid;
      grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
      gap: 15px;
      margin-top: 15px;
    }
    
    .summary-item {
      background: white;
      border-radius: 8px;
      padding: 15px;
      display: flex;
      align-items: center;
      gap: 12px;
      box-shadow: 0 2px 8px rgba(0,0,0,0.05);
      border-left: 4px solid var(--primario);
      transition: transform 0.3s ease, box-shadow 0.3s ease;
    }
    
    body.dark .summary-item {
      background: #2a2a2a;
      box-shadow: 0 2px 12px rgba(0,0,0,0.1);
    }
    
    .summary-item:hover {
      transform: translateY(-3px);
      box-shadow: 0 4px 12px rgba(0,0,0,0.1);
    }
    
    .summary-item.total {
      border-left-color: var(--verde);
      background-color: rgba(82, 171, 152, 0.05);
    }
    
    body.dark .summary-item.total {
      background-color: rgba(82, 171, 152, 0.1);
    }
    
    .summary-icon {
      width: 40px;
      height: 40px;
      border-radius: 50%;
      background-color: var(--terciario);
      display: flex;
      align-items: center;
      justify-content: center;
      color: var(--primario);
      font-size: 1.1rem;
      flex-shrink: 0;
    }
    
    body.dark .summary-icon {
      background-color: #3a3a3a;
    }
    
    .summary-item.total .summary-icon {
      background-color: rgba(82, 171, 152, 0.2);
      color: var(--verde);
    }
    
    .summary-content {
      flex-grow: 1;
    }
    
    .summary-label {
      font-size: 0.85rem;
      color: #666;
      margin-bottom: 5px;
    }
    
    body.dark .summary-label {
      color: #aaa;
    }
    
    .summary-value {
      font-size: 1.1rem;
      font-weight: 600;
      color: var(--texto-claro);
    }
    
    .summary-item.total .summary-value {
      color: var(--verde);
      font-size: 1.2rem;
    }
  
    .chart-container {
      background: white;
      padding: 15px;
      border-radius: 8px;
      box-shadow: 0 2px 8px rgba(0,0,0,0.05);
      height: 300px;
      margin-bottom: 15px;
      border: 1px solid #e0e0e0;
    }
  
    canvas {
      width: 100% !important;
      height: 100% !important;
    }
  
    .dark-mode-btn {
      position: fixed;
      top: 20px;
      right: 20px;
      z-index: 1000;
      background-color: #2b2b2b;
      color: white;
      border: none;
      padding: 8px 16px;
      border-radius: 6px;
      cursor: pointer;
      box-shadow: 0 2px 8px rgba(0,0,0,0.1);
      font-size: 0.85rem;
      display: flex;
      align-items: center;
      gap: 4px;
      letter-spacing: 0.5px;
      text-transform: uppercase;
      font-weight: 500;
      transition: all 0.3s;
    }
    
    body.dark .dark-mode-btn {
      background-color: #f0f0f0;
      color: #2b2b2b;
    }
  
    .dark-mode-btn:hover {
      transform: translateY(-2px);
      box-shadow: 0 4px 12px rgba(0,0,0,0.15);
    }
  
    .whatsapp-btn {
      position: fixed;
      bottom: 20px;
      right: 20px;
      z-index: 999;
      background-color: #25D366;
      color: white;
      width: 50px;
      height: 50px;
      border-radius: 50%;
      display: flex;
      align-items: center;
      justify-content: center;
      box-shadow: 0 2px 8px rgba(0,0,0,0.2);
      transition: all 0.3s;
    }
  
    .whatsapp-btn:hover {
      background-color: #128C7E;
      transform: scale(1.1);
    }
  
    .whatsapp-btn i {
      font-size: 25px;
    }
  
    .results-table-container {
      margin-top: 15px;
    }
    
    .table-wrapper {
      overflow-x: auto;
      max-height: 250px;
      overflow-y: auto;
      margin-top: 12px;
      border-radius: 6px;
      box-shadow: 0 1px 2px rgba(0,0,0,0.1);
      -webkit-overflow-scrolling: touch;
      border: 1px solid #e0e0e0;
    }
    
    #tablaResultados {
      width: 100%;
      border-collapse: collapse;
      font-size: 0.85rem;
    }
    
    #tablaResultados th, 
    #tablaResultados td {
      padding: 10px 12px;
      text-align: right;
      border-bottom: 1px solid #eee;
    }
    
    #tablaResultados th {
      background-color: var(--primario);
      color: white;
      position: sticky;
      top: 0;
      text-align: center;
      font-size: 0.85rem;
      font-weight: 500;
    }
    
    #tablaResultados tr:nth-child(even) {
      background-color: #f9f9f9;
    }
    
    #tablaResultados tr:hover {
      background-color: #f1f1f1;
    }
    
    body.dark #tablaResultados th {
      background-color: var(--secundario);
    }
    
    body.dark #tablaResultados tr:nth-child(even) {
      background-color: #2a2a2a;
    }
    
    body.dark #tablaResultados tr:hover {
      background-color: #333;
    }
    
    body.dark #tablaResultados th, 
    body.dark #tablaResultados td {
      border-color: #444;
    }
  
    /* Estilos para select */
    select {
      background-image: url("data:image/svg+xml;charset=UTF-8,%3csvg xmlns='http://www.w3.org/2000/svg' viewBox='0 0 24 24' fill='none' stroke='currentColor' stroke-width='2' stroke-linecap='round' stroke-linejoin='round'%3e%3cpolyline points='6 9 12 15 18 9'%3e%3c/polyline%3e%3c/svg%3e");
      background-repeat: no-repeat;
      background-position: right 10px center;
      background-size: 1em;
    }
    
    body.dark select {
      background-image: url("data:image/svg+xml;charset=UTF-8,%3csvg xmlns='http://www.w3.org/2000/svg' viewBox='0 0 24 24' fill='none' stroke='%23e0e0e0' stroke-width='2' stroke-linecap='round' stroke-linejoin='round'%3e%3cpolyline points='6 9 12 15 18 9'%3e%3c/polyline%3e%3c/svg%3e");
    }
    
    /* Mensajes de error */
    .error-mensaje {
      color: #dc3545 !important;
      font-size: 0.8rem;
      margin-top: 5px;
    }
    
    body.dark .error-mensaje {
      color: #ff6b6b !important;
    }
    
    .warning-message {
      background-color: #fff3cd;
      color: #856404;
      padding: 10px;
      border-radius: 4px;
      margin-bottom: 15px;
      font-size: 0.9rem;
    }
    
    body.dark .warning-message {
      background-color: #343a40;
      color: #ffeeba;
    }
    
    /* Estilos para las preguntas frecuentes */
    .faq-item {
      margin-bottom: 10px;
      border-radius: 6px;
      overflow: hidden;
      border: 1px solid #e0e0e0;
    }
    
    body.dark .faq-item {
      border-color: #444;
    }
    
    .faq-question {
      width: 100%;
      padding: 12px 15px;
      text-align: left;
      background-color: var(--terciario);
      border: none;
      cursor: pointer;
      font-weight: 500;
      font-size: 0.95rem;
      display: flex;
      justify-content: space-between;
      align-items: center;
      transition: background-color 0.3s;
    }
    
    body.dark .faq-question {
      background-color: #3a3a3a;
    }
    
    .faq-question:hover {
      background-color: #f8d7e5;
    }
    
    body.dark .faq-question:hover {
      background-color: #4a4a4a;
    }
    
    .faq-question::after {
      content: '+';
      font-size: 1.2rem;
      transition: transform 0.3s;
    }
    
    .faq-question.active::after {
      content: '-';
    }
    
    .faq-answer {
      padding: 0;
      max-height: 0;
      overflow: hidden;
      transition: max-height 0.3s ease-out, padding 0.3s ease;
      background-color: white;
    }
    
    body.dark .faq-answer {
      background-color: #1e1e1e;
    }
    
    .faq-answer.show {
      padding: 15px;
      max-height: 1000px;
    }
    
    .faq-answer p {
      margin-top: 0;
      margin-bottom: 10px;
    }
    
    /* Responsive para pantallas más grandes */
    @media (min-width: 768px) {
      .calculadora-grid {
        grid-template-columns: 1.1fr 2fr;
      }
      
      .chart-container {
        height: 400px;
      }
      
      .export-buttons {
        flex-direction: row;
      }
    }
    
    /* Ajustes para móviles */
    @media (max-width: 768px) {
      body {
        padding: 10px;
        min-width: 100%;
        overflow-x: hidden;
      }
      
      .calculadora-grid {
        display: flex;
        flex-direction: column;
        gap: 10px;
      }
      
      .input-card, .result-card, .chart-container {
        width: 100%;
        box-sizing: border-box;
        margin-left: 0;
        margin-right: 0;
      }
      
      /* Ajuste para la tabla */
      .table-wrapper {
        width: 100%;
        overflow-x: auto;
        -webkit-overflow-scrolling: touch;
        display: block;
      }
      
      #tablaResultados {
        min-width: 600px;
      }
      
      /* Ajuste para el gráfico */
      .chart-container {
        height: 300px;
        padding: 10px;
      }
      
      /* Mejor disposición de los elementos de resumen */
      .summary-grid {
        grid-template-columns: 1fr 1fr;
        gap: 10px;
      }
      
      /* Ajuste para inputs */
      .input-group input, 
      .input-group select {
        font-size: 16px;
        padding: 12px;
      }
      
      /* Botones de exportación */
      .export-buttons {
        flex-direction: column;
      }
      
      .export-btn {
        width: 100%;
        margin-bottom: 10px;
      }
      
      /* Logo hero */
      .hero-logo {
        width: 100%;
        border-radius: 10px;
        margin-bottom: 15px;
      }
      
      /* FAQ responsive */
      .faq-item {
        margin-bottom: 8px;
      }
      
      .faq-question {
        padding: 10px;
        font-size: 0.9rem;
      }
    }

    /* Ajustes específicos para pantallas muy pequeñas (menos de 480px) */
    @media (max-width: 480px) {
      .summary-grid {
        grid-template-columns: 1fr;
      }
      
      .input-card, .result-card {
        padding: 12px;
      }
      
      .chart-container {
        height: 250px;
      }
      
      /* Reducir padding en móviles pequeños */
      body {
        padding: 8px;
      }
      
      /* Ajustar tamaño de fuente en inputs */
      input, select {
        font-size: 14px;
      }

      /* Estilos específicos para la tabla en móviles pequeños */
      #tablaResultados th, 
      #tablaResultados td {
        padding: 6px 4px;
        font-size: 12px;
      }
      
      #tablaResultados th {
        font-size: 11px;
        padding: 8px 4px;
      }
      
      .table-wrapper {
        -webkit-overflow-scrolling: touch;
        border: 1px solid #e0e0e0;
        border-radius: 6px;
        box-shadow: 0 1px 3px rgba(0,0,0,0.1);
      }
      
      body.dark .table-wrapper {
        border-color: #444;
      }
    }

    /* Prevenir desbordamiento horizontal en todos los elementos */
    * {
      max-width: 100%;
      box-sizing: border-box;
    }
    
    /* Asegurar que el contenedor principal no cause overflow */
    .calculadora-grid {
      width: 100%;
      overflow: hidden;
    }
  </style>
</head>
<body>
  <button class="dark-mode-btn" onclick="toggleDarkMode()"><i class="fas fa-moon"></i> Modo Oscuro</button>

  <!-- Logo hero -->
  <div class="logo-hero">
    <img src="Soluna oficial.png" alt="Logo Soluna" class="hero-logo">
  </div>

  <div class="calculadora-grid">
    <!-- Columna izquierda - Inputs -->
    <div class="input-section">
      <div class="input-card">
        <h3><i class="fas fa-fire"></i> Cera
          <div class="tooltip-container">
            <i class="fas fa-question-circle tooltip-icon"></i>
            <span class="tooltip-text">Información sobre los costos de cera para tus velas</span>
          </div>
        </h3>
        <div class="input-group">
          <label for="costoCera">Costo de 1kg de cera ($):
            <div class="tooltip-container">
              <i class="fas fa-question-circle tooltip-icon"></i>
              <span class="tooltip-text">Precio por kilogramo de cera que utilizas</span>
            </div>
          </label>
          <input type="number" id="costoCera" step="0.01" placeholder="0.00" value="25.00">
        </div>
        <div class="input-group">
          <label for="gramosCera">Cera por vela (gramos):
            <div class="tooltip-container">
              <i class="fas fa-question-circle tooltip-icon"></i>
              <span class="tooltip-text">Cantidad de cera que usa cada vela</span>
            </div>
          </label>
          <input type="number" id="gramosCera" step="0.1" placeholder="0" value="150">
        </div>
      </div>

      <div class="input-card">
        <h3><i class="fas fa-wind"></i> Fragancia
          <div class="tooltip-container">
            <i class="fas fa-question-circle tooltip-icon"></i>
            <span class="tooltip-text">Costos relacionados con las fragancias</span>
          </div>
        </h3>
        <div class="input-group">
          <label for="costoFragancia">Costo del bote de fragancia ($):
            <div class="tooltip-container">
              <i class="fas fa-question-circle tooltip-icon"></i>
              <span class="tooltip-text">Precio del envase completo de fragancia</span>
            </div>
          </label>
          <input type="number" id="costoFragancia" step="0.01" placeholder="0.00" value="15.00">
        </div>
        <div class="input-group">
          <label for="tamanoFragancia">Tamaño del bote (gramos):
            <div class="tooltip-container">
              <i class="fas fa-question-circle tooltip-icon"></i>
              <span class="tooltip-text">Peso total del envase de fragancia</span>
            </div>
          </label>
          <input type="number" id="tamanoFragancia" step="1" placeholder="0" value="500">
        </div>
        <div class="input-group">
          <label for="porcentajeFragancia">Porcentaje de fragancia (%):
            <div class="tooltip-container">
              <i class="fas fa-question-circle tooltip-icon"></i>
              <span class="tooltip-text">Porcentaje de fragancia en relación al peso de la cera</span>
            </div>
          </label>
          <input type="number" id="porcentajeFragancia" step="0.1" placeholder="0" value="8">
        </div>
      </div>

      <div class="input-card">
        <h3><i class="fas fa-palette"></i> Colorante
          <div class="tooltip-container">
            <i class="fas fa-question-circle tooltip-icon"></i>
            <span class="tooltip-text">Costos de colorantes para tus velas</span>
          </div>
        </h3>
        <div class="input-group">
          <label for="costoColorante">Costo del frasco de colorante ($):
            <div class="tooltip-container">
              <i class="fas fa-question-circle tooltip-icon"></i>
              <span class="tooltip-text">Precio del frasco completo de colorante</span>
            </div>
          </label>
          <input type="number" id="costoColorante" step="0.01" placeholder="0.00" value="12.00">
        </div>
        <div class="input-group">
          <label for="usoColorante">Uso por vela (gramos):
            <div class="tooltip-container">
              <i class="fas fa-question-circle tooltip-icon"></i>
              <span class="tooltip-text">Cantidad de colorante que usa cada vela</span>
            </div>
          </label>
          <input type="number" id="usoColorante" step="0.1" placeholder="0" value="5">
        </div>
        <div class="input-group">
          <label for="cantidadColorante">Cantidad del frasco (gramos):
            <div class="tooltip-container">
              <i class="fas fa-question-circle tooltip-icon"></i>
              <span class="tooltip-text">Peso total del frasco de colorante</span>
            </div>
          </label>
          <input type="number" id="cantidadColorante" step="1" placeholder="0" value="100">
        </div>
      </div>

      <div class="input-card">
        <h3><i class="fas fa-tools"></i> Otros Costos
          <div class="tooltip-container">
            <i class="fas fa-question-circle tooltip-icon"></i>
            <span class="tooltip-text">Costos adicionales de producción</span>
          </div>
        </h3>
        <div class="input-group">
          <label for="costoMecha">Costo de mecha por vela ($):
            <div class="tooltip-container">
              <i class="fas fa-question-circle tooltip-icon"></i>
              <span class="tooltip-text">Costo de la mecha o pabilo para cada vela</span>
            </div>
          </label>
          <input type="number" id="costoMecha" step="0.01" placeholder="0.00" value="2.50">
        </div>
        <div class="input-group">
          <label for="costoFrasco">Costo del frasco por vela ($):
            <div class="tooltip-container">
              <i class="fas fa-question-circle tooltip-icon"></i>
              <span class="tooltip-text">Costo del contenedor o frasco para cada vela</span>
            </div>
          </label>
          <input type="number" id="costoFrasco" step="0.01" placeholder="0.00" value="8.00">
        </div>
        <div class="input-group">
          <label for="costoEtiqueta">Costo de etiqueta por vela ($):
            <div class="tooltip-container">
              <i class="fas fa-question-circle tooltip-icon"></i>
              <span class="tooltip-text">Costo de etiquetas y branding por vela</span>
            </div>
          </label>
          <input type="number" id="costoEtiqueta" step="0.01" placeholder="0.00" value="1.50">
        </div>
        <div class="input-group">
          <label for="manoObra">Mano de obra por vela ($):
            <div class="tooltip-container">
              <i class="fas fa-question-circle tooltip-icon"></i>
              <span class="tooltip-text">Costo de mano de obra para producir cada vela</span>
            </div>
          </label>
          <input type="number" id="manoObra" step="0.01" value="10.00" placeholder="0.00">
        </div>
        <div class="input-group">
          <label for="costoIndirecto">Costos indirectos por vela ($):
            <div class="tooltip-container">
              <i class="fas fa-question-circle tooltip-icon"></i>
              <span class="tooltip-text">Costos indirectos como electricidad, empaque, etc.</span>
            </div>
          </label>
          <input type="number" id="costoIndirecto" step="0.01" value="3.00" placeholder="0.00">
        </div>
      </div>

      <div class="input-card">
        <h3><i class="fas fa-chart-line"></i> Producción y Margen
          <div class="tooltip-container">
            <i class="fas fa-question-circle tooltip-icon"></i>
            <span class="tooltip-text">Configuración de producción y márgenes de ganancia</span>
          </div>
        </h3>
        <div class="input-group">
          <label for="margen">Margen de beneficio deseado (%):
            <div class="tooltip-container">
              <i class="fas fa-question-circle tooltip-icon"></i>
              <span class="tooltip-text">Porcentaje de ganancia que deseas obtener</span>
            </div>
          </label>
          <input type="number" id="margen" step="0.1" value="35" placeholder="35">
        </div>
        <div class="input-group">
          <label for="cantidadVelas">Número de velas a producir:
            <div class="tooltip-container">
              <i class="fas fa-question-circle tooltip-icon"></i>
              <span class="tooltip-text">Cantidad total de velas que planeas producir</span>
            </div>
          </label>
          <input type="number" id="cantidadVelas" step="1" value="1" placeholder="1">
        </div>
        
        <!-- Botones de exportación -->
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

    <!-- Columna derecha - Resultados -->
    <div class="results-section">
      <div class="result-card">
        <h3><i class="fas fa-calculator"></i> Resumen de Costos</h3>
        <div class="summary-grid">
          <div class="summary-item">
            <div class="summary-icon">
              <i class="fas fa-fire"></i>
            </div>
            <div class="summary-content">
              <div class="summary-label">Costo de cera</div>
              <div class="summary-value" id="res-cera">$0.00</div>
            </div>
          </div>
          
          <div class="summary-item">
            <div class="summary-icon">
              <i class="fas fa-wind"></i>
            </div>
            <div class="summary-content">
              <div class="summary-label">Costo de fragancia</div>
              <div class="summary-value" id="res-fragancia">$0.00</div>
            </div>
          </div>
          
          <div class="summary-item">
            <div class="summary-icon">
              <i class="fas fa-palette"></i>
            </div>
            <div class="summary-content">
              <div class="summary-label">Costo de colorante</div>
              <div class="summary-value" id="res-colorante">$0.00</div>
            </div>
          </div>
          
          <div class="summary-item">
            <div class="summary-icon">
              <i class="fas fa-tools"></i>
            </div>
            <div class="summary-content">
              <div class="summary-label">Otros costos</div>
              <div class="summary-value" id="res-otros">$0.00</div>
            </div>
          </div>
          
          <div class="summary-item total">
            <div class="summary-icon">
              <i class="fas fa-dollar-sign"></i>
            </div>
            <div class="summary-content">
              <div class="summary-label">Costo total unitario</div>
              <div class="summary-value" id="res-total-unitario">$0.00</div>
            </div>
          </div>
          
          <div class="summary-item total">
            <div class="summary-icon">
              <i class="fas fa-tag"></i>
            </div>
            <div class="summary-content">
              <div class="summary-label">Precio de venta sugerido</div>
              <div class="summary-value" id="res-precio-venta">$0.00</div>
            </div>
          </div>
        </div>
      </div>

      <div class="chart-container">
        <canvas id="graficaCostos"></canvas>
      </div>
      
      <!-- Tabla de resultados detallados -->
      <div class="results-table-container">
        <div class="input-card">
          <h3><i class="fas fa-table"></i> Desglose de Costos por Vela</h3>
          <div class="table-wrapper">
            <table id="tablaResultados">
              <thead>
                <tr>
                  <th>Concepto</th>
                  <th>Costo Unitario</th>
                  <th>Porcentaje</th>
                </tr>
              </thead>
              <tbody>
                <!-- Aquí se insertarán las filas dinámicamente -->
              </tbody>
            </table>
          </div>
        </div>
      </div>
    </div>
  </div>

  <!-- Sección de Preguntas Frecuentes -->
  <div class="faq-section" style="margin-top: 40px; margin-bottom: 60px;">
    <div class="input-card">
      <h3><i class="fas fa-question-circle"></i> Preguntas frecuentes</h3>
      
      <div class="faq-item">
        <button class="faq-question">¿Cómo calcular el porcentaje de fragancia?</button>
        <div class="faq-answer">
          <p>El porcentaje de fragancia se calcula en relación al peso de la cera. Por ejemplo:</p>
          <ul>
            <li>Si usas 150g de cera y 8% de fragancia: 150g × 8% = 12g de fragancia por vela</li>
            <li>Para un bote de 500g de fragancia a $15: $15 ÷ 500g = $0.03 por gramo</li>
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

  <!-- Botón flotante de WhatsApp -->
  <a href="https://wa.me/523331490596?text=Hola,%20me%20interesa%20saber%20m%C3%A1s%20sobre%20la%20calculadora%20de%20costos%20de%20velas%20%F0%9F%94%A5" class="whatsapp-btn" target="_blank" title="Contactar por WhatsApp">
    <i class="fab fa-whatsapp"></i>
  </a>

  <script>
    // Variables globales
    let chartCostos = null;
    let resultadosCalculo = {};

    // Función mejorada para obtener valores numéricos
    function obtenerValorNumerico(id, valorPorDefecto = 0) {
      const elemento = document.getElementById(id);
      const valor = parseFloat(elemento.value);
      // Si el valor es NaN o negativo, devolvemos el valor por defecto
      return isNaN(valor) || valor < 0 ? valorPorDefecto : valor;
    }

    // Inicialización
    document.addEventListener('DOMContentLoaded', function() {
      // Configura eventos de input para cálculo automático
      document.querySelectorAll('input').forEach(input => {
        input.addEventListener('input', calcularCostos);
      });

      // Funcionalidad para desplegar las respuestas de FAQ
      document.querySelectorAll('.faq-question').forEach(button => {
        button.addEventListener('click', () => {
          const faqItem = button.parentElement;
          const answer = button.nextElementSibling;
          
          // Cerrar otros items abiertos
          document.querySelectorAll('.faq-answer').forEach(item => {
            if (item !== answer && item.classList.contains('show')) {
              item.classList.remove('show');
              item.previousElementSibling.classList.remove('active');
            }
          });
          
          // Alternar el actual
          button.classList.toggle('active');
          answer.classList.toggle('show');
        });
      });

      // Calcular inicialmente
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
      if (chartCostos) {
        chartCostos.update();
      }
    }

    function calcularCostos() {
      // Limpiar mensajes de error previos
      document.querySelectorAll('.error-mensaje').forEach(el => el.remove());
      
      // Obtener valores de los inputs usando la función mejorada
      const costoCera = obtenerValorNumerico('costoCera', 0);
      const gramosCera = obtenerValorNumerico('gramosCera', 0);
      const costoFragancia = obtenerValorNumerico('costoFragancia', 0);
      const tamanoFragancia = obtenerValorNumerico('tamanoFragancia', 1); // Evitar división por cero
      const porcentajeFragancia = obtenerValorNumerico('porcentajeFragancia', 0);
      const costoColorante = obtenerValorNumerico('costoColorante', 0);
      const usoColorante = obtenerValorNumerico('usoColorante', 0);
      const cantidadColorante = obtenerValorNumerico('cantidadColorante', 1); // Evitar división por cero
      const costoMecha = obtenerValorNumerico('costoMecha', 0);
      const costoFrasco = obtenerValorNumerico('costoFrasco', 0);
      const costoEtiqueta = obtenerValorNumerico('costoEtiqueta', 0);
      const manoObra = obtenerValorNumerico('manoObra', 0);
      const costoIndirecto = obtenerValorNumerico('costoIndirecto', 0);
      const margen = obtenerValorNumerico('margen', 35);
      const cantidadVelas = obtenerValorNumerico('cantidadVelas', 1);

      // Cálculos de costos unitarios (con protección contra división por cero)
      const costoPorGramoCera = costoCera / 1000;
      const costoCeraPorVela = costoPorGramoCera * gramosCera;

      const gramosFragancia = gramosCera * (porcentajeFragancia / 100);
      const costoPorGramoFragancia = tamanoFragancia > 0 ? costoFragancia / tamanoFragancia : 0;
      const costoFraganciaPorVela = gramosFragancia * costoPorGramoFragancia;

      const costoColorantePorGramo = cantidadColorante > 0 ? costoColorante / cantidadColorante : 0;
      const costoColorantePorVela = usoColorante * costoColorantePorGramo;

      const otrosCostosPorVela = costoMecha + costoFrasco + costoEtiqueta + manoObra + costoIndirecto;

      const costoTotalUnitario = costoCeraPorVela + costoFraganciaPorVela + costoColorantePorVela + otrosCostosPorVela;
      const precioVentaUnitario = costoTotalUnitario * (1 + margen / 100);
      const gananciaUnitario = precioVentaUnitario - costoTotalUnitario;

      const costoTotal = costoTotalUnitario * cantidadVelas;
      const precioTotalVenta = precioVentaUnitario * cantidadVelas;
      const gananciaTotal = gananciaUnitario * cantidadVelas;

      // Guardar resultados en objeto global
      resultadosCalculo = {
        cantidadVelas,
        costoCeraPorVela,
        costoFraganciaPorVela,
        costoColorantePorVela,
        otrosCostosPorVela,
        costoTotalUnitario,
        margen,
        precioVentaUnitario,
        gananciaUnitario,
        costoTotal,
        precioTotalVenta,
        gananciaTotal
      };

      // Actualizar resumen
      document.getElementById('res-cera').textContent = formatCurrency(costoCeraPorVela);
      document.getElementById('res-fragancia').textContent = formatCurrency(costoFraganciaPorVela);
      document.getElementById('res-colorante').textContent = formatCurrency(costoColorantePorVela);
      document.getElementById('res-otros').textContent = formatCurrency(otrosCostosPorVela);
      document.getElementById('res-total-unitario').textContent = formatCurrency(costoTotalUnitario);
      document.getElementById('res-precio-venta').textContent = formatCurrency(precioVentaUnitario);

      // Generar gráfico y tabla
      generarGraficoCostos();
      generarTablaCostos();
    }

    function mostrarError(inputId, mensaje) {
      const input = document.getElementById(inputId);
      const existingError = input.parentNode.querySelector('.error-mensaje');
      
      if (!existingError) {
        const error = document.createElement('div');
        error.className = 'error-mensaje';
        error.innerHTML = `<i class="fas fa-exclamation-circle"></i> ${mensaje}`;
        input.parentNode.appendChild(error);
      }
    }

    function generarGraficoCostos() {
      const ctx = document.getElementById('graficaCostos').getContext('2d');
      const esMovil = window.innerWidth < 768;
      
      if (chartCostos) {
        chartCostos.destroy();
      }

      const data = {
        labels: ['Cera', 'Fragancia', 'Colorante', 'Otros Costos'],
        datasets: [{
          data: [
            resultadosCalculo.costoCeraPorVela || 0,
            resultadosCalculo.costoFraganciaPorVela || 0,
            resultadosCalculo.costoColorantePorVela || 0,
            resultadosCalculo.otrosCostosPorVela || 0
          ],
          backgroundColor: [
            'rgba(217, 122, 155, 0.8)',
            'rgba(227, 141, 168, 0.8)',
            'rgba(255, 234, 240, 0.8)',
            'rgba(82, 171, 152, 0.8)'
          ],
          borderColor: [
            'rgba(217, 122, 155, 1)',
            'rgba(227, 141, 168, 1)',
            'rgba(255, 234, 240, 1)',
            'rgba(82, 171, 152, 1)'
          ],
          borderWidth: 1
        }]
      };

      const options = {
        responsive: true,
        maintainAspectRatio: false,
        plugins: {
          legend: {
            position: esMovil ? 'bottom' : 'right',
            labels: {
              boxWidth: 12,
              padding: 20,
              usePointStyle: true
            }
          },
          tooltip: {
            callbacks: {
              label: (context) => {
                const label = context.label || '';
                const value = context.raw || 0;
                const total = context.dataset.data.reduce((a, b) => a + b, 0);
                const percentage = total > 0 ? ((value / total) * 100).toFixed(1) : '0.0';
                return `${label}: ${formatCurrency(value)} (${percentage}%)`;
              }
            }
          }
        }
      };

      chartCostos = new Chart(ctx, {
        type: 'pie',
        data: data,
        options: options
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
        { nombre: 'Etiqueta', valor: obtenerValorNumerico('costoEtiqueta', 0) },
        { nombre: 'Mano de obra', valor: obtenerValorNumerico('manoObra', 0) },
        { nombre: 'Costos indirectos', valor: obtenerValorNumerico('costoIndirecto', 0) }
      ];

      conceptos.forEach(concepto => {
        if (concepto.valor > 0 || total > 0) {
          const porcentaje = total > 0 ? ((concepto.valor / total) * 100).toFixed(1) : '0.0';
          const fila = document.createElement('tr');
          
          fila.innerHTML = `
            <td style="text-align: left;">${concepto.nombre}</td>
            <td>${formatCurrency(concepto.valor)}</td>
            <td>${porcentaje}%</td>
          `;
          
          tbody.appendChild(fila);
        }
      });

      // Fila total
      const filaTotal = document.createElement('tr');
      filaTotal.style.fontWeight = 'bold';
      filaTotal.style.backgroundColor = 'rgba(82, 171, 152, 0.1)';
      
      filaTotal.innerHTML = `
        <td style="text-align: left;">TOTAL</td>
        <td>${formatCurrency(total)}</td>
        <td>100%</td>
      `;
      
      tbody.appendChild(filaTotal);
    }

    function formatCurrency(value) {
      // Asegurarse de que el valor sea un número válido
      const numero = isNaN(value) || value === null ? 0 : value;
      return new Intl.NumberFormat('es-MX', { 
        style: 'currency', 
        currency: 'MXN',
        minimumFractionDigits: 2,
        maximumFractionDigits: 2
      }).format(numero);
    }

    // Función para exportar a PDF
    async function exportToPDF() {
      const { jsPDF } = window.jspdf;
      const doc = new jsPDF({
        orientation: 'portrait',
        unit: 'mm',
        format: 'a4'
      });

      // Configuración de estilos
      const primaryColor = '#d97a9b';
      const accentColor = '#52ab98';
      const textColor = '#333333';
      
      // Margenes y dimensiones
      const margin = 15;
      const pageWidth = doc.internal.pageSize.getWidth();
      const contentWidth = pageWidth - margin * 2;
      let yPosition = margin;

      // 1. Encabezado del reporte
      doc.setFontSize(18);
      doc.setTextColor(primaryColor);
      doc.setFont('helvetica', 'bold');
      doc.text('Reporte de Costos de Velas - Soluna', pageWidth / 2, yPosition, { align: 'center' });
      
      doc.setFontSize(10);
      doc.setTextColor(100);
      doc.setFont('helvetica', 'normal');
      yPosition += 8;
      doc.text(`Generado el: ${new Date().toLocaleDateString('es-MX')}`, pageWidth / 2, yPosition, { align: 'center' });
      
      yPosition += 15;

      // 2. Resumen Ejecutivo
      doc.setFontSize(14);
      doc.setTextColor(primaryColor);
      doc.text('Resumen de Costos', margin, yPosition);
      doc.setDrawColor(primaryColor);
      doc.setLineWidth(0.3);
      doc.line(margin, yPosition + 2, margin + 40, yPosition + 2);
      
      yPosition += 10;

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
        doc.setFontSize(isTotal ? 11 : 10);
        doc.setFont('helvetica', isTotal ? 'bold' : 'normal');
        doc.setTextColor(isTotal ? accentColor : textColor);
        
        doc.text(item.label, margin, yPosition);
        doc.text(formatCurrency(item.value), pageWidth - margin, yPosition, { align: 'right' });
        yPosition += 6;
      });

      yPosition += 10;

      // 3. Gráfico de costos
      try {
        const canvas = document.getElementById('graficaCostos');
        const canvasImage = await html2canvas(canvas, {
          scale: 2,
          logging: false,
          useCORS: true
        });

        const imgData = canvasImage.toDataURL('image/png');
        const imgProps = doc.getImageProperties(imgData);
        const imgWidth = contentWidth * 0.6;
        const imgHeight = (imgProps.height * imgWidth) / imgProps.width;
        
        doc.addImage(imgData, 'PNG', margin + (contentWidth - imgWidth) / 2, yPosition, imgWidth, imgHeight);
        yPosition += imgHeight + 10;
      } catch (error) {
        console.error("Error al generar gráfico:", error);
      }

      // 4. Tabla de desglose
      doc.setFontSize(14);
      doc.setTextColor(primaryColor);
      doc.text('Desglose de Costos Unitarios', margin, yPosition);
      doc.setDrawColor(primaryColor);
      doc.line(margin, yPosition + 2, margin + 60, yPosition + 2);
      
      yPosition += 10;

      const tableData = [];
      const headers = ['Concepto', 'Costo Unitario', 'Porcentaje'];
      
      // Obtener datos de la tabla
      const rows = document.querySelectorAll('#tablaResultados tbody tr');
      rows.forEach(row => {
        const cells = row.querySelectorAll('td');
        if (cells.length === 3) {
          tableData.push([
            cells[0].textContent.trim(),
            cells[1].textContent.trim(),
            cells[2].textContent.trim()
          ]);
        }
      });

      doc.autoTable({
        head: [headers],
        body: tableData,
        startY: yPosition,
        theme: 'grid',
        headStyles: {
          fillColor: primaryColor,
          textColor: 255,
          fontStyle: 'bold',
          fontSize: 9
        },
        bodyStyles: {
          fontSize: 8,
          cellPadding: 2
        },
        columnStyles: {
          0: { cellWidth: 'auto', halign: 'left' },
          1: { cellWidth: 'auto', halign: 'right' },
          2: { cellWidth: 'auto', halign: 'right' }
        },
        margin: { left: margin, right: margin },
        tableWidth: contentWidth
      });

      // Guardar el PDF
      doc.save(`Reporte_Costos_Velas_${new Date().toISOString().slice(0,10)}.pdf`);
    }

    // Función para exportar a Excel
    function exportToExcel() {
      const wb = XLSX.utils.book_new();
      
      // Preparar datos
      const summaryData = [
        ['REPORTE DE COSTOS DE VELAS - SOLUNA'],
        ['Generado el:', new Date().toLocaleDateString()],
        [],
        ['RESUMEN DE COSTOS'],
        ['Costo unitario total:', formatCurrency(resultadosCalculo.costoTotalUnitario || 0)],
        ['Precio de venta sugerido:', formatCurrency(resultadosCalculo.precioVentaUnitario || 0)],
        ['Ganancia por unidad:', formatCurrency(resultadosCalculo.gananciaUnitario || 0)],
        [`Total para ${resultadosCalculo.cantidadVelas || 0} velas:`, formatCurrency(resultadosCalculo.costoTotal || 0)],
        ['Venta total sugerida:', formatCurrency(resultadosCalculo.precioTotalVenta || 0)],
        ['Ganancia total:', formatCurrency(resultadosCalculo.gananciaTotal || 0)],
        ['Margen aplicado:', (resultadosCalculo.margen || 35) + '%'],
        [],
        ['DESGLOSE DE COSTOS UNITARIOS']
      ];
      
      // Datos de la tabla
      const tableHeaders = ['Concepto', 'Costo Unitario', 'Porcentaje'];
      const tableData = [tableHeaders];
      
      const rows = document.querySelectorAll('#tablaResultados tbody tr');
      rows.forEach(row => {
        const rowData = [];
        row.querySelectorAll('td').forEach(cell => {
          rowData.push(cell.textContent.trim());
        });
        tableData.push(rowData);
      });
      
      // Combinar todos los datos
      const allData = [...summaryData, ...tableData];
      
      const ws = XLSX.utils.aoa_to_sheet(allData);
      
      // Aplicar estilos
      if(!ws['!merges']) ws['!merges'] = [];
      ws['!merges'].push({ s: { r: 0, c: 0 }, e: { r: 0, c: 2 } });
      ws['!merges'].push({ s: { r: 3, c: 0 }, e: { r: 3, c: 2 } });
      ws['!merges'].push({ s: { r: 12, c: 0 }, e: { r: 12, c: 2 } });
      
      // Establecer anchos de columnas
      ws['!cols'] = [
        { wch: 25 },
        { wch: 20 },
        { wch: 15 }
      ];
      
      XLSX.utils.book_append_sheet(wb, ws, "Reporte de Costos");
      XLSX.writeFile(wb, 'Reporte_Costos_Velas.xlsx');
    }
  </script>
</body>
</html>
