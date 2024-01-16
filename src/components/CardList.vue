<script setup>
import Card from './Card.vue'

defineProps({
  items: Array
})
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
      :on-click-add="onClickAdd"
      :on-click-favourite=""
    />
  </div>
</template>
