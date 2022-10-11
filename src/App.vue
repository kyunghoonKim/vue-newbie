<template>
  <!-- <div class="p-4">
    <ForPrac class="p-1" />
    <IfPrac class="p-1" />
    <ForIfPrac class="p-1" />
    <hr>
    <div>
      num1: <input
        v-model="num1"
        type="text"
      >
    </div>
    <div>
      num2: <input
        v-model.lazy="num2"
        type="text"
      >
    </div>
  </div>
  <SumNumber
    :num1="num1"
    :num2="num2"
  /> -->
  <!-- <div class="text-center">
    <button @click="goAbout">
      go About
    </button>
  </div>
  <hr> -->
  <div
    id="app"
    class="w-screen h-screen"
  >
    <router-view />
  </div>
</template>

<script setup lang="ts">
import SumNumber from './components/SumNumber.vue'
import ForPrac from './components/ForPrac.vue'
import IfPrac from './components/IfPrac.vue'
import ForIfPrac from './components/ForIfPrac.vue'
import { useRouter } from 'vue-router'

let num1 = $ref('')
let num2 = $ref('')

fetch('http://localhost:3001/users/')
  .then((res)=> res.json())
  .then(res => console.log(res))


const $router = useRouter()

const goAbout = () => {
  $router.push('/about')
}
</script>
