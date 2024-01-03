<script setup>
import CartItem from './CartItem.vue';
import { inject } from "vue";

const cart = inject("cart")

</script>

<template>
    <div class="flex flex-col gap-4" v-auto-animate>
        <CartItem v-for="c in cart.cart.value" :key="c.id" :cartItem="c" @onClickRemove="() => cart.addToCart(c)"/>
    </div>
   
</template>