<template>
    <inicio v-if="showInicio" @mostrar="mostrar" />
    <div class="container">
        <div class="row">


            <div class="col" v-if="!showInicio">
                <p>¿Qué quieres hacer?</p>
                <form @submit.prevent="handleSubmit">
                    <div>
                        <label>Nombre del presupuesto:</label>
                        <input type="text" v-model="presupuesto" class="m-2" name="presupuesto">
                    </div>

                    <div>
                        <label>Nombre del cliente:</label>
                        <input type="text" v-model="cliente" class="m-2" name="cliente">
                    </div>

                    <div class="m-2">
                        <input type="checkbox" @change="total(500), show(), checkValue()" id="paginaWeb">
                        <label>Una pàgina web (500€)</label>
                    </div>

                    <Panell @sumPagines="sumPagines" @sumIdiomes="sumIdiomes" @incrementPag="incrementPag"
                        @decrementPag="decrementPag" @incrementIdio="incrementIdio" @decrementIdio="decrementIdio"
                        v-if="showPanell" class="m-2" />

                    <div class="m-2">
                        <input type="checkbox" @change="total(300), checkValue()"  id="SEO">
                        <label>Una consultoria SEO (300€)</label>
                    </div>

                    <div class="m-2">
                        <input type="checkbox" @change="total(200), checkValue()" id="Ads">
                        <label>Una campanya de Google Ads (200€)</label>
                    </div>


                    <div class="m-2">
                        <button class="btn btn-primary">Submit</button>
                    </div>


                </form>

                <p class="m-2">total:{{ suma }}€</p>

                <button @click="mostrar" class="m-2">Atras</button>

                <div class="mb-5 mt-3">
                    
                    <input type="search" v-model="buscar" placeholder="Buscar presupuesto">
                    
                    <div class="mt-3">
                        <ul v-for="search in array">
                            <div class="border border-dark rounded">
                                <p class="m-2">Presupuesto {{ search.id }}: {{ search.Presupuesto }}</p>
                                <p class="m-2">Nombre del cliente: {{ search.Cliente }}</p>
                                <p class="m-2">Precio total: {{ search.Total }}€</p>
                                <p class="m-2">{{ search.Fecha }}</p>
                            </div>

                        </ul>
                    </div>
                </div>

            </div>



            <pressupostList :listaPresupuestos="listaPresupuestos" @ordenAlfabetico="ordenAlfabetico"
                @ordenFecha="ordenFecha" @reinicio="reinicio" v-if="!showInicio" />


        </div>
    </div>


</template>

<script setup>
import { ref, reactive, watch } from 'vue';
import Panell from '@/components/Panell.vue';
import inicio from '@/components/inicio.vue'
import pressupostList from '@/components/pressupostList.vue'

import { useRoute, useRouter } from 'vue-router';

const route = useRoute();  
const router = useRouter();



let presupuesto = '';

let cliente = '';

let pagines = ref(1);

let idiomes = ref(1);

let final = ref(0);

const jobs = [];

let precio = ref(0);

let suma = ref(0);

let id = 0;

let showPanell = ref(false)
let showInicio = ref(true);

let listaPresupuestos = reactive([]);

const sumPagines = (valorArecuperar) => {
    pagines.value = valorArecuperar;
    final.value = (pagines.value * idiomes.value) * 30;
    suma.value = precio.value + final.value;

}

const sumIdiomes = (valorArecuperar) => {
    idiomes.value = valorArecuperar;
    final.value = (pagines.value * idiomes.value) * 30;
    suma.value = precio.value + final.value;
}

const incrementPag = (valorArecuperar) => {
    valorArecuperar++
    sumPagines(valorArecuperar)
}

const decrementPag = (valorArecuperar) => {
    valorArecuperar--
    if (valorArecuperar <= 1) {
        valorArecuperar = 1;
    }
    sumPagines(valorArecuperar)
}

const incrementIdio = (valorArecuperar) => {
    valorArecuperar++
    sumIdiomes(valorArecuperar)
}

const decrementIdio = (valorArecuperar) => {
    valorArecuperar--
    if (valorArecuperar <= 1) {
        valorArecuperar = 1;
    }
    sumIdiomes(valorArecuperar)
}

const total = (e) => {
    if (!jobs.includes(e)) {
        jobs.push(e);
        precio.value = jobs.reduce((a, b) => a + b, 0);
        precio.value = precio.value + final.value;
        suma.value = precio.value


    } else {
        for (let i = 0; i < jobs.length; i++) {
            if (jobs[i] == e) {
                jobs.splice(i, 1);
                precio.value = jobs.reduce((a, b) => a + b, 0);
                precio.value = precio.value + final.value;
                suma.value = precio.value
            }
        }
    }
}

const show = () => {
    if (showPanell.value == true) {
        showPanell.value = false;
        pagines.value = 0;
        idiomes.value = 0;
        sumPagines(0);
        sumIdiomes(0);
    } else {
        showPanell.value = true
        sumPagines(1);
        sumIdiomes(1);
    };
};

const mostrar = () => {
    if (showInicio.value == true) {
        showInicio.value = false;
    } else {
        showInicio.value = true;
    }
}

const handleSubmit = () => {
    const date = new Date();
    id++

    let obj = {
        id: id,
        Fecha: date,
        Presupuesto: presupuesto,
        Cliente: cliente,
        Total: suma.value
    };

    listaPresupuestos.push(obj);

}


const ordenAlfabetico = () => {

    let sort = listaPresupuestos.sort((a, b) => a.Presupuesto.localeCompare(b.Presupuesto));
}

const ordenFecha = () => {

    let sort = listaPresupuestos.sort((a, b) => a.Fecha - b.Fecha);

}

const reinicio = () => {

    let sort = listaPresupuestos.sort((a, b) => a.id - b.id)
}

let webValue = ref();
let seoValue = ref();
let adsValue = ref();

const checkValue = () => {
    
    webValue = paginaWeb.checked;
    seoValue = SEO.checked;
    adsValue = Ads.checked

    router.replace({path: '/', query:{Nombre: presupuesto, Cliente: cliente, paginaWeb: webValue, SEO: seoValue, Ads: adsValue}})
    
}

let array = reactive([])
let buscar = ref('');




watch(buscar, (newVal, oldVal) => {
    array.length = 0;
    for (let i = 0; i < listaPresupuestos.length; i++) {
        if (listaPresupuestos[i].Presupuesto.includes(buscar.value) && buscar.value !== '' || listaPresupuestos[i].Cliente.includes(buscar.value) && buscar.value !== '') {
            array.push(listaPresupuestos[i])
        }
    }
})



watch(pagines, (newVal, oldVal) => {
    
    router.replace({path: '/', query:{Nombre: presupuesto, Cliente: cliente, paginaWeb: webValue, pagines: pagines.value, idiomas: idiomes.value, SEO: seoValue, Ads: adsValue}})
});

watch(idiomes, (newVal, oldVal) => {
    
    router.replace({path: '/', query:{Nombre: presupuesto, Cliente: cliente, paginaWeb: webValue, pagines: pagines.value, idiomas: idiomes.value, SEO: seoValue, Ads: adsValue}})
});




</script>

<style>
.home {
    justify-content: left;
    text-align: left;

}
</style>