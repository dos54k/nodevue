<template>
  <div>
    <h1>Get All Users</h1>
    <div v-if="users.length">
      <div>จำนวนผู้ใช้งาน: {{ users.length }}</div>
      <p>
        <button v-on:click="navigateTo('/user/create')">add new user</button>
      </p>
      <div v-for="user in users" v-bind:key="user.id">
        <div>id: {{ user.id }}</div>
        <div>ชื่อ สกุล :{{ user.name }} - {{ user.lastname }}</div>
        <div>email: {{ user.email }}</div>
        <div>password: {{ user.password }}</div>
        <p>
          <button v-on:click="navigateTo('/user/'+user.id)">view</button>
          <button v-on:click="navigateTo('/user/edit/'+ user.id)">edit</button>
          <button v-on:click="deleteUser(user)">delete</button>
        </p>
      </div>
    </div>
  </div>
</template>
<script>
import UsersService from "@/services/UsersService";
export default {
  data() {
    return {
      users: []
    };
  },
  async created() {
    this.users = (await UsersService.index()).data;
  },
  methods: {
    navigateTo(route) {
      this.$router.push(route);
    },
    async deleteUser(user) {
      let result = confirm("Want to delete?");
      if (result) {
        try {
          await UsersService.delete(user);
          this.refreshData();
        } catch (err) {
          console.log(err);
        }
      }
    },
    async refreshData() {
      this.users = (await UsersService.index()).data;
    }
  }
};
</script> <style scoped>
</style>