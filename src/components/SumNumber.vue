<template>
  <div class="px-4">
    <!-- {{ num1 }} + {{ num2 }} = {{ num1 + num2 }} -->
    <span>field's value is
      {{ len }} character(s) long.</span> 
  </div>
</template>

<script setup lang="ts">
const props = defineProps<{
  num1: string
  num2: string
}>()

// useMemo 해당하는게 $computed
// const len = useMemo(() => (props.num1 + props.num2).length, [props.num1, props.num2])
const len = $computed(() => (props.num1 + props.num2).length)
</script>

