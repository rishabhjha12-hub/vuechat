<template>
  <div>
  <div><input v-model="username" placeholder="Enter username"/></div>
    <h1>Chat App</h1>
    <div v-for="message in messages">
      {{ !message.username?"anonyms":message.username }}: {{ message.text }}
    </div>
    <div>
      
      <input v-model="text" placeholder="Enter message" @keyup.enter="sendMessage" />
    </div>
  </div>
</template>

<script>
import axios from 'axios';

export default {
  data() {
    return {
      messages: [],
      username: '',
      text: ''
    };
  },
  mounted() {
    this.getMessages();
    setInterval(this.getMessages, 1000);
  },
  methods: {
    async getMessages() {
      try {
        const response = await axios.get('http://localhost:3000/api/messages');
        this.messages = response.data;
      } catch (error) {
        console.error(error);
      }
    },
    async sendMessage() {
      try {
        await axios.post('http://localhost:3000/api/messages', {
          username: this.username,
          text: this.text
        });
        this.text = '';
      } catch (error) {
        console.error(error);
      }
    }
  }
};
</script>
