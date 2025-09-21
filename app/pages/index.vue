<script lang="ts" setup>
import { useHead } from "@unhead/vue";
import { onBeforeUnmount, onMounted, ref } from "vue";

useHead({
  title: "Time Up | Timer",
});

const countdown = ref({
  days: 0,
  hours: 0,
  minutes: 0,
  seconds: 0,
  progress: 0,
  secondsPercentage: 0,
  minutesPercentage: 0,
  hoursPercentage: 0,
});
const timer = ref();
const startingTime = ref("");
const startingDate = ref("");
const timerFunc = () => {
  if (!startingTime.value || !startingDate.value) {
    return;
  }

  const target = new Date(`${startingDate.value}T${startingTime.value}:00`);
  const now = new Date();

  let diff = now.getTime() - target.getTime();

  if (diff < 0) diff = 0;

  const days = Math.floor(diff / (1000 * 60 * 60 * 24));
  const hours = Math.floor((diff % (1000 * 60 * 60 * 24)) / (1000 * 60 * 60));
  const minutes = Math.floor((diff % (1000 * 60 * 60)) / (1000 * 60));
  const seconds = Math.floor((diff % (1000 * 60)) / 1000);
  const secondsPercentage = Math.floor((seconds / 60) * 100);
  const minutesPercentage = Math.floor((minutes / 60) * 100);
  const hoursPercentage = Math.floor((hours / 24) * 100);
  let progress = secondsPercentage;
  if (hours) {
    progress = minutesPercentage;
  }
  if (days) {
    progress = hoursPercentage;
  }
  countdown.value = {
    days,
    hours,
    minutes,
    seconds,
    progress,
    secondsPercentage,
    minutesPercentage,
    hoursPercentage,
  };
};

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
  if (!timer.value) {
    await tmpGetLocalStorage();
    timerFunc();
    timer.value = setInterval(() => {
      timerFunc();
    }, 1000);
  }
});

onBeforeUnmount(() => {
  if (timer.value) {
    clearTimeout(timer.value);
  }
});
</script>

<template>
  <div class="d-flex flex-column ga-4">
    <v-card class="border elevation-0">
      <v-card-title>Timer</v-card-title>
    </v-card>
    <template v-if="startingDate && startingTime">
      <v-card class="border elevation-0">
        <v-card-text>
          You have been there for
          <span class="text-primary font-weight-bold">{{
            countdown.days
          }}</span>
          days,
          <span class="text-primary font-weight-bold">{{
            countdown.hours
          }}</span>
          hours and
          <span class="text-primary font-weight-bold">{{
            countdown.minutes
          }}</span>
          minutes!
        </v-card-text>
      </v-card>
      <div class="d-flex justify-center align-center mt-8">
        <v-progress-circular
          size="256"
          :model-value="countdown.hoursPercentage"
          color="warning"
        >
          <v-progress-circular
            size="248"
            :model-value="countdown.minutesPercentage"
            color="error"
          >
            <v-progress-circular
              size="240"
              :model-value="countdown.secondsPercentage"
              color="warning"
            >
              <template v-if="countdown.days">
                <div
                  class="d-flex flex-column ga-4 justify-center align-center"
                >
                  <div class="text-h1 font-weight-black">
                    {{ countdown.days }}
                  </div>
                  <div>Days</div>
                </div>
              </template>
              <template v-else-if="countdown.hours">
                <div
                  class="d-flex flex-column ga-4 justify-center align-center"
                >
                  <div class="text-h1 font-weight-black">
                    {{ countdown.hours }}
                  </div>
                  <div>Hours</div>
                </div>
              </template>
              <template v-else-if="countdown.minutes">
                <div
                  class="d-flex flex-column ga-4 justify-center align-center"
                >
                  <div class="text-h1 font-weight-black">
                    {{ countdown.minutes }}
                  </div>
                  <div>Minutes</div>
                </div>
              </template>
              <template v-else-if="countdown.seconds">
                <div
                  class="d-flex flex-column ga-4 justify-center align-center"
                >
                  <div class="text-h1 font-weight-black">
                    {{ countdown.seconds }}
                  </div>
                  <div>Seconds</div>
                </div>
              </template>
            </v-progress-circular>
          </v-progress-circular>
        </v-progress-circular>
      </div>
    </template>
    <template v-else>
      <v-card class="border elevation-0">
        <v-card-text>
          You need to select date and time first from
          <nuxt-link to="/options">the options</nuxt-link>.
        </v-card-text>
      </v-card>
    </template>
  </div>
</template>
