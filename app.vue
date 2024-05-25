<template>
  <UContainer>
    <UCard class="mt-10">
      <template #header>
        <div class="flex justify-between">
          <h1>Soluna Calculator</h1>
          <ColorScheme><USelect v-model="$colorMode.preference" :options="['system', 'light', 'dark']" /></ColorScheme>
        </div>
      </template>
      <!-- <img ref="isShowing" v-if="isShowing" src="~/assets/svg/full-moon.svg" /> -->
      <UButton class="mx-auto" @click="getData">Get Lunar Data</UButton>
      {{ moon }}
    </UCard>
  </UContainer>
</template>

<script setup>
import SunCalc from 'suncalc'

const isShowing = ref('isShowing', false)
const moon = useState('moon', () => {})
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
  moon.value = SunCalc.getMoonPosition(Date.now(), position.coords.latitude, position.coords.longitude)
  const ill = SunCalc.getMoonIllumination(Date.now())
  Notification.requestPermission().then((result) => {
    const img = "/full-moon.svg";
    const text = `The moon is ${moon.value.altitude} radian above the horizons and ${moon.value.distance} km away.`;
    const notification = new Notification(`${ill.fraction}% illuminated moon today`, { body: text, icon: img });
    console.log(notification)
  });
}

function error(e) {
  alert(e.message)
}


</script>