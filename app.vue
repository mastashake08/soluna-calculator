<template>
  <UContainer>
    <UCard class="mt-10">
      <template #header>
        <div class="flex justify-between">
          <h1>Soluna Calculator</h1>
          <ColorScheme><USelect v-model="$colorMode.preference" :options="['system', 'light', 'dark']" /></ColorScheme>
        </div>
      </template>
      <img ref="isShowing" v-if="isShowing" src="~/assets/svg/full-moon.svg" />
      <UButton class="mx-auto" @click="getData">Get Lunar Data</UButton>
    </UCard>
  </UContainer>
</template>

<script setup>
import SunCalc from 'suncalc'

const isShowing = ref('isShowing', false)
function getData() {
  if (!navigator.geolocation) {
    alert("Geolocation is not supported by your browser")
  } else {
    navigator.geolocation.getCurrentPosition(getTimes, error);
  }
}
function getTimes(position) {
  console.log(position)
  var times = SunCalc.getTimes(Date.now(), position.coords.latitude, position.coords.longitude);
  console.log(times)
  isShowing.value = !isShowing.value
}

function error(e) {
  alert(e.message)
}


</script>