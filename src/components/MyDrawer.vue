<script setup>
import { ref, computed, inject } from 'vue'
import axios from 'axios'
import DrawerHead from './DrawerHead.vue'
import CartItemList from './CartItemList.vue'
import InfoBlock from './InfoBlock.vue'

const props = defineProps({
  totalPrice: Number,
  vatPrice: Number,
  fullPrice: Number
})
const { cart } = inject('cart')

const orderId = ref(null)

const isCreating = ref(false)
const createOrder = async () => {
  try {
    isCreating.value = true
    const { data } = await axios.post('https://6d9651a0a25158f4.mokky.dev/orders', {
      items: cart.value,
      totalPrice: props.totalPrice.value
    })
    cart.value = []

    orderId.value = data.id
  } catch (err) {
    console.log(err)
  } finally {
    isCreating.value = false
  }
}
const cartIsEmpty = computed(() => cart.value.length === 0)
const buttonDisabled = computed(() => isCreating.value || cartIsEmpty.value)
</script>

<template>
  <div class="fixed top-0 left-0 h-full w-full bg-black z-10 opacity-70"></div>
  <div class="bg-orange-50 w-96 h-full fixed right-0 top-0 z-20 p-8">
    <DrawerHead />

    <div v-if="!totalPrice || orderId" class="flex h-full items-center">
      <infoBlock
        v-if="!totalPrice && !orderId"
        title="KORZINA PUSTA"
        description="DOBAV CHTO NIT"
        imageUrl="/package-icon.png"
      />
      <infoBlock
        v-if="orderId"
        title="POKUPKA OFORMLENA"
        :description="`SKIINTE DENIFI NA MY NOMER I NASLAJDATES POKUPKA (NOMER ZAKAZA №${orderId})`"
        imageUrl="/PilotP.png"
      />
    </div>
    <CartItemList v-if="totalPrice" />

    <div v-if="totalPrice" class="flex flex-col gap-4 mb-6 mt-7">
      <div class="flex gap-2">
        <span>ITOGO ZA KROSSOVKA:</span>
        <div class="flex-1 border-b border-dashed"></div>
        <b>{{ totalPrice }} рубликов</b>
      </div>

      <div class="flex gap-2">
        <span>KOMISSIYA 80%:</span>
        <div class="flex-1 border-b border-dashed"></div>
        <b>{{ vatPrice }} рубликов</b>
      </div>

      <div class="flex gap-2">
        <span>ITOGO:</span>
        <div class="flex-1 border-b border-dashed"></div>
        <b>{{ fullPrice }} рубликов</b>
      </div>

      <button
        :disabled="buttonDisabled"
        @click="createOrder"
        class="mt-4 bg-lime-500 w-full rounded-2xl py-4 text-white disabled:bg-slate-400 hover:bg-lime-600 cursor-pointer trasition active:bg-lime-700"
      >
        OFORMIT POKUPKU
      </button>
    </div>
  </div>
</template>
