<script setup>
import { ref, computed, inject } from 'vue';
import axios from 'axios';
import DrawerHead from './DrawerHead.vue';
import CartItemList from './CartItemList.vue';
import InfoBlock from './InfoBlock.vue';

const props = defineProps({
  totalPrice: Number,
  vatPrice: Number,
});

const { cart, closeDrawer } = inject('cart');

const isCreatingOrder = ref(false);
const orderId = ref(null);

const createOrder = async () => {
  try {
    isCreatingOrder.value = true;
    const { data } = await axios.post(`https://677dff27d54d1aeb.mokky.dev/orders`, {
      items: cart.value,
      totalPrice: props.totalPrice.value,
    });

    cart.value = [];

    orderId.value = data.id;
  } catch (err) {
    console.log(err);
  } finally {
    isCreatingOrder.value = false;
  }
};

const cartIsEmpty = computed(() => cart.value.length === 0);
const buttonDisabled = computed(() => isCreatingOrder.value || cartIsEmpty.value);
</script>

<template>
  <div class="fixed top-0 left-0 w-full h-full bg-black z-10 opacity-70"></div>
  <div class="bg-white w-96 h-full fixed right-0 top-0 z-20 p-8">
    <DrawerHead />

    <div v-if="!totalPrice || orderId" class="flex h-full items-center">
      <InfoBlock
        v-if="orderId"
        title="Заказ оформлен"
        :description="`Ваш заказ №${orderId} скоро будет передан курьерской службе`"
        imageUrl="/order-success-icon.png"
      />
      <InfoBlock
        v-if="!totalPrice && !orderId"
        title="Корзина пуста"
        description="Добавьте хотя бы одну пару кроссовок, чтобы сделать заказ"
        imageUrl="/package-icon.png"
      />
    </div>

    <div v-else>
      <CartItemList />

      <div class="flex flex-col gap-4 mt-7">
        <div class="flex gap-2">
          <span>Итого:</span>
          <div class="flex-1 border-b border-dashed"></div>
          <b>{{ totalPrice }} usd</b>
        </div>

        <div class="flex gap-2">
          <span>Налог 5%</span>
          <div class="flex-1 border-b border-dashed"></div>
          <b>{{ vatPrice }} usd</b>
        </div>
        <button
          @click="createOrder"
          :disabled="buttonDisabled"
          class="mt-4 transition hover:bg-lime-600 bg-lime-500 w-full rounded-xl py-3 text-white active:bg-lime-700 disabled:bg-slate-300 cursor-pointer"
        >
          Оформить заказ
        </button>
      </div>
    </div>
  </div>
</template>
