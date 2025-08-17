<script lang="ts" setup>
import { useHead } from "@unhead/vue";
import { computed, onMounted, ref, watch } from "vue";
import { useTheme } from "vuetify";

useHead({
  title: "Time Up | Options",
});

const _useTheme = useTheme();

const theme = computed(() => _useTheme.name.value);
const startingTime = ref("");
const startingDate = ref("");

watch(startingTime, (v) => {
  localStorage.setItem("timer_start_time", v);
});
watch(startingDate, (v) => {
  localStorage.setItem("timer_start_date", v);
});
watch(theme, (v) => {
  localStorage.setItem("timer_theme", v);
});

const tmpGetLocalStorage = () => {
  return new Promise((resolve) => {
    try {
      const _startingDate = localStorage.getItem("timer_start_date");
      const _startingTime = localStorage.getItem("timer_start_time");
      if (_startingDate) {
        startingDate.value = _startingDate;
      }
      if (_startingTime) {
        startingTime.value = _startingTime;
      }
      resolve(true);
    } catch {
      resolve(false);
    }
  });
};

onMounted(async () => {
  await tmpGetLocalStorage();
});
</script>

<template>
  <div class="d-flex flex-column ga-4">
    <v-card class="border elevation-0">
      <v-card-title>Options</v-card-title>
    </v-card>
    <v-list class="pb-4 border elevation-0">
      <v-list-subheader class="mb-4">TIMER OPTIONS</v-list-subheader>
      <div class="d-flex flex-column ga-6 mb-4">
        <div class="px-4">
          <v-text-field
            v-model="startingDate"
            variant="outlined"
            hint="Set starting date of the timer"
            persistent-hint
            type="date"
            label="Date"
            clearable
          />
        </div>
        <div class="px-4">
          <v-text-field
            v-model="startingTime"
            variant="outlined"
            hint="Set starting time of the timer"
            persistent-hint
            type="time"
            label="Time"
            clearable
          />
        </div>
      </div>
      <v-list-subheader class="mb-2">THEME</v-list-subheader>
      <div class="px-4">
        <v-btn-toggle :model-value="theme" variant="outlined" color="primary">
          <v-btn value="dark" @click="_useTheme.change('dark')">
            <div class="d-flex justify-center align-center ga-2">
              <v-icon>mdi-weather-night</v-icon>
              Dark
            </div>
          </v-btn>
          <v-btn value="light" @click="_useTheme.change('light')">
            <div class="d-flex justify-center align-center ga-2">
              <v-icon>mdi-weather-sunny</v-icon>
              Light
            </div>
          </v-btn>
        </v-btn-toggle>
      </div>
    </v-list>
  </div>
</template>
