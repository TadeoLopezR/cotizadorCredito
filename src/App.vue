<script setup>
import { ref, computed ,watch} from "vue";
import  Header  from "./components/Header.vue";
import  Button  from "./components/Button.vue";
import { calcularTotal } from "./helpers";


const cantidad = ref(10000); //state
const meses = ref(6); //state
const total = ref(0); //state

const MIN=0;
const MAX=20000;
const STEP=100;


const formatearDinero = (valor) =>{
  return new Intl.NumberFormat('en-US',{style:'currency',currency:'USD'}).format(valor)
}

watch([cantidad,meses], () => {
  total.value = calcularTotal(cantidad.value, meses.value)
})

const pagoMensual = computed(() => {
  return total.value / meses.value
})

const handleChangeDecremento = (mensaje) =>{
  const valor = cantidad.value - STEP
  if (valor<MIN) {
    alert(mensaje)
    
  }
  cantidad.value = cantidad.value - STEP
}

const handleChangeIncremento = (mensaje) =>{
  const valor = cantidad.value + STEP
  if (valor>MAX) {
    alert(mensaje)
  }
  cantidad.value = cantidad.value + STEP
}

</script>

<template>
  <div class="my-20 max-w-lg mx-auto bg-white shadow p-10">
    <Header/>
    <div class="flex justify-between mt-10">
      <Button
        :operador="'-'"
        @fn="handleChangeDecremento"/>

      <Button
        :operador="'+'"
        @fn="handleChangeIncremento"/>

    </div>
    <div class="my-5">
      <input 
        type="range"
        class="w-full bg-gray-200 accent-lime-500 hover:accent-lime-600"
        :min="MIN"
        :max="MAX"
        :step="STEP"
        v-model.number="cantidad"
      >
      <p class="text-center my-10 text-5xl text-indigo-600 font-extrabold">{{formatearDinero(cantidad)}}</p>

      <h2 class="text-2xl font-extrabold text-gray-500 text-center">
        Elige un <span class="text-indigo-600">Plazo</span> a pagar
      </h2>
      <select :value="meses"
        class="w-full p-2 bg-white border mt-5 border-gray-300 rounded-lg text-center text-xl font-bold text-gray-500"
        v-model.number="meses">
        <option value="6">6 Meses</option>
        <option value="12">12 Meses</option>
        <option value="24">24 Meses</option>
      </select>
    </div>  
    <div v-if="total" class="my-5 space-y-3 bg-gray-50 p-5">
      <h2 class="text-2xl font-extrabold text-gray-500 text-center">
        Resumen <span class="text-indigo-600">de pagos</span>
      </h2>
      <p class=" tex-xl text-gray-500 font-bold text-center"> {{meses}} Meses</p>
      <p class=" tex-xl text-gray-500 font-bold text-center">Total a Pagar: {{formatearDinero(total)}}</p>
      <p class=" tex-xl text-gray-500 font-bold text-center">Mensuales: {{formatearDinero (pagoMensual)}}</p>
    </div>
    <p v-else class="text-center">Añade una Cantidad y un Plazo a Pagar</p>
  </div>
</template>