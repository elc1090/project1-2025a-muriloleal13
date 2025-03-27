<template>
  <div
    class="relative overflow-hidden bg-cover bg-center"
    :style="{ backgroundImage: `url('${data.image}')` }"
    @mouseenter="isHoverInfo = true"
    @mouseleave="isHoverInfo = false"
  >
    <div
      class="absolute bottom-0 left-0 right-0 bg-gradient-to-t from-blue-800 to-transparent z-5"
      :style="{ height: isMainCard ? '300px' : '200px' }"
    ></div>
    <div class="absolute bottom-0 left-0 right-0 z-10 flex flex-col gap-3 p-6">
      <div v-if="data.steam">
        <q-chip color="info" text-color="white" icon="bi-steam" label="Steam" />
      </div>
      <span class="font-bold" :class="`text-${isMainCard ? 'h4' : 'h5'}`">
        {{ data.title }}
      </span>
      <span class="text-h6" v-if="isMainCard">{{ data.description }}</span>
      <CardInfo
        :data="data"
        :isMainCard="isMainCard"
        :isHoverInfo="isHoverInfo"
      />
    </div>
  </div>
</template>
<script setup>
import { ref } from "vue";
import CardInfo from "./CardInfo.vue";

defineProps({
  data: Object,
  isMainCard: {
    type: Boolean,
    default: false,
  },
});

const isHoverInfo = ref(false);
</script>
