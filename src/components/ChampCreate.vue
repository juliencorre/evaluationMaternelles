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
    const emitNewChamp = defineEmits(['newChamp'])

    // exposition de l'ID du domaine dans lequel on ajoute le champ'
    const myProps = defineProps<{
        id?: string
    }>()

    const myVariables = reactive({
        competence: null,
        menuSelected : false
    });

    const newChamp = reactive({
       // id:1,
        nom: null,
        description: null
    });

    const nomChamp = null
    const descriptionChamp = null
    /*
    *   Function that display the details of a specific competence
    */
    function addChamp() {
  
        //.post("http://localhost:3000/competences/domaines/"+myProps.id+"/champs", newChamp)
        axios
            .post("http://localhost:3000/competences/domaines/1/champs", newChamp)
             .then((result) => {
                    console.log("Retour du service, :result.data: " + result.data)
                    emitNewChamp('newChamp', result.data)
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
        <h3>Création d'un nouveau champ</h3>
        <div class="mb-3 ">
            <label for="staticEmail" class="col-sm-2 form-label">Nom</label>
            <div class="col-sm-10">
                <input v-model="newChamp.nom" type="text" class="form-control" id="staticEmail" placeholder="">
            </div>
        </div>
        <div class="mb-3 ">
            <label for="inputPassword" class="col-sm-2 form-label">Description</label>
            <div class="col-sm-10">
                <textarea v-model="newChamp.description" type="text" class="form-control" id="inputPassword"></textarea>
            </div>
        </div>

        <button type="button" class=" btn btn-sm btn-primary " v-on:click="addChamp">
            <span data-feather="calendar" class="align-text-bottom"></span>
            Créer
        </button>
        <button type="button" class="ms-2 btn btn-sm btn-outline-primary " v-on:click="showAjouterDomaines">
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