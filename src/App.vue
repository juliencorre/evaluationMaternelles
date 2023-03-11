<script setup lang="ts">
import ClasseEval from "./components/ClasseEval.vue";
import Resultats from "./components/Resultats.vue";
import Eleves from "./components/Eleves.vue";
import Competences from "./components/Competences.vue";

import { ref, computed } from 'vue'

const routes = {
  '/': ClasseEval,
  '/resultats': Resultats,
  '/eleves': Eleves,
  '/competences': Competences
}


const currentPath = ref(window.location.hash)

window.addEventListener('hashchange', () => {
  currentPath.value = window.location.hash
})

const currentView = computed(() => {
  return routes[currentPath.value.slice(1) || '/'] || NotFound
})

</script>

<style>

  header{

  }
  td,th{
    font-size:small;
  }
</style>

<template>
  <header>
  <nav id="menuPrincipal" class="navbar navbar-expand-lg  navbar-dark bg-dark  ">
  <div  class=" container-fluid">
    <a class="navbar-brand" href="#">Ma classe</a>
    <button class="navbar-toggler" type="button" data-bs-toggle="collapse" data-bs-target="#navbarNavAltMarkup" aria-controls="navbarNavAltMarkup" aria-expanded="false" aria-label="Toggle navigation">
      <span class="navbar-toggler-icon"></span>
    </button>
    <div class="collapse navbar-collapse" id="navbarNavAltMarkup">
      <div class="navbar-nav">
        <a class="nav-link active" aria-current="page" href="#/">Evaluations</a>
        <a class="nav-link"  href="#/resultats">Résultats</a>
        <a class="nav-link" href="#/eleves">Eleves</a>
        <a class="nav-link" href="#/competences">Compétences</a>
      </div>
    </div>
  </div>
</nav>
</header>

<div id="contenuPrincipal" >

    <component :is="currentView" />


</div>

</template>

<style>
    #menuPrincipal {
        position: absolute;
        top: 0rem;
        left: 0rem;
        right: 0rem;
        height: 5rem;
    }

    #contenuPrincipal {
        position: absolute;
        top: 5rem;
        left: 0rem;
        right: 0rem;
        bottom: 0rem;
    }
  
</style>