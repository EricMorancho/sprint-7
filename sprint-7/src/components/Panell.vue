<template>
    <div class="container">
        <div class="row border border-dark rounded">
            <div class="m-2">

                <label class="col-4">Número de páginas</label>

                <img src="@/img/plus-circle.svg" alt="plus"
                    @click.prevent="$emit('incrementPag', pagines), updateInput(1)" class="ms-2 me-2">

                <input type="number" v-model="pagines" v-on:change="$emit('sumPagines', pagines)" class="col-2" min="1" readonly>

                <img src="@/img/dash-circle.svg" alt="minus"
                    @click.prevent="$emit('decrementPag', pagines), updateInput(2)" class="ms-2">


                <!-- Button trigger modal -->
                <img src="@/img/info-circle-fill.svg" alt="" data-bs-toggle="modal" data-bs-target="#modalPagines"
                    class="ms-2">

                <!-- Modal -->
                <div class="modal fade" id="modalPagines" data-bs-backdrop="static" data-bs-keyboard="false"
                    tabindex="-1" aria-labelledby="staticBackdropLabel" aria-hidden="true">
                    <div class="modal-dialog">
                        <div class="modal-content">
                            <div class="modal-header">
                                <h5 class="modal-title" id="exampleModalLabel">Info:</h5>
                                <button type="button" class="btn-close" data-bs-dismiss="modal"
                                    aria-label="Close"></button>
                            </div>
                            <div class="modal-body mb-5">
                                En este componente se debe indicar el número de páginas que tendrá su sitio web.
                            </div>
                        </div>
                    </div>
                </div>
            </div>

            <div class="m-2">
                <label class="col-4">Número de idiomas</label>

                <img src="@/img/plus-circle.svg" alt="plus"
                    @click.prevent="$emit('incrementIdio', idiomes), updateInput(3)" class="ms-2 me-2">

                <input type="number" v-model="idiomes" v-on:change="$emit('sumIdiomes', idiomes)" name="precio"
                    class="col-2" min="1" readonly>

                <img src="@/img/dash-circle.svg" alt="minus"
                    @click.prevent="$emit('decrementIdio', idiomes), updateInput(4)" class="ms-2">

                <!-- Button trigger modal -->
                <img src="@/img/info-circle-fill.svg" alt="" data-bs-toggle="modal" data-bs-target="#modalIdiomes"
                    class="ms-2">

                <!-- Modal -->
                <div class="modal fade" id="modalIdiomes" data-bs-backdrop="static" data-bs-keyboard="false"
                    tabindex="-1" aria-labelledby="staticBackdropLabel" aria-hidden="true">
                    <div class="modal-dialog">
                        <div class="modal-content">
                            <div class="modal-header">
                                <h5 class="modal-title" id="exampleModalLabel">Info:</h5>
                                <button type="button" class="btn-close" data-bs-dismiss="modal"
                                    aria-label="Close"></button>
                            </div>
                            <div class="modal-body mb-5">
                                En este componente se debe indicar el número de idiomas que tendrá su sitio web.
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </div>

</template>

<script setup>

import { ref } from 'vue';

let pagines = ref(1);
let idiomes = ref(1);

const updateInput = (e) => {
    switch (e) {
        case 1:
            pagines.value++;
            break;
        case 2:
            pagines.value--;
            if (pagines.value <= 1) {
                pagines.value = 1;
            };
            break;
        case 3:
            idiomes.value++;
            break;
        case 4:
            idiomes.value--;
            if (idiomes.value <= 1) {
                idiomes.value = 1;
            };
            break;
    }
}
</script>

<style>

</style>

