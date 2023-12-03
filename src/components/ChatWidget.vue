<template>
  <div class="chat-widget">
    <div class="chat-header">Чат с ботом</div>
    <div class="chat-messages" ref="messagesContainer">
      <ChatMessage
        v-for="(message, index) in messages"
        :key="index"
        :message="message"
        :class="{ 'new-message': index === messages.length - 1 }"
      />
    </div>
    <div class="chat-input">
      <input v-model="userInput" @keyup.enter="sendMessage" placeholder="Напишите сообщение..." />
      <div class="button-container">
        <button v-for="(button, index) in buttons" :key="index" @click="handleButtonClick(button)">
          {{ button }}
        </button>
      </div>
    </div>
  </div>
</template>

<script>
import ChatMessage from "./ChatMessage.vue";

export default {
  name: "ChatWidget",
  components: {
    ChatMessage,
  },
  data() {
    return {
      userInput: "",
      messages: [
        { text: "Привет! Что я могу для Вас сделать?", isBot: true, showButtons: true },
      ],
      buttons: ["Заказать пиццу", "Установить будильник", "Вывести погоду"],
    };
  },
  methods: {
    sendMessage() {
      if (!this.userInput.trim()) return;

      const userMessage = { text: this.userInput, isBot: false };
      this.messages.push(userMessage);

      const botResponse = { text: `Хорошо, я ${this.getActionDescription(this.userInput)}. Что еще могу для вас сделать?`, isBot: true };
      this.messages.push(botResponse);

      this.userInput = "";

      this.$nextTick(() => {
        this.scrollToBottom();
      });
    },
    handleButtonClick(button) {
      if (!button) return; 

      const userMessage = { text: button, isBot: false };
      this.messages.push(userMessage);

      const botResponse = { text: `Хорошо, я ${this.getActionDescription(button)}. Что еще могу сделать?`, isBot: true };
      this.messages.push(botResponse);

      this.$nextTick(() => {
        this.scrollToBottom();
      });
    },
    getActionDescription(action) {
      const actionDescriptions = {
        "заказать пиццу": "приму заказ на пиццу",
        "установить будильник": "установлю будильник для вас",
        "вывести погоду": "предоставлю вам информацию о погоде"
      };

      return actionDescriptions[action.toLowerCase()] || `выполню "${action.toLowerCase()}"`;
    },
    scrollToBottom() {
      const container = this.$refs.messagesContainer;
      if (container) {
        container.scrollTop = container.scrollHeight;
      }
    },
    getVerbForAction(action) {
      const actionVerbs = {
        "заказать пиццу": "приму заказ на пиццу",
        "установить будильник": "установлю будильник для вас",
        "вывести погоду": "предоставлю вам информацию о погоде"
      };

      return actionVerbs[action.toLowerCase()] || action.toLowerCase();
    },
  },
  computed: {
    verbButtons() {
      return this.buttons.map(button => ({
        label: button,
        action: this.getVerbForAction(button),
      }));
    },
  },
  mounted() {
    this.scrollToBottom();
  },
};
</script>

<style scoped>
.chat-widget {
  max-width: 400px;
  margin: auto;
  border: 1px solid #ccc;
  border-radius: 5px;
  overflow: hidden;
}

.chat-header {
  background-color: #007bff;
  color: white;
  padding: 10px;
  text-align: center;
  font-size: 18px;
}

.chat-messages {
  padding: 10px;
  max-height: 300px;
  overflow-y: auto;
}

.chat-input {
  display: flex;
  flex-direction: column;
  padding: 10px;
}

.chat-input input {
  flex: 1;
  margin-bottom: 10px;
  padding: 8px;
  border: 1px solid #ccc;
  border-radius: 3px;
  outline: none;
}

.button-container {
  display: flex;
  gap: 10px;
}

.button-container button {
  flex: 1;
  padding: 8px;
  border: 1px solid #007bff;
  border-radius: 3px;
  background-color: #007bff;
  color: white;
  cursor: pointer;
}

.button-container button:hover {
  background-color: #0056b3;
}

.new-message {
  opacity: 0;
  animation: fadeIn 0.5s ease-out forwards;
}

@keyframes fadeIn {
  from {
    opacity: 0;
    transform: translateY(10px);
  }
  to {
    opacity: 1;
    transform: translateY(0);
  }
}
</style>