<script setup>
import { computed, provide, ref, watch } from 'vue'

import MyHeader from './components/MyHeader.vue'

import MyDrawer from './components/MyDrawer.vue'

const cart = ref([]) 

const drawerOpen = ref(false)

const totalPrice = computed(() => cart.value.reduce((acc, item) => acc + item.price, 0))
const vatPrice = computed(() => Math.round((totalPrice.value * 80) / 100))
const fullPrice = computed(() => totalPrice.value + vatPrice.value)

const closeDrawer = () => {
  drawerOpen.value = false
}

const openDrawer = () => {
  drawerOpen.value = true
}

const addToCart = (item) => {
  cart.value.push(item)
  item.isAdded = true
}

const removeFromCart = (item) => {
  cart.value.splice(cart.value.indexOf(item), 1)
  item.isAdded = false
}

watch(
  cart,
  () => {
    localStorage.setItem('cart', JSON.stringify(cart.value))
  },
  { deep: true }
)

provide('cart', {
  cart,
  closeDrawer,
  openDrawer,
  addToCart,
  removeFromCart
})
</script>

<template>
  <MyDrawer
    v-if="drawerOpen"
    :total-price="totalPrice"
    :vat-price="vatPrice"
    :full-price="fullPrice"
    :cart-button-disabled="cartButtonDisabled"
  />

  <div class="bg-orange-50 w-4/5 m-auto rounded-xl shadow-xl mt-14">
    <MyHeader :total-price="totalPrice" @open-drawer="openDrawer" />
    <div class="p-7 mt-2">
      <router-view></router-view>
    </div>
  </div>
</template>
