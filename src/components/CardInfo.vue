<template>
  <div class="flex flex-nowrap items-center gap-5">
    <div
      class="bg-gradient-to-r from-[#FE2771] to-[#F36A0E] px-3 py-1 rounded-lg text-white"
      v-if="data.discount"
    >
      <span class="font-bold" :class="`text-${isMainCard ? 'xl' : 'md'}`">
        -{{ data.discount }}%
      </span>
    </div>
    <div class="flex flex-col gap-1">
      <span v-if="data.oldPrice" class="line-through">
        R$ {{ data.oldPrice }}
      </span>
      <span class="font-bold" :class="`text-${isMainCard ? '3xl' : 'lg'}`">
        R$ {{ data.price }}
      </span>
    </div>
    <q-btn
      class="font-bold text-lg"
      icon="bi-cart-plus"
      label="Comprar"
      color="positive"
      text-color="black"
      push
      no-caps
      glossy
      v-if="isMainCard"
    />
    <q-btn
      icon="bi-heart"
      color="white"
      text-color="black"
      padding="sm"
      push
      no-caps
      v-if="isMainCard"
    />
  </div>
</template>
<script setup>
defineProps({
  data: Object,
  isMainCard: {
    type: Boolean,
    default: true,
  },
});
</script>
