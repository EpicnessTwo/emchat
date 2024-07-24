<template>
  <img :src="emoteUrl" :alt="alt" class="emote inline h-8 w-auto" />
</template>

<script setup>
import { defineProps } from 'vue';

const props = defineProps({
  id: {
    type: String,
    required: true
  },
  alt: {
    type: String,
    required: true
  },
  emoteSource: {
    type: String,
    required: false,
    default: 'twitch'
  }
});

let emoteUrl;

switch (props.emoteSource) {
  case 'twitch':
    emoteUrl = `https://static-cdn.jtvnw.net/emoticons/v2/${props.id}/default/dark/3.0`;
    break;
  case '7tv':
    emoteUrl = `https://cdn.7tv.app/emote/${props.id}/4x.webp`;
    break;
}
</script>