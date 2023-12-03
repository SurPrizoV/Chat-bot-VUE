<template>
    <div class="chat-message" :class="{ 'user-message': !message.isBot, 'bot-message': message.isBot }">
      {{ message.text }}
    </div>
  </template>
  
  <script>
  export default {
    name: "ChatMessage",
    props: {
      message: {
        type: Object,
        required: true,
      },
    },
  };
  </script>
  
  <style scoped>
  .chat-message {
    margin-bottom: 10px;
    padding: 10px;
    border-radius: 5px;
  }
  
  .user-message {
    background-color: #e0f7fa;
    align-self: flex-end;
  }
  
  .bot-message {
    background-color: #c8e6c9;
    align-self: flex-start;
  }
  </style>