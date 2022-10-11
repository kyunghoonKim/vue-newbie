<template>
  <div>
    <input
      v-model="fruitName"
      type="text"
      placeholder="과일을 입력하세요."
    >
    <hr>
    <div>
      <div v-if="findedFruit?.stock === 0">
        {{ fruitName }} 이 있지만, 재고가 없습니다.
      </div>

      <div v-else-if="findedFruit">
        <div
          v-for="(value, key) in findedFruit"
          :key="key"
        >
          {{ key }}: {{ value }}
        </div>
      </div>
    
      <div v-else>
        입력한 과일이 없습니다.
      </div>
    </div>
  </div>
</template>

<script lang="ts" setup>
const fruits = $ref({
  apple: {
    name: '사과',
    price: 10000,
    stock: 1,
  },
  banana: {
    name: '바나나',
    price: 1000,
    stock: 2,
  },
  grape: {
    name: '포도',
    price: 500,
    stock: 0,
  }
})
let fruitName = $ref('')
const findedFruit = $computed(() => fruits[fruitName.toLowerCase()])
</script>