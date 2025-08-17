<script lang="ts" setup>
import { onMounted } from "vue";
import { useTheme } from "vuetify";

const _useTheme = useTheme();

const tmpGetTheme = () => {
  return new Promise((resolve) => {
    try {
      const _theme = localStorage.getItem("timer_theme");
      if (_theme) {
        _useTheme.change(_theme);
      }
      resolve(true);
    } catch {
      resolve(false);
    }
  });
};

onMounted(async () => {
  await tmpGetTheme();
});
</script>
<template>
  <NuxtLayout>
    <NuxtPage />
  </NuxtLayout>
</template>
