<template>
  <div class="container" style="max-width: 800px">
    <h1 class="text-center mt-5 titulo">ToDo Coppel</h1>

    <div class="d-flex mt-5">
      <input
        type="text"
        v-model="tarea"
        placeholder="Ingresa una Tarea"
        class="w-100 form-control"
      />
      <button class="btn btn-primary rounded-2" @click="Agregar">
        Agregar
      </button>
    </div>

    <table class="table table-hover mt-5 table-warning">
      <thead>
        <tr>
          <th scope="col" style="width: 120px">Tarea</th>
          <th scope="col" style="width: 100px">Fecha</th>
          <th scope="col" style="width: 100px" class="text-center">
            Prioridad
          </th>
          <th scope="col" style="width: 100px">Estado</th>
          <th scope="col" style="width: 120px">Acciones</th>
        </tr>
      </thead>
      <tbody>
        <tr v-for="(tarea, i) in tareas" :key="i">
          <td>
            <span :class="{ linea: tarea.estado === 'Terminada' }">
              {{ tarea.datos }}
            </span>
          </td>
          <td>
            <div>
              <p>{{ regresafecha() }}</p>
            </div>
          </td>
          <td class="text-center">
            <span
              class="cursor seleccion"
              @click="prioridad(i)"
              :class="{
                'text-success': tarea.prioridad === 'Baja',
                'text-warning': tarea.prioridad === 'Media',
                'text-danger': tarea.prioridad === 'Alta',
              }"
            >
              {{ tarea.prioridad }}
            </span>
          </td>

          <td>
            <span
              class="cursor seleccion"
              @click="estado(i)"
              :class="{
                'text-danger': tarea.estado === 'Nueva',
                'text-warning': tarea.estado === 'En proceso',
                'text-success': tarea.estado === 'Terminada',
              }"
            >
              {{ tarea.estado }}
            </span>
          </td>
          <td class="text-center">
            <button
              class="btn btn-outline-success rounded-2"
              @click="editar(i)"
            >
              Editar
            </button>
            |
            <button
              class="btn btn-outline-danger rounded-2"
              @click="eliminar(i)"
            >
              Borrar
            </button>
          </td>
        </tr>
      </tbody>
    </table>
  </div>
</template>

<script>
export default {
  name: "ToDoCoppel",
  props: {
    msg: String,
  },
  data() {
    return {
      tarea: "",
      editado: null,
      estados: ["Nueva", "En proceso", "Terminada"],
      prioridades: ["Baja", "Media", "Alta"],

      tareas: [],
    };
  },
  methods: {
    Agregar() {
      if (this.tarea.length === 0) return;
      if (this.editado != null) {
        this.tareas[this.editado].datos = this.tarea;
        this.editado = null;
      } else {
        this.tareas.push({
          datos: this.tarea,
          estado: "Nueva",
          prioridad: "Baja",
        });
      }
      this.tarea = "";
    },

    editar(index) {
      this.tarea = this.tareas[index].datos;
      this.editado = index;
    },

    eliminar(index) {
      this.tareas.splice(index, 1);
    },

    estado(index) {
      let nindex = this.estados.indexOf(this.tareas[index].estado);
      if (++nindex > 2) nindex = 0;
      this.tareas[index].estado = this.estados[nindex];
    },

    prioridad(index) {
      let nindex = this.prioridades.indexOf(this.tareas[index].prioridad);
      if (++nindex > 2) nindex = 0;
      this.tareas[index].prioridad = this.prioridades[nindex];
    },

    regresafecha() {
      return new Date().toLocaleDateString();
    },
  },
};
</script>

<style scoped>
.titulo {
  color: #59b5fe;
  -webkit-text-stroke: 1px #fcff3d;
  font-size: xx-large;
}

.linea {
  text-decoration: line-through;
}
.seleccion {
  user-select: none;
}
.cursor {
  cursor: pointer;
}
</style>
