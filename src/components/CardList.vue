<script setup>
import Card from './Card.vue'

const onClickAdd = () => {
  
}

const onClickFavourite = () => {
  
}
</script>

<template>
  <div class="grid grid-cols-4 gap-5">
    <Card 
      image-url="/sneakers/sneakers-1.jpg" 
      title="Nike Blazer Mid Suede" 
      :price="129.99" 
      :is-added="false"
      :on-click-add="onClickAdd"
      :on-click-favourite="onClickFavourite"
      />
    <Card />
    <Card />
    <Card />
    <Card />
    <Card />
  </div>
</template>
