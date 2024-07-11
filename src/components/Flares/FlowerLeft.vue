<template>
  <svg :class="classes" fill="currentColor" viewBox="0 0 63 273" version="1.1" xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink" xml:space="preserve" xmlns:serif="http://www.serif.com/" style="fill-rule:evenodd;clip-rule:evenodd;stroke-linejoin:round;stroke-miterlimit:2;">
  <rect id="Artboard1" x="0" y="0" width="62.5" height="272.791" style="fill:none;" />
    <path d="M37.5,56.25c0,0 -14.583,-22.5 -14.583,-37.5c-0,-9.199 6.534,-16.667 14.583,-16.667c8.049,0 14.583,7.468 14.583,16.667c0,15 -14.583,37.5 -14.583,37.5Z" style="fill:currentColor;" />
    <path d="M37.594,56.25c-0,0 -20.395,-5.216 -28.382,-13.871c-4.897,-5.307 -5.104,-13.095 -0.46,-17.381c4.644,-4.286 12.39,-3.456 17.288,1.851c7.987,8.654 11.554,29.401 11.554,29.401Z" style="fill:currentColor;" />
    <path d="M37.5,214.583c0,0 -14.583,22.5 -14.583,37.5c-0,9.199 6.534,16.667 14.583,16.667c8.049,0 14.583,-7.468 14.583,-16.667c0,-15 -14.583,-37.5 -14.583,-37.5Z" style="fill:currentColor;" />
    <path d="M37.594,214.583c-0,0 -20.395,5.217 -28.382,13.871c-4.897,5.307 -5.104,13.096 -0.46,17.381c4.644,4.286 12.39,3.457 17.288,-1.851c7.987,-8.654 11.554,-29.401 11.554,-29.401Z" style="fill:currentColor;" />
</svg>
</template>

<script setup>
import { defineProps } from 'vue';

const props = defineProps({
  classes: {
    type: Object,
    required: false,
    default: () => ({
      'w-12': true,
      'h-12': true,
      'text-teal-300': true,
    })
  }
});
</script>