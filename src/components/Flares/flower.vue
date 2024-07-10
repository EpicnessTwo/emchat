<template>
  <svg :class="classes" fill="currentColor" viewBox="0 0 122 129" version="1.1" xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink" xml:space="preserve" xmlns:serif="http://www.serif.com/" style="fill-rule:evenodd;clip-rule:evenodd;stroke-linecap:round;stroke-linejoin:round;stroke-miterlimit:1.5;">
    <rect id="mod_sub_teal_flower" x="0" y="0" width="121.982" height="128.707" style="fill:none;" />
      <g>
      <path d="M63.176,64.458c-0,-0 -14.583,-22.5 -14.583,-37.5c-0,-9.199 6.534,-16.667 14.583,-16.667c8.049,0 14.583,7.468 14.583,16.667c0,15 -14.583,37.5 -14.583,37.5Z" style="fill:none;stroke:currentColor;stroke-width:5.98px;" />
        <path d="M63.176,64.358c-0,0 12.194,-23.879 25.184,-31.379c7.966,-4.6 17.701,-2.675 21.726,4.296c4.024,6.97 0.824,16.363 -7.143,20.963c-12.99,7.5 -39.767,6.12 -39.767,6.12Z" style="fill:none;stroke:currentColor;stroke-width:5.98px;" />
        <path d="M63.176,64.558c-0,-0 26.777,-1.38 39.767,6.12c7.967,4.6 11.167,13.993 7.143,20.963c-4.025,6.971 -13.76,8.896 -21.726,4.296c-12.99,-7.5 -25.184,-31.379 -25.184,-31.379Z" style="fill:none;stroke:currentColor;stroke-width:5.98px;" />
        <path d="M61.824,64.358c0,0 -12.194,-23.879 -25.184,-31.379c-7.966,-4.6 -17.701,-2.675 -21.726,4.296c-4.024,6.97 -0.824,16.363 7.143,20.963c12.99,7.5 39.767,6.12 39.767,6.12Z" style="fill:none;stroke:currentColor;stroke-width:5.98px;" />
        <path d="M61.824,64.558c0,-0 -26.777,-1.38 -39.767,6.12c-7.967,4.6 -11.167,13.993 -7.143,20.963c4.025,6.971 13.76,8.896 21.726,4.296c12.99,-7.5 25.184,-31.379 25.184,-31.379Z" style="fill:none;stroke:currentColor;stroke-width:5.98px;" />
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