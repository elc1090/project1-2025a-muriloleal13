<template>
  <q-page class="text-white flex flex-col gap-8 px-12 py-16">
    <q-carousel
      v-model="activeSlide"
      ref="carousel"
      animated
      swipeable
      infinite
      height="80vh"
      class="rounded-3xl bg-transparent overflow-hidden"
    >
      <q-carousel-slide v-for="(item, i) in bannerSlides" :key="i" :name="i">
        <div class="flex flex-nowrap gap-12 w-full h-[70vh]">
          <CardBanner
            class="w-2/3 h-full rounded-xl text-white"
            :data="item[0]"
            :isMainCard="true"
          />
          <div class="grid grid-rows-2 h-full gap-12 w-1/3">
            <CardBanner class="rounded-xl text-white" :data="item[1]" />
            <CardBanner class="rounded-xl text-white" :data="item[2]" />
          </div>
        </div>
      </q-carousel-slide>
      <template v-slot:control>
        <q-carousel-control
          class="flex justify-center gap-3"
          position="bottom"
          :offset="[18, 18]"
        >
          <q-btn
            class="rounded-xl"
            dense
            color="white"
            text-color="black"
            icon="bi-chevron-left"
            @click="$refs.carousel.previous()"
          />
          <div class="flex flex-nowrap items-center gap-1">
            <q-btn
              class="flex flex-nowrap items-center"
              icon="bi-circle-fill"
              dense
              round
              flat
              size="xs"
              :color="activeSlide == item ? 'accent' : 'white'"
              @click.prevent="activeSlide = item"
              v-for="item in Array.from(
                { length: bannerSlides.length },
                (_, i) => i
              )"
              :key="item"
            />
          </div>
          <q-btn
            class="rounded-xl"
            dense
            color="white"
            text-color="black"
            icon="bi-chevron-right"
            @click="$refs.carousel.next()"
          />
        </q-carousel-control>
      </template>
    </q-carousel>

    <div
      class="bg-primary rounded-3xl flex flex-nowrap items-center justify-around py-8 mb-10"
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
          <span class="text-3xl font-extrabold">{{ card.description }}</span>
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
import CardBanner from "src/components/CardBanner.vue";
import { ref } from "vue";

const bannerSlides = [
  [
    {
      title: "Dia da caça ou dia do caçador?",
      description:
        "Mostre pra esses monstros enormes quem é que manda em até 6x sem juros!",
      price: "249,00",
      image:
        "https://assets.nuuvem.com/image/upload/t_quality_80/v1/highlights/67939ed52c274a56bc3218d4/ngzudvizqzxs2fypysdt.jpg",
      discount: 10,
      steam: true,
    },
    {
      title: "Cupom 5NUUPS",
      price: "350,00",
      image:
        "https://assets.nuuvem.com/image/upload/t_quality_80/v1/highlights/67d9abdd0acf20b70c07952d/xqborxsa7efnn1ijamae.jpg",
    },
    {
      title: "Jogos Gratuitos",
      price: "176,90",
      image:
        "https://assets.nuuvem.com/image/upload/t_quality_80/v1/highlights/67ace97543b799f95512da4a/rv4djrlshtyny8gigqmm.jpg",
      discount: 10,
      steam: true,
    },
  ],
  [
    {
      title: "Cupom TLOU",
      description:
        "Garanta já 16%OFF com cupom e ainda parcele em 6x SEM JUROS! Tá esperando o que?",
      price: "R$ 199,00",
      image:
        "https://assets.nuuvem.com/image/upload/t_quality_80/v1/highlights/679162677282e358487ab51d/lf2o1vvyzd6rzwzsqxdx.jpg",
      steam: true,
    },
    {
      price: "R$ 54,50",
      image:
        "https://assets.nuuvem.com/image/upload/t_quality_80/v1/highlights/64da95c6d6cc6e0018ccedde/bxhu05h67vulmu5wajpv.jpg",
      discount: 50,
      steam: true,
    },
    {
      price: "R$ 184,99",
      image:
        "https://assets.nuuvem.com/image/upload/t_quality_80/v1/highlights/673279713153bcf0eb2ed1a5/ajnbtmlj84sgpxbaz8j6.jpg",
      discount: 38,
      steam: true,
    },
  ],
  [
    {
      title: "Keys liberadas!",
      description:
        "A Soul Society precisa de você! Experimente toda história da série em até 6x sem juros!",
      price: "234,90",
      image:
        "https://assets.nuuvem.com/image/upload/t_quality_80/v1/highlights/67d3098f4bd6b3449a984240/msrrdtim4xosxqre7dok.jpg",
      discount: 10,
      steam: true,
    },
    {
      title: "Cupom 5NUUPS",
      price: "350,00",
      image:
        "https://assets.nuuvem.com/image/upload/t_quality_80/v1/highlights/67dd90e9513fe4e190744e24/smimjb6rtc5xpcoahmol.jpg",
    },
    {
      price: "35,99",
      image:
        "https://assets.nuuvem.com/image/upload/t_quality_80/v1/highlights/65245b34165bdb00145a8d4f/sfeqe6sydaw5l64rfnyz.jpg",
      discount: 80,
      steam: true,
    },
  ],
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
    title: "Jogos incríveis por menos de 20 reais",
    description:
      "Final do mês chegou e vamos salvar a sua gameplay de baixo custo",
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
    title: "Warner",
    description: "Jogos com até 95% de desconto!",
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

const carousel = ref(null);
const activeSlide = ref(0);
</script>
