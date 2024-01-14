<script setup>
defineProps({
  imageUrl: String,
  title: String,
  price: Number,
  isFavourite: Boolean,
  isAdded: Boolean,
  onClickAdd: Function,
  onClickFavourite: Function
})

</script>

<template>
  <div
    class="relative bg-white border border-slate-200 rounded-3xl p-8 cursor-pointer transition hover:-translate-y-2 hover:shadow-xl"
  >
    <img 
      :src="!isFavourite ? '/like-1.svg' : '/like-2.svg'" 
      alt="Like 1" 
      class="absolute top-8 left-8" 
      @click="onClickFavourite"/>
    
    <img :src="imageUrl" alt="Sneaker" />

    <p class="mt-2">{{ title }}</p>

    <div class="flex justify-between mt-5">
      <div class="flex flex-col">
        <span class="text-slate-400"> Price:</span>
        <span>{{ price }} $</span>
      </div>

      <img 
        :src="!isAdded ? '/plus.svg' : '/checked.svg'" 
        alt="Plus" 
        @click="onClickAdd"/>
    </div>
  </div>
</template>
