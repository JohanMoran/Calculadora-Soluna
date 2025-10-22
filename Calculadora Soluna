<html lang="es">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Calculadora de Costos de Velas</title>
  <style>
    body {
      font-family: 'Segoe UI', sans-serif;
      background-color: #fff5f8;
      color: #333;
      padding: 20px;
      display: flex;
      gap: 30px;
      max-width: 1400px;
      margin: auto;
    }
    .contenedor-principal {
      flex: 1;
    }
    h1, h2 {
      color: #d97a9b;
    }
    label {
      font-weight: bold;
      display: block;
      margin-top: 10px;
    }
    input {
      width: 100%;
      padding: 8px;
      margin-top: 4px;
      margin-bottom: 12px;
      border: 1px solid #ccc;
      border-radius: 5px;
    }
    .section {
      background-color: #ffeaf0;
      padding: 20px;
      border-radius: 12px;
      margin-bottom: 20px;
      box-shadow: 0 2px 5px rgba(0,0,0,0.05);
    }
    button {
      background-color: #e38da8;
      color: white;
      border: none;
      padding: 10px 20px;
      font-size: 16px;
      border-radius: 6px;
      cursor: pointer;
      margin-right: 10px;
      margin-bottom: 10px;
    }
    button:hover {
      background-color: #d5779a;
    }
    #resultado {
      margin-top: 20px;
      font-size: 18px;
      background-color: #fbe5eb;
      padding: 15px;
      border-radius: 10px;
    }
    #logo {
      display: block;
      margin: 0 auto 20px auto;
      max-width: 100%;
      height: auto;
    }
    .desglose {
      flex: 1;
      background-color: #ffeaf0;
      padding: 20px;
      border-radius: 12px;
      box-shadow: 0 2px 5px rgba(0,0,0,0.05);
      height: fit-content;
    }
    .desglose h3 {
      color: #d97a9b;
    }
    .botones-descarga {
      display: flex;
      flex-wrap: wrap;
      gap: 10px;
      margin-top: 20px;
    }
    .boton-pdf {
      background-color: #ff6b6b;
    }
    .boton-excel {
      background-color: #51cf66;
    }
  </style>
  <!-- Librerías para generar PDF y Excel -->
  <script src="https://cdnjs.cloudflare.com/ajax/libs/jspdf/2.5.1/jspdf.umd.min.js"></script>
  <script src="https://cdnjs.cloudflare.com/ajax/libs/jspdf-autotable/3.5.25/jspdf.plugin.autotable.min.js"></script>
  <script src="https://cdn.sheetjs.com/xlsx-0.19.3/package/dist/xlsx.full.min.js"></script>
</head>
<body>
  <div class="contenedor-principal">
    <img src="Soluna.png" alt="Logo Soluna" id="logo">
    <h1 style="text-align: center;">Calculadora de Costos de Velas Artesanales</h1>

    <div class="section">
      <h2>Cera</h2>
      <label>Costo de 1kg de cera: ($)</label>
      <input type="number" id="costoCera">
      <label>Cera por vela (gramos):</label>
      <input type="number" id="gramosCera">
    </div>

    <div class="section">
      <h2>Fragancia</h2>
      <label>Costo del bote de fragancia: ($)</label>
      <input type="number" id="costoFragancia">
      <label>Tamaño del bote de fragancia (gramos):</label>
      <input type="number" id="tamanoFragancia">
      <label>Porcentaje de fragancia usado en la vela (%):</label>
      <input type="number" id="porcentajeFragancia">
    </div>

    <div class="section">
      <h2>Colorante</h2>
      <label>Costo del colorante por frasco: ($)</label>
      <input type="number" id="costoColorante">
      <label>Uso por vela (gramos):</label>
      <input type="number" id="usoColorante">
      <label>Cantidad del frasco (gramos):</label>
      <input type="number" id="cantidadColorante">
    </div>

    <div class="section">
      <h2>Otros Costos</h2>
      <label>Costo de la mecha/pabilo (por vela): ($)</label>
      <input type="number" id="costoMecha">
      <label>Costo del frasco/contenedor (por vela): ($)</label>
      <input type="number" id="costoFrasco">
      <label>Costo de la etiqueta (por vela): ($)</label>
      <input type="number" id="costoEtiqueta">
      <label>Mano de obra por vela ($):</label>
      <input type="number" id="manoObra" value="0">
      <label>Costos indirectos (por vela): ($)</label>
      <input type="number" id="costoIndirecto" value="0">
    </div>

    <div class="section">
      <h2>Margen de Beneficio</h2>
      <label>Margen de beneficio deseado (%):</label>
      <input type="number" id="margen" value="35">
    </div>

    <div class="section">
      <h2>Producción</h2>
      <label>Número de velas a producir:</label>
      <input type="number" id="cantidadVelas" value="1">
    </div>

    <button onclick="calcularCosto()">Calcular</button>
    
    <div class="botones-descarga" id="botonesDescarga" style="display: none;">
      <button class="boton-pdf" onclick="generarPDF()">Descargar PDF</button>
      <button class="boton-excel" onclick="generarExcel()">Descargar Excel</button>
    </div>
  </div>

  <div class="desglose">
    <h3>Desglose de Costos</h3>
    <div id="resultado"></div>
  </div>

  <script>
    // Variables globales para almacenar los resultados
    let resultadosCalculo = {};
    
    function calcularCosto() {
      const costoCera = parseFloat(document.getElementById('costoCera').value);
      const gramosCera = parseFloat(document.getElementById('gramosCera').value);
      const costoFragancia = parseFloat(document.getElementById('costoFragancia').value);
      const tamanoFragancia = parseFloat(document.getElementById('tamanoFragancia').value);
      const porcentajeFragancia = parseFloat(document.getElementById('porcentajeFragancia').value);
      const costoColorante = parseFloat(document.getElementById('costoColorante').value);
      const usoColorante = parseFloat(document.getElementById('usoColorante').value);
      const cantidadColorante = parseFloat(document.getElementById('cantidadColorante').value);
      const costoMecha = parseFloat(document.getElementById('costoMecha').value);
      const costoFrasco = parseFloat(document.getElementById('costoFrasco').value);
      const costoEtiqueta = parseFloat(document.getElementById('costoEtiqueta').value);
      const manoObra = parseFloat(document.getElementById('manoObra').value);
      const costoIndirecto = parseFloat(document.getElementById('costoIndirecto').value);
      const margen = parseFloat(document.getElementById('margen').value);
      const cantidadVelas = parseFloat(document.getElementById('cantidadVelas').value);

      const costoPorGramoCera = costoCera / 1000;
      const costoCeraPorVela = costoPorGramoCera * gramosCera;

      const gramosFragancia = gramosCera * (porcentajeFragancia / 100);
      const costoPorGramoFragancia = costoFragancia / tamanoFragancia;
      const costoFraganciaPorVela = gramosFragancia * costoPorGramoFragancia;

      const costoColorantePorGramo = costoColorante / cantidadColorante;
      const costoColorantePorVela = usoColorante * costoColorantePorGramo;

      const costoTotalUnitario = costoCeraPorVela + costoFraganciaPorVela + costoColorantePorVela + 
                               costoMecha + costoFrasco + costoEtiqueta + manoObra + costoIndirecto;
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
        costoMecha,
        costoFrasco,
        costoEtiqueta,
        manoObra,
        costoIndirecto,
        costoTotalUnitario,
        margen,
        precioVentaUnitario,
        gananciaUnitario,
        costoTotal,
        precioTotalVenta,
        gananciaTotal
      };

      document.getElementById('resultado').innerHTML =
        `<p><strong>Velas a producir:</strong> ${cantidadVelas}</p>` +
        `<p><strong>Costo de cera por vela:</strong> $${costoCeraPorVela.toFixed(2)}</p>` +
        `<p><strong>Costo de fragancia por vela:</strong> $${costoFraganciaPorVela.toFixed(2)}</p>` +
        `<p><strong>Costo de colorante por vela:</strong> $${costoColorantePorVela.toFixed(2)}</p>` +
        `<p><strong>Mecha/Pabilo:</strong> $${costoMecha.toFixed(2)}</p>` +
        `<p><strong>Frasco:</strong> $${costoFrasco.toFixed(2)}</p>` +
        `<p><strong>Etiqueta:</strong> $${costoEtiqueta.toFixed(2)}</p>` +
        `<p><strong>Mano de obra:</strong> $${manoObra.toFixed(2)}</p>` +
        `<p><strong>Costos indirectos:</strong> $${costoIndirecto.toFixed(2)}</p>` +
        `<hr><p><strong>Costo total unitario:</strong> $${costoTotalUnitario.toFixed(2)}</p>` +
        `<p><strong>Precio sugerido de venta por unidad (con ${margen}% de margen):</strong> $${precioVentaUnitario.toFixed(2)}</p>` +
        `<p><strong>Ganancia estimada por unidad:</strong> $${gananciaUnitario.toFixed(2)}</p>` +
        `<hr><p><strong>Costo total para ${cantidadVelas} velas:</strong> $${costoTotal.toFixed(2)}</p>` +
        `<p><strong>Precio total sugerido de venta:</strong> $${precioTotalVenta.toFixed(2)}</p>` +
        `<p><strong>Ganancia total estimada:</strong> $${gananciaTotal.toFixed(2)}</p>`;
      
      // Mostrar botones de descarga
      document.getElementById('botonesDescarga').style.display = 'flex';
    }

    function generarPDF() {
      const { jsPDF } = window.jspdf;
      const doc = new jsPDF();
      
      // Logo y título
      doc.setFontSize(20);
      doc.setTextColor(217, 122, 155);
      doc.text('Soluna - Calculadora de Costos de Velas', 105, 20, { align: 'center' });
      
      // Información general
      doc.setFontSize(12);
      doc.setTextColor(0, 0, 0);
      doc.text(`Fecha: ${new Date().toLocaleDateString()}`, 14, 30);
      doc.text(`Velas a producir: ${resultadosCalculo.cantidadVelas}`, 14, 40);
      
      // Tabla de costos unitarios
      doc.setFontSize(14);
      doc.setTextColor(217, 122, 155);
      doc.text('Costos Unitarios por Vela', 14, 50);
      
      doc.setFontSize(10);
      doc.setTextColor(0, 0, 0);
      doc.autoTable({
        startY: 55,
        head: [['Concepto', 'Costo ($)']],
        body: [
          ['Cera', `$${resultadosCalculo.costoCeraPorVela.toFixed(2)}`],
          ['Fragancia', `$${resultadosCalculo.costoFraganciaPorVela.toFixed(2)}`],
          ['Colorante', `$${resultadosCalculo.costoColorantePorVela.toFixed(2)}`],
          ['Mecha/Pabilo', `$${resultadosCalculo.costoMecha.toFixed(2)}`],
          ['Frasco', `$${resultadosCalculo.costoFrasco.toFixed(2)}`],
          ['Etiqueta', `$${resultadosCalculo.costoEtiqueta.toFixed(2)}`],
          ['Mano de obra', `$${resultadosCalculo.manoObra.toFixed(2)}`],
          ['Costos indirectos', `$${resultadosCalculo.costoIndirecto.toFixed(2)}`],
          ['TOTAL', `$${resultadosCalculo.costoTotalUnitario.toFixed(2)}`]
        ],
        theme: 'grid',
        headStyles: {
          fillColor: [227, 141, 168],
          textColor: [255, 255, 255]
        }
      });
      
      // Resultados finales
      doc.setFontSize(14);
      doc.setTextColor(217, 122, 155);
      doc.text('Resultados Finales', 14, doc.autoTable.previous.finalY + 15);
      
      doc.setFontSize(10);
      doc.setTextColor(0, 0, 0);
      doc.autoTable({
        startY: doc.autoTable.previous.finalY + 20,
        columns: [
          { header: 'Concepto', dataKey: 'concepto' },
          { header: 'Unitario', dataKey: 'unitario' },
          { header: 'Total', dataKey: 'total' }
        ],
        body: [
          {
            concepto: 'Costo de producción',
            unitario: `$${resultadosCalculo.costoTotalUnitario.toFixed(2)}`,
            total: `$${resultadosCalculo.costoTotal.toFixed(2)}`
          },
          {
            concepto: `Precio de venta (${resultadosCalculo.margen}% margen)`,
            unitario: `$${resultadosCalculo.precioVentaUnitario.toFixed(2)}`,
            total: `$${resultadosCalculo.precioTotalVenta.toFixed(2)}`
          },
          {
            concepto: 'Ganancia estimada',
            unitario: `$${resultadosCalculo.gananciaUnitario.toFixed(2)}`,
            total: `$${resultadosCalculo.gananciaTotal.toFixed(2)}`
          }
        ],
        theme: 'grid',
        headStyles: {
          fillColor: [227, 141, 168],
          textColor: [255, 255, 255]
        }
      });
      
      // Pie de página
      const pageCount = doc.internal.getNumberOfPages();
      for(let i = 1; i <= pageCount; i++) {
        doc.setPage(i);
        doc.setFontSize(10);
        doc.setTextColor(150, 150, 150);
        doc.text(`Página ${i} de ${pageCount}`, 105, 287, { align: 'center' });
      }
      
      doc.save('Calculadora_Costos_Velas_Soluna.pdf');
    }

    function generarExcel() {
      // Crear libro de Excel
      const wb = XLSX.utils.book_new();
      
      // Datos para la hoja de cálculo
      const datos = [
        ["Concepto", "Valor Unitario ($)", "Valor Total ($)"],
        ["Cera", resultadosCalculo.costoCeraPorVela, resultadosCalculo.costoCeraPorVela * resultadosCalculo.cantidadVelas],
        ["Fragancia", resultadosCalculo.costoFraganciaPorVela, resultadosCalculo.costoFraganciaPorVela * resultadosCalculo.cantidadVelas],
        ["Colorante", resultadosCalculo.costoColorantePorVela, resultadosCalculo.costoColorantePorVela * resultadosCalculo.cantidadVelas],
        ["Mecha/Pabilo", resultadosCalculo.costoMecha, resultadosCalculo.costoMecha * resultadosCalculo.cantidadVelas],
        ["Frasco", resultadosCalculo.costoFrasco, resultadosCalculo.costoFrasco * resultadosCalculo.cantidadVelas],
        ["Etiqueta", resultadosCalculo.costoEtiqueta, resultadosCalculo.costoEtiqueta * resultadosCalculo.cantidadVelas],
        ["Mano de obra", resultadosCalculo.manoObra, resultadosCalculo.manoObra * resultadosCalculo.cantidadVelas],
        ["Costos indirectos", resultadosCalculo.costoIndirecto, resultadosCalculo.costoIndirecto * resultadosCalculo.cantidadVelas],
        ["TOTAL COSTOS", resultadosCalculo.costoTotalUnitario, resultadosCalculo.costoTotal],
        ["", "", ""],
        [`Precio venta (${resultadosCalculo.margen}% margen)`, resultadosCalculo.precioVentaUnitario, resultadosCalculo.precioTotalVenta],
        ["Ganancia estimada", resultadosCalculo.gananciaUnitario, resultadosCalculo.gananciaTotal],
        ["", "", ""],
        ["Resumen", "", ""],
        ["Velas a producir", resultadosCalculo.cantidadVelas, ""],
        ["Costo unitario", resultadosCalculo.costoTotalUnitario, ""],
        ["Precio unitario", resultadosCalculo.precioVentaUnitario, ""],
        ["Ganancia unitaria", resultadosCalculo.gananciaUnitario, ""]
      ];
      
      const ws = XLSX.utils.aoa_to_sheet(datos);
      
      // Añadir estilo a los encabezados
      if(!ws['!cols']) ws['!cols'] = [];
      ws['!cols'][0] = {width: 25};
      ws['!cols'][1] = {width: 15};
      ws['!cols'][2] = {width: 15};
      
      // Añadir hoja al libro
      XLSX.utils.book_append_sheet(wb, ws, "Costos Velas");
      
      // Generar archivo Excel
      XLSX.writeFile(wb, "Calculadora_Costos_Velas_Soluna.xlsx");
    }
  </script>
</body>
</html>
