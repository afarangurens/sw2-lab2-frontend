<template>
  <div class="container-fluid">
    <div class="row">
      <div class="col-12 col-sm-10 col-md-8 offset-sm-1 offset-md-2">
        <div class="mt-5"> 
          <div class="border border-primary rounded form-inline">
            <h2 class="col-12 text-center text-primary mt-3 mb-5">
              Laboratorio 2
            </h2>
            <router-link
              class="btn btn-primary m-2"
              to="/mis-cursos"
            >
              Mis cursos
            </router-link>

            <router-link
              v-if = "isTeacher"
              class="btn btn-primary m-2"
              :to = "{ name: 'create-course', params: {isTeacher:isTeacher} }"
            >
              Nuevo curso
            </router-link>

            <router-link
            class="btn btn-primary m-2"
            to="/principal/roles"
            >
              Roles
            </router-link>
            
            <router-link
            class="btn btn-primary m-2"
            to="/principal/nuevo-rol"
            >
              Nuevo Rol
            </router-link>
            
            <router-view/>
          </div>
        </div>
      </div>
    </div>
    <div class="row">
              <h4 class="col-12 text-center text-primary mt-3 mb-5" 
                v-if="isTeacher">Soy profesor
              </h4>
            </div>
  </div>
</template>

<script>
import { getAuthenticationToken } from "@/dataStorage";
import axios from "axios";

export default {
  name: "Home",

  data: function() {
    return {
      isTeacher: false,
    };
  },

  beforeCreate() {
    if (!getAuthenticationToken()) {
      this.$router.push({ name: "login" });
    }

    axios
      .get(this.$store.state.backURL + "/mis-roles", {
        params: { access_token: getAuthenticationToken() },
      })
      .then((response) => {
        if (response.status !== 200) {
          alert("Error Obteniendo sus roles");
        } else {
          for (let r of response.data) {
            if (r.id === 2) {
              this.isTeacher = true;
              break;
            }
          }
        }
      })
      .catch((error) => {
        alert("Error en la petición");
        console.log(error);
      });
  },
};
</script>

<style></style>
