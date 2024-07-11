<template>
  <div class="chat-message bg-white px-5 py-4 mt-12 mx-4 rounded-xl relative max-w-full" :class="fancy" :style="{
    transform: `rotate(${randomRotation()}deg)`
  }">
    <Flower
        :classes="{
          'w-8': true,
          'h-8': true,
          'top-1/2': true,
          '-mt-4': true,
          'block': true,
          'absolute': true,
          'rounded-full': true,
          '-left-4': true,
          'text-teal-300': true,
        }"
    ></Flower>
    <Flower
        :classes="{
          'w-8': true,
          'h-8': true,
          'top-1/2': true,
          '-mt-4': true,
          'block': true,
          'absolute': true,
          'rounded-full': true,
          '-right-4': true,
          'text-teal-300': true,
        }"
    ></Flower>
    <p class="text-gray-800 text-center font-bold">
      {{ username }} {{ message }}
    </p>
  </div>
</template>

<script setup>
import { defineProps, computed } from 'vue';
import Flower from "./Flares/flower.vue";

const props = defineProps({
  username: {
    type: String,
    required: true
  },
  message: {
    type: String,
    required: true
  },
  tilt: {
    type: Boolean,
    required: false,
    default: true
  },
  fancy: {
    type: String,
    required: false,
    default: ''
  }
});

function randomRotation() {
  if (!props.tilt) return 0;
  return (Math.floor(Math.random() * 500) - 200) / 100;
}

</script>

<style scoped>
.chat-message:after {
  content: "";
  position: absolute;
  top: -2px;
  left: -2px;
  width: calc(100% + 4px);
  height: calc(100% + 4px);
  background-size: 400%;
  filter: blur(16px);
  border-radius: 0.75rem;
  opacity: 0.9;
  z-index: -2;
}

.chat-message:before {
  content: "";
  position: absolute;
  background: white;
  border-radius: 0.75rem;
  width: 100%;
  height: 100%;
  top: 0;
  left: 0;
  z-index: -1;
}

.sub:after {
  background: #A1ECD9;
}

.raid:after {
  background: #EEEB9F;
}

.cheer:after {
  background: #E89FEE;
}
</style>
