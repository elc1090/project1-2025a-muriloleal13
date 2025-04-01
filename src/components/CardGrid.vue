<template>
  <q-card
    class="bg-gradient-to-r from-accent to-accent-2 text-white rounded-3xl hover:border-4 hover:border-blue-800 hover:shadow-lg hover:shadow-blue-800 transition-transform flex flex-col h-full"
    @mouseenter="isHoverInfo = true"
    @mouseleave="isHoverInfo = false"
    v-intersection="onIntersect"
  >
    <div class="flex items-center justify-center gap-3 w-full pt-2 pb-1">
      <q-icon name="bi-steam" />
      <span class="text-white font-bold">Steam</span>
    </div>
    <q-skeleton
      v-if="!loaded"
      class="w-full"
      :height="$q.screen.gt.md ? '160px' : '80px'"
      animation="wave"
      square
    />
    <img
      v-if="shouldLoad && !loaded"
      :src="data.image"
      :alt="data.title"
      loading="lazy"
      @load="handleLoad"
      class="absolute w-0 h-0 opacity-0"
    />
    <img
      v-if="loaded"
      :src="data.image"
      :alt="data.title"
      class="w-full object-cover transition-opacity duration-1000"
      :class="$q.screen.gt.md ? 'h-40' : 'h-20'"
    />
    <q-card-section class="grow flex flex-col gap-3">
      <template v-if="!loaded">
        <q-skeleton type="text" width="80%" height="1.2rem" />
      </template>
      <template v-else>
        <span class="text-lg font-bold truncate text-wrap">{{
          data.title
        }}</span>
      </template>

      <template v-if="!loaded">
        <div class="flex gap-2">
          <q-skeleton v-for="n in 3" :key="n" width="60px" height="24px" />
        </div>
      </template>
      <template v-else>
        <div class="flex gap-1 w-full">
          <q-chip
            v-for="item in data.platform"
            :key="item"
            square
            :label="item"
            class="bg-primary text-white font-bold rounded-lg"
          />
        </div>
      </template>
    </q-card-section>

    <q-card-section class="flex flex-col gap-3">
      <template v-if="!loaded">
        <div class="flex items-center gap-3">
          <q-skeleton type="circle" size="16px" />
          <q-skeleton type="text" width="100px" />
        </div>
      </template>
      <template v-else>
        <div class="flex flex-nowrap items-center gap-3">
          <q-icon color="white" name="bi-clock-fill" />
          <span>
            {{ remainingTime || "Tempo limitado" }}
          </span>
        </div>
      </template>
      <div class="flex flex-nowrap items-center gap-3">
        <template v-if="!loaded">
          <div class="w-full flex gap-3">
            <q-skeleton width="40%" height="1rem" />
            <q-skeleton width="30%" height="1rem" />
          </div>
        </template>
        <template v-else>
          <CardInfo
            :data="data"
            :isMainCard="false"
            :isHoverInfo="isHoverInfo"
            :isGrid="true"
          />
        </template>
      </div>
    </q-card-section>
  </q-card>
</template>
<script setup>
const props = defineProps({ data: Object });

import { onBeforeUnmount, onMounted, ref } from "vue";
import CardInfo from "./CardInfo.vue";

const isHoverInfo = ref(false);
const shouldLoad = ref(false);
const loaded = ref(false);
const remainingTime = ref("");
const timer = ref(null);

const onIntersect = (entry) => {
  if (entry.isIntersecting && !shouldLoad.value) {
    shouldLoad.value = true;
  }
};

const handleLoad = () => {
  setTimeout(() => {
    loaded.value = true;
    startCountdown();
  }, 2000);
};

const parseCustomTimeFormatToSeconds = (timeStr) => {
  let days = 0,
    hours = 0,
    minutes = 0,
    seconds = 0;

  const dayMatch = timeStr.match(/(\d+)\s*d/);
  const hourMatch = timeStr.match(/(\d+)\s*h/);
  const minMatch = timeStr.match(/(\d+)\s*m/);
  const secMatch = timeStr.match(/(\d+)\s*s/);

  if (dayMatch) days = parseInt(dayMatch[1]);
  if (hourMatch) hours = parseInt(hourMatch[1]);
  if (minMatch) minutes = parseInt(minMatch[1]);
  if (secMatch) seconds = parseInt(secMatch[1]);

  return days * 86400 + hours * 3600 + minutes * 60 + seconds;
};

const formatSecondsToExtendedFormat = (seconds) => {
  const d = Math.floor(seconds / 86400);
  const h = Math.floor((seconds % 86400) / 3600);
  const m = Math.floor((seconds % 3600) / 60);
  const s = seconds % 60;

  let result = "";

  if (d > 0) result += `${d}d `;
  if (h > 0 || d > 0) result += `${h}h `;
  if (m > 0 || h > 0 || d > 0) result += `${m}m `;

  result += `${s}s`;

  return result.trim();
};

const startCountdown = () => {
  if (!props.data.time || typeof props.data.time !== "string") return;

  let secondsLeft = parseCustomTimeFormatToSeconds(props.data.time);
  if (!secondsLeft || secondsLeft <= 0) {
    remainingTime.value = "Tempo expirado";
    return;
  }

  remainingTime.value = formatSecondsToExtendedFormat(secondsLeft);

  timer.value = setInterval(() => {
    secondsLeft--;
    if (secondsLeft <= 0) {
      clearInterval(timer.value);
      remainingTime.value = "Tempo expirado";
    } else {
      remainingTime.value = formatSecondsToExtendedFormat(secondsLeft);
    }
  }, 1000);
};

onBeforeUnmount(() => {
  if (timer.value) clearInterval(timer.value);
});
</script>
