<script setup lang="ts">
import axios from 'axios';
import {
  onMounted,
  reactive
} from 'vue';

defineProps<{
  //msg: string;
}>();

const competences = reactive({
  domaines : null
});

const competence = reactive({
    competence: null
});

const variables = reactive({
  myModal: null,
  modalToggle: null,
});

const newDomaine = reactive({
  nom: null,
  description: null,
});

function initialisation() {

  variables.myModal = new bootstrap.Modal('#newDomaineModal', {
    keyboard: false
  });

  variables.modalToggle = document.getElementById('newDomaineModal');


};

/**
 * Récupérer les compétences
 */
function getCompetences() {

  axios({
    method: "GET",
    "url": "/competences.json"
    //"url": "http://localhost:5173/competences.json"
    //"url": "http://127.0.0.1:8000/api/eleves"

  }).then(result => {

    console.log("Retour du service, :result.data: " + result.data);
    competences.domaines = result.data.domaines;

  }, error => {
    console.error(error);
  });

};

/**
 * Afficher la fenetre modale d'ajout d'un domaine
 */
function showAjouterDomaines() {
  console.log("showAjouterDomaine");
  variables.myModal.show(variables.modalToggle);

  /*
    editEleve.id = eleve.id;
    editEleve.prenom = eleve.prenom;
    editEleve.nom = eleve.nom;
    editEleve.date_naissance = eleve.date_naissance;
    variables.editerEleveModal.show(variables.editerEleveModalToggle);
    */

    };

    function showCompetence(id) {
        console.log("showCompetence: " + id);

        axios({
            method: "GET",
            "url": "/competences_"+id+".json"
        }).then(result => {

            console.log("Retour du service, :result.data: " + result.data);
            competence.competence = result.data;

        }, error => {
            console.error(error);
        });

    }

onMounted(() => initialisation(), getCompetences())
</script>


<template>
  <div class="competence-page container-fluid">


    <div class="d-flex justify-content-between flex-wrap flex-md-nowrap align-items-center pt-3  ">
      <h1 class="h2">Compétences</h1>
      <div class="btn-toolbar mb-2 mb-md-0">

        <button type="button" class="btn btn-sm btn-outline-primary " v-on:click="showAjouterDomaines">
          <span data-feather="calendar" class="align-text-bottom"></span>
          Nouvelle compétence
        </button>
      </div>
    </div>
  <hr>

  <div class="row">
      <div class="col eval-menu">

            <ul class="list-unstyled ps-0">
              <!--Liste des domaines-->
              <li class="mb-0" v-for="domaine in competences.domaines">
                <button class="btn btn-toggle d-inline-flex align-items-center rounded border-0 collapsed"
                  data-bs-toggle="collapse" :data-bs-target="`#domaine-collapse-${domaine.id}`" aria-expanded="true">
                  {{ domaine.nom }}
                </button>
                <div class="collapse lignVerticale" :id="`domaine-collapse-${domaine.id}`">
                  <ul class="btn-toggle-nav list-unstyled fw-normal pb-0 small">
                    <!--Liste des champs-->
                    <li class="mb-0" v-for="champ in domaine.champs">
                      <button class="btn btn-toggle d-inline-flex align-items-center rounded border-0 collapsed"
                        data-bs-toggle="collapse" :data-bs-target="`#champ-collapse-${champ.id}`" aria-expanded="true">
                        {{ champ.nom }}
                      </button>
                      <div class="collapse lignVerticale" :id="`champ-collapse-${champ.id}`">
                        <ul class="btn-toggle-nav list-unstyled fw-normal pb-0 small">
                          <!--Liste des competence-->
                          <li class="mb-0" v-for="competence in champ.competences">
                            <button class="btn btn-toggle d-inline-flex align-items-center rounded border-0 collapsed"
                              data-bs-toggle="collapse" :data-bs-target="`#competence-collapse-${competence.id}`"
                              aria-expanded="true">
                              {{ competence.nom }}
                            </button>
                            <div class="collapse lignVerticale" :id="`competence-collapse-${competence.id}`">
                              <ul class="btn-toggle-nav list-unstyled fw-normal pb-0 small">
                                <!--Liste des competences_spe-->
                                <li v-for="competences_spe in competence.competences_spe">
                                  <button class="btn-selector btn btn-list d-inline-flex align-items-center rounded border-0 collapsed"
                                    aria-expanded="true" v-on:click="showCompetence(competences_spe.id)">
                                    {{ competences_spe.nom }}
                                  </button>
                                </li>
                              </ul>
                            </div>
                          </li>
                        </ul>
                      </div>
                    </li>
                  </ul>
                </div>
              </li>
            </ul>
        
      </div>

      <div v-if="competence.competence" class="col competence-detail">

        <div class="mt-4 d-flex">
              <div class="w-100">
                  <h5 class="card-title">{{ competence.competence.nom }}</h5>
              </div>
              <div class="flex-shrink-1 ">
                <a type="button" class=" btn  p-0 "><i class="fs-4 bi bi-three-dots-vertical"></i></a>
              </div>
            </div>
            

            <h6 class="card-subtitle mb-2 text-muted">Card subtitle</h6>
            <p class="card-text">{{ competence.competence.description }}</p>

            <table class="table">
              <tbody>
                <tr>
                  <td>Type de valeur</td>
                  <td>numérique</td>
                </tr>
                <tr>
                  <td>Valeur minimale</td>
                  <td>0</td>
                </tr>
                <tr>
                  <td>Valeur maximale </td>
                  <td>50</td>
                </tr>
              </tbody>
            </table>
      </div>
    </div>
  </div>
  <!-- Modal d'ajout d'un eleve -->
  <div class="modal fade" id="newDomaineModal" tabindex="-1" aria-labelledby="exampleModalLabel" aria-hidden="true">
    <div class="modal-dialog">
      <div class="modal-content">
        <div class="modal-header">
          <h1 class="modal-title fs-5" id="exampleModalLabel">Créer une nouvelle compétence</h1>
          <button type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Close"></button>
        </div>
        <div class="modal-body">
          <div class="mb-3 row">
            <label for="staticEmail" class="col-sm-2 col-form-label">nom</label>
            <div class="col-sm-10">
              <input type="text" class="form-control" id="prenom" v-model="newDomaine.nom">
            </div>
          </div>
          <div class="mb-3 row">
            <label for="inputPassword" class="col-sm-2 col-form-label">Description</label>
            <div class="col-sm-10">
              <input type="text" class="form-control" id="nom" v-model="newDomaine.description">
            </div>
          </div>
        </div>
        <div class="modal-footer">
          <button type="button" class="btn btn-secondary" data-bs-dismiss="modal">Annuler</button>
          <button type="button" class="btn btn-primary">Créer</button>
        </div>
      </div>
    </div>
  </div>
</template>
<style scoped>
.list-group {
  --bs-list-group-border-radius: 0rem;
}
ul {
  padding-left: 0.5rem;
  padding-top: 0;
  padding-bottom: 0;
}
li {
  padding-right: 0;
  padding-top: 0;
  padding-bottom: 0;
  border-left-width: 1;
  border-right-width: 0;
  border-top-width: 0;
  border-bottom-width: 0;
}
/*
.eval-selector:hover {
  opacity: 0.7;
  background-color: rgb(242, 242, 242);
  cursor: pointer;
}
.eval-selector:hover>.bouton-selection {
  display: block
}
*/
.card {
  border-radius: 1rem;
  background-color: rgb(255, 255, 255);
  margin-top:1rem;
}
.eval-menu{
  margin-top:1rem;
  position: absolute;
  left: 0;
  bottom: 0;
  top: 130px;
  width: 50%;
}
.competence-detail
{
  border-left:solid;
  border-left-width: 0.1rem;
  border-color: #dbdada;
  position: absolute;
  right: 0;
  top: 135px;
  bottom: 0;
  width: 50%;
  background-color: white;
  padding: 1rem 1rem 1rem 1rem;
}
hr{
  margin-bottom: 0;
}
.dropdown-toggle {
  outline: 0;
}
.btn-toggle {
  padding: .25rem .5rem;
  font-weight: 600;
  color: rgba(0, 0, 0, .65);
  background-color: transparent;
  padding: 0;
}
.btn-list {
  padding: 0 1.25rem;
}
 .btn-selector:hover,
 .btn-selector:hover {
        color: rgba(0, 0, 0, .85);
        background-color: #d2f4ea;
    }
.btn-toggle:hover,
.btn-toggle:focus {
  color: rgba(0, 0, 0, .85);
  background-color: #d2f4ea;
}
.btn-toggle::before {
  width: 1.25em;
  line-height: 0;
  content: url("data:image/svg+xml,%3csvg xmlns='http://www.w3.org/2000/svg' width='16' height='16' viewBox='0 0 16 16'%3e%3cpath fill='none' stroke='rgba%280,0,0,.5%29' stroke-linecap='round' stroke-linejoin='round' stroke-width='2' d='M5 14l6-6-6-6'/%3e%3c/svg%3e");
  transition: transform .35s ease;
  transform-origin: .5em 50%;
}
.btn-toggle[aria-expanded="true"] {
  color: rgba(0, 0, 0, .85);
}
.btn-toggle[aria-expanded="true"]::before {
  transform: rotate(90deg);
}
.btn-toggle-nav a {
  padding: .1875rem .5rem;
  margin-top: .125rem;
  margin-left: 1.25rem;
}
.btn-toggle-nav a:hover,
.btn-toggle-nav a:focus {
  background-color: #d2f4ea;
}
.scrollarea {
  overflow-y: auto;
}
    /*
.collapse {
  border: solid;
  border-width: 2;
  margin-left: 0.50rem;
  border-width: 0 0 0 .1rem;
  border-color: #dbdada;
}
    */
    .lignVerticale {
        border: solid;
        border-width: 2;
        margin-left: 0.50rem;
        border-width: 0 0 0 .1rem;
        border-color: #dbdada;
    }
</style>
