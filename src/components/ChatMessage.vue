<template>
  <div class="chat-message bg-white px-6 py-4 pb-6 mt-12 rounded-xl relative" :class="{
    'rainbow': messageFlare === 'rainbow',
    'w-full': !isSingleEmote,
    'text-center': isSingleEmote,
  }" :style="{
    transform: `rotate(${randomRotation()}deg)`
  }">
    <div class="absolute h-full w-full top-0 left-0 pointer-events-none" :class="flareColour">
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
        }"
      ></Flower>
      <FlowerLeft
          :classes="{
            'w-20': true,
            'h-20': true,
            'top-1/2': true,
            '-mt-10': true,
            'block': true,
            'absolute': true,
            'rounded-full': true,
            '-left-10': true,
          }"
          v-if="props.tags.subscriber === true"
      ></FlowerLeft>
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
        }"
      ></Flower>
    </div>
    <div class="user -mt-9 mb-4 text-center font-bold">
      <span
          class="username py-1.5 px-4 text-sm rounded-lg relative"
          :class="messageAccent"
      >
        <span class="absolute right-full top-0 badges mr-1 block w-full text-right" :class="flareColour">
          <Sub v-if="userLevel === 'sub'"></Sub>
          <Vip v-if="userLevel === 'vip'"></Vip>
          <Mod v-if="userLevel === 'mod'"></Mod>
          <Host v-if="userLevel === 'host'"></Host>
        </span>
        <EpicKitty v-if="isEpicKitty"></EpicKitty>
        {{ username }}
      </span>
    </div>
    <p class="text-gray-800">
      <EmoteSpam v-if="props.tags['animation-id'] === 'simmer'"></EmoteSpam>
      <template v-if="isSingleEmote">
        <component :is="Emote" :id="parsedMessage[0].id" :alt="parsedMessage[0].alt" :emote-source="parsedMessage[0].emoteSource" class="!h-32 w-auto"></component>
      </template>
      <template v-else>
        <span v-for="part in parsedMessage" :key="part.key">
          <component v-if="part.type === 'emote'" :is="Emote" :id="part.id" :alt="part.alt" :emote-source="part.emoteSource"></component>
          <span v-else>{{ part.text }}</span>
          <span class="inline-block w-1"> </span>
        </span>
      </template>
    </p>

  </div>
</template>

<script setup>
import { defineProps, computed } from 'vue';
import Flower from "./Flares/flower.vue";
import Emote from "./Emote.vue";
import Mod from "./Badges/Mod.vue";
import Sub from "./Badges/Sub.vue";
import Vip from "./Badges/Vip.vue";
import Host from "./Badges/Host.vue";
import EpicKitty from "./Badges/EpicKitty.vue";
import FlowerLeft from "./Flares/FlowerLeft.vue";
import EmoteSpam from "./Flares/EmoteSpam.vue";

const props = defineProps({
  username: {
    type: String,
    required: true
  },
  message: {
    type: String,
    required: true
  },
  tags: {
    type: Object,
    required: true
  },
  tilt: {
    type: Boolean,
    required: false,
    default: true
  },
  stvEmotes: {
    type: Object,
    required: false,
    default: () => ({})
  }
});

let messageAccent = 'bg-yellow-200 text-black';
let flareColour = 'text-yellow-300';
let messageFlare = '';
let userLevel = '';
let isEpicKitty = false;

if (props.tags['user-id'] === props.tags['room-id']) {
  messageAccent = 'bg-red-500 text-white';
  flareColour = 'text-red-500';
  userLevel = 'host';
} else if (props.tags.mod === true) {
  messageAccent = 'bg-teal-400 text-white';
  flareColour = 'text-teal-300';
  userLevel = 'mod';
} else if (props.tags.vip === true) {
  messageAccent = 'bg-purple-400 text-white';
  flareColour = 'text-purple-400';
  userLevel = 'vip';
} else if (props.tags.subscriber === true) {
  messageAccent = 'bg-pink-400 text-white';
  flareColour = 'text-pink-400';
  userLevel = 'sub';
}

if (props.tags['display-name'] === 'EpicKittyXP') {
  isEpicKitty = true;
}

// Animations
if (props.tags['animation-id'] === 'rainbow-eclipse') {
  messageFlare = 'rainbow';
}

const parsedMessage = computed(() => parseMessage(props.message, props.tags.emotes));
const isSingleEmote = computed(() => parsedMessage.value.length === 1 && parsedMessage.value[0].type === 'emote');

function parseMessage(message, emotes) {
  let response = message;

  response = parseMessageTwitch(response, emotes); // Parse Twitch emotes
  response = parseMessage7tv(response, props.stvEmotes); // Parse 7TV emotes

  return response;
}

function parseMessageTwitch(message, emotes) {
  if (!emotes) {
    return message.split(/\s+/).map((word, index) => ({
      type: 'text',
      text: word,
      key: `text-${index}`
    }));
  }

  const emotePositions = [];
  for (const emoteId in emotes) {
    const positions = emotes[emoteId];
    positions.forEach(position => {
      const [start, end] = position.split('-').map(Number);
      emotePositions.push({ start, end, id: emoteId });
    });
  }

  emotePositions.sort((a, b) => a.start - b.start);

  const parsed = [];
  let lastIndex = 0;
  emotePositions.forEach(({ start, end, id }, index) => {
    if (start > lastIndex) {
      const textPart = message.substring(lastIndex, start);
      textPart.split(/\s+/).forEach((word, wordIndex) => {
        if (word) {
          parsed.push({
            type: 'text',
            text: word,
            key: `text-${lastIndex + wordIndex}`
          });
        }
      });
    }
    parsed.push({
      type: 'emote',
      emoteSource: 'twitch',
      id,
      alt: message.substring(start, end + 1),
      key: `emote-${index}`
    });
    lastIndex = end + 1;
  });

  if (lastIndex < message.length) {
    const textPart = message.substring(lastIndex);
    textPart.split(/\s+/).forEach((word, wordIndex) => {
      if (word) {
        parsed.push({
          type: 'text',
          text: word,
          key: `text-${lastIndex + wordIndex}`
        });
      }
    });
  }

  return parsed;
}

function parseMessage7tv(message, emotes) {
  const parsed = [];

  message.forEach((part) => {
    if (part.type === 'text') {
      if (emotes[part.text]) {
        parsed.push({
          type: 'emote',
          emoteSource: '7tv',
          id: emotes[part.text].id,
          alt: part.text
        });
      } else {
        parsed.push(part);
      }
    } else {
      parsed.push(part);
    }
  });

  return parsed;
}

function randomRotation() {
  if (!props.tilt) return 0;
  return (Math.floor(Math.random() * 500) - 200) / 100;
}

</script>
<style scoped>
.rainbow:after {
  content: "";
  position: absolute;
  top: -10px;
  left: -10px;
  width: calc(100% + 20px);
  height: calc(100% + 20px);
  background: linear-gradient(45deg, #fb0094, #0000ff, #00ff00, #ffff00, #ff0000, #fb0094, #0000ff, #00ff00, #ffff00, #ff0000);
  background-size: 400%;
  animation: rainbow 20s linear infinite;
  filter: blur(16px);
  border-radius: 0.75rem;
  opacity: 0.9;
  z-index: -2;
}

.rainbow:before {
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

@keyframes rainbow {
  0% {
    background-position: 0 0;
  }
  50.01% {
    background-position: 200% 0;
  }
  100% {
    background-position: 0 0;
  }
}
</style>
