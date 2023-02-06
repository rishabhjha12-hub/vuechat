<template>
  <div>
    <h2>Chat</h2>
    <ul>
    {{name}}
      <li v-for="message in messages">user:{{name}},message:{{ message }}</li>
    </ul>
  
    <form @submit.prevent="sendMessage">
      <input type="text" v-model="newMessage" required />
      <button type="submit">Send</button>
    </form>
  </div>
</template>

<script>
import axios from "axios";

export default {
  data() {
    return {
      messages: [],
      newMessage: ""
    };
  },
  created() {
    this.name = prompt("Enter your name:");
    this.fetchMessages();
  },
  methods: {
    sendMessage() {
      axios
        .post("http://localhost:3000/api/messages", { message: this.newMessage })
        .then(() => {
          this.newMessage = "";
        });
    },
    fetchMessages() {
      axios
        .get("http://localhost:3000/api/messages")
        .then(res => {
          this.messages = res.data.messages;
          setTimeout(this.fetchMessages, 1000);
        });
    }
  }
};
</script>
