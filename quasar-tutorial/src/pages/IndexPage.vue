<template>
  <q-page
    v-touch-pan.vertical.prevent.mouse="handlePan"
    class="flex flex-center text-white">
    <div class="absolute-center q-mb-xl">
      <q-input v-model="data.name" input-class="text-center text-h5 text-white" color="teal" placeholder="Counter"/>
    </div>

    <div class="full-width flex justify-around items-center"
         style="position: absolute; top: 50%; left: 50%; transform: translate(-50%, -50%);">
      <q-btn @click="decreaseCounter" v-touch-repeat:300:300:300:300:50.mouse="decreaseCounter" round icon="remove"
             size="xl"/>

      <div class="text-h2 text-center"> {{ data.counter }}</div>

      <q-btn @click="increaseCounter" v-touch-repeat:300:300:300:300:50.mouse="increaseCounter" round icon="add"
             size="xl"/>
    </div>

    <div class="absolute-center q-mt-xl">
      <q-btn @click="resetCounter" round icon="restart_alt" size="xl"/>
    </div>
  </q-page>
</template>

<style scoped>
.q-page {
  max-width: 700px;
  margin: 0 auto;
  position: relative;
  height: 100vh;
}

.absolute-center {
  position: absolute;
  left: 50%;
  transform: translateX(-50%);
}

.q-mb-xl {
  top: 10%;
}

.q-mt-xl {
  bottom: 10%;
}
</style>

<script setup>
import { reactive, watch } from "vue";
import { useQuasar } from "quasar";

const $q = useQuasar();


const data = reactive({
  counter: 0,
  name: "",
});

const savedData = $q.localStorage.getItem("data");
if (savedData) Object.assign(data, savedData);

watch(data, value => {
  $q.localStorage.set("data", value);
});

const increaseCounter = () => {
  data.counter++;
};

const decreaseCounter = () => {
  if (data.counter > 0) {
    data.counter--;
  }
};

const resetCounter = () => {
  data.counter = 0;
};

const handlePan = e => {
  console.log(e.delta.y);
  if (e.delta.y < 0) increaseCounter();
  else decreaseCounter();
};
</script>
