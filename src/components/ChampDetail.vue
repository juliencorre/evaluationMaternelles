<script setup lang="ts">

    /* 
        Composant d'affichage des details d'une competence spécifique
    */
    
    import {
        reactive,
        watch,
        ref,
        onMounted
    } from 'vue'

    import axios from 'axios'

    const myProps = defineProps<{
        id?: string
    }>()

    const vClickOutside = {
        mounted: (el, binding, vnode) => {
            el.clickOutsideEvent = function (event) {
                if (!(el === event.target || el.contains(event.target))) {
                    hideMenu()
                }
            }
            document.body.addEventListener('click', el.clickOutsideEvent);
        },
        unmounted: (el) => {
            document.body.removeEventListener('click', el.clickOutsideEvent);
        }
    }

    // On observe le changement de valeur de l'identifiant de la competence specifique à afficher
    watch(myProps, async (newProps) => {
        myVariables.menuSelected = false
        showCompetence(newProps.id)
    })

    const myVariables = reactive({
        competence: null,
        menuSelected : false
    });


    /*
    *   Function that display the details of a specific competence
    */
    function showCompetence(id) {

        if (id != null) {
        console.log("showDomaine: " + id);

        axios({
            method: "GET",
            "url": "http://localhost:3000/competences/champs/" + id
            //"url": "/champs_" + id + ".json"
        }).then(result => {
            console.log("Retour du service, :result.data: " + result.data);
            myVariables.competence = result.data;

        }, error => {
            console.error(error);
        });
        }

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




    <div v-if="myVariables.competence">

        <div class=" d-flex">
            <div class="w-100">
                <h5 class="card-title">{{ myVariables.competence.nom }}</h5>
            </div>
            <div class="flex " v-clickOutside>
                <a id="btnMenu" type="button" @click="showMenu" class=" btn  p-0 "><i class="fs-4 bi bi-three-dots-vertical"></i></a>
                <div id="menu" class="list-group shadow " v-if="myVariables.menuSelected" >
                    <a href="#" class="list-group-item list-group-item-action d-flex justify-content-between align-items-start">Editer<i class=" ms-2 bi bi-pencil"></i></a>
                    <a href="#" class="list-group-item list-group-item-action d-flex justify-content-between align-items-start">Supprimer<i class="ms-2 bi bi-trash"></i></a>
                </div>
            </div>
        </div>

        <p class="card-text">{{ myVariables.competence.description }}</p>

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