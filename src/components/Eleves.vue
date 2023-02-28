<script setup lang="ts">
import axios from 'axios';
import {
  onMounted,
  reactive
} from 'vue';

defineProps < {
  //msg: string;
} > ();

const newEleve = reactive({
  prenom: null,
  nom: null
});

const editEleve = reactive({
  id: null,
  prenom: null,
  nom: null
});

const eleves = reactive({
  eleves: {}
});

const variables = reactive({
  addProgress: 0,
  myModal: null,
  modalToggle: null,
  editerEleveModal: null,
  editerEleveModalToggle: null,
});


// myModalEl.addEventListener('hidden.bs.modal', event => {
//   console.log("ok")
// });

function initialisation() {

  variables.myModal = new bootstrap.Modal('#v', {
    keyboard: false
  });

  variables.modalToggle = document.getElementById('exampleModal');
  // modalToggle.addEventListener('hidden.bs.modal', event => {
  //   console.log("ok")
  // });

  variables.editerEleveModal = new bootstrap.Modal('#editerEleveModal', {
    keyboard: false
  });

  variables.editerEleveModalToggle = document.getElementById('editerEleveModal');

};

function modifierEleve() {
  console.log("modifierEleve");
  modifyEleve();
};

function showAjouterEleve() {
  console.log("showModal");
  variables.myModal.show(variables.modalToggle);
};


function showEditerEleve(eleve) {
  console.log("editerEleve");
  //newEleve.prenom=eleve.prenom;
  //newEleve.nom=eleve.nom;
  editEleve.id = eleve.id;
  editEleve.prenom = eleve.prenom;
  editEleve.nom = eleve.nom;
  editEleve.date_naissance = eleve.date_naissance;
  variables.editerEleveModal.show(variables.editerEleveModalToggle);
};

function getEleves() {

  axios({
    method: "GET",
    //"url": "http://localhost:5173/eval1.json"
    //"url": "https://virtserver.swaggerhub.com/corre/evaluation/1.0.0/eleves"
    "url": "http://127.0.0.1:8000/api/eleves"

  }).then(result => {

    console.log("Retour du service, :result.data: " + result.data);
    eleves.eleves = result.data;

  }, error => {
    console.error(error);
  });

};

function incrementerProgressBar() {
  console.log(variables.addProgress);
  variables.addProgress = variables.addProgress + 10;

}

function addEleve() {
  console.log("addEleve");
  //const progressBar = document.getElementById('addProgressBar');
  // variables.addProgress=0;
  // var progression = setInterval(function()
  //   {
  //     if(variables.addProgress < 100)
  //     {
  //       variables.addProgress += 100;
  //       console.log(variables.addProgress );
  //     }
  //     else
  //     {
  //       variables.addProgress = 0;
  //     }
  //
  //   }, 1000);

  const eleve = {
    prenom: newEleve.prenom,
    nom: newEleve.nom,
    date_naissance: newEleve.date_naissance
  };

  axios({
    method: "POST",
    data: eleve,
    //"url": "http://localhost:5173/eval1.json"
    //"url": "https://virtserver.swaggerhub.com/corre/evaluation/1.0.0/eleves"
    "url": "http://localhost:8000/api/eleves"

  }).then(result => {
    //clearInterval(progression);
    //variables.addProgress=100;

    console.log("Retour du service, :result.data: " + result.data);
    variables.myModal.hide(variables.modalToggle);
    //variables.addProgress=0;
    getEleves();

    //eleves.eleves.push(result.data);


  }, error => {
    console.error(error);
  });

};

function deleteEleve(eleve) {

  axios({
    method: "DELETE",
    //"url": "http://localhost:5173/eval1.json"
    //"url": "https://virtserver.swaggerhub.com/corre/evaluation/1.0.0/eleves/" + eleve.id
    "url": "http://127.0.0.1:8000/api/eleves/"+ eleve.id

  }).then(result => {

    console.log("Retour du service, :result.data: " + result.data);
    getEleves();

    // for (let index = 0; index < eleves.eleves.length; ++index)
    // {
    //   const element = eleves.eleves[index];
    //   console.log(eleve.id+" / "+element.id)
    //   if(eleve.id == element.id)
    //   {
    //     console.log(eleve.id);
    //     delete eleves.eleves[index];
    //   }
    // }
    //eleves.eleves.push(result.data);
    //this.getEleves();
    getEleves();

  }, error => {
    console.error(error);
  });

};
function localizeDate(date) {
    //if (!date || !date.includes('-')) return date
    const [yyyy, mm, dd] = date.split('-')
    return (new Date(`${mm}/${dd}/${yyyy}`).toLocaleDateString("fr-FR"))
  };

function modifyEleve() {

  const eleve = {
    id: editEleve.id,
    prenom: editEleve.prenom,
    nom: editEleve.nom,
    date_naissance: editEleve.date_naissance
  };

  axios({
    method: "PUT",
    data: eleve,
    //"url": "http://localhost:5173/eval1.json"
    //"url": "https://virtserver.swaggerhub.com/corre/evaluation/1.0.0/eleves/" + eleve.id
    "url": "http://127.0.0.1:8000/api/eleves/"+ eleve.id

  }).then(result => {

    console.log("Retour du service, :result.data: " + result.data);
    variables.editerEleveModal.hide(variables.editerEleveModalToggle);
    //  getEleves();

    for (let index = 0; index < eleves.eleves.length; ++index) {
      const element = eleves.eleves[index];
      console.log(result.data.id + " / " + element.id)
      if (result.data.id == element.id) {
        console.log(element);
        eleves.eleves[index] = result.data
      }
    }
    //eleves.eleves.push(result.data);
    //this.getEleves();
    getEleves();

  }, error => {
    console.error(error);
  });

};

function majEleve(item, index, arr) {
  console.log(item);
}

onMounted(() => initialisation(), getEleves())
</script>


<template>
<div class="d-flex justify-content-between flex-wrap flex-md-nowrap align-items-center pt-3 pb-2 mb-0 border-bottom">
  <h1 class="h2">Eleves</h1>
  <div class="btn-toolbar mb-2 mb-md-0">

    <button type="button" class="btn btn-sm btn-outline-primary " v-on:click="showAjouterEleve">
      <span data-feather="calendar" class="align-text-bottom"></span>
      Nouvel élève
    </button>
  </div>
</div>

<ul class="list-group list-group-flush" style="font-size:small">
  <li class="list-group-item" style="font-weight: bold;background-color: whitesmoke">
    <div class="d-flex w-100 ">
      <div class="col" style="">N°</div>
      <div class="col">Prénom</div>
      <div class="col">Nom</div>
      <div class="col">Date de naissance</div>
      <!--div class="col " style="width:100px">Status</div-->
      <div class="" style="width:100px"></div>
    </div>
  </li>
  <li class="list-group-item" v-for="eleve in eleves.eleves">
    <div class="d-flex w-100">
      <div class="col" style="">{{ eleve.id }}</div>
      <div class="col">{{ eleve.prenom }}</div>
      <div class="col ">{{ eleve.nom }}</div>
      <div class="col ">{{ localizeDate(eleve.date_naissance)}}</div>
      <!--div class="col " style="width:100px"><span class="badge text-bg-success">Nouveau</span></div-->
      <div class="" style="width:100px">
        <button class="btn btn-outline-secondary btn-sm pt-0 pb-0" v-on:click="showEditerEleve(eleve)"><i class="bi bi-pencil" ></i></button>
        <button class="btn btn-outline-secondary btn-sm pt-0 pb-0 ms-2" v-on:click="deleteEleve(eleve)"><i class="bi bi-trash" ></i></button>
      </div>
    </div>
  </li>

</ul>

<!-- Modal d'ajout d'un eleve -->
<div class="modal fade" id="exampleModal" tabindex="-1" aria-labelledby="exampleModalLabel" aria-hidden="true">
  <div class="modal-dialog">
    <div class="modal-content">
      <div class="progress" style="height: 4px;">
        <div id="addProgressBar" class="progress-bar " role="progressbar" aria-label="Example 20px high" :style="{ width: variables.addProgress+ '%' }" aria-valuenow="25" aria-valuemin="0" aria-valuemax="100"></div>
      </div>
      <div class="modal-header">
        <h1 class="modal-title fs-5" id="exampleModalLabel">Ajout d'un nouvel élève</h1>
        <button type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Close"></button>
      </div>
      <div class="modal-body">
        <div class="mb-3 row">
          <label for="staticEmail" class="col-sm-2 col-form-label">Prénom</label>
          <div class="col-sm-10">
            <input type="text" class="form-control" id="prenom" v-model="newEleve.prenom">
          </div>
        </div>
        <div class="mb-3 row">
          <label for="inputPassword" class="col-sm-2 col-form-label">Nom</label>
          <div class="col-sm-10">
            <input type="text" class="form-control" id="nom" v-model="newEleve.nom">
          </div>
        </div>
        <div class="mb-3 row">
          <label for="inputPassword" class="col-sm-2 col-form-label">Date de naissance</label>
          <div class="col-sm-10">
            <input type="date" class="form-control" id="date_naissance" v-model="newEleve.date_naissance">
          </div>
        </div>
      </div>
      <div class="modal-footer">
        <button type="button" class="btn btn-secondary" data-bs-dismiss="modal">Annuler</button>
        <button type="button" class="btn btn-primary" v-on:click="addEleve">Enregistrer</button>
      </div>
    </div>
  </div>
</div>

<!-- Modal d'ajout d'un eleve -->
<div class="modal fade" id="editerEleveModal" tabindex="-1" aria-labelledby="exampleModalLabel" aria-hidden="true">
  <div class="modal-dialog">
    <div class="modal-content">
      <div class="modal-header">
        <h1 class="modal-title fs-5" id="exampleModalLabel">Modifier un élève</h1>
        <button type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Close"></button>
      </div>
      <div class="modal-body">
        <div class="mb-3 row">
          <label for="staticEmail" class="col-sm-2 col-form-label">Prénom</label>
          <div class="col-sm-10">
            <input type="text" class="form-control" id="prenom" v-model="editEleve.prenom">
          </div>
        </div>
        <div class="mb-3 row">
          <label for="inputPassword" class="col-sm-2 col-form-label">Nom</label>
          <div class="col-sm-10">
            <input type="text" class="form-control" id="nom" v-model="editEleve.nom">
          </div>
        </div>
        <div class="mb-3 row">
          <label for="date_naissance" class="col-sm-2 col-form-label">Date de naissance</label>
          <div class="col-sm-10">
            <input type="date" class="form-control" id="date_naissance" v-model="editEleve.date_naissance">
          </div>
        </div>
      </div>
      <div class="modal-footer">
        <button type="button" class="btn btn-secondary" data-bs-dismiss="modal">Annuler</button>
        <button type="button" class="btn btn-primary" v-on:click="modifierEleve">Modifier</button>
      </div>
    </div>
  </div>
</div>

<!--button type="button" class="btn btn-sm btn-primary ">
  <span data-feather="calendar" class="align-text-bottom"></span>
  Enregistrer
</button>
<button type="button" class="btn btn-sm btn-outline-primary ms-2">
  <span data-feather="calendar" class="align-text-bottom"></span>
  Annuler
</button-->
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
