<script setup>
import { computed, provide, ref } from 'vue';
import axios from 'axios';

import Header from './components/Header.vue';

import Drawer from './components/Drawer.vue';

const cart = ref([]);
const isCreatingOrder = ref(false);
const isCartOpen = ref(false);
const totalPrice = computed(
  () => cart.value.reduce((acc, item) => acc + item.price, 0)
);
const vatPrice = computed(
  () => Math.round(totalPrice.value * 0.05)
);

const cartIsEmpty = computed(
  () => cart.value.length === 0
);

const disabledCartBtn = computed(
    () => isCreatingOrder.value || cartIsEmpty.value
);

const closeCart = () => {
  isCartOpen.value = false
}

const openCart = () => {
  isCartOpen.value = true
}

const addToCart = (item) => {
  if (!item.isAdded) {
    cart.value.push(item)
    item.isAdded = true
  } else {
    cart.value.splice(cart.value.indexOf(item), 1)
    item.isAdded = false
  }
  console.log(cart.value)
}


const createOrder = async () => {
  try {
    isCreatingOrder.value = true;
    const { data } = await axios.post("https://604781a0efa572c1.mokky.dev/orders", {
      items: cart.value,
      totalPrice: totalPrice.value
    })

    cart.value = []

    return data;
  } catch (err) {
    console.log(err)
  } finally {
    isCreatingOrder.value = false;
    cart.value = []
  }
}

provide("cart", {
  cart,
  closeCart,
  openCart,
  addToCart,
});  

</script>

<template>
  <Drawer v-if="isCartOpen" :totalPrice="totalPrice" :vatPrice="vatPrice" @createOrder="createOrder" :disabledBtn="disabledCartBtn"/>
  <div class="bg-white w-4/5 m-auto rounded-xl shadow-xl mt-14">
    <Header :totalPrice="totalPrice" @openCart="openCart" />

    <div class="p-10">
      <router-view></router-view>
    </div>
    
  </div>

</template>

<style scoped>

</style>
