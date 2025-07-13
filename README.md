# malla_college_comercialuc}
<!DOCTYPE html>
<html lang="es">
<head>
  <meta charset="UTF-8">
  <title>Malla Ingeniería Comercial CCSS 2023</title>
  <style>
    body {
      font-family: sans-serif;
      background: #ffe4e1;
      color: #333;
      margin: 20px;
    }
    .semestre {
      margin-bottom: 30px;
    }
    .curso {
      display: inline-block;
      margin: 5px;
      padding: 10px;
      border: 2px solid #ffb6c1;
      border-radius: 8px;
      background-color: #ffd6dd;
      cursor: pointer;
      position: relative;
      min-width: 160px;
      text-align: center;
    }
    .curso.bloqueado {
      opacity: 0.5;
      border-style: dashed;
      cursor: not-allowed;
    }
    .curso.aprobado {
      background-color: #c8e6c9;
      text-decoration: line-through;
    }
    .teddy {
      position: absolute;
      top: -10px;
      right: -10px;
      width: 30px;
      height: 30px;
      background: url('https://i.imgur.com/UdBdSWF.png') no-repeat center center;
      background-size: contain;
    }
  </style>
</head>
<body>
  <h1>Malla Ingeniería Comercial CCSS - Admisión 2023 🧸</h1>

  <div id="malla"></div>

  <script>
    // Definición de datos
    const semestres = [
      ["CLG0901", "MAT1000", "SOL100", "EAE1110", "PSI1101", "AFG Teológico"],
      ["SOL312", "Historia", "EAA1210", "MAT1610", "MAT1279"],
      ["EAE1210", "EAA1110", "EAA1510", "MAT1620", "IIC1103"],
      ["Ciencia Política", "EAF2010", "EAA1220", "EAE1220", "EAA1520"],
      ["GEO103", "EAE2110", "EAE2510", "EAA2410", "FIL2001", "AFG Pensamiento Matemático"],
      ["Investigación", "EAE2210", "EAA2310", "EAA2210", "AFG Artes", "AFG Ciencia y Tecnología"],
      ["EAA2220","EAA2230","EAA2071","EAA209L","EAA209C","EAA338B","FIL209"],
      ["EAA2110","EAA2095","AFG Sustentabilidad","AFG Salud y Bienestar"],
      ["EAE2120","EAA2420","EAA2320"],
      ["EAA2240","EAE2220","EAF2500"],
      ["EAA3401","EAA3201","EAA3501","EAA3XX","EAX3XX"],
      ["EAA3601","EAA3101","EAA3XX","EAX3XX","EAA3500"]
    ];

    const prereqs = {
      "EAF2010": ["MAT1620"],
      "EAA1220": ["EAA1510"],
      "EAA1520": ["EAA1510"],
      "EAE2510": ["EAA1510"],
      "EAA2310": ["EAA1510"],
      "EAE2110": ["EAE1220", "EAF2010"],
      "EAA2410": ["EAA1220"],
      "EAE2210": ["EAE1220", "EAE2510"],
      "EAA2210": ["EAA1520", "EAA1220", "IIC1103", "EAE2210", "IIC1103"],
      "EAA2220": ["EAA2410"],
      "EAA2230": ["EAA2410"],
      "FIL209": ["EAA2410", "EAE1220"],
      "EAA2110": ["FIL209"],
      "EAE2120": ["EAE2110"],
      "EAA2420": ["EAA2410", "EAE2120"],
      "EAA2320": ["EAE2510", "EAA2310"],
      "EAA2240": ["EAA2210", "EAA2220"],
      "EAE2220": ["EAE2210"],
      "EAF2500": ["EAA2110","EAA2220","EAA2310","EAE2120","EAE2210"],
      "EAA3201": ["EAA2240"],
      "EAA3401": ["EAA2110","EAA2240","EAA2320"],
      "EAA3501": ["EAA2420","EAE2510","EAE2130"],
      "EAA3601": ["EAA3401","EAA3201","EAA2420"],
      "EAA3101": ["EAA2110","EAA3401","EAA305A","EAA240A"],
      "EAA209L": ["EAA2410"],
      "EAA209C": ["EAA2410"],
      "EAA338B": ["EAA2310"],
      "EAA2095": ["EAA2410","EAA2310"],
      "EAA2071": ["EAA2310","EAA2410"],
    };

    const aprobados = new Set();

    function generarMalla() {
      const cont = document.getElementById("malla");
      semestres.forEach((lista, i) => {
        const divs = document.createElement("div");
        divs.className = "semestre";
        divs.innerHTML = `<h2>Semestre ${i + 1}</h2>`;
        lista.forEach(c => {
          const cu = document.createElement("div");
          cu.className = "curso";
          cu.id = c;
          cu.innerHTML = `<div class="teddy"></div><strong>${c}</strong>`;
          cu.onclick = () => toggle(cu);
          divs.appendChild(cu);
        });
        cont.appendChild(divs);
      });
      actualizaEstados();
    }

    function toggle(elem) {
      const c = elem.id;
      if (elem.classList.contains("bloqueado")) return;
      if (aprobados.has(c)) aprobados.delete(c);
      else aprobados.add(c);
      actualizaEstados();
    }

    function actualizaEstados() {
      document.querySelectorAll(".curso").forEach(el => {
        const c = el.id;
        if (aprobados.has(c)) {
          el.classList.add("aprobado");
          el.classList.remove("bloqueado");
        } else {
          const pre = prereqs[c] || [];
          const ok = pre.every(r => aprobados.has(r));
          if (pre.length > 0 && ok) el.classList.remove("bloqueado");
          else if (pre.length > 0) el.classList.add("bloqueado");
          else el.classList.remove("bloqueado");
          el.classList.remove("aprobado");
        }
      });
    }

    generarMalla();
  </script>
</body>
</html>
