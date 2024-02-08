<script setup>
import { ref, computed, inject } from 'vue'
import axios from 'axios'
import CartItemList from './CartList.vue'
import ShoppingCartHead from './ShoppingCartHead.vue'
import InfoBlock from './InfoBlock.vue'

const API_KEY = import.meta.env.VITE_MOKKY_API_KEY

const props = defineProps({
  shoppingCartOpen: Boolean,
  totalPrice: Number,
  vatPrice: Number
})

const { cartItems } = inject('cart')

const isCreating = ref(false)
const orderId = ref(null)

const createOrder = async () => {
  try {
    isCreating.value = true
    const { data } = await axios.post(`${API_KEY}/orders`, {
      items: cartItems.value,
      totalPrice: props.totalPrice.value
    })
    clearCart()
    orderId.value = data.id
  } catch (err) {
    console.log(err)
  } finally {
    isCreating.value = false
  }
}
const clearCart = () => {
  cartItems.value.map((item) => (item.isAdded = false))
  cartItems.value = []
}
const cartIsEmpty = computed(() => cartItems.value.length === 0)
const buttonDisabled = computed(() => isCreating.value || cartIsEmpty.value)
</script>
<template>
  <Transition name="fade">
    <div
      v-if="shoppingCartOpen"
      class="fixed top-0 left-0 h-full w-full bg-black z-10 opacity-70"
    ></div>
  </Transition>
  <Transition name="slide">
    <div v-show="shoppingCartOpen" class="bg-white w-96 h-full fixed right-0 top-0 z-20 p-8">
      <div v-if="!totalPrice || orderId" class="flex h-full items-center">
        <InfoBlock
          v-if="!totalPrice && !orderId"
          title="Cart is empty"
          description="Add something to your cart to make a purchase "
          image-url="/package-icon.png"
        />
        <InfoBlock
          v-if="orderId"
          title="Order is processed!"
          :description="`Your order #${orderId} will be transferred to courier delivery soon`"
          image-url="/order-success-icon.png"
        />
      </div>
      <div v-else>
        <ShoppingCartHead />
        <CartItemList />
        <div class="flex flex-col gap-4 mt-7">
          <div class="flex gap-2">
            <span>Total: </span>
            <div class="flex-1 border-b border-dashed"></div>
            <b>{{ totalPrice }} $</b>
          </div>
          <div class="flex gap-2">
            <span>Vat 5%:</span>
            <div class="flex-1 border-b border-dashed"></div>
            <b> {{ vatPrice }} $</b>
          </div>
          <button
            :disabled="buttonDisabled"
            @click="createOrder"
            class="mt-4 transition bg-lime-500 w-full rounded-xl py-3 text-white disabled:bg-slate-300 hover:bg-lime-600 active:bg-lime-700 cursor-pointer"
          >
            Checkout
          </button>
          <button
            :disabled="buttonDisabled"
            @click="clearCart"
            class="mt-4 transition bg-red-500 w-full rounded-xl py-3 text-white disabled:bg-slate-300 hover:bg-red-700 active:bg-lime-700 cursor-pointer"
          >
            Clear Cart
          </button>
        </div>
      </div>
    </div>
  </Transition>
</template>
<style scoped>
.slide-enter-active,
.slide-leave-active {
  transition: transform 0.5s ease;
}

.slide-enter-from {
  transform: translateX(50%);
}
.slide-leave-to {
  transform: translateX(100%);
}

.fade-enter-active,
.fade-leave-active {
  transition: opacity 0.5s ease;
}

.fade-enter-from,
.fade-leave-to {
  opacity: 0;
}
</style>
