<script setup>
import Card from './Card.vue'

defineProps({
  items: Array
})

const emit = defineEmits(['addToFavourite', 'addtoCart'])
</script>

<template>
  <div class="grid grid-cols-4 gap-5">
    <Card
      v-for="item in items"
      :key="item.id"
      :id="item.id"
      :image-url="item.imageUrl"
      :title="item.title"
      :price="item.price"
      :is-added="item.isAdded"
      :is-favourite="item.isFavourite"
      :on-click-add="() => emit('addToCart', item)"
      :on-click-favourite="() => emit('addToFavourite', item)"
    />
  </div>
</template>
