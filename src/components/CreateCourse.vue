<template>
  <div class="col-12 col-sm-10 col-md-8 offset-sm-1 offset-md-2">
    <div class="mt-5">
      <form class="border border-primary rounded form-inline" @submit="newCourse">

        <h2 class="col-12 text-center text-primary mt-3 mb-5">Cree un nuevo curso</h2>

        <div class="form-group col-12">
            <label for="">Nombre del curso</label>
            <input type="text" v-model="course.courseName" />
        </div>

        <div class="form-group col-12">
            <label for="">Duracion en horas</label>
            <input type="number" v-model="course.durationHours" />
        </div>
        <div class="col-12 mb-3">
          <button class="col-sm-6 col-md-4 offset-sm-5 offset-md-7 btn btn-primary" type="submit">
            Crear Curso
          </button>
        </div>

      </form>
    </div>
  </div>
</template>

<script>
import axios from "axios";
import { getAuthenticationToken } from "@/dataStorage";

export default {
  name: "AddCourse",
  props: {
    isTeacher : Boolean,
    
  },
  data() {
    return {
      course: {
        courseName: "",
        durationHours: null,
      },
    };
  },
  created() {
    console.log(this.isTeacher)
    if (!(this.isTeacher || false)) {
      this.$router.push({name: "home"});
    }
  },
  methods: {
    newCourse(event) {
      axios
        .post(
          this.buildURI(),
          this.course,
          {
            params: {
              access_token: getAuthenticationToken(),
            },
          }
        )
        .then((response) => {
          if (response.status !== 201) {
            alert("Error en la petición. Intente nuevamente");
          } else {
            alert("Se ha asignado exitosamente el nuevo curso");
          }
        })
        .catch((response) => {
          if (response.response.status === 401) {
            alert(
              "¡Ups! Al parecer tu contraseña es incorrecta o la sesión ha finalizado"
            );
          } else if (response.response.status === 400) {
            alert("Error");
          } else {
            alert("No es posible conectar con el backend en este momento");
          }
        });
      event.preventDefault();
    },
    buildURI() {
      let creationPath = "/profesor/crear-curso";
      return this.$store.state.backURL + creationPath;
    },
  },
};
</script>

<style scoped>
.form-inline .form-group {
  margin-bottom: 1rem;
}
</style>