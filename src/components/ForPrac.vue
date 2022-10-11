<template>
  <template
    v-for="(item, index) in list"
    :key="index"
  >
    <div 
      v-if="item.discountRate >= 0.25"
    >
      <!-- {{ index }} -->
      {{ item.name }}
      {{ item.price }}
      {{ item.discountRate }}
      <span 
        :style="(item.price * item.discountRate) <= 5000 ? 'color: red;' : 'color: blue'"
      >
        {{ item.price * item.discountRate }}
      </span>
    </div>
  </template>
</template>

<script lang="ts" setup>
import random from 'lodash-es/random'

const list = $ref(Array.from({ length: 1e3 }, (_, i) => ({
  name: String.fromCharCode(random(65, 122)) + i,
  price: random(1e3, 1e5),
  discountRate: +random(0.01, 0.5).toFixed(2)
})))
</script>