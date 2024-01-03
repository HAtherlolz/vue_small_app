<script setup>
// import { inject } from 'vue' # It could be used, why not
import Card from './Card.vue'

defineProps({
    items: Array,
})

const emit = defineEmits(["addToFavorite", "addToCart"])

// const addToFavorite = inject("addFavorite");  # It could be used instead of emits but it suits for deeper childrend components

</script>


<template>
    <div class="grid grid-cols-4 gap-5" v-auto-animate>
        <Card 
            v-for="item in items"
            :key="item.id"
            :id="item.id"
            :title="item.title" 
            :imageUrl="item.imageUrl" 
            :price="item.price"
            :isAdded="item.isAdded"
            :isFavorite="item.isFavorite"
            :onClickAdd="() => emit('addToCart', item)"
            :onClickIsFavorite="() => emit('addToFavorite', item)"
        />
      </div>
</template>