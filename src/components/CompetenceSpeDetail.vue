
<script setup lang="ts">

    import {
        onMounted,
        reactive,
        watch
    } from 'vue'

    import axios from 'axios'

    const id = defineProps(['id'])

    watch(id, async (newId) => {
        console.log(id.id);
        showCompetence(newId.id)
    })

    const competence = reactive({
        competence: null
    });

    function showCompetence(id) {
        console.log("showCompetence: " + id);

        axios({
            method: "GET",
            "url": "/competences_" + id + ".json"
        }).then(result => {

            console.log("Retour du service, :result.data: " + result.data);
            competence.competence = result.data;

        }, error => {
            console.error(error);
        });

    }

</script>

<template>
   
    <div>{{id.id}}</div>
    <div v-if="competence.competence" >

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
</template>

<style scoped>


</style>