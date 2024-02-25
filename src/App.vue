<script setup>
import { onMounted, ref, watch, reactive, provide } from 'vue'
import axios from 'axios'

import Header from './components/Header.vue'
import CardList from './components/CardList.vue'
import Drawer from './components/Drawer.vue'

const items = ref([])

const drawerOpen = ref(false)

const filters = reactive({
  sortBy: 'title',
  searchQuerry: ''
})

const onChangeInput = (event) => {
  filters.searchQuerry = event.target.value
}
const onChangeSelect = (event) => {
  filters.sortBy = event.target.value
}

const fetchFavorites = async () => {
  try {
    const { data: favorites } = await axios.get(`https://08e636163cffaac9.mokky.dev/favorites`)

    items.value = items.value.map((item) => {
      const favorite = favorites.find((favorite) => favorite.parentId === item.id)

      if (!favorite) {
        return item
      }
      return {
        ...item,
        isFavorite: true,
        favoriteId: favorite.id
      }
    })
  } catch (err) {
    console.log(err)
  }
}

const addToFavorites = async (item) => {
  try {
    item.isFavorite = !item.isFavorite
    if (item.isFavorite) {
      const obj = {
        parentId: item.id
      }
      const { data } = await axios.post(`https://08e636163cffaac9.mokky.dev/favorites`, obj)

      item.favoriteId = data.id

      console.log(data)
    } else {
      await axios.delete(`https://08e636163cffaac9.mokky.dev/favorites/${item.favoriteId}`)
      item.favoriteId = null
    }
  } catch (err) {
    console.log(err)
  }
}

const fetchItems = async () => {
  try {
    const params = {
      sortBy: filters.sortBy
    }

    if (filters.searchQuerry) {
      params.title = `*${filters.searchQuerry}*`
    }
    const { data } = await axios.get(`https://08e636163cffaac9.mokky.dev/items`, {
      params
    })
    items.value = data.map((obj) => ({
      ...obj,
      isFavorite: false,
      favoriteId: null,
      isAdded: false
    }))
  } catch (err) {
    console.log(err)
  }
}
onMounted(async () => {
  await fetchItems()
  await fetchFavorites()
})
watch(filters, fetchItems)

provide('addToFavorites', addToFavorites)
</script>

<template>
  <Drawer v-if="drawerOpen" />
  <div class="bg-white w-4/5 m-auto rounded-xl shadow-xl mt-10">
    <Header @drawerOpen="drawerOpen" />

    <div class="p-10">
      <div class="flex justify-between items-center">
        <h2 class="text-3xl font-bold mb-10">All sneakers</h2>

        <div class="flex gap-4">
          <select
            @change="onChangeSelect"
            class="py-2 px-3 border rounded-md outline-none"
            name=""
            id=""
          >
            <option value="name">By name</option>
            <option value="price">By price (cheap)</option>
            <option value="-price">By price (expensive)</option>
          </select>
          <div class="relative">
            <img class="absolute left-4 top-3" src="/search.svg" alt="" />
            <input
              @input="onChangeInput"
              type="text"
              placeholder="Search"
              class="border rounded-md py-2 pl-11 pr-4 outline-none focus:border-gray-400"
            />
          </div>
        </div>
      </div>
      <div class="mt-10">
        <CardList :items="items" @addToFavorites="addToFavorites" />
      </div>
    </div>
  </div>
</template>

<style scoped></style>
