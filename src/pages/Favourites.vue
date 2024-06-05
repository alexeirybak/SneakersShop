<script setup>
import { onMounted, ref } from 'vue'
import axios from 'axios'
import CardList from '@/components/CardList.vue'

const favourites = ref([])

onMounted(async () => {
  try {
    const { data } = await axios.get(
      `https://31a86048d2f90b55.mokky.dev/favourites?_relations=items`
    )
    favourites.value = data.map((obj) => obj.item)
  } catch (error) {
    console.log(error)
  }
})
</script>

<template>
    <h2 class="text-3xl font-bold mb-8">Мои закладки</h2>
  <CardList :items="favourites" is-favourites/>
</template>
