<script setup>
defineProps({
  totalPrice: Number
})

const emit = defineEmits(['openDrawer'])
</script>

<template>
  <header class="flex justify-between border-b border-slate-200 px-10 py-8">
    <router-link to="/">
      <div class="flex items-center gap-4">
        <img src="/logo.png" alt="logo" class="w-10 mr-5" />
        <div>
          <h2 class="text-xl font-bold uppercase">JESTKI SHOP</h2>
          <p class="text-slate-400">SHOP TOP SHMOT</p>
        </div>
      </div></router-link
    >

    <ul class="flex items-center gap-10">
      <li
        @click="() => emit('openDrawer')"
        class="flex items-center gap-3 cursor-pointer text-gray-400 hover:text-black"
      >
        <img src="/cart.svg" alt="cart" />
        <b>{{ totalPrice }} рубликов</b>
      </li>

      <router-link to="/favorites">
        <li class="flex items-center gap-3 cursor-pointer text-gray-400 hover:text-black">
          <img src="/heart.svg" alt="cart" />
          <span>Закладки</span>
        </li>
      </router-link>

      <router-link to="/profile">
        <li class="flex items-center gap-3 cursor-pointer text-gray-400 hover:text-black">
          <img src="/profile.svg" alt="cart" />
          <span>Профиль</span>
        </li>
      </router-link>
    </ul>
  </header>
</template>
