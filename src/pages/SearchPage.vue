<template>
  <div class="flex flex-col items-center justify-center gap-3 w-full p-5">
    <span class="text-3xl font-bold">Catálogo</span>
    <q-input
      class="w-[650px] shadow-xl !rounded-xl mb-12"
      bg-color="white"
      color="accent"
      rounded
      filled
      standout
      v-model="search"
    />
    <div class="flex flex-nowrap items-center justify-between w-full">
      <div>
        <span>Sem resultados</span>
        <q-btn
          label="limpar filtros"
          icon="bi-x-circle"
          flat
          color="negative"
        />
      </div>
    </div>
    <q-table
      class="w-full bg-transparent"
      flat
      dark
      :rows="[]"
      :columns="cols"
    ></q-table>
    <q-img
      class="w-[250px] mt-[150px]"
      src="https://assets.nuuvem.com/assets/fe/illustration-monster-investigator-WpQfG60B.png"
    />
  </div>
</template>
<script setup>
import { ref } from "vue";
import { useRoute } from "vue-router";

const route = useRoute();
const search = ref(route.params.search);
const cols = [
  { label: "Plataforma" },
  { label: "Ativação" },
  { label: "Sistema" },
  { label: "Tipo" },
  { label: "Categorias" },
  { label: "Distribuidor" },
  { label: "Preço" },
];
console.log("🚀 ~ route:", route);
</script>
