<template>
  <div ref="chatBox" class="chat-box p-4 h-screen w-full overflow-hidden justify-center flex flex-wrap" :style="{
    'transform': easterEggs.flip ? 'rotate(180deg)' : '',
  }" :class="{
    'bg-gradient-to-r from-purple-200 to-purple-300': easterEggs.debugBackground,
  }">
    <transition-group name="list" tag="ul" class="w-full px-4">
      <li v-for="(message, index) in messages" :key="message.id" class="mb-2 break-words w-full justify-center flex flex-wrap" :class="{
        'party': easterEggs.party,
      }">
        <ChatMessage
            :username="message.username"
            :message="message.message"
            :stvEmotes="message.stvEmotes"
            :tags="message.tags"
            :tilt="easterEggs.tilt"
            v-if="message.type === 'message'"
            v-once
        ></ChatMessage>
        <ChatNotice
          :username="message.username"
          :message="message.message"
          :fancy="message.fancy"
          :tilt="easterEggs.tilt"
          v-if="message.type === 'notice'"
          v-once
      ></ChatNotice>
      </li>
    </transition-group>
  </div>
</template>

<script setup>
import { ref, onMounted, onUpdated, defineProps } from 'vue';
import tmi from 'tmi.js';
import ChatMessage from "./ChatMessage.vue";
import ChatNotice from "./ChatNotice.vue";

const props = defineProps({
  channel: {
    type: String,
    required: true
  }
});

const messages = ref([]);
const vips = ref([]);
const chatBox = ref(null);
let stvEmotes = {};
const pruneLength = 20;
const easterEggs = {
  'flip': false,
  'tilt': true,
  'party': false,
  'debugBackground': false,
};
const admins = [
    'EpicKittyXP',
    'grazedpoem',
    'crazedpoem',
    'robotpoem',
    'TheLegendaryNarrator',
];

const initializeChat = () => {
  const client = new tmi.Client({
    options: { debug: false },
    connection: {
      reconnect: true,
      secure: true
    },
    channels: [ props.channel ] // Replace with your Twitch channel
  });

  client.connect();

  client.on('message', (channel, tags, message, self) => {
    let commandRan = false;
    if (admins.includes(tags['display-name'])) {
      switch (message) {
        case '~reload':
          messages.value.push({
            type: 'notice',
            id: tags.id,
            username: tags['display-name'],
            message: `is reloading the chat...`
          });
          setTimeout(() => {
            window.location.reload();
          }, 5000);
          commandRan = true;
          break;
        case '~flip':
          easterEggs.flip = !easterEggs.flip;
          commandRan = true;
          break;
        case '~tilt':
          easterEggs.tilt = !easterEggs.tilt;
          commandRan = true;
          break;
        case '~party':
          easterEggs.party = !easterEggs.party;
          commandRan = true;
          break;
        case '~bg':
          easterEggs.debugBackground = !easterEggs.debugBackground;
          commandRan = true;
          break;
        case '~fakesub':
          messages.value.push({
            type: 'notice',
            fancy: 'sub',
            id: tags.id,
            username: tags['display-name'],
            message: `Subscribed!`
          });
          break;
        case '~fakebits':
          messages.value.push({
            type: 'notice',
            fancy: 'cheer',
            id: tags.id,
            username: tags['display-name'],
            message: `Cheered 100 bits!`
          });
          break;
        case '~fakegiftsub':
          messages.value.push({
            type: 'notice',
            fancy: 'sub',
            id: tags.id,
            username: tags['display-name'],
            message: `Gifted a subscription to EpicKittyXP!`
          });
          break;
        case '~fakeraid':
          messages.value.push({
            type: 'notice',
            fancy: 'raid',
            id: tags.id,
            username: tags['display-name'],
            message: `has raided with 100 viewers!`
          });
          break;
        case '~fakemysterygift':
          messages.value.push({
            type: 'notice',
            fancy: 'sub',
            id: tags.id,
            username: tags['display-name'],
            message: `Gifted 5 Tier 1 Subscriptions!`
          });
          messages.value.push({
            type: 'notice',
            fancy: 'sub',
            id: tags.id++,
            username: 'SillyUser1',
            message: `Subscribed!`
          });
          messages.value.push({
            type: 'notice',
            fancy: 'sub',
            id: tags.id++,
            username: 'SillyUser2',
            message: `Subscribed!`
          });
          messages.value.push({
            type: 'notice',
            fancy: 'sub',
            id: tags.id++,
            username: 'SillyUser3',
            message: `Subscribed!`
          });
          messages.value.push({
            type: 'notice',
            fancy: 'sub',
            id: tags.id++,
            username: 'SillyUser4',
            message: `Subscribed!`
          });
          messages.value.push({
            type: 'notice',
            fancy: 'sub',
            id: tags.id++,
            username: 'SillyUser5',
            message: `Subscribed!`
          });
          break;
      }
    }

    if (commandRan) return;

    // truncate username to max of 20 characters and add ellipsis if necessary
    tags['display-name'] = tags['display-name'].length > 20 ? tags['display-name'].substring(0, 20) + '...' : tags['display-name'];

    let filteredStvEmotes = {};
    for (const word of message.split(' ')) {
      if (stvEmotes[word]) {
        filteredStvEmotes[word] = stvEmotes[word];
      }
    }

    let hexMessage = message.split('').map((char) => char.charCodeAt(0).toString(16)).join(' ');

    // Dear 7TV,
    //
    // You may think it's funny to inject invisible characters into messages to bypass sending limits, but it's not.
    // It's not funny at all.
    //
    // I'm enjoying the thought of you reading this message and feeling bad about it.
    //
    // Sincerely,
    // EpicKitty
    message = message.replace(/\uDB40/g, '').replace(/\uDC00/g, '').trimEnd();


    messages.value.push({
      type: 'message',
      id: tags.id,
      username: tags['display-name'],
      message: message,
      hexMessage: hexMessage,
      tags: tags,
      stvEmotes: filteredStvEmotes
    });

    if (messages.value.length >= pruneLength) {
      messages.value = messages.value.slice(-pruneLength);
    }
  });

  client.on('messagedeleted', (channel, username, deletedMessage, userstate) => {
    messages.value = messages.value.filter((message) => message.id !== userstate['target-msg-id']);
  });

  // Notice message stuffs

  client.on('anongiftpaidupgrade', (channel, username, userstate) => {
    messages.value.push({
      type: 'notice',
      fancy: true,
      id: userstate.id,
      username: username,
      message: `An anonymous user has upgraded to a Tier 1 subscription!`
    });
  });

  client.on('cheer', (channel, userstate, message) => {
    messages.value.push({
      type: 'notice',
      fancy: true,
      id: userstate.id,
      username: userstate['display-name'],
      message: `Cheered ${userstate.bits} bits!\n${message}`
    });
  });

  client.on('giftpaidupgrade', (channel, username, sender, userstate) => {
    messages.value.push({
      type: 'notice',
      fancy: true,
      id: userstate.id,
      username: username,
      message: `has upgraded to a Tier 1 subscription!`
    });
  });

  client.on('raided', (channel, username, viewers) => {
    messages.value.push({
      type: 'notice',
      id: username,
      username: username,
      message: `has raided with ${viewers} viewers!`
    });
  });

  client.on('resub', (channel, username, months, message, userstate, methods) => {
    if (months === 0) {
      messages.value.push({
        type: 'notice',
        fancy: true,
        id: userstate.id,
        username: username,
        message: `Resubscribed!`
      });
      return;
    }

    messages.value.push({
      type: 'notice',
      fancy: true,
      id: userstate.id,
      username: username,
      message: `Resubscribed for ${months} months!`
    });
  });

  client.on('subgift', (channel, username, streakMonths, recipient, methods, userstate) => {
    messages.value.push({
      type: 'notice',
      fancy: true,
      id: userstate.id,
      username: username,
      message: `Gifted a subscription to ${recipient}!`
    });
  });

  client.on('submysterygift', (channel, username, numbOfSubs, methods, userstate) => {
    messages.value.push({
      type: 'notice',
      fancy: true,
      id: userstate.id,
      username: username,
      message: `Gifted ${numbOfSubs} Tier 1 Subscriptions!`
    });
  });

  client.on('subscription', (channel, username, method, message, userstate) => {
    messages.value.push({
      type: 'notice',
      fancy: true,
      id: userstate.id,
      username: username,
      message: `Subscribed!`
    });
  });
};

const initializeStvEmotes = () => {
  fetch('https://7tv.io/v3/emote-sets/62dd39816ed60a7d8e340577').then(response => response.json()).then(data => {
    data.emotes.forEach(emote => {
      stvEmotes[emote.name] = {
        id: emote.id,
        name: emote.name
      };
    });
  });
};

const scrollToBottom = () => {
  if (chatBox.value) {
    chatBox.value.scrollTop = chatBox.value.scrollHeight
  }
}

onUpdated(scrollToBottom)
setInterval(scrollToBottom, 1000);

onMounted(() => {
  initializeStvEmotes();
  initializeChat();
  scrollToBottom();
});
</script>

<style scoped>
.list-enter-active, .list-leave-active {
  transition: all .5s;
}
.list-enter, .list-leave-to {
  transform: scale(0.8) translateY(30px);
  opacity: 0;
}

.party {
  animation: party 1s infinite linear;
}

@keyframes party {
  0% {
    filter: hue-rotate(0deg);
  }
  100% {
    filter: hue-rotate(360deg);
  }
}
</style>
