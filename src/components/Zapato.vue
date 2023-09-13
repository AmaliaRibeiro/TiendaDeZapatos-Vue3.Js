<script setup>

const props = defineProps({
    zapato: {
     type: Object,
     required: true
    }
})

defineEmits(['agregar-carrito'])
</script>

<template>
<div class="col-md-6 col-lg-4 my-4 row align-items-center">
    <div class="col-4">
        <img class="img-fluid" 
        v-bind:src="'/img/' + zapato.imagen + '.png'" 
        :alt="'imagen guitarra' + zapato.nombre">
    </div>
    <div class="col-8">
        <h3 class="text-black fs-4 fw-bold text-uppercase">{{ zapato.nombre}}</h3>
        <p>{{ numero }}</p>
        <p>{{ zapato.descripcion }}</p>
        <p class="fw-bold">{{ zapato.talle }}</p>
        <p class="fw-black text-primary fs-3">${{ zapato.precio }}</p>
        <button 
            type="button"
            class="btn btn-dark w-100 "
            v-on:click="$emit('agregar-carrito', zapato)"
        >Agregar al Carrito</button>
    </div>
</div><!-- FIN GUITARRA -->
</template>