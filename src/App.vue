<script>
export default {
  data() {
    return {
      titulo: "GYM APP CON VUE",
      ejercicios: [],
      nuevoEjercicio: "",
      seriesPorEjercicio: 1,
      peso: 0,
      repeticiones: 0
    };
  },
  created() {
    let datosLocalStorage = JSON.parse(localStorage.getItem("gym-vue"));
    this.ejercicios = datosLocalStorage || [];
  },
  methods: {
    añadirEjercicioSelect() {
      if (this.nuevoEjercicio === "Elige un ejercicio" || this.nuevoEjercicio === "") return;
      const series = Array.from({ length: this.seriesPorEjercicio }, () => ({
        peso: this.peso,
        repeticiones: this.repeticiones,
        completada: false
      }));
      this.ejercicios.push({
        nombre: this.nuevoEjercicio,
        series: series
      });
      this.resetForm();
      localStorage.setItem("gym-vue", JSON.stringify(this.ejercicios));
    },
    añadirEjercicioInput() {
      if (this.nuevoEjercicio === "") return;
      const series = Array.from({ length: this.seriesPorEjercicio }, () => ({
        peso: this.peso,
        repeticiones: this.repeticiones,
        completada: false
      }));
      this.ejercicios.push({
        nombre: this.nuevoEjercicio,
        series: series
      });
      this.resetForm();
      localStorage.setItem("gym-vue", JSON.stringify(this.ejercicios));
    },
    resetForm() {
      this.nuevoEjercicio = "";
      this.seriesPorEjercicio = 1;
      this.peso = 0;
      this.repeticiones = 0;
    },
    cambiarEstadoSerie(ejercicioIndex, serieIndex) {
      const serie = this.ejercicios[ejercicioIndex].series[serieIndex];
      serie.completada = !serie.completada;
      localStorage.setItem("gym-vue", JSON.stringify(this.ejercicios));
    },
    esEjercicioCompleto(ejercicio) {
      return ejercicio.series.every(serie => serie.completada);
    },
    eliminarEjercicio(index) {
      alert("Vas a eliminar este ejercicio");
      this.ejercicios.splice(index, 1);
      localStorage.setItem("gym-vue", JSON.stringify(this.ejercicios));
    },
    eliminarTodo() {
      alert("Vas a eliminar toda la lista");
      this.ejercicios = [];
      localStorage.setItem("gym-vue", JSON.stringify(this.ejercicios));
    }
  }
};
</script>

<template>
  <main>
    <h1>{{ titulo }}</h1>

    <select v-model="nuevoEjercicio" class="form-select mb-4">
      <option value="" selected disabled>Elige un ejercicio</option>
      <option value="Sentadillas">Sentadillas</option>
      <option value="Press Banca">Press Banca</option>
      <option value="Press Militar">Press Militar</option>
      <option value="Peso Muerto">Peso Muerto</option>
      <option value="Curl Bíceps">Curl Bíceps</option>
      <option value="Tríceps Polea">Tríceps Polea</option>
    </select>

    <input type="text" class="form-control mb-4" v-model="nuevoEjercicio" placeholder="Escribe un ejercicio...">

    <label>Series</label>
    <input type="number" class="form-control mb-2 w-25" v-model="seriesPorEjercicio" min="1">
    <label>Peso</label>
    <input type="number" class="form-control mb-2 w-25" v-model="peso" min="0">
    <label>Repeticiones</label>
    <input type="number" class="form-control mb-4 w-25" v-model="repeticiones" min="1">

    <div class="d-flex justify-content-between align-items-center">
      <button class="btn btn-primary" @click="añadirEjercicioInput">Añadir</button>
      <button class="btn btn-danger" @click="eliminarTodo">Eliminar todo</button>
    </div>

    <div
      class="mt-4 w-180"
      v-for="(ejercicio, index) in ejercicios"
      :key="index"
      :class="['alert', 'p-2', esEjercicioCompleto(ejercicio) ? 'alert-success' : 'alert-danger']"
    >
      <div class="d-flex justify-content-between align-items-center">
        <div>
          <h4>{{ index + 1 }} - {{ ejercicio.nombre }}</h4>
          <ul>
            <li
              v-for="(serie, serieIndex) in ejercicio.series"
              :key="serieIndex"
              class="d-flex justify-content-between align-items-center"
            >
              -Serie {{ serieIndex + 1 }}: Peso: {{ serie.peso }}kg Repeticiones: {{ serie.repeticiones }}
              <button
                class="btn btn-success btn-sm ms-2"
                @click="cambiarEstadoSerie(index, serieIndex)"
              >
                {{ serie.completada ? "Completada" : "¿Hecho?" }}
              </button>
              <br>
            </li>
          </ul>
        </div>
        <button class="btn btn-danger btn-sm" @click="eliminarEjercicio(index)">X</button>
      </div>
    </div>
  </main>
</template>
