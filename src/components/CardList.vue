<script setup>
import Card from './Card.vue'

defineProps({
  items: Array,
  isFavourites: Boolean
})

const emit = defineEmits(['addToFavourite', 'addToCart'])
</script>

<template>
  <div class="flex flex-row flex-wrap gap-4 justify-center" v-auto-animate>
    <Card
      v-for="item in items"
      :key="item.id"
      :id="item.id"
      :title="item.title"
      :imageUrl="item.imageUrl"
      :price="item.price"
      :onClickAdd="isFavourites ? null : () => emit('addToCart', item)"
      :onClickFavourite="isFavourites ? null : () => emit('addToFavourite', item)"
      :isFavourite="item.isFavourite"
      :isAdded="item.isAdded"
    />
  </div>
</template>

