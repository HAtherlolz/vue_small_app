<script setup>
import { Carousel, Navigation, Pagination, Slide } from 'vue3-carousel'

import 'vue3-carousel/dist/carousel.css'

defineProps({
    items: Array
});

</script>

<template>
    <Carousel :autoplay="5000" :transition="700">
    <Slide v-for="item in items" :key="item.id">
      <div class="carousel__item">
        <img :src="item.imageUrl" :alt="item.title">
      </div>
    </Slide>

    <template #addons>
      <Navigation />
      <Pagination />
    </template>
  </Carousel>
</template>