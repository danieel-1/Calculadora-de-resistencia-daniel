# Calculadora-de-resistencia-daniel
<!DOCTYPE html>
<html lang="ca">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Codi de Colors de Resistors - Aplicació Interactiva</title>
    <style>
        :root {
            /* Definició de colors per CSS */
            --color-Negre: #000000;
            --color-Marró: #8B4513;
            --color-Vermell: #FF0000;
            --color-Taronja: #FF8C00;
            --color-Groc: #FFFF00;
            --color-Verd: #008000;
            --color-Blau: #0000FF;
            --color-Violeta: #8A2BE2;
            --color-Gris: #808080;
            --color-Blanc: #FFFFFF;
            --color-Or: #FFD700;
            --color-Plata: #C0C0C0;
            --color-Sense_color: #E6E6FA; /* Lavanda clar per simular sense color */
            --fons-aplicacio: #f4f4f9;
            --color-text: #333;
            --color-bord: #ccc;
            --color-primari: #007BFF;
            --color-secundari: #6c757d;
        }

        body {
            font-family: Arial, sans-serif;
            margin: 0;
            padding: 20px;
            background-color: var(--fons-aplicacio);
            color: var(--color-text);
        }

        .container {
            max-width: 1200px;
            margin: 0 auto;
            background: #fff;
            padding: 30px;
            border-radius: 8px;
            box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
        }

        h1, h2 {
            color: var(--color-primari);
            border-bottom: 2px solid var(--color-primari);
            padding-bottom: 10px;
            margin-top: 20px;
        }

        /* --- PESTANYES --- */
        .tabs {
            display: flex;
            margin-bottom: 20px;
            border-bottom: 1px solid var(--color-bord);
        }

        .tab-button {
            padding: 10px 15px;
            cursor: pointer;
            border: none;
            background: transparent;
            font-size: 16px;
            color: var(--color-secundari);
            transition: all 0.3s ease;
        }

        .tab-button.active {
            border-bottom: 3px solid var(--color-primari);
            color: var(--color-primari);
            font-weight: bold;
        }

        .tab-content {
            display: none;
        }

        .tab-content.active {
            display: block;
        }

        /* --- VISUALITZACIÓ DEL RESISTOR --- */
        .resistor-display {
            display: flex;
            align-items: center;
            justify-content: center;
            margin: 30px 0;
        }

        .resistor-cos {
            width: 250px;
            height: 60px;
            background-color: #A9A9A9; /* Gris fosc per al cos */
            border-radius: 10px;
            position: relative;
            display: flex;
            justify-content: space-between;
            align-items: center;
            padding: 0 5px;
        }

        .resistor-terminal {
            width: 50px;
            height: 5px;
            background-color: var(--color-secundari);
        }

        .resistor-banda {
            width: 15px;
            height: 100%;
            margin: 0 5px;
            background-color: var(--color-Negre); /* Color inicial */
        }

        /* Posicionament de les bandes (ajustar per a 4 bandes) */
        #banda1, #banda2 { margin-right: 2px; }
        #banda3 { margin-left: 10px; margin-right: 15px; } /* Separació per al multiplicador */
        #banda4 { margin-left: auto; margin-right: 5px; } /* A la dreta per a la tolerància */

        /* --- TAULA DE CODI DE COLORS --- */
        table {
            width: 100%;
            border-collapse: collapse;
            margin: 20px 0;
        }

        th, td {
            border: 1px solid var(--color-bord);
            padding: 8px;
            text-align: center;
        }

        th {
            background-color: var(--color-primari);
            color: white;
        }

        /* Estil per als camps de formulari */
        .input-group {
            margin-bottom: 15px;
            padding: 10px;
            border: 1px solid var(--color-bord);
            border-radius: 5px;
        }

        label {
            display: block;
            margin-bottom: 5px;
            font-weight: bold;
        }

        select, input[type="number"] {
            width: 100%;
            padding: 10px;
            border: 1px solid var(--color-bord);
            border-radius: 4px;
            box-sizing: border-box;
            font-size: 16px;
        }

        .resultat {
            margin-top: 20px;
            padding: 15px;
            border: 2px solid var(--color-secundari);
            border-radius: 5px;
            background-color: #e9ecef;
            font-size: 1.1em;
        }

        .resultat p {
            margin: 5px 0;
        }

        .error {
            color: red;
            font-weight: bold;
        }
    </style>
</head>
<body>
    <div class="container">
        <h1>Codi de Colors de Resistors: Aplicació Interactiva</h1>

        <h2>Teoria</h2>
        <p>Les resistències tenen com a missió limitar el pas del corrent per un circuit. Les més comunes estan formades per carboni (que és un mal conductor de l'electricitat) recobertes d'un material ceràmic.</p>
        <p>El valor de les resistències es mesura en **ohms ($\Omega$)**. Per identificar el valor de les resistències s'utilitza un **codi d'anells de colors** que tenen pintats. Normalment, una resistència té quatre anells o bandes de colors, l'últim dels quals acostuma a ser daurat o platejat.</p>

        <h2>Taula de Codi de Colors</h2>
        <table id="taula-colors">
            <thead>
                <tr>
                    <th>Color</th>
                    <th>Ordre (Significat)</th>
                    <th>1a xifra</th>
                    <th>2a xifra</th>
                    <th>Multiplicador</th>
                    <th>Tolerància</th>
                </tr>
            </thead>
            <tbody>
                </tbody>
        </table>

        <h2>Càlcul Interactiu</h2>

        <div class="tabs">
            <button class="tab-button active" onclick="openTab(event, 'ColorToValue')">1. Color → Valor 🎨➡️V</button>
            <button class="tab-button" onclick="openTab(event, 'ValueToColor')">2. Valor → Color V➡️🎨</button>
        </div>

        <div id="ColorToValue" class="tab-content active">
            <h3>Calcula el valor a partir dels 4 colors</h3>
            
            <div class="resistor-display">
                <div class="resistor-terminal"></div>
                <div class="resistor-cos">
                    <div class="resistor-banda" id="banda1"></div>
                    <div class="resistor-banda" id="banda2"></div>
                    <div class="resistor-banda" id="banda3"></div>
                    <div class="resistor-banda" id="banda4"></div>
                </div>
                <div class="resistor-terminal"></div>
            </div>

            <div class="input-group">
                <label for="color1">1a Franja (1a Xifra):</label>
                <select id="color1" onchange="updateResistor()"></select>
            </div>
            <div class="input-group">
                <label for="color2">2a Franja (2a Xifra):</label>
                <select id="color2" onchange="updateResistor()"></select>
            </div>
            <div class="input-group">
                <label for="color3">3a Franja (Multiplicador):</label>
                <select id="color3" onchange="updateResistor()"></select>
            </div>
            <div class="input-group">
                <label for="color4">4a Franja (Tolerància):</label>
                <select id="color4" onchange="updateResistor()"></select>
            </div>
            
            <button onclick="calculateValue()" style="padding: 10px 20px; background-color: var(--color-primari); color: white; border: none; border-radius: 5px; cursor: pointer;">Calcular Valor</button>
            
            <div class="resultat" id="resultat-valor">
                <p><strong>Valor Nominal:</strong> --</p>
                <p><strong>Tolerància:</strong> --</p>
                <p><strong>Interval:</strong> [Mínim: -- | Màxim: --]</p>
            </div>
        </div>

        <div id="ValueToColor" class="tab-content">
            <h3>Calcula els 4 colors a partir del valor</h3>
            
            <div class="input-group">
                <label for="input-valor">Valor Nominal ($\Omega$):</label>
                <input type="number" id="input-valor" value="3200" min="0">
            </div>
            <div class="input-group">
                <label for="input-tolerancia">Tolerància (Color de la 4a franja):</label>
                <select id="input-tolerancia">
                    <option value="Or">Or (±5%)</option>
                    <option value="Plata">Plata (±10%)</option>
                    <option value="Sense_color">Sense color (±20%)</option>
                    <option value="Marró">Marró (±1%)</option>
                    <option value="Vermell">Vermell (±2%)</option>
                </select>
            </div>

            <button onclick="calculateColors()" style="padding: 10px 20px; background-color: var(--color-primari); color: white; border: none; border-radius: 5px; cursor: pointer;">Calcular Colors</button>
            
            <div class="resultat" id="resultat-colors">
                <p><strong>1a Franja:</strong> --</p>
                <p><strong>2a Franja:</strong> --</p>
                <p><strong>3a Franja (Multiplicador):</strong> --</p>
                <p><strong>4a Franja (Tolerància):</strong> --</p>
                <p id="colors-warning" class="error" style="display: none;">Nota: El valor introduit s'ha aproximat a la combinació de colors més propera.</p>
            </div>
        </div>
    </div>

    <script>
        // --- DADES DEL CODI DE COLORS (Extretes de la imatge) ---
        const colorData = [
            { color: "Negre", xifra: 0, mult: 1, tol: "" },
            { color: "Marró", xifra: 1, mult: 10, tol: "±1%" },
            { color: "Vermell", xifra: 2, mult: 100, tol: "±2%" },
            { color: "Taronja", xifra: 3, mult: 1000, tol: "" },
            { color: "Groc", xifra: 4, mult: 10000, tol: "" },
            { color: "Verd", xifra: 5, mult: 100000, tol: "" },
            { color: "Blau", xifra: 6, mult: 1000000, tol: "" },
            { color: "Violeta", xifra: 7, mult: 10000000, tol: "" },
            { color: "Gris", xifra: 8, mult: 100000000, tol: "" },
            { color: "Blanc", xifra: 9, mult: 1000000000, tol: "" },
            { color: "Or", xifra: "", mult: 0.1, tol: "±5%" },
            { color: "Plata", xifra: "", mult: 0.01, tol: "±10%" },
            { color: "Sense_color", xifra: "", mult: "", tol: "±20%" }
        ];

        // --- FUNCIÓ D'INICIALITZACIÓ ---
        document.addEventListener('DOMContentLoaded', () => {
            populateTable();
            populateSelects();
            updateResistor(); // Estat inicial del resistor
        });

        // Omple la taula de colors amb les dades
        function populateTable() {
            const tbody = document.querySelector('#taula-colors tbody');
            tbody.innerHTML = '';
            colorData.forEach(d => {
                const tr = document.createElement('tr');
                tr.innerHTML = `
                    <td style="background-color: var(--color-${d.color.replace(' ', '_')}); color: ${d.color === 'Negre' || d.color === 'Marró' || d.color === 'Blau' ? 'white' : 'black'};">${d.color}</td>
                    <td>${d.xifra !== "" ? d.xifra : "--"}</td>
                    <td>${d.xifra !== "" ? d.xifra : "--"}</td>
                    <td>${d.xifra !== "" ? d.xifra : "--"}</td>
                    <td>${d.mult !== "" ? (d.mult >= 1000 ? 'x ' + d.mult.toLocaleString('ca') : 'x ' + d.mult) : "--"}</td>
                    <td>${d.tol !== "" ? d.tol : "--"}</td>
                `;
                tbody.appendChild(tr);
            });
        }

        // Omple els selectors de color
        function populateSelects() {
            const select1 = document.getElementById('color1');
            const select2 = document.getElementById('color2');
            const select3 = document.getElementById('color3');
            const select4 = document.getElementById('color4');

            // Colors per a la 1a i 2a xifra (Sense Or, Plata, Sense_color)
            const xifraColors = colorData.filter(d => d.xifra !== "" && d.color !== "Sense_color");
            xifraColors.forEach(d => {
                const option = `<option value="${d.color}">${d.color} (${d.xifra})</option>`;
                select1.innerHTML += option;
                select2.innerHTML += option;
            });

            // Colors per al Multiplicador (Tots excepte Sense_color)
            const multColors = colorData.filter(d => d.mult !== "" && d.color !== "Sense_color");
            multColors.forEach(d => {
                const option = `<option value="${d.color}">${d.color} (x${d.mult})</option>`;
                select3.innerHTML += option;
            });

            // Colors per a la Tolerància (Només els que tenen tolerància)
            const tolColors = colorData.filter(d => d.tol !== "");
            tolColors.forEach(d => {
                const option = `<option value="${d.color}">${d.color} (${d.tol})</option>`;
                select4.innerHTML += option;
            });

            // Set default values (e.g., Taronja - Vermell - Vermell - Or)
            select1.value = 'Taronja';
            select2.value = 'Vermell';
            select3.value = 'Vermell';
            select4.value = 'Or';
        }

        // Funció per actualitzar visualment el resistor
        function updateResistor() {
            const c1 = document.getElementById('color1').value;
            const c2 = document.getElementById('color2').value;
            const c3 = document.getElementById('color3').value;
            const c4 = document.getElementById('color4').value;

            document.getElementById('banda1').style.backgroundColor = `var(--color-${c1})`;
            document.getElementById('banda2').style.backgroundColor = `var(--color-${c2})`;
            document.getElementById('banda3').style.backgroundColor = `var(--color-${c3})`;
            document.getElementById('banda4').style.backgroundColor = `var(--color-${c4})`;
        }

        // --- PESTANYA 1: FUNCIÓ DE CÀLCUL (COLOR A VALOR) ---
        function calculateValue() {
            const c1 = document.getElementById('color1').value;
            const c2 = document.getElementById('color2').value;
            const c3 = document.getElementById('color3').value;
            const c4 = document.getElementById('color4').value;

            const d1 = colorData.find(d => d.color === c1);
            const d2 = colorData.find(d => d.color === c2);
            const d3 = colorData.find(d => d.color === c3);
            const d4 = colorData.find(d => d.color === c4);

            const xifra1 = d1.xifra;
            const xifra2 = d2.xifra;
            const mult = d3.mult;
            const tolText = d4.tol;

            if (xifra1 === "" || xifra2 === "" || mult === "" || tolText === "") {
                document.getElementById('resultat-valor').innerHTML = `<p class="error">Error: Selecció de colors invàlida per a les 4 bandes.</p>`;
                return;
            }

            // Càlcul del valor nominal
            const valorNominal = (xifra1 * 10 + xifra2) * mult;

            // Càlcul de la tolerància
            const tolPercent = parseFloat(tolText.replace('±', '').replace('%', '')) / 100;
            const tolAbsoluta = valorNominal * tolPercent;
            const valorMinim = valorNominal - tolAbsoluta;
            const valorMaxim = valorNominal + tolAbsoluta;

            // Funció per formatejar els Ohms (amb kΩ)
            function formatOhm(ohm) {
                if (ohm >= 1000000) return (ohm / 1000000).toLocaleString('ca') + ' MΩ';
                if (ohm >= 1000) return (ohm / 1000).toLocaleString('ca') + ' kΩ';
                return ohm.toLocaleString('ca') + ' Ω';
            }

            const resultatDiv = document.getElementById('resultat-valor');
            resultatDiv.innerHTML = `
                <p><strong>Valor Nominal:</strong> ${formatOhm(valorNominal)} (${valorNominal.toLocaleString('ca')} Ω)</p>
                <p><strong>Tolerància:</strong> ${tolText} (±${formatOhm(tolAbsoluta)})</p>
                <p><strong>Interval:</strong> [Mínim: ${formatOhm(valorMinim)} | Màxim: ${formatOhm(valorMaxim)}]</p>
                <p style="font-size: 0.9em;">(Càlcul: (${xifra1}${xifra2}) x ${d3.mult.toLocaleString('ca')} $\Omega$)</p>
            `;
        }

        // --- PESTANYA 2: FUNCIÓ DE CÀLCUL (VALOR A COLOR) ---
        function calculateColors() {
            const inputValor = parseFloat(document.getElementById('input-valor').value);
            const inputTolColor = document.getElementById('input-tolerancia').value;
            const resultatDiv = document.getElementById('resultat-colors');
            const warningP = document.getElementById('colors-warning');

            if (isNaN(inputValor) || inputValor < 0) {
                resultatDiv.innerHTML = `<p class="error">Error: Introdueix un valor numèric vàlid per a la resistència.</p>`;
                warningP.style.display = 'none';
                return;
            }

            // Pas 1: Aïllar el valor nominal per a les xifres i el multiplicador
            let valorObjectiu = inputValor;
            let millorCombinacio = null;
            let errorMinim = Infinity;

            const xifraColors = colorData.filter(d => d.xifra !== "" && d.color !== "Sense_color");
            const multColors = colorData.filter(d => d.mult !== "" && d.color !== "Sense_color");

            // Iterar per trobar la millor combinació xifra1, xifra2, multiplicador
            for (const multD of multColors) {
                const mantissaReal = valorObjectiu / multD.mult;

                // Intentar arrodonir la mantissa a un nombre enter de 2 xifres (10-99)
                let mantissa = Math.round(mantissaReal);

                // Assegurar que la mantissa estigui dins del rang 00-99 (0-99), tot i que en 4 bandes hauria de ser 10-99 o 01-99
                if (mantissa > 99) continue; 
                if (mantissa < 0) mantissa = 0; // Només positiu

                const valorCalculat = mantissa * multD.mult;
                const errorActual = Math.abs(valorCalculat - valorObjectiu);

                if (errorActual < errorMinim) {
                    errorMinim = errorActual;
                    millorCombinacio = {
                        valor: valorCalculat,
                        mantissa: mantissa,
                        mult: multD.color,
                        xifra1: xifraColors.find(d => d.xifra === Math.floor(mantissa / 10)).color,
                        xifra2: xifraColors.find(d => d.xifra === mantissa % 10).color,
                        tolerancia: inputTolColor
                    };
                }
            }

            if (!millorCombinacio) {
                resultatDiv.innerHTML = `<p class="error">No es va poder trobar una combinació de 4 bandes per a aquest valor.</p>`;
                warningP.style.display = 'none';
                return;
            }

            // Format del resultat
            resultatDiv.innerHTML = `
                <p><strong>Valor Seleccionat:</strong> ${millorCombinacio.valor.toLocaleString('ca')} Ω</p>
                <p><strong>1a Franja (Mantissa ${Math.floor(millorCombinacio.mantissa / 10)}):</strong> ${millorCombinacio.xifra1}</p>
                <p><strong>2a Franja (Mantissa ${millorCombinacio.mantissa % 10}):</strong> ${millorCombinacio.xifra2}</p>
                <p><strong>3a Franja (Multiplicador):</strong> ${millorCombinacio.mult}</p>
                <p><strong>4a Franja (Tolerància):</strong> ${millorCombinacio.tolerancia.replace('_', ' ')}</p>
            `;

            // Mostrar l'advertència si el valor s'ha hagut d'aproximar
            if (errorMinim > 0) {
                warningP.style.display = 'block';
            } else {
                warningP.style.display = 'none';
            }
        }

        // --- FUNCIÓ DE CANVI DE PESTANYA ---
        function openTab(evt, tabName) {
            let i, tabcontent, tablinks;

            // Amaga tots els continguts de les pestanyes
            tabcontent = document.getElementsByClassName("tab-content");
            for (i = 0; i < tabcontent.length; i++) {
                tabcontent[i].style.display = "none";
            }

            // Desactiva tots els botons de les pestanyes
            tablinks = document.getElementsByClassName("tab-button");
            for (i = 0; i < tablinks.length; i++) {
                tablinks[i].className = tablinks[i].className.replace(" active", "");
            }

            // Mostra el contingut de la pestanya actual i marca el botó com a actiu
            document.getElementById(tabName).style.display = "block";
            evt.currentTarget.className += " active";
        }
    </script>
</body>
</html>
