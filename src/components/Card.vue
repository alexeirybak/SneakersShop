<script setup>
const props = defineProps({
  id: Number,
  title: String,
  imageUrl: String,
  price: Number,
  isFavourite: Boolean,
  isAdded: Boolean,
  onClickAdd: Function,
  onClickFavourite: Function
})

const visibleFavouriteButton = Boolean(props.onClickAdd)
</script>

<template>
  <div
    class="relative bg-white border-slate-100 rounded-3xl p-8 cursor-pointer hover:-translate-y-2 hover:shadow-xl transition"
  >
    <img
      v-if="onClickFavourite"
      :src="!isFavourite ? '/like-2.svg' : '/like-1.svg'"
      alt="Like 1"
      class="absolute top-8 left-8"
      @click="onClickFavourite"
    />

    <img :src="imageUrl" alt="Sneaker" />
    <p class="mt-2">{{ title }}</p>
    <div class="flex justify-between mt-5">
      <div class="flex flex-col">
        <span class="text-slate-400">Цена:</span>
        <span>{{ price }} руб.</span>
      </div>
      <img
        v-if="onClickAdd"
        @click="onClickAdd"
        :src="isAdded ? '/checked.svg' : '/plus.svg'"
        alt="Plus"
      />
    </div>
  </div>
</template>
