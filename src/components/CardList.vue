<script setup>
import Card from './Card.vue'

defineProps({
  items: Array
})
const onClickAdd = () => {}

const onClickFavourite = () => {}
</script>

<template>
  <div class="grid grid-cols-4 gap-5">
    <Card
      v-for="item in items"
      :key="item.id"
      :image-url="item.imageUrl"
      :title="item.title"
      :price="item.price"
      :is-added="false"
      :on-click-add="onClickAdd"
      :is-favourite="false"
      :on-click-favourite="onClickFavourite"
    />
  </div>
</template>
