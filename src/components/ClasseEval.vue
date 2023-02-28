<script setup lang="ts">
import axios from 'axios';
import {
  onMounted,
  reactive
} from 'vue';

defineProps < {
  //msg: string;
} > ();

const evaluation = reactive({
  evaluations: {}
});




const entetes = reactive({
  entetes: {}
})


function getLocationDetails() {

  axios({
    method: "GET",
    //"url": "http://localhost:5173/eval1.json"
    "url": "https://virtserver.swaggerhub.com/corre/evaluation/1.0.0/evaluations/123"

  }).then(result => {

    console.log("Retour du service, :result.data.evaluations: "+result.data.evaluations);
    evaluation.evaluations = result.data.evaluations;
    console.log("Retour du service, :result.data.entetes: "+result.data.entetes);
    entetes.entetes = result.data.entetes;

  }, error => {
    console.error(error);
  });

};

onMounted(() => getLocationDetails())
</script>


<template>
<div class="d-flex justify-content-between flex-wrap flex-md-nowrap align-items-center pt-3 pb-2 mb-3 border-bottom">
  <h1 class="h2">Evaluation n°1</h1>
  <div class="btn-toolbar mb-2 mb-md-0">
    <!--div class="btn-group me-2">
      <button type="button" class="btn btn-sm btn-outline-secondary">Share</button>
      <button type="button" class="btn btn-sm btn-outline-secondary">Export</button>
    </div-->
    <!--button type="button" class="btn btn-sm btn-outline-secondary dropdown-toggle">
      <span data-feather="calendar" class="align-text-bottom"></span>
      This week
    </button-->
    <button type="button" class="btn btn-sm btn-outline-primary " disabled>
      <span data-feather="calendar" class="align-text-bottom"></span>
      Nouvelle évaluation
    </button>
  </div>
</div>

<table class="table table-bordered table-sm table-striped mt-4">
  <thead>
    <tr>
      <th scope="col" style="background-color: coral;" class="text-center" v-for="evaluation in entetes.entetes.evaluation">{{ evaluation }}</th>
      <th scope="col" style="background-color: azure;" class="text-center" v-for="eleve in entetes.entetes.eleves">{{ eleve }}</th>
    </tr>
  </thead>
  <tbody>
    <!--tr v-for="ligne in evaluation.evaluations">
                <td>{{ ligne.domaine }}</td>
                <td>{{ ligne.champ }}</td>
                <td>{{ ligne.compétence }}</td>
                <td>{{ ligne.compétence_spé }}</td>
                <td class="text-center">
                  <select class="form-select form-select-sm text-center" aria-label=".form-select-sm ">
                    <option selected>?</option>
                    <option value="1">Oui</option>
                    <option value="2">Non</option>
                  </select>
                </td>
                <td class="text-center">
                  <select class="form-select form-select-sm text-center" aria-label=".form-select-sm ">
                    <option selected>?</option>
                    <option value="1">Oui</option>
                    <option value="2">Non</option>
                  </select>
                </td>
              </tr-->
    <tr v-for="ligne in evaluation.evaluations">
      <td>{{ ligne.domaine }}</td>
      <td>{{ ligne.champ }}</td>
      <td>{{ ligne.compétence }}</td>
      <td>{{ ligne.compétence_spé }}</td>
      <td class="text-center" v-for="eleve in ligne.eleves">
        <div>
          <input class="input-control" v-model="eleve.resultat">
        </div>
      </td>
    </tr>
    <!--tr v-for="ligne in evaluation.evaluations">
                <td>test</td>
                <td>test</td>
                <td>test</td>
                <td>test</td>
                <td class="text-center">
                  <select class="form-select form-select-sm text-center" aria-label=".form-select-sm ">
                    <option selected>?</option>
                    <option value="1">Oui</option>
                    <option value="2">Non</option>
                  </select>
                </td>
                <td class="text-center">
                  <select class="form-select form-select-sm text-center" aria-label=".form-select-sm ">
                    <option >?</option>
                    <option selected value="1">Oui</option>
                    <option value="2">Non</option>
                  </select>
                </td>
              </tr-->
  </tbody>
</table>

<button type="button" class="btn btn-sm btn-primary ">
  <span data-feather="calendar" class="align-text-bottom"></span>
  Enregistrer
</button>
<button type="button" class="btn btn-sm btn-outline-primary ms-2">
  <span data-feather="calendar" class="align-text-bottom"></span>
  Annuler
</button>

</template>

<style scoped>
.form-select {
  padding: 0 0 0 0;
  background-image: none;
  width: auto;
  display: initial;
  border: none;
}

.input-control {
  border: none;
  width: 40px;
  text-align: center;
  background: transparent;
}
</style>
