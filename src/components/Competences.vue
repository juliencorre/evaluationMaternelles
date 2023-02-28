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
  c: []
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
    "url": "http://localhost:5173/competences.json"
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

onMounted(() => initialisation(),getCompetences() )
</script>


<template>

  <div class="container-fluid">

  <div class="d-flex justify-content-between flex-wrap flex-md-nowrap align-items-center pt-3 pb-2 ">
    <h1 class="h2">Compétences</h1>
    <div class="btn-toolbar mb-2 mb-md-0">

      <button type="button" class="btn btn-sm btn-outline-primary " v-on:click="showAjouterDomaines">
        <span data-feather="calendar" class="align-text-bottom"></span>
        Nouveau domaine
      </button>
    </div>
  </div>

    <ul class="list-group">
      <li class="list-group-item pb-0 border-bottom-0" v-for="domaine in competences.domaines">
        <div class="d-flex justify-content-between align-items-center pb-2">
          <div>
            <i class="bi bi-chevron-down"></i>
            <span class="ms-2">{{ domaine.nom }}</span>
            <span class="ms-2 fst-italic" >{{ domaine.description}}</span>
          </div>
          <div class="" >
            <a type="button" class="btn btn-outline-secondary btn-sm pt-0 pb-0"><i class="bi bi-plus" ></i></a>
            <a type="button" class="btn btn-outline-secondary btn-sm pt-0 pb-0 ms-2"><i class="bi bi-pencil" ></i></a>
            <a type="button" class="btn btn-outline-secondary btn-sm pt-0 pb-0 ms-2" ><i class="bi bi-trash" ></i></a>
          </div>
        </div>
        <ul class="list-group">
          <li class="list-group-item pb-0 border-bottom-0" v-for="champ in domaine.champs">
            <div class="d-flex justify-content-between align-items-center pb-2">
              <div><i class="bi bi-chevron-down"></i><span class="ms-2">{{ champ.nom }}</span>
                <span class="ms-2 fst-italic" >{{ champ.description}}</span>
              </div>
              <div class="" >
                <a type="button" class="btn btn-outline-secondary btn-sm pt-0 pb-0"><i class="bi bi-plus" ></i></a>
                <a type="button" class="btn btn-outline-secondary btn-sm pt-0 pb-0 ms-2"><i class="bi bi-pencil" ></i></a>
                <a type="button" class="btn btn-outline-secondary btn-sm pt-0 pb-0 ms-2" ><i class="bi bi-trash" ></i></a>
              </div>
            </div>
            <ul class="list-group">
              <li class="list-group-item pb-0 border-bottom-0" v-for="competence in champ.competences">
                <div class="d-flex justify-content-between align-items-center pb-2">
                  <div><i class="bi bi-chevron-down"></i><span class="ms-2">{{ competence.nom }}</span>
                    <span class="ms-2 fst-italic" >{{ competence.description}}</span>
                  </div>
                  <div class="" >
                    <a type="button" class="btn btn-outline-secondary btn-sm pt-0 pb-0"><i class="bi bi-plus" ></i></a>
                    <a type="button" class="btn btn-outline-secondary btn-sm pt-0 pb-0 ms-2"><i class="bi bi-pencil" ></i></a>
                    <a type="button" class="btn btn-outline-secondary btn-sm pt-0 pb-0 ms-2" ><i class="bi bi-trash" ></i></a>
                  </div>
                </div>
                <ul class="list-group ps-4">
                  <li class="list-group-item" v-for="competences_spe in competence.competences_spe">
                    <div class="d-flex justify-content-between align-items-center pb-1">
                      <div>
                        <span class="">{{ competences_spe.nom }}</span>
                        <span class="ms-2 fst-italic" >{{ competences_spe.description}}</span>
                      </div>
                      <div class="" >
                        <a type="button" class="btn btn-outline-secondary btn-sm pt-0 pb-0 ms-2"><i class="bi bi-pencil" ></i></a>
                        <a type="button" class="btn btn-outline-secondary btn-sm pt-0 pb-0 ms-2" ><i class="bi bi-trash" ></i></a>
                      </div>
                    </div>
                  </li>
                </ul>
              </li>
              <!--li class="list-group-item pb-0 border-bottom-0">
                <div class="d-flex justify-content-between align-items-center" >
                  <div><i class="bi bi-chevron-right"></i><span class="ms-2">Compétence</span></div>
                  <div class="" >
                    <a type="button" class="btn btn-outline-secondary btn-sm pt-0 pb-0"><i class="bi bi-plus" ></i></a>
                    <a type="button" class="btn btn-outline-secondary btn-sm pt-0 pb-0 ms-2"><i class="bi bi-pencil" ></i></a>
                    <a type="button" class="btn btn-outline-secondary btn-sm pt-0 pb-0 ms-2" ><i class="bi bi-trash" ></i></a>
                  </div>
                </div>
              </li-->
            </ul>
          </li>
        </ul>
      </li>

    </ul>

  <!--ul class="list-group">
    <li class="list-group-item pb-0 border-bottom-0" >
      <div class="d-flex justify-content-between align-items-center pb-2">
        <div><i class="bi bi-chevron-down"></i><span class="ms-2">Domaine</span></div>
        <div class="" >
          <a type="button" class="btn btn-outline-secondary btn-sm pt-0 pb-0"><i class="bi bi-plus" ></i></a>
          <a type="button" class="btn btn-outline-secondary btn-sm pt-0 pb-0 ms-2"><i class="bi bi-pencil" ></i></a>
          <a type="button" class="btn btn-outline-secondary btn-sm pt-0 pb-0 ms-2" ><i class="bi bi-trash" ></i></a>
        </div>
      </div>
      <ul class="list-group">
        <li class="list-group-item pb-0 border-bottom-0">
          <div class="d-flex justify-content-between align-items-center pb-2">
            <div><i class="bi bi-chevron-down"></i><span class="ms-2">Champ</span></div>
            <div class="" >
              <a type="button" class="btn btn-outline-secondary btn-sm pt-0 pb-0"><i class="bi bi-plus" ></i></a>
              <a type="button" class="btn btn-outline-secondary btn-sm pt-0 pb-0 ms-2"><i class="bi bi-pencil" ></i></a>
              <a type="button" class="btn btn-outline-secondary btn-sm pt-0 pb-0 ms-2" ><i class="bi bi-trash" ></i></a>
            </div>
          </div>
          <ul class="list-group">
            <li class="list-group-item pb-0 border-bottom-0">
              <div class="d-flex justify-content-between align-items-center pb-2">
                <div><i class="bi bi-chevron-down"></i><span class="ms-2">Compétence</span></div>
                <div class="" >
                  <a type="button" class="btn btn-outline-secondary btn-sm pt-0 pb-0"><i class="bi bi-plus" ></i></a>
                  <a type="button" class="btn btn-outline-secondary btn-sm pt-0 pb-0 ms-2"><i class="bi bi-pencil" ></i></a>
                  <a type="button" class="btn btn-outline-secondary btn-sm pt-0 pb-0 ms-2" ><i class="bi bi-trash" ></i></a>
                </div>
              </div>
              <ul class="list-group">
                <li class="list-group-item">
                  <div class="d-flex justify-content-between align-items-center pb-1">
                  <div><span class="ms-4">Compétence spé n°1</span></div>
                  <div class="" >
                    <a type="button" class="btn btn-outline-secondary btn-sm pt-0 pb-0 ms-2"><i class="bi bi-pencil" ></i></a>
                    <a type="button" class="btn btn-outline-secondary btn-sm pt-0 pb-0 ms-2" ><i class="bi bi-trash" ></i></a>
                  </div>
                  </div>
                </li>
                <li class="list-group-item">
                  <div class="d-flex justify-content-between align-items-center pb-1">
                    <div><span class="ms-4">Compétence spé n°2</span></div>
                    <div class="" >
                      <a type="button" class="btn btn-outline-secondary btn-sm pt-0 pb-0 ms-2"><i class="bi bi-pencil" ></i></a>
                      <a type="button" class="btn btn-outline-secondary btn-sm pt-0 pb-0 ms-2" ><i class="bi bi-trash" ></i></a>
                    </div>
                  </div>
                </li>
            </ul>
          </li>
            <li class="list-group-item pb-0 border-bottom-0">
              <div class="d-flex justify-content-between align-items-center" >
                <div><i class="bi bi-chevron-right"></i><span class="ms-2">Compétence</span></div>
                <div class="" >
                  <a type="button" class="btn btn-outline-secondary btn-sm pt-0 pb-0"><i class="bi bi-plus" ></i></a>
                  <a type="button" class="btn btn-outline-secondary btn-sm pt-0 pb-0 ms-2"><i class="bi bi-pencil" ></i></a>
                  <a type="button" class="btn btn-outline-secondary btn-sm pt-0 pb-0 ms-2" ><i class="bi bi-trash" ></i></a>
                </div>
              </div>
            </li>
        </ul>
        </li>
      </ul>
    </li>

  </ul-->

  </div>


  <!-- Modal d'ajout d'un eleve -->
  <div class="modal fade" id="newDomaineModal" tabindex="-1" aria-labelledby="exampleModalLabel" aria-hidden="true">
    <div class="modal-dialog">
      <div class="modal-content">
        <div class="modal-header">
          <h1 class="modal-title fs-5" id="exampleModalLabel">Créer un nouveau domaine</h1>
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
          <button type="button" class="btn btn-primary" >Créer</button>
        </div>
      </div>
    </div>
  </div>

</template>

<style scoped>
li {
  border-left-width: 0;
  border-right-width: 0;
  border-top-width: 1;
  border-bottom-width: 1;
  padding-right: 0;
}



</style>
