<template>
  <template v-for="num in list">
    <div
      v-if="num % 2 !== 0"
      :key="num"
    >
      {{ num }}
    </div>
  </template>
</template>

<script lang="ts" setup>
const list = $ref(Array.from({ length: 10 }, (_, i) => i + 1))

// for(let i = 0; i < list.length; i++) {
//   const num = list[i]

//   if (num % 2 === 0) {
//     console.log(num)
//   }
// }
</script>