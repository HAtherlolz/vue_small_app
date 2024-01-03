<script setup>
const emit = defineEmits(["openCart"])
defineProps({
   totalPrice: Number,
})
</script>

<template>
   <header class="flex justify-between border-b border-slate-200 px-10 py-8">

      <router-link to="/">
         <div class="flex items-center gap-4">
            <img src="/logo.png" alt="Logo" class="w-10"/>
            <div>
               <h2 class="text-xl font-bold uppercase">Niko Sneakers</h2>
               <p class="text-slate-400">The store of the best sneakers</p>
            </div>
         </div>
      </router-link>

      

      <ul class="flex items-center gap-10">
      <li @click="() => emit('openCart')" class="flex items-center gap-3 cursor-pointer text-slate-500 hover:text-black">
         <img src="/cart.svg" alt="Cart" />
         <span>{{ totalPrice }} usd</span>
      </li>

      <router-link to="/favorites">
         <li class="flex items-center gap-3 cursor-pointer text-slate-500 hover:text-black"> 
            <img src="/heart.svg" alt="Cart" />
            <span>Bookmarks</span>
         </li>
      </router-link>
      

      <li class="flex items-center gap-3 cursor-pointer text-slate-500 hover:text-black">
         <img src="/profile.svg" alt="Cart" />
         <span>Profile</span>
      </li>
      </ul>

   </header>
</template>