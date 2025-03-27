<template>
  <q-card
    class="bg-gradient-to-r from-accent to-accent-2 text-white rounded-xl hover:border-4 hover:border-blue-800 hover:shadow-lg hover:shadow-blue-800 transition-transform flex flex-col h-full"
    @mouseenter="isHoverInfo = true"
    @mouseleave="isHoverInfo = false"
  >
    <div class="flex items-center justify-center gap-3 w-full pt-2">
      <q-icon name="bi-steam" />
      <span class="text-white font-bold">Steam</span>
    </div>
    <img :src="data.image" :alt="data.title" class="w-full h-40 object-cover" />
    <q-card-section class="flex flex-col gap-3">
      <span class="text-lg font-bold truncate text-wrap">{{ data.title }}</span>
      <div class="flex gap-1 w-full">
        <q-chip
          square
          :label="item"
          class="bg-primary text-white font-bold rounded-lg"
          v-for="item in data.platform"
          :key="item"
        />
      </div>
      <div class="flex flex-nowrap items-center gap-3">
        <q-icon color="white" name="bi-clock-fill" />
        <span>
          {{ data.time ? data.time : "Tempo limitado" }}
        </span>
      </div>
    </q-card-section>
    <q-card-section class="flex flex-nowrap items-center gap-3">
      <CardInfo
        :data="data"
        :isMainCard="false"
        :isHoverInfo="isHoverInfo"
        :isGrid="true"
      />
    </q-card-section>
  </q-card>
</template>
<script setup>
defineProps({ data: Object });

import { ref } from "vue";
import CardInfo from "./CardInfo.vue";

const isHoverInfo = ref(false);
</script>
