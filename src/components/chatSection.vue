<template>
  <div class="card mr-10">
    <ul class="messages" ref="container">
      <li
        :class="{ 'current-user': userInfo[0].userID == i.userID }"
        v-for="i in chatLog"
        :key="i"
        class="text-color-black"
      >
        {{ i.mesaj }}
        <small>{{ i.time }}</small>
      </li>
    </ul>
    <div class="text-container d-flex justify-content-start align-items-start">
      <input @keypress.enter="sendMessage" v-model="message" type="text" />
      <button :disabled="isDisable" class="btn-primary" @click="sendMessage">
        Send
      </button>
    </div>
  </div>
</template>

<script>
import axios from "axios";
export default {
  props: ["userInfo", "changeToUser"],
  data() {
    return {
      chatLog: [],
      message: "",
    };
  },

  methods: {
    addZero(i) {
      if (i < 10) {
        i = "0" + i;
      }
      return i;
    },
    scrollToEnd() {
      let content = this.$refs.container;
      content.scrollTop = content.scrollHeight;
    },
    sendMessage() {
      const currentTime = new Date();
      const currentHour = this.addZero(currentTime.getHours());
      const currentMinute = this.addZero(currentTime.getMinutes());

      while (this.message !== "") {
        let chat = {
          userID: this.userInfo[0].userID,
          mesaj: this.message,
          to: this.changeToUser.userID,
          time: currentHour +":"+currentMinute
        };
        this.message = "";

        axios
          .post("http://localhost:3000/chat", chat)
          .then((save_message) => {
            console.log("save_message", save_message);
            this.chatLog.push(save_message.data);
          })
          .catch((e) => {
            console.log("e ", e);
          });
        console.log(chat);
      }
    },
  },
  computed: {
    isDisable() {
      return this.message == "" ? true : false;
    },
  },

  updated() {
    this.scrollToEnd();
  },
  mounted() {
    this.scrollToEnd();
  },
  watch: {
    changeToUser() {
      axios
        .get(
          "http://localhost:3000/chat?userID=" +
            this.userInfo[0].userID +
            "&to=" +
            this.changeToUser.userID +
            "&to=" +
            this.userInfo[0].userID +
            "&userID=" +
            this.changeToUser.userID
        )
        .then((response) => {
          console.log("response chat", response);
          this.chatLog = [];
          this.chatLog.push(...response.data);
        })
        .catch((e) => {
          console.log("e", e);
        });
    },
  },
};
</script>
<style scoped>
.text-color-black {
  color: black;
}
</style>
