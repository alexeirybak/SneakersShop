<script setup>
import DrawerHead from './DrawerHead.vue'
import CartItemList from './CartItemList.vue'

defineProps({
  totalPrice: Number,
  vatPrice: Number
})
</script>

<template>
  <div class="fixed top-0 left-0 h-full w-full bg-black z-10 opacity-70"></div>
  <div class="bg-white w-96 h-full fixed right-0 top-0 z-20 p-8 flex flex-col justify-between">
    <DrawerHead />
    <CartItemList />
    <div class="flex flex-col gap-4 mt-7">
      <div class="flex gap-2">
        <span>Итого</span>
        <div class="flex-1 border-b border-dashed"></div>
        <b>{{ totalPrice }} руб.</b>
      </div>
      <div class="flex gap-2">
        <span>Скидка 5%</span>
        <div class="flex-1 border-b border-dashed"></div>
        <b>{{ vatPrice }} руб.</b>
      </div>
    </div>
    <button
      disabled=""
      class="mt-7 bg-lime-500 w-full rounded-xl py-3 text-white hover:bg-lime-600 active:bg-lime-700 transition disabled:bg-slate-300 cursor-pointer"
    >
      Оформить заказ
    </button>
  </div>
</template>