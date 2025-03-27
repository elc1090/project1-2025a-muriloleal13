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
      class="bg-primary rounded-3xl flex flex-nowrap items-center justify-between p-8 mb-10"
    >
      <div
        class="flex flex-nowrap items-center gap-5"
        v-for="(card, i) in promoCards"
        :key="i"
      >
        <q-icon
          :name="card.icon"
          class="text-transparent bg-clip-text bg-gradient-to-r from-[#FE2771] to-[#F36A0E]"
          size="60px"
        />
        <div class="flex flex-col items-center justify-start text-white">
          <span class="text-2xl text-start w-full font-bold">
            {{ card.title }}
          </span>
          <span class="text-3xl font-extrabold">{{ card.description }}</span>
        </div>
      </div>
    </div>
    <template v-for="(sect, idx) in sectionData" :key="idx">
      <template v-if="sect.type == 'grid'">
        <section
          class="flex flex-col gap-5 mb-12"
          v-for="section in sect.gamesData"
          :key="section.title"
        >
          <div class="flex flex-nowrap justify-between">
            <div class="flex flex-col gap-3">
              <h2 class="text-5xl font-bold">{{ section.title }}</h2>
              <h2 class="text-2xl font-bold">{{ section.description }}</h2>
            </div>
            <div class="flex items-end">
              <q-btn
                class="rounded-xl"
                color="white"
                label="Ver mais"
                outline
                no-caps
                size="16px"
              />
            </div>
          </div>
          <div class="grid grid-cols-4 gap-12">
            <CardGrid
              :data="game"
              v-for="(game, index) in section.games"
              :key="index"
            />
          </div>
        </section>
      </template>
      <q-card
        class="relative bg-cover bg-center w-full h-[265px] rounded-xl"
        :style="{
          backgroundImage: `url(${sect.image})`,
        }"
        v-else
      >
        <div
          class="absolute bottom-0 left-0 right-0 bg-gradient-to-t from-blue-800 to-transparent z-5"
          :style="{ height: '200px' }"
        ></div>
        <div
          class="absolute bottom-0 left-0 right-0 z-10 flex flex-nowrap justify-between p-5 pb-2"
        >
          <div class="flex flex-col gap-3 w-full">
            <div>
              <q-chip
                icon="bi-hurricane"
                label="Ubisoft Connect"
                color="primary"
                text-color="white"
                square
                class="rounded-lg"
              />
            </div>
            <div class="flex flex-nowrap items-center justify-between w-full">
              <span class="text-md text-start text-white w-full">
                {{ sect.desc }}
              </span>
              <CardInfo :data="sect.info" />
            </div>
          </div>
        </div>
      </q-card>
    </template>
  </q-page>
</template>

<script setup>
import CardBanner from "src/components/CardBanner.vue";
import CardGrid from "src/components/CardGrid.vue";
import CardInfo from "src/components/CardInfo.vue";
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

const sectionData = [
  {
    type: "grid",
    gamesData: [
      {
        title: "Jogos incríveis por menos de 20 reais",
        description:
          "Final do mês chegou e vamos salvar a sua gameplay de baixo custo",
        games: [
          {
            title: "Silent Hill Homecoming",
            platform: ["Windows"],
            time: "8d 12h 34m",
            discount: 80,
            price: "6,99",
            image:
              "https://assets.nuuvem.com/image/upload/t_banner_big/v1/products/557dbbf869702d0a9c24c700/banners/flolkkxvf5dq0datgihj.jpg",
          },
          {
            title: "LEGO Harry Potter Years 5-7",
            platform: ["Windows"],
            time: "0d 2h 34m",
            discount: 90,
            price: "8,99",
            image:
              "https://assets.nuuvem.com/image/upload/t_banner_big/v1/products/557dbb7669702d0a9c3a9900/banners/xtesdj70q0taq7r3apmr.jpg",
          },
          {
            title: "Middle-earth: Shadow of Mordor - Game of the Year Edition",
            platform: ["Windows", "Linux", "macOS"],
            time: "2d 5h 9m",
            discount: 95,
            price: "4,49",
            image:
              "https://assets.nuuvem.com/image/upload/t_banner_big/v1/products/557dbcb269702d0a9c600801/banners/lsv97mpj5fk77u9ghc1a.jpg",
          },
          {
            title: "Jotun - Valhalla Edition",
            platform: ["Windows", "Linux", "macOS"],
            time: "0d 0h 7m",
            discount: 85,
            price: "4,19",
            image:
              "https://assets.nuuvem.com/image/upload/t_banner_big/v1/products/560964e169702d07050000a7/banners/mxqhylr7nf7o0x3guchq.jpg",
          },
        ],
      },
      {
        title: "Warner",
        description: "Jogos com até 95% de desconto!",
        games: [
          {
            title: "LEGO Star Wars™: The Skywalker Saga",
            platform: ["Windows"],
            time: "8d 12h 34m",
            discount: 86,
            price: "26,29",
            image:
              "https://assets.nuuvem.com/image/upload/t_banner_big/v1/products/61f186bb90f1e2247b551b7f/banners/sjqph3geidroun5v2mvn.jpg",
          },
          {
            title: "Batman Arkham Collection",
            platform: ["Windows"],
            time: "8d 12h 34m",
            discount: 91,
            price: "25,00",
            image:
              "https://assets.nuuvem.com/image/upload/t_banner_big/v1/products/61437f66a3f8b1386fb570e3/banners/dzgtvpre3btb2l6i9ife.jpg",
          },
          {
            title: "Back 4 Blood",
            platform: ["Windows"],
            time: "8d 12h 34m",
            discount: 91,
            price: "24,52",
            image:
              "https://assets.nuuvem.com/image/upload/t_banner_big/v1/products/60087598a3f8b147910a6750/banners/dmsyjen4zc3whgheq6x4.jpg",
          },
          {
            title: "Mortal Kombat 11",
            platform: ["Windows"],
            time: "8d 12h 34m",
            discount: 91,
            price: "20,12",
            image:
              "https://assets.nuuvem.com/image/upload/t_banner_big/v1/products/5c5093428810242173c0bb5c/banners/d2m04tkhgphzxvcskrwk.jpg",
          },
        ],
      },
    ],
  },
  {
    type: "banner",
    image:
      "https://assets.nuuvem.com/image/upload/t_quality_80/v1/highlights/6377be29890ca400149068d5/sjqjwws97g0lmyu4gpfb.jpg",
    desc: "Nós temos Assassin's Creed em casa. O Assassin's Creed lá em casa:",
    info: { discount: 80, oldPrice: "199,99", price: "39,99" },
  },
  {
    type: "grid",
    gamesData: [
      {
        title: "Jogão por até 100zão",
        description: "Se parcelar em 6x sem juros você nem sente!",
        games: [
          {
            title: "Mortal Kombat 1",
            platform: ["Windows"],
            time: "4d 14h 50m",
            discount: 75,
            price: "69,97",
            image:
              "https://assets.nuuvem.com/image/upload/t_banner_big/v1/products/646686699af000001520ca83/banners/o4m5kyahvxzhp2ak5v3b.jpg",
          },
          {
            title: "Gas Station Simulator",
            platform: ["Windows", "macOS"],
            time: "1d 1h 34m",
            discount: 30,
            price: "41,99",
            image:
              "https://assets.nuuvem.com/image/upload/t_banner_big/v1/products/6165e28f4af556001658b6a5/banners/mld47owggpk5gx2yla4y.jpg",
          },
          {
            title: "Hogwarts Legacy",
            platform: ["Windows"],
            time: "8d 12h 34m",
            discount: 75,
            price: "62,49",
            image:
              "https://assets.nuuvem.com/image/upload/t_banner_big/v1/products/6307dc85ba5b7c0014a7501e/banners/jyktyairzzxragu26anw.jpg",
          },
          {
            title: "Batman Arkham Collection",
            platform: ["Windows"],
            time: "8d 12h 34m",
            discount: 91,
            price: "25,00",
            image:
              "https://assets.nuuvem.com/image/upload/t_banner_big/v1/products/61437f66a3f8b1386fb570e3/banners/dzgtvpre3btb2l6i9ife.jpg",
          },
        ],
      },
      {
        title: "Preçus Extraordinárius!",
        description:
          "Você tomou uma poção Felix Felicis? Por que encontrar esses descontos é ter MUITA sorte!",
        games: [
          {
            title: "Hogwarts Legacy",
            platform: ["Windows"],
            time: "8d 12h 34m",
            discount: 65,
            price: "62,49",
            image:
              "https://assets.nuuvem.com/image/upload/t_banner_big/v1/products/6307dc85ba5b7c0014a7501e/banners/jyktyairzzxragu26anw.jpg",
          },
          {
            title: "Harry Potter: Campeões do Quadribol",
            platform: ["Windows"],
            time: "8d 12h 34m",
            discount: 75,
            price: "24,99",
            image:
              "https://assets.nuuvem.com/image/upload/t_banner_big/v1/products/66a91ae6aec08df3cd873fd9/banner/axrmaedlcwlj2tqydqsw.jpg",
          },
          {
            title: "LEGO Harry Potter: Years 1- 4",
            platform: ["Windows"],
            time: "8d 12h 34m",
            discount: 95,
            price: "4,49",
            image:
              "https://assets.nuuvem.com/image/upload/t_banner_big/v1/products/557dbaf869702d0a9cb06200/banners/cd7b56upasvezmxxqccd.jpg",
          },
          {
            title: "LEGO Harry Potter Years 5-7",
            platform: ["Windows"],
            time: "8d 12h 34m",
            discount: 90,
            price: "8,99",
            image:
              "https://assets.nuuvem.com/image/upload/t_banner_big/v1/products/557dbb7669702d0a9c3a9900/banners/xtesdj70q0taq7r3apmr.jpg",
          },
        ],
      },
    ],
  },
  {
    type: "banner",
    image:
      "https://assets.nuuvem.com/image/upload/t_quality_80/v1/highlights/64a55b9a56062d0016292782/ev6lg4t9pg9whbx4yqma.jpg",
    desc: "Agora com suporte oficial a mods!",
    info: { discount: 75, oldPrice: "249,99", price: "62,49" },
  },
];

const carousel = ref(null);
const activeSlide = ref(0);
</script>
