<template>
  <div
    class="relative flex flex-nowrap items-center gap-3"
    :class="{ 'w-full': isGrid }"
  >
    <div
      class="bg-gradient-to-r from-[#FE2771] to-[#F36A0E] px-3 py-1 rounded-lg text-white"
      v-if="data.discount"
    >
      <span class="font-bold" :class="`text-${isMainCard ? 'xl' : 'md'}`">
        -{{ data.discount }}%
      </span>
    </div>
    <div
      class="flex flex-col gap-1 text-nowrap"
      v-if="isMainCard || !isHoverInfo"
    >
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
      no-wrap
      push
      no-caps
      glossy
      v-if="isMainCard"
    />
    <q-btn
      class="font-bold text-lg opacity-0 transition-opacity duration-300 ease-in-out"
      :class="{ grow: isGrid, 'opacity-100': isHoverInfo }"
      icon="bi-cart-plus"
      :label="`R$${data.price}`"
      color="positive"
      text-color="black"
      push
      padding="xs"
      no-caps
      glossy
      v-show="isHoverInfo"
    />
    <q-btn
      :class="{
        'absolute top-0 right-0 -translate-y-[230px]': isGrid,
      }"
      icon="bi-heart"
      color="white"
      text-color="black"
      padding="sm"
      push
      no-caps
      v-if="isMainCard || isHoverInfo"
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
  isHoverInfo: Boolean,
  isGrid: {
    type: Boolean,
    default: false,
  },
});
</script>
