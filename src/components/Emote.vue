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
  }
});

const emoteUrl = `https://static-cdn.jtvnw.net/emoticons/v2/${props.id}/default/dark/3.0`;
</script>