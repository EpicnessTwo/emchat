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
  'emotesv2_d56827b2bd1f4e5caa1fa2badbc32dd2', // epicki8Bonk
  'emotesv2_7963925aad0a4b90b9ba180b411330fa', // epicki8Eepy
  'emotesv2_7bda4acb962745c8beec4afe03d237d3', // epicki8Gasm
  'emotesv2_c190480832614b66b2fcb56af5128b7f', // epicki8Gun
  'emotesv2_acdaea20117044cd9054a609e66017db', // epicki8Laugh
  'emotesv2_d6be962453ff4972a1c005f0980d0911', // epicki8LewdSign
  'emotesv2_5dc28e703a0e437799ec4eacafd88d08', // epicki8Love
  'emotesv2_408f4e8040b64191ae8069f36fbadbf2', // epicki8Lurk
  'emotesv2_07e9eb5b748a430f82aa1c0b32e09ce8', // epicki8MeowSign
  'emotesv2_d72bf5c3d06a41e79dbf2789b77fd05f', // epicki8Pat
  'emotesv2_e5a23693f88745bc9a24910ea4f2ec08', // epicki8Pleading
  'emotesv2_4c0bcfbcdba743e997046b2f39ba3e5d', // epicki8Smug
  'emotesv2_1c6915386a4c4c56b8b493bbc59f419b', // epicki8ThumbsUp
  'emotesv2_a1e2d46ccd3348a9b21e82b332f3b3db', // epicki8Wave
];

const randomEmoji = 'https://static-cdn.jtvnw.net/emoticons/v2/' + emojis[Math.floor(Math.random() * emojis.length)] + '/default/dark/3.0';
</script>