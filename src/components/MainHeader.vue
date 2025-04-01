<template>
  <q-header class="bg-primary text-gray-800 shadow px-[40px] py-[10px]">
    <div class="mx-auto flex items-center justify-between">
      <div class="flex items-center gap-2">
        <q-icon name="bi-list" color="white" size="lg" v-if="$q.screen.lt.lg" />
        <router-link to="/">
          <img
            src="/assets/logo-nuuvem.svg"
            alt="Nuuvem"
            :class="$q.screen.gt.md ? 'w-[212px] h-[40px]' : 'w-[120px] '"
          />
        </router-link>
      </div>
      <div
        class="flex-1 flex justify-center items-center relative transition-all"
      >
        <nav
          v-if="showNavAndActions && $q.screen.gt.md"
          class="flex gap-8 text-sm font-medium transition-all duration-300"
        >
          <HoverMenu
            :label="item.label"
            :icon="item.icon"
            :color="item.color"
            v-for="item in menuData"
            :key="item.label"
          >
            <q-list
              class="font-bold text-lg"
              v-for="sub in item.items"
              :key="sub.label"
            >
              <hr class="block my-4" v-if="sub.divider" />
              <q-item class="rounded-xl text-dark px-5 py-3" clickable v-else>
                <q-item-section>
                  <div class="flex flex-nowrap items-center gap-2">
                    <q-icon color="dark" :name="sub.icon" v-if="sub.icon" />
                    <span class="font-medium"> {{ sub.label }} </span>
                  </div>
                </q-item-section>
              </q-item>
            </q-list>
          </HoverMenu>
        </nav>
        <q-input
          v-model="search"
          :rounded="$q.screen.gt.md || searchActive"
          :standout="$q.screen.gt.md || searchActive"
          :borderless="$q.screen.lt.lg && !searchActive"
          :dark="true"
          :placeholder="$q.screen.gt.md ? 'Buscar' : ''"
          color="white"
          debounce="300"
          class="transition-all duration-300 ease-in-out text-dark"
          :class="[
            searchActive
              ? 'absolute w-full z-10 mx-auto max-w-3xl '
              : $q.screen.gt.md
              ? 'ml-6 w-[120px]'
              : 'w-[60px]',
          ]"
          @focus="onFocus"
          @blur="onBlur"
          @keyup.enter="router.push(`/search/${search}`)"
        >
          <template v-slot:append v-if="$q.screen.gt.md">
            <q-icon name="search" />
          </template>
          <template v-slot:default v-else>
            <q-icon name="search" size="md" />
          </template>
        </q-input>
      </div>
      <div class="flex items-center gap-4">
        <q-btn
          :color="chartData.length ? 'positive' : 'secondary'"
          text-color="white"
          class="font-semibold"
          padding="md xl"
          rounded
          no-caps
          :glossy="chartData.length"
          v-if="$q.screen.gt.md"
        >
          <template v-slot:default>
            <div class="flex items-center gap-2">
              <q-icon
                :color="chartData.length ? 'black' : 'white'"
                :class="{ 'opacity-30': !chartData.length }"
                name="bi-cart-fill"
              />
              <span
                :class="{
                  'opacity-30 text-lg text-white': !chartData.length,
                  'text-black': chartData.length,
                }"
              >
                {{ chartData.length }}
              </span>
            </div>
            <q-menu
              class="bg-gradient-to-tr from-secondary to-accent rounded-3xl py-3 px-5 text-black shadow-xl !w-[450px]"
              :offset="[0, 10]"
              anchor="bottom middle"
              self="top right"
            >
              <!-- <q-scroll-area
                :style="{
                  height: '350px',
                  width: '100%',
                }"
              > -->
              <div class="flex flex-col gap-3 h-full">
                <div
                  class="flex flex-nowrap items-center justify-between w-full"
                >
                  <span class="text-xl text-white font-bold">Carrinho</span>
                  <q-btn icon="bi-x" color="white" flat v-close-popup />
                </div>
                <q-list class="flex flex-col gap-1">
                  <q-item v-for="(item, idx) in chartData" :key="idx">
                    <q-item-section>
                      <q-img class="rounded-xl" :src="item.image" />
                    </q-item-section>
                    <q-item-section>
                      <q-item-label class="text-white font-bold">
                        {{ item.title }}
                      </q-item-label>
                      <q-item-label caption class="text-white">
                        R$ {{ item.price }}
                      </q-item-label>
                    </q-item-section>
                    <q-item-section side>
                      <q-btn
                        icon="bi-trash2"
                        color="negative"
                        dense
                        push
                        @click.prevent="
                          chartStore.storageChartSave([
                            ...chartData.filter((it) => it.title != item.title),
                          ])
                        "
                      />
                    </q-item-section>
                  </q-item>
                </q-list>
                <q-separator dark />
                <div
                  class="flex flex-nowrap items-center justify-between w-full"
                >
                  <span class="text-white font-bold text-lg">
                    R$ {{ getTotal }}</span
                  >
                  <q-btn
                    class="rounded-xl"
                    label="Finalizar"
                    color="positive"
                    icon="bi-cart-fill"
                    text-color="black"
                    glossy
                  />
                </div>
              </div>
              <!-- </q-scroll-area> -->
            </q-menu>
          </template>
        </q-btn>
        <q-btn
          color="primary"
          text-color="white"
          icon="bi-person-fill"
          label="Entrar"
          class="font-bold text-lg"
          padding="sm"
          to="/login"
          rounded
          flat
          no-caps
        />
      </div>
    </div>
  </q-header>
</template>
<script setup>
import { computed, ref, watch } from "vue";
import HoverMenu from "./HoverMenu.vue";
import { useChartStore } from "src/stores/chartStore";
import { useRouter } from "vue-router";

const menuData = [
  {
    label: "Destaques",
    icon: "bi-star-fill",
    color: "#F36711",
    items: [
      { label: "Ver todos os produtos" },
      { label: "Campanha Nuuvem", icon: "bi-box-arrow-up-right" },
      { label: "Nuuvem Gift Cards" },
      { divider: true },
      { label: "Todas as promoções" },
      { label: "Cupons disponíveis" },
      { label: "Abaixo de R$20,00" },
      { label: "Grátis com seus drops" },
      { divider: true },
      { label: "Enigma do medo", icon: "bi-box-arrow-up-right" },
      { label: "Asleep", icon: "bi-box-arrow-up-right" },
      { label: "Granblue Fantasy", icon: "bi-box-arrow-up-right" },
      { label: "Copycat", icon: "bi-box-arrow-up-right" },
      { label: "Bagdex", icon: "bi-box-arrow-up-right" },
      { label: "wYzards", icon: "bi-box-arrow-up-right" },
    ],
  },
  {
    label: "PC",
    icon: "bi-pc-display",
    color: "#94199e",
    items: [
      { label: "Todos os produtos PC" },
      { label: "Promoções" },
      { label: "Lançamentos" },
      { label: "Pré-vendas" },
      { label: "Mais populares" },
      { label: "Free to Play" },
      { label: "Jogos PlayStation para PC", icon: "bi-box-arrow-up-right" },
      { divider: true },
      { label: "Riot Points" },
      { label: "Valorant Points" },
      { label: "Abaixo de R$20,00" },
      { label: "Grátis com seus drops" },
      { divider: true },
      { label: "Steam", icon: "bi-steam" },
      { label: "Epic Games", icon: "bi-epic-games" },
      { label: "Roblox", icon: "bi-roblox" },
      { label: "Ubisoft Connect", icon: "bi-ubisoft" },
      { label: "Rockstart Social Club", icon: "bi-rockstar" },
      { label: "DRM Free", icon: "bi-drm" },
    ],
  },
  {
    label: "Consoles",
    icon: "bi-controller",
    color: "#4169e1",
    items: [
      { label: "Playstation", icon: "bi-playstation" },
      { label: "Xbox", icon: "bi-xbox" },
      { label: "Nintendo", icon: "bi-nintendo-switch" },
    ],
  },
  {
    label: "Mobile",
    icon: "bi-phone",
    color: "#ff7e82",
    items: [
      { label: "Moedas e Gift Cards" },
      { label: "Cartão App Store", icon: "bi-apple" },
    ],
  },
  {
    icon: "bi-three-dots",
    color: "#87a96b",
    items: [
      { label: "Suporte", icon: "bi-headset" },
      { label: "Sobre", icon: "bi-info-circle" },
    ],
  },
];

const router = useRouter();
const chartStore = useChartStore();
const { chartData } = chartStore;
const search = ref("");
const searchActive = ref(false);
const showNavAndActions = ref(true);

watch(chartData, (newValue) => {
  console.log("🚀 ~ watch ~ newValue:", newValue);
  // lstChart.value = newValue
});

const getTotal = computed(() =>
  chartData.reduce(
    (acc, it) => acc + parseFloat(it.price.replaceAll(",", ".")),
    0
  )
);

const onFocus = () => {
  showNavAndActions.value = false;
  searchActive.value = true;
};

const onBlur = () => {
  searchActive.value = false;

  setTimeout(() => {
    showNavAndActions.value = true;
  }, 300);
};
</script>
