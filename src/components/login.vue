<template>
  <h1 class="text-center mt-5 text-size">MESSENGER</h1>

  <div>
    
    <div class="container centered">
      <div>
        <form class="px-4 py-3">
          <div class="mb-3">
            <label class="form-label white text-light font-weight-bold">Kullanıcı Adı</label>
            <input
              @keypress.enter="checkLogin"
              type="text"
              v-model="username"
              class="form-control"
            />
          </div>
          <div class="mb-3">
            <label class="form-label text-light font-weight-bold">Şifre</label>
            <input
              @keypress.enter="checkLogin"
              type="password"
              v-model="pswrd"
              class="form-control"
            />
          </div>
          <div class="mb-3"></div>
        </form>
        <button @click="checkLogin" class="btn btn-danger centered button font-weight-bold">
          Giriş yap
        </button>
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";
export default {
  emits:["user-info", "login"],
  data() {
    return {
      username: null,
      pswrd: null,
      userList: null,
      status: true,
    };
  },

  methods: {
    checkLogin() {
      if (this.pswrd == null || this.username == null) {
        alert("Lütfen Tüm alanları Doldurunuz!");
      } else {
        axios
          .get(
            "http://localhost:3000/users?password="+this.pswrd+"&username="+this.username
          )
          .then((res) => {
            this.userList = res.data;
            console.log(res);
            if (this.userList.length > 0) {
              this.status = false;
              this.$emit("login", this.status);
              this.$emit("user-info", this.userList);
            } else {
              alert("Hatalı kullanıcı adı/Şifre");
              this.status = true;
            }
          })
          .catch((e) => {
            console.log("e", e);
          });
      }
    },
      
  },
};
</script>
<style scoped>
.centered {
  margin-top: 10%;
}
.centered button {
  margin-left: 31%;
  margin-top: -10%;
}
.text-size{
  font-size: 56px !important;
}

</style>