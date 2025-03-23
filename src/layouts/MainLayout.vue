<template>
  <q-layout view="hHh lpR fFf">
    <MainHeader />
    <q-page-container class="bg-secondary">
      <router-view />
    </q-page-container>
    <q-page-sticky position="bottom-right" :offset="[18, 18]">
      <q-btn fab icon="bi-chat-left-dots-fill" color="accent" />
    </q-page-sticky>
  </q-layout>
</template>
<script setup lang="ts">
import MainHeader from "src/components/MainHeader.vue";
</script>
