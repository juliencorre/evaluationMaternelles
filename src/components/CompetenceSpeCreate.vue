<script setup lang="ts">

    /* 
        Composant d'affichage des details d'une competence spécifique
    */
    
    import {
        reactive,
        defineEmits
    } from 'vue'

    import axios from 'axios'

    // evenement emis quand le champ est créé
    const emitNewCompetenceSpe = defineEmits(['newCompetenceSpe'])

    // exposition de l'ID du domaine dans lequel on ajoute le champ'
    const myProps = defineProps<{
        id?: string
    }>()

    const myVariables = reactive({
        competenceSpe: null,
        menuSelected : false
    });

    const newCompetenceSpe = reactive({
       // id:1,
        nom: null,
        description: null
    });

    const nomCompetenceSpe = null
    const descriptionCompetenceSpe = null
    /*
    *   Function that display the details of a specific competence
    */
    function addCompetenceSpe() {
  
        //.post("http://localhost:3000/competences/domaines/"+myProps.id+"/champs", newChamp)
        axios
            .post("http://localhost:3000/competences/domaines/1/champs/1/competences/1/competences_spe", newCompetenceSpe)
             .then((result) => {
                    console.log("Retour du service, :result.data: " + result.data)
                    emitNewCompetenceSpe('newCompetenceSpe', result.data)
             })
            
            

    }

    /**
    * Afficher le menu
    */
    function showMenu() {
        console.log("showMenu");
        myVariables.menuSelected=true

    };

    /**
    * Cacher le menu
    */
    function hideMenu() {
        console.log("hideMenu");
        if (myVariables.menuSelected) { 
            myVariables.menuSelected = false
        }
    };

      
  
</script>

<template>


    <div>
        <h3>Création d'une nouvelle compétence spécifique</h3>
        <div class="mb-3 ">
            <label for="staticEmail" class="col-sm-2 form-label">Nom</label>
            <div class="col-sm-10">
                <input v-model="newCompetenceSpe.nom" type="text" class="form-control" id="staticEmail" placeholder="">
            </div>
        </div>
        <div class="mb-3 ">
            <label for="inputPassword" class="col-sm-2 form-label">Description</label>
            <div class="col-sm-10">
                <textarea v-model="newCompetenceSpe.description" type="text" class="form-control" id="inputPassword"></textarea>
            </div>
        </div>

        <button type="button" class=" btn btn-sm btn-primary " v-on:click="addCompetenceSpe">
            <span data-feather="calendar" class="align-text-bottom"></span>
            Créer
        </button>
        <button type="button" class="ms-2 btn btn-sm btn-outline-primary " v-on:click="showAjouterCompetenceSpe">
            <span data-feather="calendar" class="align-text-bottom"></span>
            Annuler
        </button>

    </div>
            


</template>

<style scoped>

    #menu {
        position: absolute;
        top: 1rem;
        right: 1rem;
        border-radius: unset;
        font-size: small;
    }

</style>