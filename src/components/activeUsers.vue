<template>
  <div class="card card-light w-40">
    <div class="card--header">
      <h3>Arkadaşlar</h3>
    </div>
    <div
      class="card--body list-group"
      @click="changeValue(i)"
      v-for="i in activeUser"
      :key="i.id"
    >
      <div
        class="active-user-item d-flex justify-content-start align-items-center"
      >
        <div
          class="badge d-flex justify-content-center align-items-center mr-5"
        >
          <img class="rounded-circle mr-5" :src="i.img" />
        </div>
        <button
          type="button"
          class="list-group-item list-group-item-action" :class="{'active': toUser.id ==i.id}"
          aria-current="true"
        >
          {{ i.username }}
        </button>
      </div>
    </div>
  </div>
</template>
<script>
import axios from "axios";
export default {
  props:["userInfo"],
  data() {
    return {
      userList: [],
      selectUser: [],
      toUser: "",
    };
  },
  methods: {
   
    changeValue(e) {
      console.log("USERİNFO",e);
      this.toUser = e;
      this.$emit("toUserChange", this.toUser);
    },
  },
  created() {
    axios
      .get("http://localhost:3000/users")
      .then((response) => {
        console.log("active list users", response);
        this.userList.push(...response.data);
      })
      .catch((e) => {
        console.log("e", e);
      });
  },
  computed:{
    activeUser(){
    return this.userList.filter((f)=>{
       return f.id !== this.userInfo[0].id
     })
    }
  }
};
</script>

<style scoped>
</style>