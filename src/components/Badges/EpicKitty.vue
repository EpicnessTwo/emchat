<template>
  <img class="mr-1 inline-block" :class="classes" :src="randomEmoji" alt="EpicKittyXP" />
</template>
<script setup>
import { defineProps } from 'vue';

const props = defineProps({
  classes: {
    type: Object,
    required: false,
    default: () => ({
      'w-5': true,
      'h-5': true,
      'text-teal-300': true,
    })
  }
});

const emojis = [
    'https://static-cdn.jtvnw.net/emoticons/v2/emotesv2_d56827b2bd1f4e5caa1fa2badbc32dd2/default/dark/3.0',
    'https://static-cdn.jtvnw.net/emoticons/v2/emotesv2_7bda4acb962745c8beec4afe03d237d3/default/dark/3.0',
    'https://static-cdn.jtvnw.net/emoticons/v2/emotesv2_c190480832614b66b2fcb56af5128b7f/default/dark/3.0',
    'https://static-cdn.jtvnw.net/emoticons/v2/emotesv2_5dc28e703a0e437799ec4eacafd88d08/default/dark/3.0',
    'https://static-cdn.jtvnw.net/emoticons/v2/emotesv2_a1e2d46ccd3348a9b21e82b332f3b3db/default/dark/3.0',
];

const randomEmoji = emojis[Math.floor(Math.random() * emojis.length)];
</script>