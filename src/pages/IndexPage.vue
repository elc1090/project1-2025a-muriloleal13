<template>
  <q-page class="bg-dark text-white flex flex-col gap-8 p-8">
    <q-carousel
      v-model="activeSlide"
      animated
      swipeable
      navigation
      infinite
      height="350px"
      class="rounded-lg bg-dark overflow-hidden shadow-lg mb-8"
      control-color="white"
    >
      <q-carousel-slide v-for="(slide, i) in bannerSlides" :key="i" :name="i">
        <div
          class="absolute inset-0 bg-black/50 flex items-center justify-center text-red px-6"
        >
          <div>
            <h2 class="text-3xl md:text-4xl font-bold mb-2">
              {{ slide.title }}
            </h2>
            <p class="text-lg">{{ slide.subtitle }}</p>
          </div>
        </div>
      </q-carousel-slide>
    </q-carousel>

    <div
      class="bg-dark rounded-3xl flex flex-nowrap items-center justify-around mb-10"
    >
      <div
        class="flex flex-nowrap items-center gap-3"
        v-for="(card, i) in promoCards"
        :key="i"
      >
        <q-icon
          :name="card.icon"
          class="text-transparent bg-clip-text bg-gradient-to-r from-[#FE2771] to-[#F36A0E]"
          size="xl"
        />
        <div class="flex flex-col items-center justify-start text-white">
          <span class="text-xl text-start w-full font-bold">
            {{ card.title }}
          </span>
          <spanp class="text-3xl font-extrabold">{{ card.description }}</spanp>
        </div>
      </div>
    </div>

    <section v-for="section in gameSections" :key="section.title" class="mb-12">
      <h2 class="text-2xl font-bold mb-4">{{ section.title }}</h2>
      <div
        class="grid grid-cols-2 sm:grid-cols-3 md:grid-cols-4 lg:grid-cols-6 gap-4"
      >
        <div
          v-for="(game, index) in section.games"
          :key="index"
          class="bg-white text-dark rounded-lg overflow-hidden shadow hover:scale-[1.02] transition-transform"
        >
          <img
            :src="game.image"
            :alt="game.title"
            class="w-full h-40 object-cover"
          />
          <div class="p-3">
            <h3 class="text-sm font-semibold truncate">{{ game.title }}</h3>
            <div class="text-xs text-gray-600">R$ {{ game.price }}</div>
          </div>
        </div>
      </div>
    </section>
  </q-page>
</template>

<script setup>
import { ref } from "vue";

const activeSlide = ref(0);

const bannerSlides = [
  {
    title: "Promoção de Fim de Semana",
    subtitle: "Jogos com até 90% OFF",
  },
  {
    title: "Pré-venda exclusiva",
    subtitle: "Garanta já seu jogo antecipado",
  },
  {
    title: "Jogos Gratuitos",
    subtitle: "Aproveite games free-to-play agora!",
  },
];

const promoCards = [
  {
    title: "Loja",
    description: "100% Oficial",
    icon: "bi-patch-check",
  },
  {
    title: "Experiência",
    description: "Segura e simples",
    icon: "bi-shield-check",
  },
  {
    title: "Divirta-se com",
    description: "Os melhores jogos",
    icon: "bi-controller",
  },
];

const gameSections = [
  {
    title: "Mais Vendidos",
    games: [
      {
        title: "Red Dead Redemption 2",
        price: "99,90",
      },
      {
        title: "Cyberpunk 2077",
        price: "59,90",
      },
      {
        title: "FIFA 24",
        price: "79,90",
      },
    ],
  },
  {
    title: "Lançamentos",
    games: [
      {
        title: "Like a Dragon: Infinite Wealth",
        price: "199,90",
      },
      {
        title: "Tekken 8",
        price: "249,90",
      },
      {
        title: "Palworld",
        price: "89,90",
      },
    ],
  },
];
</script>
