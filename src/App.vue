<script setup>
import { onMounted, ref, reactive, watch, provide } from 'vue'
import axios from 'axios'

import Header from './components/Header.vue'
import CardList from './components/CardList.vue'
import Drawer from './components/Drawer.vue'

const items = ref([])
const cart = ref([])

const drawerOpen = ref(false)

const closeDrawer = () => {
  drawerOpen.value = false
}

const openDrawer = () => {
  drawerOpen.value = true
}

const filters = reactive({
  sortBy: 'title',
  searchQuery: ''
})

const onChangeSelect = (e) => {
  filters.sortBy = e.target.value
}

const onChangeSearch = (e) => {
  filters.searchQuery = e.target.value
}

const addToCart = (item) => {
  if (!item.isAdded) {
    cart.value.push(item)
  } else {
    cart.value.splice(cart.value.indexOf(item), 1)
  }
}

const addToFavourite = async (item) => {
  try {
    if (!item.isFavourite) {
      item.isFavourite = true
      const obj = {
        parentId: item.id
      }

      const { data } = await axios.post(`https://3c0d2296a5b88a3d.mokky.dev/favourites`, obj)

      item.favouriteId = data.id
    } else {
      item.isFavourite = false
      await axios.delete(`https://3c0d2296a5b88a3d.mokky.dev/favourites/${item.favouriteId}`)
      item.favouriteId = null
    }
  } catch (err) {
    console.log(err)
  }
}

const fetchFavourites = async () => {
  try {
    const { data: favourites } = await axios.get(`https://3c0d2296a5b88a3d.mokky.dev/favourites`)

    items.value = items.value.map((obj) => {
      const favourite = favourites.find((favorite) => favorite.parentId === obj.id)

      if (!favourite) {
        return obj
      }

      return {
        ...obj,
        isFavourite: true,
        favoriteId: favourite.id
      }
    })
  } catch (err) {
    console.log(err)
  }
}

const fetchItems = async () => {
  try {
    const params = {
      sortBy: filters.sortBy
    }

    if (filters.searchQuery) {
      params.title = `*${filters.searchQuery}*`
    }
    const { data } = await axios.get(`https://3c0d2296a5b88a3d.mokky.dev/items`, { params })

    items.value = data.map((obj) => ({
      ...obj,
      isFavourite: false,
      isAdded: false,
      favouriteId: null
    }))
  } catch (err) {
    console.log(err)
  }
}

onMounted(async () => {
  await fetchItems()
  await fetchFavourites()
})
watch(filters, fetchItems)

provide('cartActions', {
  openDrawer,
  closeDrawer
})
</script>

<template>
  <Drawer v-if="drawerOpen" />
  <div class="bg-white w-4/5 m-auto rounded-xl shadow-xl mt-14 mb-14">
    <Header @open-drawer="openDrawer" />

    <div class="p-10">
      <div class="flex justify-between items-center">
        <h2 class="text-3xl font-bold mb-8">All Sneakers</h2>
        <div class="flex gap-4">
          <select @change="onChangeSelect" class="py-2 px-3 border rounded-md outline-none">
            <option value="name">By name</option>
            <option value="price">By price (less expensive)</option>
            <option value="-price">By price (more expensive)</option>
          </select>

          <div class="relative">
            <img class="absolute left-4 top-3" src="/search.svg" alt="" />
            <input
              @input="onChangeSearch"
              class="border rounded-md py-2 pl-11 pr-4 outline-none focus:border-gray-400"
              type="text"
              placeholder="Search..."
            />
          </div>
        </div>
      </div>

      <div class="mt-10">
        <CardList :items="items" @add-to-favourite="addToFavourite" @add-to-cart="addToCart" />
      </div>
    </div>
  </div>
</template>
