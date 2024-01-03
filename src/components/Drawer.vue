<script setup>
import DrawerHead from './DrawerHead.vue'
import CartListItem from './CartListItem.vue'
import InfoBlock from './InfoBlock.vue'

defineProps({
    totalPrice: Number,
    vatPrice: Number,
    disabledBtn: Boolean
});

const emit = defineEmits(["createOrder"])

</script>


<template>
    <div class="fixed top-0 left-0 h-full w-full bg-black z-10 opacity-70"></div>
    <div class="bg-white w-96 h-full fixed right-0 top-0 z-20 p-8">
        <DrawerHead />

        <div v-if="!totalPrice" class="flex h-full items-center">
            <InfoBlock 
                title="Empty cart" 
                description="Add atleast one item to make an order" 
                imageUrl="/package-icon.png"
            />
        </div>
        

        <CartListItem />

        <div v-if="totalPrice" class="flex flex-col gap-4 mt-7">

            <div class="flex gap-2">
                <span>Last cost:</span>
                <div class="flex-1 border-b border-dashed"></div>
                <b>{{ totalPrice }} $</b>
            </div>

            <div class="flex gap-2">
                <span>Tax 5%:</span>
                <div class="flex-1 border-b border-dashed"></div>
                <b>{{ vatPrice }} $</b>
            </div>

            <button
                @click="emit('createOrder')"
                :disabled="disabledBtn"
                class="mt-4 bg-lime-500 w-full rounded-xl py-3 text-white disabled:bg-slate-400 cursor-pointer hover:bg-lime-600 transition active:bg-lime-700">
                Make an order
            </button>
        </div>

    </div>
</template>