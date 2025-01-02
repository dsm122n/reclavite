<script setup>
import HelloWorld from './components/HelloWorld.vue'

const form = document.getElementById("reclabForm");
const examContainer = document.querySelector(".examenes");
const listado = document.getElementById("listado_ex");
var datosPaciente = {
    nombre: "",
    rut: "",
    fecha_nacimiento: "",
    edad: "",
    sexo: "",
    hipotesis_diagnóstica: ""

}
var datosPrescriptor = {
    nombre: "Daniel Arturo San Martín Martínez",
    profesion: "Médico Cirujano",
    cedula: "20.283.646-1",
    numero_registro: "861577",
    mail:"dasanmartinm@gmail.com",
    direccion: "Av. Independencia 1027, 8380453 Santiago, Independencia, Región Metropolitana"
}
const solicitud = [];
document.addEventListener("DOMContentLoaded", () => {


    form.addEventListener("click", (event) => {
        if (event.target.classList.contains("add")) {
            event.preventDefault();
            addExamToList(event.target.closest(".examen"));
        } else if (event.target.classList.contains("remove")) {
            event.preventDefault();
            removeLastExam();
        }
    });

    document.addEventListener("keydown", (event) => {
        const activeElement = document.activeElement;

        console.log(activeElement);
        if (activeElement.className === "examen-select" ) {
            if (event.key === "Enter") {
                const activeExam = document.querySelector(".examen-select");
                if (activeExam) {
                    addExamToList(activeExam.closest(".examen"));
                }
            } 
            // delete last exam if ctrl + backspace is pressed
            else if (event.key === "Backspace" && event.ctrlKey) {
                removeLastExam();
            }

        }
        if (activeElement.className === "examen-otro" && document.querySelector(".examen-select").innerHTML !== "") {
                
                if (event.key === "Enter") {
                    const activeExam = document.querySelector(".examen-otro");
                    if (activeExam) {
                        addExamToList(activeExam.closest(".examen"));
                    }
                } else if (event.key === "Backspace" && event.ctrlKey) {
                    removeLastExam();
                }
        } 
    });

    // fecha de nacimiento
    document.getElementById("fecha_nacimiento").addEventListener("change", (event) => {
        const age = calculateAge(event.target.value);
        document.getElementById("edad").value = age;
    }
    );


    function addExamToList(examElement) {
        const selectValue = examElement.querySelector(".examen-select").value;
        const otherValue = examElement.querySelector(".examen-otro").value;
        const examName = otherValue || selectValue;

        if (examName) {
            solicitud.push(examName);
            updateListado();
            document.querySelector(".examen-otro").value = "";
        }
    }

    function removeLastExam() {
        if (solicitud.length > 0) {
            solicitud.pop();
            updateListado();
        }
    }

    function updateListado() {
        listado.innerHTML = ""; // Clear previous list
        solicitud.forEach((exam) => {
            const listItem = document.createElement("option");
            listItem.textContent = exam;
            listado.appendChild(listItem);
        });
    }

    

    
    
});

// calculate age from date of birth
function calculateAge(dateOfBirth) {
    const dob = new Date(dateOfBirth);
    const now = new Date();
    const diff = now - dob;
    const ageDate = new Date(diff);
    return Math.abs(ageDate.getUTCFullYear() - 1970);
}

function printPDF() {
    console.log("Printing PDF...");
const printWindow = window.open("", "_blank");
datosPaciente.nombre = document.getElementById("nombre").value;
datosPaciente.rut = document.getElementById("rut").value;
datosPaciente.fecha_nacimiento = document.getElementById("fecha_nacimiento").value;
datosPaciente.edad = document.getElementById("edad").value;
datosPaciente.hipotesis_diagnóstica = document.getElementById("hipotesis_diagnostica").value;
datosPaciente.sexo = document.getElementById("sexo").value;

// Dynamic HTML content with patient and prescriptor data
const htmlContent = `
    <!DOCTYPE html>
    <html lang="en">
    <head>
        <meta charset="UTF-8">
        <meta name="viewport" content="width=device-width, initial-scale=1.0">
        <title>Prescripción de Exámenes</title>
        <link rel="stylesheet" href="print.css" type="text/css" media="print"> 
        <link rel="stylesheet" href="style.css" type="text/css" media="screen">
    </head>
    <body>
    <div class="page">
    <div class="header">

                    <h1>Solicitud de Exámenes</h1>
                        <div class="px-data">
                            <p><strong>Nombre del Paciente:</strong> ${datosPaciente.nombre}</p>
                            <p><strong>RUT:</strong> ${datosPaciente.rut} &nbsp &nbsp &nbsp &nbsp &nbsp &nbsp &nbsp &nbsp &nbsp &nbsp &nbsp &nbsp &nbsp &nbsp &nbsp &nbsp &nbsp &nbsp &nbsp <strong>Sexo:</strong> ${datosPaciente.sexo}</p> 
                            <p><strong>Fecha de Nacimiento:</strong> ${datosPaciente.fecha_nacimiento} &nbsp &nbsp &nbsp  &nbsp &nbsp &nbsp <strong>Edad:</strong> ${datosPaciente.edad} años</p>
                            <p><strong>Hipótesis Diagnóstica:</strong> ${datosPaciente.hipotesis_diagnóstica}</p>
                        </div>
                    </div>
        <div class="content">
            Exámenes solicitados:<br>
                <ul>
                    ${solicitud.map((exam) => `<li>${exam}</li>`).join("")}
                </ul>
        </div>
        <div class="footer center">
            <p> Dr. ${datosPrescriptor.nombre} - ${datosPrescriptor.profesion}</p>
            <p><strong>Cédula:</strong> ${datosPrescriptor.cedula}   -   <strong>N° Registro:</strong> ${datosPrescriptor.numero_registro}</p>
            <p><strong>Contacto:</strong> ${datosPrescriptor.mail}</p>
            <p><strong>Dirección:</strong> ${datosPrescriptor.direccion}</p>
            <p>Página <span class="pageNumber"></span></p>
        </div>
        </div>
    </body>
    </html>
`;

// Write the HTML content to the new window and print
printWindow.document.open();
printWindow.document.write(htmlContent);
printWindow.document.close();
printWindow.print();
}

</script>

<template>
  <div class="flex">
        <h1>Reclab</h1>

        <div class="container">
            <form id="reclabForm">
                <label for="nombre">Nombre y Apellidos</label>
                <input type="text" name="nombre" id="nombre" required>

                <label for="rut">RUT</label>
                <input type="text" name="rut" id="rut" required>
                
                <label for="fecha">Fecha Nacimiento</label>
                <input type="date" name="fecha" id="fecha_nacimiento" required>


                <label for="edad">Edad</label>
                <input type="number" name="edad" id="edad" >

                <label for="sexo">Sexo</label>
                <select name="sexo" id="sexo" required style="width: 100%;">
                    <option value="" disabled selected>Seleccione una opción</option>
                    <option value="Masculino">Masculino</option>
                    <option value="Femenino">Femenino</option>
                </select>

                <label for="hipotesis_diagnostica">Hipótesis Diagnóstica</label>
                <input type="text" name="hipotesis_diagnostica" id="hipotesis_diagnostica" required>

                <label for="examen">Exámenes</label>
                <div class="examenes">
                    <div class="examen">
                        <select name="examen" class="examen-select" id="input_examen">
                            <option value="" disabled selected>Seleccione exámenes del listado</option>
                            <option value="Perfil hematológico">Perfil hematológico</option>
                            <option value="Hemograma">Hemograma</option>
                            <option value="Recuento de Plaquetas">Recuento de Plaquetas</option>
                            <option value="Recuento de leucocitos">Recuento de leucocitos</option>
                            <option value="Gases venosos">Gases venosos</option>
                            <option value="Gases arteriales">Gases arteriales</option>
                            <option value="Electrolitos plasmáticos">Electrolitos plasmáticos</option>
                            <option value="Calcio total">Calcio Total</option>
                            <option value="Calcio iónico">Calcio iónico</option>
                            <option value="Fosforo">Fosforo</option>
                            <option value="Magnesio">Magnesio</option>
                            <option value="Creatinina">Creatinina</option>
                            <option value="Nitrógeno uréico">Nitrógeno uréico</option>
                            <option value="LDH">LDH</option>
                            <option value="PROTEINA C REACTIVA">PROTEINA C REACTIVA</option>
                            <option value="TIEMPO DE TROMBOPLASTINA">TIEMPO DE TROMBOPLASTINA</option>
                            <option value="TIEMPO DE PROTROMBINA %">TIEMPO DE PROTROMBINA %</option>
                            <option value="INR">INR</option>
                            <option value="Perfil bioquímico">Perfil bioquímico</option>
                            <option value="Perfil hepático">Perfil hepático</option>
                            <option value="Bilirrubina total">Bilirrubina total</option>
                            <option value="Bilirrubina directa">Bilirrubina directa</option>
                            <option value="Proteínas totales">Proteínas totales</option>
                            <option value="Albumina">Albumina</option>
                            <option value="GGT">GGT</option>
                            <option value="Fosfatasa alcalina">Fosfatasa alcalina</option>
                            <option value="GPT/ALT">GPT/ALT</option>
                            <option value="GOT/AST">GOT/AST</option>
                        </select>
                        <input type="text" name="examen_otro" class="examen-otro" placeholder="Ingresa texto">
                        <button type="button" class="add">+</button>
                        <button type="button" class="remove">-</button>
                    </div>
                </div>

                <select name="listado" id="listado_ex" size="10" style="width: 100%; height: 10em;">
                    <!-- List will populate dynamically -->
                </select>
                
                
                <button onclick="printPDF()" type="button">Imprimir 🖨️</button>
            </form>
            <p>Mientras está posicionado en las celdas de selección de exámenes </p>
                <li> Añada a la lista con el botón + o con <b>Enter</b> </li>
                <li> Elimine de la lista el último elemento con el botón - o con <b>ctrl + Del</b></li>
        </div>
    </div>

  <HelloWorld msg="Vite + Vue xd" yo_soy_dani="que tal ah?" />
</template>

<style scoped>
.logo {
  height: 6em;
  padding: 1.5em;
  will-change: filter;
  transition: filter 300ms;
}
.logo:hover {
  filter: drop-shadow(0 0 2em #646cffaa);
}
.logo.vue:hover {
  filter: drop-shadow(0 0 2em #42b883aa);
}

/* copia de styles */

body {
    font-family: Arial, sans-serif;
    background-color: #f4f4f9;
    margin: 0;
    padding: 0;
    color: #333;
}

.container {
    max-width: 1000px;
    margin: 20px auto;
    padding: 20px;
    background: #ffffff;
    box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
    border-radius: 10px;
}
.flex {
    display: flex;
    flex-wrap: wrap;
    justify-content: space-between;
    align-items: top;
}

h1 {
    text-align: left;
    padding: 0.5em;
    color: #6381d2;
    font-size: 2em;
    margin-bottom: 20px;
}

form {
    display: flex;
    flex-direction: column;
    gap: 10px;
}

label {
    font-weight: bold;
    margin-bottom: 5px;
}

input, select, button {
    padding: 10px;
    font-size: 1em;
    border: 1px solid #ddd;
    border-radius: 5px;
    width: 100%;
    box-sizing: border-box;
}

button {
    cursor: pointer;
    background-color: #6381d2;
    color: white;
    border: none;
    transition: background-color 0.3s;
}

button:hover {
    background-color: #abd263;
}

.examen {
    display: flex;
    gap: 10px;
    align-items: center;
}

.examen input[type="text"], .examen select {
    flex: 1;
}

.add, .remove {
    flex: 0 0 auto;
    width: auto;
    padding: 0 15px;
}

#listado_ex {
    border: 1px solid #ddd;
    padding: 5px;
    border-radius: 5px;
    background-color: #fafafa;
}

#listado_ex option {
    padding: 5px;
    font-size: 0.9em;
}


</style>
