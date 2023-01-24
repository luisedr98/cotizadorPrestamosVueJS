<script setup>
import Header from "./components/Header.vue";
import Button from "./components/Button.vue";
import {calcularTotalPagar} from "./helpers"
import { ref, watch, computed } from "vue";


const max = 20000,
  min = 0,
  step = 100;

const cantidad = ref(max / 2);
const meses = ref(6);
const total = ref(0);

watch([meses, cantidad], ()=>{
    total.value = calcularTotalPagar(cantidad.value, meses.value);
});


const formatearDinero = (valor) => {
  const formater = new Intl.NumberFormat("en-US", {
    style: "currency",
    currency: "USD",
  });

  return formater.format(valor);
};

const handleOnclickIncremento = () => {
      const valor = cantidad.value + step;
      if (valor <= max) cantidad.value = valor;
      else alert('¡Valor máximo alcanzado!');
  }

  const handleOnclickDecremento = () => {
    const valor = cantidad.value - step;
    if (valor >= min) cantidad.value = valor;
    else alert('¡Valor mínimo alcanzado!');
  }

  const calcularTotal = computed(()=> total.value / meses.value);
</script>

<template>
  <div class="my-20 max-w-lg mx-auto bg-white shadow p-10">
    <Header />
    <div class="my-5">
      <input type="range" class="w-full bg-gray-200 accent-lime-400 hover:accent-lime-600" :min="min" :step="step"
        :max="max" v-model.number="cantidad" />
    </div>
    <p className="text-center text-5xl font-extrabold text-indigo-600">
      {{ formatearDinero(cantidad) }}
    </p>
    <div class="flex justify-between mt-10">
      <Button 
      :operador="'-'"
      @fn="handleOnclickDecremento"/>
      
      <Button 
      :operador="'+'"
      @fn="handleOnclickIncremento"/>
    </div>

    <h2 className="text-2xl font-extrabold text-gray-500 text-center my-5">
        Elige un <span className="text-indigo-600">Plazo </span> a pagar
      </h2>

      <select className="w-full p-3 bg-white border border-gray-300 rounded-lg text-center text-xl font-bold text-gray-500"
              :value="meses"
              v-model.number="meses"
            >
        <option value="6">6 Meses</option>
        <option value="12">12 Meses</option>
        <option value="24">24 Meses</option>
      </select>

    <div v-if="total > 0" className="space-y-3 bg-gray-50 my-5 py-5">
        <h2 className="text-2xl font-extrabold text-gray-500 text-center">
          Resumen de <span className="text-indigo-600">Pago </span>
        </h2>
        <p className="text-xl text-gray-500 text-center font-bold">{{meses}} Meses</p>
        <p className="text-xl text-gray-500 text-center font-bold">{{formatearDinero(total)}} Total a pagar</p>
        <p className="text-xl text-gray-500 text-center font-bold">{{formatearDinero(calcularTotal)}} Mensuales</p> 
      </div>

      <p v-else class="text-center text-red-400 font-bold py-5">Añade una cantidad y un plazo a pagar</p>
    
      <p className="text-gray-500 text-center font-bold mb-5">
        Realizado por
        <span className="text-indigo-500 italic">Luis Dominguez</span>
      </p>

      <div className="font-bold rounded flex justify-evenly items-center uppercase">
        <div className="bg-indigo-600 hover:bg-indigo-800 w-5/12 mx-auto rounded">
          <a
            class="text-white flex justify-center items-center"
            href="https://github.com/luisedr98"
          >
            <div>
              <svg
                xmlns="http://www.w3.org/2000/svg"
                class="icon icon-tabler icon-tabler-brand-github"
                width="44"
                height="44"
                viewBox="0 0 24 24"
                stroke-width="1.5"
                stroke="#ffffff"
                fill="none"
                stroke-linecap="round"
                stroke-linejoin="round"
              >
                <path stroke="none" d="M0 0h24v24H0z" fill="none" />
                <path d="M9 19c-4.3 1.4 -4.3 -2.5 -6 -3m12 5v-3.5c0 -1 .1 -1.4 -.5 -2c2.8 -.3 5.5 -1.4 5.5 -6a4.6 4.6 0 0 0 -1.3 -3.2a4.2 4.2 0 0 0 -.1 -3.2s-1.1 -.3 -3.5 1.3a12.3 12.3 0 0 0 -6.2 0c-2.4 -1.6 -3.5 -1.3 -3.5 -1.3a4.2 4.2 0 0 0 -.1 3.2a4.6 4.6 0 0 0 -1.3 3.2c0 4.6 2.7 5.7 5.5 6c-.6 .6 -.6 1.2 -.5 2v3.5" />
              </svg>
            </div>
            <p>Mi github</p>
          </a>
        </div>
      </div>
  </div>
</template>

<style scoped>

</style>
