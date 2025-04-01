<template>
  <div
    class="flex flex-col items-center justify-center gap-3 w-screen h-screen"
  >
    <q-tabs
      v-model="tab"
      class="text-accent"
      active-color="white"
      indicator-color="white"
      align="justify"
      narrow-indicator
    >
      <q-tab name="signIn" label="Entrar" />
      <q-tab name="signUp" label="Criar minha conta" />
    </q-tabs>

    <q-card class="w-[450px] bg-gradient-to-br from-blue-800 to-blue-600">
      <q-tab-panels v-model="tab" class="bg-transparent">
        <q-tab-panel name="signIn">
          <q-card-section
            class="flex flex-col items-center justify-center gap-3"
          >
            <q-input
              class="w-full !rounded-3xl"
              label="*Email"
              color="primary"
              bg-color="white"
              filled
              standout
            >
              <template v-slot:prepend> <q-icon name="bi-person" /> </template>
            </q-input>
            <q-input
              class="w-full !rounded-3xl"
              label="*Senha"
              color="primary"
              bg-color="white"
              filled
              standout
            >
              <template v-slot:prepend> <q-icon name="bi-key" /> </template>
            </q-input>
            <div class="flex flex-nowrap items-center justify-between w-full">
              <q-checkbox label="Lembrar" color="primary" />
              <span class="text-secondary">Esqueci minha senha</span>
            </div>
          </q-card-section>
        </q-tab-panel>
        <q-tab-panel name="signUp">
          <q-card-section
            class="flex flex-col items-center justify-center gap-3"
          >
            <q-input
              class="w-full !rounded-3xl"
              label="*Email"
              color="primary"
              bg-color="white"
              filled
              standout
            >
              <template v-slot:prepend> <q-icon name="bi-person" /> </template>
            </q-input>
            <q-input
              class="w-full !rounded-3xl"
              label="*Senha"
              color="primary"
              bg-color="white"
              filled
              standout
            >
              <template v-slot:prepend> <q-icon name="bi-key" /> </template>
            </q-input>
            <span class="text-start w-full">Siga essas dicas:</span>
            <ul class="">
              <li class="list-disc">Mínimo 6 caracteres</li>
              <li class="list-disc">
                Tente uma combinação com letras maiúsculas e minúsculas, números
                e caracteres especiais
              </li>
              <li class="list-disc">Evite sequências como "12345" ou "abc"</li>
              <li class="list-disc">
                Evite informações pessoais e palavras comuns
              </li>
            </ul>
            <q-checkbox label="Quero receber ofertas iradas por email" />
          </q-card-section>
        </q-tab-panel>
      </q-tab-panels>
      <div
        class="flex flex-col items-center justify-center gap-3 w-full px-7 mb-5"
      >
        <q-btn
          :label="tab == 'signIn' ? 'Acessar' : 'Criar'"
          color="secondary"
          class="w-full"
          push
        />
        <div class="flex flex-nowrap items-center justify-center gap-3 w-full">
          <q-separator dark />
          <span>ou conecte-se via</span>
          <q-separator dark />
        </div>
        <div class="flex flex-nowrap items-center justify-center gap-3 w-full">
          <q-btn
            :icon="`bi-${item}`"
            color="white"
            text-color="black"
            v-for="item in ['google', 'steam', 'discord']"
            :key="item"
            push
          />
        </div>

        <span v-if="tab == 'signUp'"
          >Ao se cadastrar, você aceita os
          <a class="hover:underline text-blue-400 hover:text-blue-200"
            >Termos de Uso</a
          >
          e a
          <a class="hover:underline text-blue-400 hover:text-blue-200"
            >Política de Privacidade</a
          ></span
        >
      </div>
    </q-card>
  </div>
</template>
<script setup>
import { ref } from "vue";

const tab = ref("signIn");
</script>
