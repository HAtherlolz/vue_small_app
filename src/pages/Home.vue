<script setup>
import CardList from '../components/CardList.vue';
import Slider from '../components/Slider.vue';
import axios from 'axios';
import { inject, reactive, watch, ref, onMounted } from 'vue';

const { cart, addToCart } = inject("cart")

const items = ref([]);

const filters = reactive({
  sortBy: "title",
  searchQ: "",
})

const onChangeSelect = (event) => {
  filters.sortBy = event.target.value
}

const onChangeSearchInput = (event) => {
  filters.searchQ = event.target.value
}

const addFavorite = async (item) => {
  // There is should be a put/patch request

  try {
    if (!item.isFavorite) {
      const obj = {
        parentId: item.id
      }

      const { data } = await axios.post(`https://604781a0efa572c1.mokky.dev/favorites`, obj)

      item.isFavorite = true;
      item.favoriteId = data.id;
    } else {
      await axios.delete(`https://604781a0efa572c1.mokky.dev/favorites/${item.favoriteId}`)
      item.isFavorite = false;
      item.favoriteId = null;
    }
  } catch (err) {
    console.log(err)
  }
}

const getFilterOfCorrectItems = (data) => {
  data = data.filter(item => item.imageUrl !== null && item.imageUrl !== undefined);
  return data
}

const getItems = async () => {

  const params = {
    sortBy: filters.sortBy,
  }

  if (filters.searchQ) {
    params.title = `*${filters.searchQ}*`;
  }

  try {
    const { data } = await axios.get(`https://604781a0efa572c1.mokky.dev/items`, {
      params
    })
    // Filtering wrong items, without images (because of bad API)
    items.value = getFilterOfCorrectItems(data);

  } catch (err) {
    console.log(err)
  }
}

const getFavorites = async () => {
  try {
    const { data: favorites } = await axios.get(`https://604781a0efa572c1.mokky.dev/favorites`)

    items.value = items.value.map(item => {
      const favorite = favorites.find(favorite => favorite.parentId === item.id)

      if (!favorite) {
        return item;
      }

      return {
        ...item,
        isFavorite: true,
        favoriteId: favorite.id,
      }
    });
  } catch (err) {
    console.log(err)
  }
}

onMounted(async () => {
  await getItems();
  await getFavorites();
});

watch(filters, getItems);
watch(cart, () => {
  items.value = items.value.map((item) => ({
    ...item,
    isAdded: false
  }))
});

</script>

<template>

  <div class="m-5 mb-10">
    <Slider :items="items"/>
  </div>

    <div class="flex justify-between items-center">
        <h2 class="text-3xl font-bold mb-8">The sneakers</h2>

        <div class="flex gap-4">
          <select @change="onChangeSelect" class="py-2 px-3 border rounded-md outline-none">
            <option value="title">By name</option>
            <option value="-price">From reach</option>
            <option value="price">From pour</option>
          </select>
          
          <div class="relative">
            <img class="absolute left-4 top-3" src="/search.svg" alt="search">
            <input @input="onChangeSearchInput" class="border rounded-md py-2 pl-11 pr-4 outline-none focus:border-gray-400" placeholder="Search...">
          </div>
        </div>
      </div>

      <div class="mt-10">
        <CardList :items="items" @addToFavorite="addFavorite" @addToCart="addToCart"/>
      </div>
</template>