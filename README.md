// === Datos de materias y requisitos ===
const materias = {
    "PRIMER AÑO": [
        { cod: "1.1", nombre: "Matemática", req: [] },
        { cod: "1.2", nombre: "Química General", req: [] },
        { cod: "1.3", nombre: "Biología General y Celular", req: [] },
        { cod: "1.4", nombre: "Física Aplicada", req: ["1.1"] },
        { cod: "1.5", nombre: "Química Inorgánica", req: ["1.2"] },
        { cod: "1.6", nombre: "Morfología", req: ["1.3"] }
    ],
    "SEGUNDO AÑO": [
        { cod: "2.1", nombre: "Química Orgánica I", req: ["1.5", "1.2"] },
        { cod: "2.2", nombre: "Bioestadística", req: ["1.1"] },
        { cod: "2.3", nombre: "Química Analítica", req: ["1.1", "1.5", "1.2"] },
        { cod: "2.4", nombre: "Química Analítica Instrumental", req: ["1.4", "1.5", "2.3"] },
        { cod: "2.5", nombre: "Química Física Biológica", req: ["2.3", "1.4"] },
        { cod: "2.6", nombre: "Química Orgánica II", req: ["2.1", "2.3"] }
    ],
    "TERCER AÑO": [
        { cod: "3.1", nombre: "Química Biológica I", req: ["2.4", "2.6"] },
        { cod: "3.2", nombre: "Ética Profesional", req: ["1.6"] },
        { cod: "3.3", nombre: "Microbiología General", req: ["2.5", "2.6"] },
        { cod: "3.4", nombre: "Farmacología", req: ["2.1", "2.4", "2.6"] },
        { cod: "3.5", nombre: "Química Biológica II", req: ["2.5", "2.6", "3.1"] },
        { cod: "3.6", nombre: "Fisiología Humana", req: ["2.5", "3.1"] },
        { cod: "3.7", nombre: "Genética y Biología Molecular", req: ["3.1"] }
    ],
    "CUARTO AÑO": [
        { cod: "4.1", nombre: "Fisiopatología", req: ["3.3", "3.5", "3.6"] },
        { cod: "4.2", nombre: "Toxicología y Química Legal", req: ["3.4", "3.6", "3.7"] },
        { cod: "4.3", nombre: "Inmunología Clínica", req: ["3.3", "3.5", "3.7"] },
        { cod: "4.4", nombre: "Parasitología Humana", req: [] },
        { cod: "4.5", nombre: "Salud Pública y Epidemiología", req: ["3.2", "3.3"] },
        { cod: "4.6", nombre: "Bromatología y Nutrición", req: ["3.5"] },
        { cod: "4.7", nombre: "Virología Clínica", req: ["4.1", "4.3", "3.7"] }
    ],
    "QUINTO AÑO": [
        { cod: "5.1", nombre: "Bacteriología y Micología Clínica", req: ["4.1", "4.3"] },
        { cod: "5.2", nombre: "Química Clínica", req: ["3.2", "4.1"] },
        { cod: "5.3", nombre: "Bioquímica Avanzada", req: ["4.3", "4.4", "4.7"] },
        { cod: "5.4", nombre: "Hematología Clínica", req: ["4.1", "5.2"] },
        { cod: "5.5", nombre: "Emergentología Bioquímica", req: ["4.2", "5.2"] },
        { cod: "5.6", nombre: "Gestión de Laboratorio", req: ["4.5", "5.1"] },
        { cod: "5.7", nombre: "Endocrinología Clínica", req: ["5.2"] }
    ],
    "FINAL": [
        { cod: "PH", nombre: "Práctica Hospitalaria", req: ["5.4", "5.5", "5.6", "5.7", "4.1", "4.7", "5.1", "5.2", "5.3"] }
    ]
};

// === Estados iniciales ===
let estado = {};
Object.values(materias).flat().forEach(m => {
    estado[m.cod] = m.req.length === 0 ? "desbloqueada" : "bloqueada";
});

// === Guardar y cargar progreso ===
function guardarProgreso() {
    localStorage.setItem("estadoMaterias", JSON.stringify(estado));
}

function cargarProgreso() {
    const guardado = localStorage.getItem("estadoMaterias");
    if (guardado) estado = JSON.parse(guardado);
}

// === Renderizar ===
function render() {
    const contenedor = document.getElementById("contenedor");
    contenedor.innerHTML = "";
    for (const [anio, lista] of Object.entries(materias)) {
        const divAnio = document.createElement("div");
        divAnio.className = "anio";
        divAnio.innerHTML = `<h2>${anio}</h2>`;
        
        const carrusel = document.createElement("div");
        carrusel.className = "carrusel";

        lista.forEach(m => {
            const btn = document.createElement("div");
            btn.className = `materia ${estado[m.cod]}`;
            btn.innerText = `${m.cod} ${m.nombre}`;
            btn.onclick = () => aprobarMateria(m.cod);
            carrusel.appendChild(btn);
        });

        divAnio.appendChild(carrusel);
        contenedor.appendChild(divAnio);
    }
}

// === Aprobar materia y desbloquear dependientes ===
function aprobarMateria(cod) {
    if (estado[cod] === "bloqueada") {
        alert("Materia bloqueada. No cumple requisitos.");
        return;
    }
    estado[cod] = "aprobada";
    // Desbloquear materias
    Object.values(materias).flat().forEach(m => {
        if (estado[m.cod] === "bloqueada") {
            if (m.req.every(r => estado[r] === "aprobada")) {
                estado[m.cod] = "desbloqueada";
            }
        }
    });
    guardarProgreso();
    render();
}

// === Calcular promedio ===
document.getElementById("btnPromedio").onclick = () => {
    let notas = [];
    Object.values(materias).flat().forEach(m => {
        const valor = prompt(`Ingrese nota para ${m.cod} - ${m.nombre} (dejar vacío para omitir)`);
        if (valor && !isNaN(valor)) notas.push(parseFloat(valor));
    });
    if (notas.length > 0) {
        const promedio = (notas.reduce((a, b) => a + b, 0) / notas.length).toFixed(2);
        document.getElementById("resultadoPromedio").innerText = `Promedio general: ${promedio}`;
    } else {
        document.getElementById("resultadoPromedio").innerText = "No se ingresaron notas.";
    }
};

// === Inicializar ===
cargarProgreso();
render();
