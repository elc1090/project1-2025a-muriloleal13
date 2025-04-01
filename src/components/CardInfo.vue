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
      :label="!!inChartList ? 'Remover' : 'Comprar'"
      color="positive"
      text-color="black"
      no-wrap
      push
      no-caps
      glossy
      @click.prevent="() => (!!inChartList ? removeFromChart() : addToChart())"
      v-if="isMainCard"
    />
    <q-btn
      class="font-bold text-lg opacity-0 transition-opacity duration-300 ease-in-out"
      :class="{ grow: isGrid, 'opacity-100': isHoverInfo }"
      :icon="`bi-cart-${!!inChartList ? 'dash' : 'plus'}`"
      :label="`R$${data.price}`"
      :color="!!inChartList ? 'negative' : 'positive'"
      text-color="black"
      push
      padding="xs md"
      no-caps
      glossy
      @click.prevent="() => (!!inChartList ? removeFromChart() : addToChart())"
      v-show="!isMainCard && isHoverInfo"
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
      @click.prevent="addToWishes"
      v-if="isMainCard || isHoverInfo"
    />
  </div>
</template>
<script setup>
import { useChartStore } from "src/stores/chartStore";
import { useWishesStore } from "src/stores/wishesStore";
import { computed } from "vue";

const props = defineProps({
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

const chartStore = useChartStore();
const wishesStore = useWishesStore();

const inChartList = computed(() =>
  chartStore.storeChartDataGetter.find((it) => it.title == props.data.title)
);

const addToChart = () => {
  chartStore.storageChartSave([...chartStore.storeChartDataGetter, props.data]);
};

const removeFromChart = () => {
  chartStore.storageChartSave([
    ...chartStore.storeChartDataGetter.filter(
      (it) => it.title != props.data.title
    ),
  ]);
};

const addToWishes = () => {
  wishesStore.storageChartSave([
    ...wishesStore.storeChartDataGetter,
    props.data,
  ]);
};
</script>
