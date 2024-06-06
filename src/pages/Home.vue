<script setup>
import axios from 'axios'
import { reactive, inject, watch, ref, onMounted } from 'vue'
import debounce from 'lodash.debounce'
import CardList from '../components/CardList.vue'

const { cart, addToCart, removeFromCart } = inject('cart')

const items = ref([])

const filters = reactive({
  sortby: 'title',
  searchQuery: ''
})

const onClickAddPlus = (item) => {
  if (!item.isAdded) {
    addToCart(item)
  } else {
    removeFromCart(item)
  }
}

const onChangeSelect = (event) => {
  filters.sortBy = event.target.value
}

const onChangeSearchInput = debounce((event) => {
  filters.searchQuery = event.target.value
}, 500)

const addToFavourite = async (item) => {
  try {
    if (!item.isFavourite) {
      const obj = {
        item_id: item.id
      }

      item.isFavourite = true

      const { data } = await axios.post(`https://31a86048d2f90b55.mokky.dev/favourites`, obj)
      item.favouriteId = data.id
    } else {
      item.isFavourite = false

      await axios.delete(`https://31a86048d2f90b55.mokky.dev/favourites/${item.favouriteId}`)
      item.favouriteId = null
    }
  } catch (error) {
    console.log(error)
  }
}

const fetchFavourites = async () => {
  try {
    const { data: favourites } = await axios.get(`https://31a86048d2f90b55.mokky.dev/favourites`)
    items.value = items.value.map((item) => {
      const favourite = favourites.find((favourite) => favourite.item_id === item.id)

      if (!favourite) {
        return item
      }

      return {
        ...item,
        isFavourite: true,
        favouriteId: favourite.id
      }
    })
  } catch (error) {
    console.log(error)
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

    const { data } = await axios.get(`https://31a86048d2f90b55.mokky.dev/items`, {
      params
    })
    items.value = data.map((obj) => ({
      ...obj,
      isFavourite: false,
      favouriteId: null,
      isAdded: false
    }))
  } catch (error) {
    console.log(error)
  }
}

onMounted(async () => {
  const localCart = localStorage.getItem('cart')
  cart.value = localCart ? JSON.parse(localCart) : []

  await fetchItems()
  await fetchFavourites()

  items.value = items.value.map((item) => ({
    ...item,
    isAdded: cart.value.some((cartItem) => cartItem.id === item.id)
  }))
})

watch(cart, () => {
  items.value = items.value.map((item) => ({
    ...item,
    isAdded: false
  }))
})

watch(filters, fetchItems)
</script>

<template>
  <img src="/sneakers-shop.jpg" alt="Магазин кроссовок" class="w-full mb-10 rounded-[30px]">
  <div class="flex flex-row flex-wrap justify-center lg:justify-between items-center gap-10">
    <h2 class="text-3xl font-bold">Все кроссовки</h2>
    <div class="flex flex-row flex-wrap gap-4 justify-center">
      <select @change="onChangeSelect" class="py-2 px-3 border rounded-md outline-none w-[200px]">
        <option value="name">По названию</option>
        <option value="price">По цене (дешевые)</option>
        <option value="-price">По цене (дорогие)</option>
      </select>
      <div class="relative ">
        <img class="absolute left-3 top-3" src="/search.svg" alt="Поиск" />
        <input
          @input="onChangeSearchInput"
          class="border rounded-md py-2 pl-8 pr-4 outline-none focus:border-gray-400 w-[200px]"
          type="search"
          placeholder="Поиск..."
        />
      </div>
    </div>
  </div>
  <div class="mt-10">
    <CardList :items="items" @add-to-favourite="addToFavourite" @add-to-cart="onClickAddPlus" />
  </div>
</template>
