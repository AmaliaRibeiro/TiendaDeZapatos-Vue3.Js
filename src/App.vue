<script setup>
import {ref, reactive, onMounted, watch } from 'vue'
import {db} from './data/zapatos'
import Zapato from './components/Zapato.vue'
import Header from './components/Header.vue'
import Footer from './components/Footer.vue'

const zapatos = ref([])
const carrito = ref([])
const zapato = ref ([])

watch(
  carrito, () =>{
    guardarLocalStorage();
  }, {
    deep: true // si es una estructura muy grande no sirve bien.
  }
)

onMounted(() => {
  zapatos.value = db
  zapato.value = db[2]

  const carritoStorage = localStorage.getItem('carrito')
  if(carritoStorage) {
    carrito.value = JSON.parse(carritoStorage)
  }
})

const guardarLocalStorage = () => {
  localStorage.setItem('carrito', JSON.stringify(carrito.value))
}
 const agregarCarrito = (zapato) => {
  const existeCarrito = carrito.value.findIndex(producto => producto.id === zapato.id)
  if (existeCarrito >= 0) {
    carrito.value[existeCarrito].cantidad++ //si quiero agregar mas de un zapato del mismo modelo se agrega .cantidad++
  } else {
    zapato.cantidad = 1;
    carrito.value.push(zapato);
  }
 }

 const decrementarCantidad = (id) => {
  const index = carrito.value.findIndex(producto => producto.id === id)
  if(carrito.value[index].cantidad <= 1) return //validacion para que no baje del 1 la cantidad
  carrito.value[index].cantidad--
 }

 const incrementarCantidad = (id) => {
  const index = carrito.value.findIndex(producto => producto.id === id)
  if(carrito.value[index].cantidad >= 5) return //para que no sume mas de 1 en cantidad.
  carrito.value[index].cantidad++
 }

 const eliminarProducto = (id) => {
    carrito.value = carrito.value.filter(producto => producto.id !== id)
 }

 const vaciarCarrito = () => {
  carrito.value = []
 } 

</script>

<template>
<Header 
  :carrito="carrito"
  :zapato="zapato"
  @incrementar-cantidad ="incrementarCantidad"
  @decrementar-cantidad ="decrementarCantidad"
  @agregar-carrito="agregarCarrito"
  @eliminar-producto="eliminarProducto"
  @vaciar-carrito="vaciarCarrito"
/> 
   
    <main class="container-xl mt-5">
        <h2 class="text-center">Nuestra Colección</h2>

        <div class="row mt-5">
           <Zapato
           v-for="zapato in zapatos"
           :key="zapato.id"
           v-bind:zapato="zapato"
           @agregar-carrito="agregarCarrito"
          />
        </div>
    </main>

<Footer/>
</template>


