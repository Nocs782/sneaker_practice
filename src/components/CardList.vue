<script setup>
import Card from './card.vue'

defineProps({
  items: Array
})

const emit = defineEmits(['addToFavorites'])
</script>
<template>
  <div class="grid grid-cols-4 gap-5">
    <Card
      v-for="item in items"
      :key="item.id"
      :id="item.id"
      :text="item.title"
      :price="item.price"
      :image-url="item.imageUrl"
      :onClickFavorite="() => emit('addToFavorites', item)"
      :isFavorite="item.isFavorite"
    />
  </div>
</template>

<!-- :is-added="false"
      :is-favorite="true"
      :onClickAdd="onClickAdd" -->
