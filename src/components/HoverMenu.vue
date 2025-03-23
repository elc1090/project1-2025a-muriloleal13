<template>
  <div
    class="relative group border-t-[4px] border-transparent"
    :style="isHovered ? { borderColor: currentColor } : {}"
    @mouseenter="openMenu"
    @mouseleave="closeMenu"
  >
    <a href="#" class="font-bold text-white p-3 text-xl">
      <div class="flex items-center gap-2">
        <q-icon :name="icon" :style="{ color: currentColor }" />
        <span v-if="label">{{ label }}</span>
        <q-icon color="white" name="bi-chevron-down" v-if="label" />
      </div>
    </a>

    <q-menu
      ref="menu"
      class="bg-white shadow-md rounded-xl p-3 text-black"
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
import { computed, ref } from "vue";

const props = defineProps({
  label: String,
  icon: String,
  color: String,
});

const menu = ref(null);
const isHovered = ref(false);
const closeTimeout = ref(null);

const currentColor = computed(() =>
  menu?.value?.contentEl ? props.color : "white"
);

const openMenu = () => {
  isHovered.value = true;
  if (closeTimeout.value) clearTimeout(closeTimeout.value);
  menu.value?.show();
};

const closeMenu = () => {
  isHovered.value = false;
  closeTimeout.value = setTimeout(() => {
    menu.value?.hide();
  }, 150);
};

const cancelClose = () => {
  if (closeTimeout.value) clearTimeout(closeTimeout.value);
};
</script>
