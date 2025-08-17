<template>
  <v-app>
    <v-main>
      <v-container>
        <slot />
      </v-container>
    </v-main>
    <v-bottom-navigation height="64" class="border elevation-0">
      <template v-for="(btn, i) in navigationBtns" :key="i">
        <v-btn
          :to="btn.link"
          active-color="primary opacity-100"
          class="opacity-50"
        >
          <div class="d-flex flex-column ga-2 justify-center align-center">
            <v-icon>{{ btn.icon }}</v-icon>
            {{ btn.label }}
          </div>
        </v-btn>
      </template>
    </v-bottom-navigation>
  </v-app>
</template>

<script lang="ts" setup>
import { ref } from "vue";

const navigationBtns = ref([
  {
    link: "/",
    icon: "mdi-clock-outline",
    label: "Timer",
  },
  {
    link: "/options",
    icon: "mdi-cog-outline",
    label: "Options",
  },
]);
</script>
