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
  <nav class="navbar navbar-expand-lg  navbar-dark bg-dark">
  <div class=" container-fluid">
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

<div class="main">

    <!--ClasseEval /-->
    <!--Resultats :height="50" /-->

    <component :is="currentView" />


</div>

<!--div class="row">
  <header>
    <img
      alt="Vue logo"
      class="logo"
      src="./assets/logo.svg"
      width="125"
      height="125"
    />

    <div class="wrapper">
      <HelloWorld msg="You did it!" />
    </div>
  </header>



  <main>
    <TheWelcome />
  </main>

</div-->
</template>

<style>
 
</style>