<script setup>
const props = defineProps({
  title: String,
  price: Number
})
</script>

<template>
  <p>{{ props.title }}</p>
  <button>Open</button>
  <slot></slot>
</template>

<style scoped>
body {
  background: red;
}
</style>
