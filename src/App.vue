<script>
export default {
  data() {
    return {
      titulo: "Gym app con Vue",
      ejercicios: [],
      nuevoEjercicio: ""
      }
    },
  created () {
      let datosLocalStorage = JSON.parse(localStorage.getItem("gym-vue"))
      if (datosLocalStorage === null) {
        this.ejercicios = []
      } else {
        this.ejercicios = datosLocalStorage
      }
    },
  methods: {
    añadirEjercicioSelect () {
      if (this.nuevoEjercicio === "Elige un ejercicio" || this.nuevoEjercicio === "") {
      return; // No hacer nada si el valor es "Elige un ejercicio"
      }
      this.ejercicios.push({
        nombre: this.nuevoEjercicio,
        estadoBoton: "¿Hecho?" 
      });
      this.nuevoEjercicio = ""
      localStorage.setItem("gym-vue", JSON.stringify(this.ejercicios))
    },
    añadirEjercicioInput () {
      if (this.nuevoEjercicio === "") {
      return; // No hacer nada si el valor es "Elige un ejercicio"
      }
      this.ejercicios.push({
        nombre: this.nuevoEjercicio,
        estadoBoton: "¿Hecho?" 
      });
      this.nuevoEjercicio = ""
      localStorage.setItem("gym-vue", JSON.stringify(this.ejercicios))
    },
    cambiarEstadoTrue (index) {
      let ejercicio = this.ejercicios[index];
      // Cambiar el estado de true a false y viceversa
      ejercicio.estado = !ejercicio.estado;
      // Cambiar el texto del botón
      ejercicio.estadoBoton = ejercicio.estado ? "Hecho" : "¿Hecho?";
      localStorage.setItem("gym-vue", JSON.stringify(this.ejercicios))
    },
    eliminarEjercicio (index) {
      alert("Vas a eliminar este ejercicio")
      this.ejercicios.splice(index, 1)
      localStorage.setItem("gym-vue", JSON.stringify(this.ejercicios))
    },
    eliminarTodo () {
    alert("Vas a eliminar toda la lista")
    this.ejercicios = []
    localStorage.setItem("gym-vue", JSON.stringify(this.ejercicios))
  }
  }
}
</script>

<template>

  <main>
    <h1 class="mt-0"> {{ titulo }}</h1>
    <h2 class="mt-4">Elige un ejercicio de la lista</h2>
    <select  v-model="nuevoEjercicio" @change="añadirEjercicioSelect" v-on:keyup.enter="añadirEjercicio" class="form-select my-3" aria-label="Default select example">
      <option selected>Elige un ejercicio</option>
      <option value="Sentadillas">Sentadillas</option>
      <option value="Press de Banca">Press Banca</option>
      <option value="Press Militar">Press Militar</option>
      <option value="Peso Muerto">Peso Muerto</option>
    </select>
    <h2>Si no está lo puedes escribir aquí</h2>

    <input type="text" class="form-control my-3" v-model="nuevoEjercicio" v-on:keyup.enter="añadirEjercicio">
    <div class="d-flex justify-content-between align-items-center">
      <button class="btn btn-primary" @click="añadirEjercicioInput">Añadir</button>
      <button class="btn btn-danger" @click="eliminarTodo">Eliminar todo</button>
    </div>

    <div class="mt-3" v-for="(ejercicio, index) of ejercicios">

      <div role="alert" :class="['alert', ejercicio.estado ? 'alert-success' : 'alert-danger']">
        <div class="d-flex justify-content-between align-items-center">
          <div>
            <h4>{{ index +1 }} - {{ ejercicio.nombre }}</h4>
          </div>
          <div>
            <button class="btn btn-success btn-sm" @click="cambiarEstadoTrue(index)">{{ ejercicio.estadoBoton }}</button>
            <button class="btn btn-danger btn-sm" @click="eliminarEjercicio(index)">X</button>
          </div>
        </div>
      </div>

    </div>

  </main>
</template>