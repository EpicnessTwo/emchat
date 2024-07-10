<template>
  <div ref="chatBox" class="chat-box p-4 h-screen w-full overflow-hidden bg-purple-200">
    <transition-group name="list" tag="ul">
      <li v-for="(message, index) in messages" :key="message.id" class="mb-2 break-words">
        <ChatMessage
            :username="message.username"
            :message="message.message"
            :tags="message.tags"
            v-if="message.type === 'message'"
            v-once
        ></ChatMessage>
        <ChatNotice
          :username="message.username"
          :message="message.message"
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
const chatBox = ref(null)
const pruneLength = 20;

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
    // truncate username to max of 20 characters and add ellipsis if necessary
    tags['display-name'] = tags['display-name'].length > 16 ? tags['display-name'].substring(0, 20) + '...' : tags['display-name'];
    messages.value.push({
      type: 'message',
      id: tags.id,
      username: tags['display-name'],
      message: message,
      tags: tags,
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
      id: userstate.id,
      username: username,
      message: `An anonymous user has upgraded to a Tier 1 subscription!`
    });
  });

  client.on('cheer', (channel, userstate, message) => {
    messages.value.push({
      type: 'notice',
      id: userstate.id,
      username: userstate['display-name'],
      message: `Cheered ${userstate.bits} bits!\n${message}`
    });
  });

  client.on('giftpaidupgrade', (channel, username, sender, userstate) => {
    messages.value.push({
      type: 'notice',
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
        id: userstate.id,
        username: username,
        message: `Resubscribed!`
      });
      return;
    }

    messages.value.push({
      type: 'notice',
      id: userstate.id,
      username: username,
      message: `Resubscribed for ${months} months!`
    });
  });

  client.on('subgift', (channel, username, streakMonths, recipient, methods, userstate) => {
    messages.value.push({
      type: 'notice',
      id: userstate.id,
      username: username,
      message: `Gifted a subscription to ${recipient}!`
    });
  });

  client.on('submysterygift', (channel, username, numbOfSubs, methods, userstate) => {
    messages.value.push({
      type: 'notice',
      id: userstate.id,
      username: username,
      message: `Gifted ${numbOfSubs} Tier 1 Subscriptions!`
    });
  });

  client.on('subscription', (channel, username, method, message, userstate) => {
    messages.value.push({
      type: 'notice',
      id: userstate.id,
      username: username,
      message: `Subscribed!`
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
</style>
