body {
    font-family: Arial, sans-serif;
    background-color: #e6ffe6;
    margin: 0;
    padding: 0;
}

header {
    background-color: #228B22;
    color: white;
    padding: 15px;
    text-align: center;
}

h1 {
    margin: 0;
}

.anio {
    padding: 15px;
}

.anio h2 {
    background-color: #ccffcc;
    padding: 10px;
    border-radius: 10px;
}

.carrusel {
    display: flex;
    overflow-x: auto;
    gap: 10px;
    padding: 10px;
}

.carrusel::-webkit-scrollbar {
    height: 8px;
}

.carrusel::-webkit-scrollbar-thumb {
    background-color: #77dd77;
    border-radius: 4px;
}

.materia {
    flex: 0 0 auto;
    width: 220px;
    padding: 10px;
    border-radius: 10px;
    color: white;
    text-align: center;
    cursor: pointer;
    transition: transform 0.2s;
}

.materia:hover {
    transform: scale(1.05);
}

/* Colores de estado */
.bloqueada {
    background-color: #006400; /* Verde oscuro */
}

.desbloqueada {
    background-color: #77dd77; /* Verde pastel */
    color: black;
}

.aprobada {
    background-color: #a0e8e5; /* Turquesa pastel */
    color: black;
}

.promedio-section {
    padding: 20px;
    text-align: center;
}

#btnPromedio {
    background-color: #99ff99;
    border: none;
    padding: 10px 20px;
    font-size: 16px;
    cursor: pointer;
    border-radius: 8px;
}

#btnPromedio:hover {
    background-color: #77dd77;
}

#resultadoPromedio {
    margin-top: 15px;
    font-weight: bold;
    font-size: 18px;
}
