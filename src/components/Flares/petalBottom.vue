<template>
  <svg :class="classes" fill="currentColor" viewBox="0 0 122 129" version="1.1" xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink" xml:space="preserve" xmlns:serif="http://www.serif.com/" style="fill-rule:evenodd;clip-rule:evenodd;stroke-linecap:round;stroke-linejoin:round;stroke-miterlimit:1.5;">
    <rect id="mod_sub_teal_flower" x="0" y="0" width="121.982" height="128.707" style="fill:none;" />
    <g>
      <path d="M63.176,64.458c-0,-0 14.583,22.5 14.583,37.5c0,9.199 -6.534,16.667 -14.583,16.667c-8.049,-0 -14.583,-7.468 -14.583,-16.667c-0,-15 14.583,-37.5 14.583,-37.5Z" style="fill:none;stroke:currentColor;stroke-width:5.98px;" />
    </g>
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