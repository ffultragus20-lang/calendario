<!DOCTYPE html>
<html lang="es">
<head>
<meta charset="UTF-8">

<meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">

<title>Calendario</title>

<style>
body {
    font-family: Arial, sans-serif;
    background: #f2f2f2;
    display: flex;
    justify-content: center;
    margin-top: 30px;
}

.contenedor {
    background: white;
    padding: 15px;
    border-radius: 14px;
    width: 320px;
}

.cabecera {
    display: flex;
    justify-content: space-between;
    align-items: center;
    margin-bottom: 10px;
}

.boton {
    cursor: pointer;
    font-size: 18px;
    padding: 4px 10px;
    border-radius: 6px;
    background: #e6e6e6;
}

.titulo {
    font-size: 20px;
    font-weight: bold;
    cursor: pointer;
}

.calendario {
    display: grid;
    grid-template-columns: repeat(7, 1fr);
    gap: 6px;
}

.dia-semana {
    text-align: center;
    font-size: 12px;
    color: #666;
    font-weight: bold;
}

.dia {
    width: 40px;
    height: 40px;
    border: 1px solid #ddd;
    text-align: center;
    line-height: 40px;
    font-weight: bold;
    cursor: pointer;
    border-radius: 8px;
    background: #fafafa;
}

.dia.marcado {
    background: #ffcccc;
    color: red;
}

.dia.hoy {
    background: #ff3b3b;
    color: white;
}

/* Vista anual */
.anual {
    display: grid;
    grid-template-columns: repeat(3, 1fr);
    gap: 10px;
}

.mes-anual {
    padding: 15px 0;
    background: #f7f7f7;
    text-align: center;
    border-radius: 10px;
    font-weight: bold;
    cursor: pointer;
}

/* ===============================
   🌙 MODO OSCURO AUTOMÁTICO
   =============================== */
@media (prefers-color-scheme: dark) {

  body {
    background: #000;
    color: #fff;
  }

  .contenedor {
    background: #1c1c1e;
  }

  .boton {
    background: #2c2c2e;
    color: #fff;
  }

  .titulo {
    color: #fff;
  }

  .dia-semana {
    color: #b0b0b5;
  }

  .dia {
    background: #2c2c2e;
    border: 1px solid #3a3a3c;
    color: #fff;
  }

  .dia:hover {
    background: #3a3a3c;
  }

  .dia.marcado {
    background: #4a1c1c;
    color: #ff6b6b;
  }

  .dia.hoy {
    background: #ff3b30;
    color: #fff;
  }

  .mes-anual {
    background: #2c2c2e;
    color: #fff;
  }
}
</style>
</head>

<body>

<div class="contenedor">
    <div class="cabecera">
        <div class="boton" id="prev">‹</div>
        <div class="titulo" id="titulo"></div>
        <div class="boton" id="next">›</div>
    </div>

    <div id="contenido"></div>
</div>

<script>
const meses = [
    "Enero","Febrero","Marzo","Abril","Mayo","Junio",
    "Julio","Agosto","Septiembre","Octubre","Noviembre","Diciembre"
];

const diasSemana = ["L","M","M","J","V","S","D"];

let hoy = new Date();
let fecha = new Date(2026, hoy.getMonth(), 1);
let vista = "mes";

const titulo = document.getElementById("titulo");
const contenido = document.getElementById("contenido");

document.getElementById("prev").onclick = () => {
    fecha.setMonth(fecha.getMonth() - 1);
    render();
};

document.getElementById("next").onclick = () => {
    fecha.setMonth(fecha.getMonth() + 1);
    render();
};

titulo.onclick = () => {
    vista = vista === "mes" ? "año" : "mes";
    render();
};

function render() {
    contenido.innerHTML = "";
    titulo.textContent = meses[fecha.getMonth()] + " " + fecha.getFullYear();
    vista === "mes" ? renderMes() : renderAño();
}

function renderMes() {
    const cal = document.createElement("div");
    cal.className = "calendario";

    diasSemana.forEach(d => {
        const ds = document.createElement("div");
        ds.className = "dia-semana";
        ds.textContent = d;
        cal.appendChild(ds);
    });

    const año = fecha.getFullYear();
    const mes = fecha.getMonth();
    const primerDia = new Date(año, mes, 1).getDay() || 7;
    const totalDias = new Date(año, mes + 1, 0).getDate();

    for (let i = 1; i < primerDia; i++) {
        cal.appendChild(document.createElement("div"));
    }

    for (let d = 1; d <= totalDias; d++) {
        const div = document.createElement("div");
        div.className = "dia";
        div.textContent = d;

        if (
            d === hoy.getDate() &&
            mes === hoy.getMonth() &&
            año === hoy.getFullYear()
        ) {
            div.classList.add("hoy");
        }

        div.onclick = () => {
            div.classList.toggle("marcado");
            div.textContent = div.classList.contains("marcado") ? "X" : d;
        };

        cal.appendChild(div);
    }

    contenido.appendChild(cal);
}

function renderAño() {
    const grid = document.createElement("div");
    grid.className = "anual";

    meses.forEach((m, i) => {
        const div = document.createElement("div");
        div.className = "mes-anual";
        div.textContent = m;
        div.onclick = () => {
            fecha.setMonth(i);
            vista = "mes";
            render();
        };
        grid.appendChild(div);
    });

    contenido.appendChild(grid);
}

render();
</script>

</body>
</html>
