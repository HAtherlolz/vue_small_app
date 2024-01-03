<script setup>
defineProps({
    cartItem: Object
});

const emit = defineEmits(["onClickRemove"])
</script>


<template>
   <div class="flex items-center border border-slate-100 p-4 rounded-xl gap-4"> 
        <img class="w-16 h-16" :src="cartItem.imageUrl" :alt="cartItem.title" />

        <div class="flex flex-col flex-1">
            <p>{{ cartItem.title }}</p>

            <div class="flex justify-between mt-2">
                <b class="flex-1">{{ cartItem.price }} $</b>
                <img @click="emit('onClickRemove')" class="opacity-40 hover:opacity-100 cursor-pointer transition" src="/close.svg" alt="">
            </div>
        </div>
   </div>
</template>