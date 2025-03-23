<template>
  <div
    class="relative overflow-hidden bg-cover bg-center"
    :style="{ backgroundImage: `url('${data.image}')` }"
  >
    <div
      class="absolute bottom-0 left-0 right-0 bg-gradient-to-t from-blue-800 to-transparent z-10"
      :style="{ height: isMainCard ? '300px' : '200px' }"
    ></div>
    <div class="absolute bottom-0 left-0 right-0 z-20 flex flex-col gap-3 p-6">
      <div v-if="data.steam">
        <q-chip color="info" text-color="white" icon="bi-steam" label="Steam" />
      </div>
      <span class="font-bold" :class="`text-${isMainCard ? 'h4' : 'h5'}`">
        {{ data.title }}
      </span>
      <span class="text-h6" v-if="isMainCard">{{ data.description }}</span>
      <div class="flex flex-nowrap items-center gap-5">
        <div
          class="bg-gradient-to-r from-[#FE2771] to-[#F36A0E] px-3 py-1 rounded-lg text-white"
          v-if="data.discount"
        >
          <span class="font-bold" :class="`text-${isMainCard ? 'xl' : 'md'}`">
            -{{ data.discount }}%
          </span>
        </div>
        <span class="font-bold" :class="`text-${isMainCard ? '3xl' : 'lg'}`">
          R$ {{ data.price }}
        </span>
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
    </div>
  </div>
</template>
<script setup>
defineProps({
  data: Object,
  isMainCard: {
    type: Boolean,
    default: false,
  },
});
</script>
