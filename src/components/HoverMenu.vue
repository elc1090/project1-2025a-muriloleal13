<template>
  <div
    class="relative group border-t-[4px] border-transparent hover:border-[#F36711]"
    @mouseenter="openMenu"
    @mouseleave="closeMenu"
  >
    <a href="#" class="font-bold text-white p-3 text-xl">
      <div class="flex items-center gap-2">
        <q-icon
          :name="icon"
          :color="menu?.contentEl ? 'orange-accent' : 'white'"
        />
        <span v-if="label">{{ label }}</span>
        <q-icon color="white" name="bi-chevron-down" v-if="label" />
      </div>
    </a>

    <q-menu
      ref="menu"
      class="bg-white shadow-md rounded-xl px-5 py-3 text-black"
      :offset="[0, 10]"
      anchor="bottom middle"
      self="top middle"
      @mouseenter="cancelClose"
      @mouseleave="closeMenu"
    >
      <slot />
    </q-menu>
  </div>
</template>

<script setup>
import { ref } from "vue";

const props = defineProps({
  label: String,
  icon: String,
});

const menu = ref(null);
const closeTimeout = ref(null);

const openMenu = () => {
  if (closeTimeout.value) clearTimeout(closeTimeout.value);
  menu.value?.show();
};

const closeMenu = () => {
  closeTimeout.value = setTimeout(() => {
    menu.value?.hide();
  }, 150);
};

const cancelClose = () => {
  if (closeTimeout.value) clearTimeout(closeTimeout.value);
};
</script>
