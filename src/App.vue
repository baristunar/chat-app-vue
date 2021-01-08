<template>
  <div>
     <login
            v-if="loginIsActive"
            @user-info="pushList"
            @login="checkLoginStatus"
          />
    <div class="container">
      <div>
        <div>
         
        </div>
        <div
          class="chat--app--container d-flex justify-content-between align-items-start"
        >
          <chat-section
            :userInfo="userInfo"
            v-if="!loginIsActive"
            :changeToUser="changeToUser"
          />
          <active-users
            :userInfo="userInfo"
            @toUserChange="changeUser"
            v-if="!loginIsActive"
          />
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";
import chatSection from "./components/chatSection.vue";
import activeUsers from "./components/activeUsers.vue";
import login from "./components/login";

/* import Login from "./components/login.vue"; */
export default {
  components: {
    chatSection,
    activeUsers,
    login,
  },
  data() {
    return {
      userInfo: [],
      loginIsActive: true,
      changeToUser: null,
    };
  },
  methods: {
    checkLoginStatus(event) {
      this.loginIsActive = event;
    },
    pushList(userList) {
      this.userInfo.push(...userList);
    },
    changeUser(toUser) {
      this.changeToUser = toUser;
    },
  },
};
</script>
<style scoped>

</style>

