<script setup>
import axios from "axios";
import { onMounted, ref } from "vue";

import CardList from '../components/CardList.vue'

const favorites = ref([]);

const getFavorites = async () => {
    try {
        const { data } = await axios.get("https://604781a0efa572c1.mokky.dev/favorites?_relations=items");
        favorites.value = data.map((obj) => obj.item)
    } catch (err) {
        console.log(err);
    }
}

onMounted(getFavorites);
</script>

<template>
   <h2 class="text-3xl font-bold mb-8">Мои закладки</h2>

    <CardList :items="favorites" isFavorite/>
</template>