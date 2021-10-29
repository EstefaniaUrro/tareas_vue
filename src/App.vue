<template>
  <div id="app">
    <div>
      <input
        @change="toggleTheme"
        id="checkbox"
        type="checkbox"
        class="switch-checkbox"/>
      <label for="checkbox" class="switch-label">
        <div
          class="switch-toggle"
          :class="{ 'switch-toggle-checked': userTheme === 'dark-theme' }"
        ></div>
      </label>
    </div>
    <main class="container">
      <h1>{{ titulo }}</h1>
        <section id="form">
          <form class="row">
            <div class="mb-3 col-12 col-md-6">
              <label for="nombre">Nueva tarea</label>
              <input type="text" class="form-control" v-model="nombreTarea" required />
              <small class="form-text">Agregar el nombre a la tarea a crear</small>
            </div>
            <div class="mb-3 col-12 col-md-6">
              <label for="descripcion">Descipción</label>
              <input type="text" class="form-control" v-model="descripcionTarea" required />
              <small class="form-text">Agregar la descripción a la tarea a crear</small>
            </div>
            <button @click="agregarTarea" type="button" v-bind:disabled="nombreTarea? false : true" class="btn btn-outline-primary w-100">Submit</button>
          </form> 
        </section>
        <div class="row alert alert-light" role="alert">
          <div class="col-12 col-md-3">
            <span>Nombre</span>
          </div>
          <div class="col-12 col-md-3">
            <span>Descipción</span>
          </div>
          <div class="col-12 col-md-3">
            <span>Estado</span>
          </div>
          <div class="col-12 col-md-3">
            <span>Acciones</span>
          </div>
        </div>
            <GestorTareas 
                v-for="(item,index) in tareas" :key="index"
                v-bind:item="item"
                v-bind:index="index"
                @cambiar="cambiarEstado"
                @eliminar="eliminarTarea"
            />
           
        </main>
    </div>
</template>

<script>
// @ is an alias to /src
import GestorTareas from '@/components/GestorTareas.vue';

export default {
  name: 'App',
  components: {
    GestorTareas
  },
  mounted() {
    const initUserTheme = this.getMediaPreference();
    this.setTheme(initUserTheme);
  },
  data() {
    return {  
        titulo: 'Gestor de Tareas',
        nombreTarea : '',
        descripcionTarea : '',
        tareas : [],
        userTheme: "light-theme",
    }
  },
    methods: {
        agregarTarea(){
            this.tareas.push({
                nombre : this.nombreTarea,
                descripcion : this.descripcionTarea,
                estado : false
            });
            this.nombreTarea = '';
            this.descripcionTarea = '';
            localStorage.setItem('tareasVue', JSON.stringify(this.tareas))
        },
        cambiarEstado(index){
            this.tareas[index].estado = !this.tareas[index].estado
            localStorage.setItem('tareasVue', JSON.stringify(this.tareas))
        },
        eliminarTarea(index){
            this.tareas.splice(index,1)
            localStorage.setItem('tareasVue', JSON.stringify(this.tareas))
        },


        toggleTheme() {
          const activeTheme = localStorage.getItem("user-theme");
          if (activeTheme === "light-theme") {
            this.setTheme("dark-theme");
          } else {
            this.setTheme("light-theme");
          }
        },
        setTheme(theme) {
          localStorage.setItem("user-theme", theme);
          this.userTheme = theme;
          document.documentElement.className = theme;
        },
        getMediaPreference() {
          const hasDarkPreference = window.matchMedia(
            "(prefers-color-scheme: dark)"
          ).matches;
          if (hasDarkPreference) {
            return "dark-theme";
          } else {
            return "light-theme";
          }
        },
    },
    created(){
        let tareasVue = JSON.parse(localStorage.getItem('tareasVue'));
        //console.log(tareasVue)

        if(tareasVue !== null){
            this.tareas = tareasVue;
        }else{
            this.tareas = [];
        }
    
  }
}
</script>
<style>
 @import './assets/style.css';
</style>
