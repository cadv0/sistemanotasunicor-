//App.vue - Camilo Andrés De la cruz 
//Repositorio: https://github.com/cadv0/sistemanotasunicor-.git




//----Codigo----

<script setup>
import { reactive } from "vue";

const alumnos = reactive([])
const asignaturas = reactive([])
const notas = reactive([])

const alumno = reactive({
  cedula : "",
  nombre: "",
  apellido: "",
  edad: 0,
  correo : ""
})

const asignatura = reactive({
  nombre: "",
  codigo: ""
})

const nota = reactive({
  cedula_estudiante: "",
  codigo_asignatura: "",
  corte1: 0,
  corte2: 0,
  corte3: 0,
  id: null
})

// FUNCIONES ALUMNOS
function registraralumno(){
  if(!alumno.cedula || !alumno.nombre || !alumno.apellido){
    alert("Por favor completa los campos requeridos");
    return;
  }
  alumnos.push({...alumno});
  alert("Alumno registrado correctamente");
  limpiar()
  const modal = document.getElementById('modalalumno');
  if(modal) {
    bootstrap.Modal.getInstance(modal)?.hide();
  }
}

// FUNCIONES ASIGNATURAS
function registrasignatura(){
  if(!asignatura.nombre || !asignatura.codigo){
    alert("Por favor completa todos los campos");
    return;
  }
  asignaturas.push({...asignatura});
  alert("Asignatura registrada correctamente");
  limpiar()
  const modal = document.getElementById('modalasignatura');
  if(modal) {
    bootstrap.Modal.getInstance(modal)?.hide();
  }
}

// FUNCIONES NOTAS
function registrarNota(){
  if(!nota.cedula_estudiante || !nota.codigo_asignatura || nota.corte1 === 0 || nota.corte2 === 0 || nota.corte3 === 0){
    alert("Por favor completa todos los campos");
    return;
  }
  
  if(nota.id !== null){
    // Editar nota existente
    const index = notas.findIndex(n => n.id === nota.id);
    if(index !== -1){
      notas[index] = {cedula_estudiante: nota.cedula_estudiante, codigo_asignatura: nota.codigo_asignatura, corte1: nota.corte1, corte2: nota.corte2, corte3: nota.corte3, id: nota.id};
    }
    alert("Nota actualizada correctamente");
  } else {
    // Registrar nueva nota
    notas.push({cedula_estudiante: nota.cedula_estudiante, codigo_asignatura: nota.codigo_asignatura, corte1: nota.corte1, corte2: nota.corte2, corte3: nota.corte3, id: Date.now()});
    alert("Nota registrada correctamente");
  }
  limpiarNota();
  const modal = document.getElementById('modalnota');
  if(modal) {
    bootstrap.Modal.getInstance(modal)?.hide();
  }
}

function limpiarNota(){
  nota.cedula_estudiante = "";
  nota.codigo_asignatura = "";
  nota.corte1 = 0;
  nota.corte2 = 0;
  nota.corte3 = 0;
  nota.id = null;
}

function editarNota(notaItem){
  nota.cedula_estudiante = notaItem.cedula_estudiante;
  nota.codigo_asignatura = notaItem.codigo_asignatura;
  nota.corte1 = notaItem.corte1;
  nota.corte2 = notaItem.corte2;
  nota.corte3 = notaItem.corte3;
  nota.id = notaItem.id;
  const modal = new bootstrap.Modal(document.getElementById('modalnota'));
  modal.show();
}

function eliminarNota(id){
  if(confirm("¿Estás seguro de que quieres eliminar esta nota?")){
    const index = notas.findIndex(n => n.id === id);
    if(index !== -1){
      notas.splice(index, 1);
      alert("Nota eliminada correctamente");
    }
  }
}

function calcularPromedio(corte1, corte2, corte3){
  return ((parseFloat(corte1) + parseFloat(corte2) + parseFloat(corte3)) / 3).toFixed(2);
}

function limpiar(){
  alumno.cedula = "";
  alumno.nombre = "";
  alumno.apellido = "";
  alumno.edad = 0;
  alumno.correo = "";
  asignatura.nombre = "";
  asignatura.codigo = "";
  limpiarNota();
}

</script>

<template>
  <div>
    <nav class="navbar navbar-expand-lg navbar-light bg-light">
  <div class="container-fluid">
    <a class="navbar-brand" href="#">SISTEMA DE CALIFICACIONES</a>
    <button class="navbar-toggler" type="button" data-bs-toggle="collapse" data-bs-target="#navbarSupportedContent" aria-controls="navbarSupportedContent" aria-expanded="false" aria-label="Toggle navigation">
      <span class="navbar-toggler-icon"></span>
    </button>
    <div class="collapse navbar-collapse" id="navbarSupportedContent">
      <ul class="navbar-nav me-auto mb-2 mb-lg-0">
       
        
        <li class="nav-item dropdown">
          <a class="nav-link dropdown-toggle" href="#" role="button" data-bs-toggle="dropdown" aria-expanded="false">
            Alumnos
          </a>
          <ul class="dropdown-menu" aria-labelledby="navbarDropdown">
            <li><a class="dropdown-item" data-bs-toggle="modal" data-bs-target="#modalalumno">Registrar</a></li>
            <li><a class="dropdown-item" data-bs-toggle="modal" data-bs-target="#modallistaralumno">Listar</a></li>
            
          </ul>
        </li>
         <li class="nav-item dropdown">
          <a class="nav-link dropdown-toggle" href="#" role="button" data-bs-toggle="dropdown" aria-expanded="false">
            Asignaturas
          </a>
          <ul class="dropdown-menu" aria-labelledby="navbarDropdown">
            <li><a class="dropdown-item" data-bs-toggle="modal" data-bs-target="#modalasignatura">Registrar</a></li>
            <li><a class="dropdown-item" data-bs-toggle="modal" data-bs-target="#modallistarasignatura">Listar</a></li>
          </ul>
        </li>        <li class="nav-item dropdown">
          <a class="nav-link dropdown-toggle" href="#" role="button" data-bs-toggle="dropdown" aria-expanded="false">
            Notas
          </a>
          <ul class="dropdown-menu" aria-labelledby="navbarDropdown">
            <li><a class="dropdown-item" data-bs-toggle="modal" data-bs-target="#modalnota">Registrar</a></li>
            <li><a class="dropdown-item" data-bs-toggle="modal" data-bs-target="#modallistarnotas">Listar</a></li>
          </ul>
        </li>        
      </ul>
    </div>
  </div>
</nav>

      <!-- Modal Alumno -->
<div class="modal fade" id="modalalumno" tabindex="-1" aria-labelledby="exampleModalLabel" aria-hidden="true">
  <div class="modal-dialog">
    <div class="modal-content">
      <div class="modal-header">
        <h5 class="modal-title" id="exampleModalLabel">Registro de Alumnos</h5>
        <button type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Close"></button>
      </div>
      <div class="modal-body">
          <div class="form-floating mb-3">
            <input type="number" class="form-control" id="floatingInput" placeholder="Cedula" v-model="alumno.cedula">
            <label for="floatingInput">Cedula</label>
          </div>
          <div class="form-floating mb-3">
            <input type="text" class="form-control" id="floatingPassword" placeholder="Nombre" v-model="alumno.nombre">
            <label for="floatingPassword">Nombre</label>
          </div>
          <div class="form-floating mb-3">
            <input type="text" class="form-control" id="floatingInput" placeholder="Apellido" v-model="alumno.apellido">
            <label for="floatingInput">Apellido</label>
          </div>
          <div class="form-floating mb-3">
            <input type="number" class="form-control" id="floatingPassword" placeholder="Edad" v-model="alumno.edad">
            <label for="floatingPassword">Edad</label>
          </div>
          <div class="form-floating mb-3">
            <input type="email" class="form-control" id="floatingInput" placeholder="correo" v-model="alumno.correo">
            <label for="floatingInput">Correo</label>
          </div>
      </div>
      <div class="modal-footer">
        <button type="button" class="btn btn-secondary" data-bs-dismiss="modal">Close</button>
        <button type="button" class="btn btn-primary" @click="registraralumno">Guardar Alumno</button>
      </div>
    </div>
  </div>
</div>

<!-- Modal Asignatura -->
<div class="modal fade" id="modalasignatura" tabindex="-1" aria-labelledby="exampleModalLabel" aria-hidden="true">
  <div class="modal-dialog">
    <div class="modal-content">
      <div class="modal-header">
        <h5 class="modal-title" id="exampleModalLabel">Registro de Asignaturas</h5>
        <button type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Close"></button>
      </div>
      <div class="modal-body">
          <div class="form-floating mb-3">
            <input type="text" class="form-control" id="floatingInput" placeholder="Nombre" v-model="asignatura.nombre">
            <label for="floatingInput">Nombre</label>
          </div>
          <div class="form-floating mb-3">
            <input type="text" class="form-control" id="floatingPassword" placeholder="Código" v-model="asignatura.codigo">
            <label for="floatingPassword">Código</label>
          </div>
      </div>
      <div class="modal-footer">
        <button type="button" class="btn btn-secondary" data-bs-dismiss="modal">Cerrar</button>
        <button type="button" class="btn btn-primary" @click="registrasignatura">Guardar Asignatura</button>
      </div>
    </div>
  </div>
</div>

<!-- Modal Registro de Notas -->
<div class="modal fade" id="modalnota" tabindex="-1" aria-labelledby="exampleModalLabel" aria-hidden="true">
  <div class="modal-dialog">
    <div class="modal-content">
      <div class="modal-header">
        <h5 class="modal-title" id="exampleModalLabel">Registro de Notas</h5>
        <button type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Close"></button>
      </div>
      <div class="modal-body">
          <div class="form-floating mb-3">
            <select class="form-control" id="selectAlumno" v-model="nota.cedula_estudiante">
              <option value="">Selecciona un estudiante</option>
              <option v-for="alumno in alumnos" :key="alumno.cedula" :value="alumno.cedula">
                {{ alumno.cedula }} - {{ alumno.nombre }} {{ alumno.apellido }}
              </option>
            </select>
            <label for="selectAlumno">Estudiante</label>
          </div>
          <div class="form-floating mb-3">
            <select class="form-control" id="selectAsignatura" v-model="nota.codigo_asignatura">
              <option value="">Selecciona una asignatura</option>
              <option v-for="asig in asignaturas" :key="asig.codigo" :value="asig.codigo">
                {{ asig.codigo }} - {{ asig.nombre }}
              </option>
            </select>
            <label for="selectAsignatura">Asignatura</label>
          </div>
          <div class="form-floating mb-3">
            <input type="number" class="form-control" id="corte1" placeholder="Corte 1" v-model.number="nota.corte1" min="1" max="5" step="0.1">
            <label for="corte1">Corte 1 (1-5)</label>
          </div>
          <div class="form-floating mb-3">
            <input type="number" class="form-control" id="corte2" placeholder="Corte 2" v-model.number="nota.corte2" min="1" max="5" step="0.1">
            <label for="corte2">Corte 2 (1-5)</label>
          </div>
          <div class="form-floating mb-3">
            <input type="number" class="form-control" id="corte3" placeholder="Corte 3" v-model.number="nota.corte3" min="1" max="5" step="0.1">
            <label for="corte3">Corte 3 (1-5)</label>
          </div>
      </div>
      <div class="modal-footer">
        <button type="button" class="btn btn-secondary" data-bs-dismiss="modal">Cerrar</button>
        <button type="button" class="btn btn-primary" @click="registrarNota">Guardar Nota</button>
      </div>
    </div>
  </div>
</div>

<!-- Listar Alumnos -->

<div class="modal fade" id="modallistaralumno" tabindex="-1" aria-labelledby="exampleModalLabel" aria-hidden="true">
  <div class="modal-dialog modal-lg">
    <div class="modal-content">
      <div class="modal-header">
        <h5 class="modal-title" id="exampleModalLabel">Listado de Alumnos</h5>
        <button type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Close"></button>
      </div>
      <div class="modal-body">
        <div v-if="alumnos.length === 0" class="alert alert-info">
          No hay alumnos registrados
        </div>
         <table v-else class="table table-hover">
          <thead class="table-light">
             <tr>
               <th>Cédula</th>
               <th>Nombre</th>
               <th>Apellido</th>
               <th>Edad</th>
               <th>Correo</th>
             </tr>
           </thead>
            <tbody>
              <tr v-for="data in alumnos" :key="data.cedula">
               <td>{{ data.cedula }}</td>
               <td>{{ data.nombre }}</td>
               <td>{{ data.apellido }}</td>
               <td>{{ data.edad }}</td>
               <td>{{ data.correo }}</td>
              </tr>
            </tbody>
        </table>
      </div>
      <div class="modal-footer">
        <button type="button" class="btn btn-secondary" data-bs-dismiss="modal">Cerrar</button>
      </div>
    </div>
  </div>
</div>

<!-- Listar Asignaturas -->

<div class="modal fade" id="modallistarasignatura" tabindex="-1" aria-labelledby="exampleModalLabel" aria-hidden="true">
  <div class="modal-dialog modal-lg">
    <div class="modal-content">
      <div class="modal-header">
        <h5 class="modal-title" id="exampleModalLabel">Listado de Asignaturas</h5>
        <button type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Close"></button>
      </div>
      <div class="modal-body">
        <div v-if="asignaturas.length === 0" class="alert alert-info">
          No hay asignaturas registradas
        </div>
         <table v-else class="table table-hover">
          <thead class="table-light">
                <tr>
                  <th>Nombre</th>
                  <th>Código</th>
                </tr>
           </thead>
            <tbody>
              <tr v-for="data in asignaturas" :key="data.codigo">
               <td>{{ data.nombre }}</td>
               <td>{{ data.codigo }}</td>
              </tr>
            </tbody>
        </table>
      </div>
      <div class="modal-footer">
        <button type="button" class="btn btn-secondary" data-bs-dismiss="modal">Cerrar</button>
      </div>
    </div>
  </div>
</div>

<!-- Listar Notas -->
<div class="modal fade" id="modallistarnotas" tabindex="-1" aria-labelledby="exampleModalLabel" aria-hidden="true">
  <div class="modal-dialog modal-xl">
    <div class="modal-content">
      <div class="modal-header">
        <h5 class="modal-title" id="exampleModalLabel">Listado de Notas</h5>
        <button type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Close"></button>
      </div>
      <div class="modal-body">
        <div v-if="notas.length === 0" class="alert alert-info">
          No hay notas registradas
        </div>
         <table v-else class="table table-hover">
          <thead class="table-light">
                <tr>
                  <th>Cédula Estudiante</th>
                  <th>Código Asignatura</th>
                  <th>Corte 1</th>
                  <th>Corte 2</th>
                  <th>Corte 3</th>
                  <th>Promedio</th>
                  <th>Acciones</th>
                </tr>
           </thead>
            <tbody>
              <tr v-for="notaItem in notas" :key="notaItem.id">
               <td>{{ notaItem.cedula_estudiante }}</td>
               <td>{{ notaItem.codigo_asignatura }}</td>
               <td>{{ notaItem.corte1 }}</td>
               <td>{{ notaItem.corte2 }}</td>
               <td>{{ notaItem.corte3 }}</td>
               <td><strong>{{ calcularPromedio(notaItem.corte1, notaItem.corte2, notaItem.corte3) }}</strong></td>
               <td>
                 <button class="btn btn-sm btn-warning" @click="editarNota(notaItem)">Editar</button>
                 <button class="btn btn-sm btn-danger" @click="eliminarNota(notaItem.id)">Eliminar</button>
               </td>
              </tr>
            </tbody>
        </table>
      </div>
      <div class="modal-footer">
        <button type="button" class="btn btn-secondary" data-bs-dismiss="modal">Cerrar</button>
      </div>
    </div>
  </div>
</div>
</div>
</template>

<style scoped></style>