<template>
  <div class="chat-message bg-white px-5 py-4 mt-12 mx-4 rounded-xl relative" :style="{
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
  }
});

function randomRotation() {
  return (Math.floor(Math.random() * 500) - 200) / 100;
}

</script>
