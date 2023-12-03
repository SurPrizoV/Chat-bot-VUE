<template>
  <div id="app">
    <ChatWidget />
  </div>
</template>

<script>
import ChatWidget from "./components/ChatWidget.vue";

export default {
  name: "App",
  components: {
    ChatWidget,
  },
};
</script>

<style>
#app {
  text-align: center;
  padding: 20px;
  font-family: Avenir, Helvetica, Arial, sans-serif;
}
</style>