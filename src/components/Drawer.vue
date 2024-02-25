<script setup>
import DrawerHead from './DrawerHead.vue'
import CartItemList from './CartItemList.vue'
</script>

<template>
  <div class="fixed top-0 left-0 h-full w-full bg-black z-10 opacity-70"></div>
  <div class="bg-white w-96 h-full fixed right-0 top-0 z-20 p-10">
    <DrawerHead />

    <CartItemList />

    <div class="flex flex-col gap-5 mb-6 mt">
      <div class="flex gap-2">
        <span>Total:</span>
        <div class="flex-1 border-b border-dashed"></div>
        <b>1205 usd</b>
      </div>

      <div class="flex gap-2">
        <span>Taxes:</span>
        <div class="flex-1 border-b border-dashed"></div>
        <b>10%</b>
      </div>

      <button
        disabled=""
        class="mt-4 bg-lime-500 rounded-xl border w-full py-4 text-white disabled:bg-slate-300 hover:bg-lime-600 active:bg-lime-700 cursor-pointer"
      >
        Make a Purachase
      </button>
    </div>
  </div>
</template>
